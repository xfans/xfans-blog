---
title: Using AndroidSVG with ImageView
author: xfans
type: post
date: 2017-09-29T11:35:09+00:00
url: /archives/8
views:
  - 661
categories:
  - Android
tags:
  - android
  - svg

---
<pre class="prism-highlight line-numbers" data-start="1"><code class="language-null">public class MainActivity extends Activity
{

   @Override
   protected void onCreate(Bundle savedInstanceState)
   {
      super.onCreate(savedInstanceState);

      ImageView  imageView = (ImageView) findViewById(R.id.yourImageView);
      imageView.setLayerType(View.LAYER_TYPE_SOFTWARE, null);
      try
      {
         SVG svg = SVG.getFromResource(this, R.raw.my_svg_file);
         Drawable drawable = new PictureDrawable(svg.renderToPicture());
         imageView.setImageDrawable(drawable);
      }
      catch(SVGParseException e)
      {}
   }

}
</code></pre>

地址:http://bigbadaboom.github.io/androidsvg/use\_with\_ImageView.html  
Github:https://github.com/BigBadaboom/androidsvg