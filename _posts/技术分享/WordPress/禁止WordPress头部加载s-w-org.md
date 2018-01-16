---
title: 禁止WordPress头部加载s.w.org
date: 2016-10-17 05:49:24
tags: [WordPress,技术分享]
categories: 
- 技术分享
- WordPress
thumbnail: https://dl.dropbox.com/s/ju6fp7yj13acoms/wordpress.jpg?dl=0
thumbnailImagePosition: top
---
WordPress在头部添加dns-prefetch，应该是为了从s.w.org预获取表情和头像，目的是提高网页加载速度 ，但s.w.org国内根本无法访问，什么预获取、什么提高速度，都是泡影，不仅没用处，反而可能会影响速度，那就禁止它。
<!-- excerpt -->
![wordpress](https://dl.dropbox.com/s/5et1y0kkomei4wx/wordpress1.jpg?dl=0)

升级到WordPress 4.6之后，有童鞋发现头部加载了一个:
```
<link rel='dns-prefetch' href='//s.w.org'>
```
WordPress在头部添加dns-prefetch，应该是为了从s.w.org预获取表情和头像，目的是提高网页加载速度 ，但s.w.org国内根本无法访问，什么预获取、什么提高速度，都是泡影，不仅没用处，反而可能会影响速度，那就禁止它。

## 将下面的代码添加到主题functions.php模板中：
### 方法一
```
remove_action( 'wp_head', 'wp_resource_hints', 2 );
```
### 方法二
```
function remove_dns_prefetch( $hints, $relation_type ) {
if ( 'dns-prefetch' === $relation_type ) {
return array_diff( wp_dependencies_unique_hosts(), $hints );
}
return $hints;
}
add_filter( 'wp_resource_hints', 'remove_dns_prefetch', 10, 2 );
```
方法二貌似兼容性更好些。
附带一个禁止加载表情代码
```
// Remove emoji script
remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
remove_action( 'wp_print_styles', 'print_emoji_styles' );
add_filter( 'emoji_svg_url', '__return_false' );
```
参考：https://wordpress.org/support/topic/remove-the-new-dns-prefetch-code/