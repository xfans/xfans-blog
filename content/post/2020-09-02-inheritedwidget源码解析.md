---
title: Flutter InheritedWidget源码解析
author: xfans
type: post
date: 2020-09-02T12:05:34+00:00
url: /archives/352
categories:
  - Flutter

---
## 作用

`InheritedWidget`是一个在`Widget`树中，从上往下传递信息的小部件。

主要从两个方面理解源码：

**1.子`Widget`可以获取到父控件中的`InheritedWidget`，从而获取到InheritedWidget的信息。**

**2.子`Widget`调用`dependOnInheritedWidgetOfExactType`后，当父`InheritedWidget`重建后，会调用子控件重建。**

## 原理

### 1.子控件可以获取到父控件中的`InheritedWidget`

在`Element`中有个Map：

<pre class="wp-block-preformatted"><code>Map&lt;Type, InheritedElement&gt; _inheritedWidgets;</code></pre>

其中保存了该`Widget`的父节点中的`InheritedWidget`。会在`mount`\`activate`

方法中赋值，赋值的代码如下：

在`Element`中

<pre class="wp-block-preformatted">void _updateInheritance() {
  assert(_active);
  //获取到父节点的_inheritedWidgets
  _inheritedWidgets = _parent?._inheritedWidgets;
}</pre>

在`InheritedElement`中

<pre class="wp-block-preformatted">@override
void _updateInheritance() {
  assert(_active);
  final Map&lt;Type, InheritedElement&gt; incomingWidgets = _parent?._inheritedWidgets;
  if (incomingWidgets != null)
    _inheritedWidgets = HashMap&lt;Type, InheritedElement&gt;.from(incomingWidgets);
  else
    _inheritedWidgets = HashMap&lt;Type, InheritedElement&gt;();
  //将自身保存在_inheritedWidgets中
  _inheritedWidgets[widget.runtimeType] = this;
}</pre>

经过上面两个方法，`Widget`本身就可以通过`_inheritedWidgets`获取到`InheritedElement`了

### 2.子`Widget`调用`dependOnInheritedWidgetOfExactType`后，当父`InheritedWidget`重建后，会调用子控件重建

在`InheritedElement`中有个Map：

<pre class="wp-block-preformatted"><code>final Map&lt;Element, Object&gt; _dependents = HashMap&lt;Element, Object&gt;();</code></pre>

当子`Widget`调用`dependOnInheritedWidgetOfExactType`方法时，会依次调用`dependOnInheritedElement`方法，`InheritedElement`的`updateDependencies`方法，`setDependencies`方法。将自己注册到父`InheritedElement`的`_dependents`中。

<pre class="wp-block-preformatted">T dependOnInheritedWidgetOfExactType&lt;T extends InheritedWidget&gt;({Object aspect}) {
  assert(_debugCheckStateIsActiveForAncestorLookup());
  final InheritedElement ancestor = _inheritedWidgets == null ? null : _inheritedWidgets[T];
  if (ancestor != null) {
    assert(ancestor is InheritedElement);
    return dependOnInheritedElement(ancestor, aspect: aspect) as T;
  }
  _hadUnsatisfiedDependencies = true;
  return null;
}
@override
InheritedWidget dependOnInheritedElement(InheritedElement ancestor, { Object aspect }) {
  assert(ancestor != null);
  _dependencies ??= HashSet&lt;InheritedElement&gt;();
  _dependencies.add(ancestor);
  ancestor.updateDependencies(this, aspect);
  return ancestor.widget;
}
@protected
void updateDependencies(Element dependent, Object aspect) {
  setDependencies(dependent, null);
}
@protected
void setDependencies(Element dependent, Object value) {
  _dependents[dependent] = value;
}</pre>

当`InheritedElement`重建时会调用`updated`方法，然后会调用到`notifyClients`方法，最终调用`Widget didChangeDependencies`方法并重建子`Widget`。

<pre class="wp-block-preformatted">void updated(InheritedWidget oldWidget) {
  if (widget.updateShouldNotify(oldWidget))
    super.updated(oldWidget);
}
void notifyClients(InheritedWidget oldWidget) {
  assert(_debugCheckOwnerBuildTargetExists('notifyClients'));
  for (final Element dependent in _dependents.keys) {
    assert(() {
    // check that it really is our descendant
    Element ancestor = dependent._parent;
    while (ancestor != this && ancestor != null)
    ancestor = ancestor._parent;
    return ancestor == this;
    }());
    // check that it really depends on us
    assert(dependent._dependencies.contains(this));
    notifyDependent(oldWidget, dependent);
  }
}
void notifyDependent(covariant InheritedWidget oldWidget, Element dependent) {
  dependent.didChangeDependencies();
}</pre>

由上边的代码可以看出，只有当子Widget调用了dependOnInheritedWidgetOfExactType才会收到InheritedWidget的重建通知。