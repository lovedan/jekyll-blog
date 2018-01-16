---
title: 获取WordPress当前分类文章所有标签
date: 2016-10-17 04:25:16
tags: [WordPress,技术分享]
categories: 
- 技术分享
- WordPress
thumbnail: https://dl.dropbox.com/s/ju6fp7yj13acoms/wordpress.jpg?dl=0
---
如果需要在分类列表页面，显示当前分类文章中添加的所有标签，方便读者阅读自己喜欢的内容，下面的代码可以帮你实现这个功能。
获取WordPress当前分类文章所有标签
<!--more-->

首先，在主题functions.php模板文件中添加以下函数：
```
function get_category_tags($args) {
    global $wpdb;
    $tags = $wpdb->get_results
    ("
        SELECT DISTINCT terms2.term_id as tag_id, terms2.name as tag_name
        FROM
            $wpdb->posts as p1
            LEFT JOIN $wpdb->term_relationships as r1 ON p1.ID = r1.object_ID
            LEFT JOIN $wpdb->term_taxonomy as t1 ON r1.term_taxonomy_id = t1.term_taxonomy_id
            LEFT JOIN $wpdb->terms as terms1 ON t1.term_id = terms1.term_id,
            $wpdb->posts as p2
            LEFT JOIN $wpdb->term_relationships as r2 ON p2.ID = r2.object_ID
            LEFT JOIN $wpdb->term_taxonomy as t2 ON r2.term_taxonomy_id = t2.term_taxonomy_id
            LEFT JOIN $wpdb->terms as terms2 ON t2.term_id = terms2.term_id
        WHERE
            t1.taxonomy = 'category' AND p1.post_status = 'publish' AND terms1.term_id IN (".$args['categories'].") AND
            t2.taxonomy = 'post_tag' AND p2.post_status = 'publish'
            AND p1.ID = p2.ID
        ORDER by tag_name
    ");
    $count = 0;
    if($tags) {
        foreach ($tags as $tag) {
            $mytag[$count] = get_term_by('id', $tag->tag_id, 'post_tag');
            $count++;
        }
    } else {
      $mytag = NULL;
    }
    return $mytag;
}
```

编译：http://www.ludou.org/wordpress-get-tags-specific-to-category.html
源代码出自：https://wordpress.org/support/topic/get-tags-specific-to-category

其次，将下面调用输出代码，添加到主题archive.php模板适当位置：

```
<?php
    $cat= single_cat_title('', false);
    $args = array( 'categories' => get_cat_ID($cat));
    $tags = get_category_tags($args);
    $content .= "<ul class='cat-tag'>";
    if(!empty($tags)) {
        foreach ($tags as $tag) {
            $content .= "<li><a href=\"".get_tag_link($tag->term_id)."\">".$tag->name."</a></li>";
        }
    }
    $content .= "</ul>";
    echo $content;
?>
```

个人感觉放到头部调用函数：
```
<?php get_header(); ?>
```

下面比较合适。
最后，再适当加上样式即可：

```css
.cat-tag{
    float: left;
    width: 100%;
}
.cat-tag li a{
    float: left;
    margin: 0 5px;
}
```
