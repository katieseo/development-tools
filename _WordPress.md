https://developer.wordpress.org/themes/basics/main-stylesheet-style-css/

https://developer.wordpress.org/themes/block-themes/creating-new-themes-using-the-site-editor/

https://developer.wordpress.org/reference/functions/wp_enqueue_style/


Plugin: Elementor Website Builder, Gutenburg

1. create files
```
assets
 - css
      > blocks.css
 - img
 - fonts
parts
   > footer.html, header.html
templates
   > 404.html, archive.html, index.html, page.html, search.html, single.html
   
functions.php
index.php
screenshot.png
styles.css
theme.json
```

2. modify styles.css , funcntions.php > upload theme and activate
3. pages > create 'Home' page
3. settings > reading settings > select 'a static page', select Homepage: 'Home'
4. Permalinks > select Postname or Custom Structure /blog/%postname%/

5.Apperance > Editor
   - template - page - add 'Post Content' block
   - pages > Home > add heading h1-h6, and button
   
6. go to theme.json file (Tools > Theme File Editor) and edit

---
---
---


## Custom

- OceanWP
- Elementor
- Elementor header&footer
- All-in-one WP migration for backup

## Locally install


#### DevKinsta (Docker)


#### XAMPP
https://www.apachefriends.org/download.html

1. Start 
-MySQL Database
-ProFTPD
-Apache Web Server

2. Go to Application

3. phpMyAdmin
localhost/phpmyadmin

4. Databases tab > "WordPress" Create

5. Download WordPress > unzip > move to xampp "Open application folder" > htdocs folder (change the folder name to projectname)

6. http://localhost/projectname > install 
- username: root, password empty
- copy following text to wordpress/wp-config-sample.php > change name to wp-config.php > run install


## Check List
http://localhost/wordpress/wp-admin/

* ico
1. setting - title, tagline
2. setting - media setting (image sizes)
3. Add content
   -add a page > settings > reading settings > your homepage display: a static page (select the page)
4. Appreance/Menus > Create Menu (Main Menu) > Select primary menu
   (Add wp_nav_menu function > Appreance/Menus > screen options, check css classes and add class name)
5. custom-logo: go to theme and select theme, [customize] > site identity > Add Logo, Select Icon > Publish


## Custom Fields

Custom Post Type UI 
ACF

