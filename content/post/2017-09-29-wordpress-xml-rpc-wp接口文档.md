---
title: WordPress XML-RPC wp接口文档
author: xfans
type: post
date: 2017-09-29T11:37:02+00:00
url: /archives/14
views:
  - 1609
categories:
  - Web
tags:
  - web

---
外边慢,备份下

## wp.getUsersBlogs

Retrieve the blogs of the users.

### Parameters

  * string username
  * string password

### Return Values

  * array 
      * struct 
          * boolean isAdmin
          * string url
          * string blogid
          * string blogName
          * string xmlrpc

<!--more-->

## wp.getTags

Get list of all tags.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * array 
      * struct 
          * int tag_id
          * string name
          * int count
          * string slug
          * string html_url
          * string rss_url

## wp.getAuthors

Get an array of users for the blog.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * array 
      * struct 
          * int user_id
          * string user_login
          * string display_name
          * string meta_value (Serialized PHP data)
      * ...

## wp.getOptions

Retrieve blog options. If passing in a struct, search for options listed within it.

This call return also settings available in the Media Settings area of wp-admin. For example: If a user has specified properties for Image Sizes such as Thumbnail Size, Medium Size, and Large Size, this call would return those properties.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct 
      * string option

### Return Values

  * array 
      * struct 
          * string desc
          * string readonly
          * string option

## wp.setOptions

Update blog options. Returns array of structs showing updated values.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct 
      * string option_name
      * string option_value

### Return Values

  * array 
      * struct 
          * string option
          * string value

## wp.getPostStatusList

Retrieve post statuses.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * struct 
      * const string 'draft'
      * const string 'pending'
      * const string 'private'
      * const string 'publish'

## wp.getPostFormats

Retrieves a list of post formats used by the site. A filter parameter could be provided that would allow the caller to filter the result. At this moment the only supported filter is 'show-supported' that enable the caller to retrieve post formats supported by the active theme.

### Parameters

  * int blog_id
  * string username
  * string password
  * Struct 
      * const string 'show-supported'

### Return Values

When no filter is specified

  * struct

e.g. [standard] => Default [aside] => Aside [chat] => Chat => Gallery [link] => Link [image] => Image [quote] => Quote [status] => Status => Video

When a filter is specified

  * struct 'all' 
      * struct
  * struct 'supported' 
      * struct

## wp.getComments

Gets a set of comments for a given post.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct 
      * post_id
      * status (defaults to approve)
      * offset
      * number

### Return Values

Returns an array of the comment structure (see wp.getComment)

  * struct 
      * datetime dateCreated (ISO.8601, always GMT)
      * string user_id
      * string comment_id
      * string parent
      * string status
      * string content
      * string link
      * string post_id
      * string post_title
      * string author
      * string author_url
      * string author_email
      * string author_ip

## wp.getCommentCount

Retrieve comment count for a specific post.

### Parameters

  * int blog_id
  * string username
  * string password
  * string post_id

### Return Values

  * array 
      * struct 
          * int approved
          * int awaiting_moderation
          * int spam
          * int total_comments

## wp.getComment

Gets a comment, given it's comment ID. Note that this isn't in 2.6.1, but is in the HEAD (so should be in anything newer than 2.6.1)

### Parameters

  * int blog_id
  * string username
  * string password
  * int comment_id

### Return Values

  * struct 
      * datetime dateCreated (ISO.8601, always GMT)
      * string user_id
      * string comment_id
      * string parent
      * string status
      * string content
      * string link
      * string post_id
      * string post_title
      * string author
      * string author_url
      * string author_email
      * string author_ip

## wp.deleteComment

Remove comment.

### Parameters

  * int blog_id
  * string username
  * string password
  * int comment_id

### Return Values

  * boolean status

## wp.editComment

Edit comment.

### Parameters

  * int blog_id
  * string username
  * string password
  * int comment_id
  * struct comment 
      * string status
      * date date\_created\_gmt
      * string content
      * string author
      * string author_url
      * string author_email

### Return Values

  * boolean status

## wp.newComment

Create new comment.

If you want to send anonymous comments, leave the second and third parameter blank and install [a filter to xmlrpc\_allow\_anonymous_comments to return true][1].

See this WordPress forum [post][2] for more details.

### Parameters

  * int blog_id
  * string username
  * string password
  * int post_id
  * struct comment 
      * int comment_parent
      * string content
      * string author
      * string author_url
      * string author_email

### Return Values

  * int comment_id

## wp.getCommentStatusList

Retrieve all of the comment status.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * struct 
      * string hold
      * string approve
      * string spam

## wp.getPageStatusList

Retrieve all of the WordPress supported page statuses.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * struct 
      * const string 'draft'
      * const string 'private'
      * const string 'publish'

## wp.getPageTemplates

Retrieve page templates.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * array 
      * struct 
          * string name
          * string description

## wp.getPage

Get the page identified by the page id.

### Parameters

  * int blog_id
  * int page_id
  * string username
  * string password

### Return Values

  * struct 
      * datetime dateCreated (ISO.8601)
      * string userid
      * int page_id
      * string page_status
      * string description
      * string title
      * string link
      * string permaLink
      * array categories 
          * string Category Name
          * ...
      * string excerpt
      * string text_more
      * int mt\_allow\_comments
      * int mt\_allow\_pings
      * string wp_slug
      * string wp_password
      * string wp_author
      * int wp\_page\_parent_id
      * string wp\_page\_parent_title
      * int wp\_page\_order
      * string wp\_author\_id
      * string wp\_author\_display_name
      * datetime date\_created\_gmt
      * array custom_fields 
          * struct 
              * string id
              * string key
              * string value
          * ...
      * string wp\_page\_template

## wp.getPages

Get an array of all the pages on a blog.

### Parameters

  * int blog_id
  * string username
  * string password
  * int max_pages (optional, default=10)

### Return Values

  * array 
      * struct Same as [wp.getPage][3]
      * ...

## wp.getPageList

Get an array of all the pages on a blog. Just the minimum details, lighter than [wp.getPages][4].

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * array 
      * struct 
          * int page_id
          * string page_title
          * int page\_parent\_id
          * datetime dateCreated
      * ...

## wp.newPage

Create a new page. Similar to metaWeblog.newPost.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct content 
      * string wp_slug
      * string wp_password
      * int wp\_page\_parent_id
      * int wp\_page\_order
      * string wp\_author\_id
      * string title
      * string description (content of post)
      * string mt_excerpt
      * string mt\_text\_more
      * int mt\_allow\_comments (0 = closed, 1 = open)
      * int mt\_allow\_pings (0 = closed, 1 = open)
      * datetime dateCreated
      * array custom_fields 
          * struct
          * Same struct data as custom_fields in [wp.getPage][3]
  * bool publish</p> 

### Return Values

  * int page_id

## wp.deletePage

Removes a page from the blog.

### Parameters

  * int blog_id
  * string username
  * string password
  * int page_id

### Return Values

  * bool true

## wp.editPage

Make changes to a blog page.

### Parameters

  * int blog_id
  * int page_id
  * string username
  * string password
  * struct content 
      * Same struct data as [newPage][5]
  * bool publish

### Return Values

  * bool true

## wp.getCategories

Get an array of available categories on a blog.

### Parameters

  * int blog_id
  * string username
  * string password

### Return Values

  * array 
      * struct 
          * int categoryId
          * int parentId
          * string description
          * string categoryName
          * string htmlUrl
          * string rssUrl
      * ...

## wp.newCategory

Create a new category.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct 
      * string name
      * string slug
      * int parent_id
      * string description

### Return Values

  * int category_id

## wp.deleteCategory

Delete a category.

### Parameters

  * int blog_id
  * string username
  * string password
  * int category_id

### Return Values

  * boolean

True on success, false on failure.

## wp.suggestCategories

Get an array of categories that start with a given string.

### Parameters

  * int blog_id
  * string username
  * string password
  * string category
  * int max_results

### Return Values

  * array 
      * struct 
          * int category_id
          * string category_name
      * ...

## wp.uploadFile

Upload a file.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct data 
      * string name
      * string type
      * base64 bits
      * bool overwrite

### Return Values

  * struct 
      * string id (Added in WordPress 3.4)
      * string file
      * string url
      * string type

## wp.getMediaLibrary

This call get a list of items in the user's Media Library with IDs, titles, descriptions, remote links, and any other relevant metadata. A filter parameter could be provided that would allow the caller to filter based on content type, file size, or other properties.

### Parameters

  * int blog_id
  * string username
  * string password
  * struct filter (optional) 
      * int number
      * int offset
      * int parent_id
      * string mime_type (e.g., 'image/jpeg', 'application/pdf')

### Return Values

  * array 
      * struct Same as wp.getMediaItem

## wp.getMediaItem

This call would get a specific item in the user's Media Library by providing an ID. The call would return the item's ID, title, description, remote link, and any other available metadata.

### Parameters

  * int blog_id
  * string username
  * string password
  * int attachment_id

### Return Values

  * struct 
      * date\_created\_gmt
      * parent
      * link
      * thumbnail
      * title
      * caption
      * description
      * metadata

 [1]: http://www.thepicklingjar.com/code/anonymous-xmlrpc-comments/
 [2]: http://wordpress.org/support/topic/304306?replies=1#post-1188046
 [3]: /XML-RPC_wp#Return_Values "XML-RPC wp"
 [4]: /XML-RPC_wp#wp.getPages "XML-RPC wp"
 [5]: /XML-RPC_wp#Parameters_3 "XML-RPC wp"