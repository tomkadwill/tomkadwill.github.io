---
layout: post
title:  "How to Override CSS Styles in Jekyll"
date:   2017-12-16 18:45:00 +0000
---

There are many themes available for Jekyll, allowing you to choose the look and feel of your site. In addition to picking a theme, Jekyll allows you to further customise the style of your site. Here are the steps required to override a theme's CSS:

1. Look at the `/_site/assets` directory to see what CSS files the theme is using. Alternatively you could view the source of your site and check which stylesheets are being included. For example, my theme has a single CSS file, `/_site/assets/main.css`, which is the file that I need to 'override'.

2. Next, create a SCSS file, with the same name, under the root dir. In my case I have a file called `/_site/assets/main.css` so I need to create a file called `/assets/main.scss`. The reason for using a SCSS file is that I want to include the existing styles so that I only have to change specific styles. SCSS files make it easy to include other stylesheets.

3. At the top of the SCSS file add the following code:

    ```css
    ---
    ---

    @import "{{ site.theme }}";
    ```

    The two lines of triple dashes tell Jekyll to replace the existing CSS file. In my case it converts the `/assets/main.scss` file into a CSS file and uses it to replace the old `/_site/assets/main.css` file.

    The `@import` line tells Jekyll to import the existing site styles. This means that you don't have to define a completely new set of site styles.

4. Now you're ready to add or modify your site's stylesheet. Just add your code under the `@import` line. Like so:

    ```css
    ---
    ---

    @import "{{ site.theme }}";

    table {
      border-collapse: collapse;
    }

    table, td, th {
      border: 1px solid black;
    }
    ```
