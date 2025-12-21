+++
title = "Blogging with Emacs Org-mode and Hugo"
author = ["Katabex"]
date = 2025-12-21T00:00:00+01:00
tags = ["Hugo", "Emacs"]
draft = false
+++

I have recently updated this site from just being a simple about page written in plain html and CSS to a statically generated site.

Finding a work flow that works seamlessly is always a challenging task. My only starting assumption was that I want to write using familiar Emacs Org-mode environment and post with as little hassle as possible focusing on the content and not on the mechanics of running the website.

After investigating a number of setups I have settled on using [Hugo](<https://gohugo.io/>) static site generator combined with [ox-hugo](<https://ox-hugo.scripter.co/>) Org-mode exporter package. This setup allows me to convert Org-mode formated text file containing all my posts to a number of Markdown files that are formated in a way that can be used by Hugo to generate the site automatically. The only additional manual step required is to push the site to GitHub when ready.


## Installing ox-hugo {#installing-ox-hugo}

Installing ox-hugo in Doom Emacs is simple, just enable it in the '.config/doom/ini.el' config file.

```emacs-lisp
(doom! :input
       (org +hugo)
       )
```

Execute 'doom sync' to install the package and restart emacs.


## Org-mode posts file {#org-mode-posts-file}

You may use separate Org-mode files for each of the posts but the preferred way is to use a single file (one post per Org tree) with the structure described below.

Include the following parameters at the top of our Org-mode file to set the destination where the exported Markdown files need to land in the Hugo site folder structure.

```org
    #+hugo_base_dir: ~/sites/your-hugo-site-home-folder/
    #+hugo_section: posts
```

The example above results in the following destination path: `~/sites/hugo-site-home-folder/content/posts/`. All posts included in the Org-file will end there as separate Markdown files ready to be processed by Hugo to render the site.

Each post will start with the main header containing the title of the post and list of tags and categories: `* My post one title :tag1:@cat1:` plus the following parameters below the title.

```org
    :PROPERTIES:
    :EXPORT_FILE_NAME: my_post_file_name
    :EXPORT_DATE: 2025-07-07
    :END:
```

Once the post is ready you can use `C-c C-e H H` to export the post that you curently edit or `C-c C-e H A` to export all posts in the file.

The basic file example including two posts is shown below.

```org
   #+hugo_base_dir: ~/sites/your-hugo-site-home-folder/
   #+hugo_section: posts

   * My first post :tag1:@category1:
   :PROPERTIES:
   :EXPORT_FILE_NAME: my-first-post-file-name
   :EXPORT_DATE: 2025-07-07
   :END:
   This is my first post body

    * My second post :tag2:@category2:
   :PROPERTIES:
   :EXPORT_FILE_NAME: my-second-post-file-name
   :EXPORT_DATE: 2025-08-07
   :END:
   This is my second post body
```


## Extras {#extras}

There are many parameters and configuration options that can be used to fine-tune the export process. One of such options is the [auto-export on save](<https://ox-hugo.scripter.co/doc/auto-export-on-saving/>) that can be enabled to streamline the process of exporting the posts, auto generating the site and live previewing it in a browser. To find out more visit [ox-hugo manual](<https://ox-hugo.scripter.co/>) page.


## Relevant links {#relevant-links}

-   [ox-hugo manual](<https://ox-hugo.scripter.co/>)
-   [ox-hugo manual in Org-mode format](<https://github.com/kaushalmodi/ox-hugo/raw/main/doc/ox-hugo-manual.org>)
