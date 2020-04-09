---
title: "Hugo"
date: 2020-04-08T20:31:23+05:30
categories: ['miscellaneous', 'static site']
tags: ['Hugo']
---
## Get Started
Create new site:
```bash
hugo new site <name>
cd <name>
```
Download required them into `themes` folder:
You can choose to create git submodule or clone the theme
```bash
git clone/sub module http://theme-from-github into themes/<name>
```
Update `config.yaml` to choose the downloaded theme.
```yaml
theme:<name>
```

Start hugo server:
```bash
hugo server
```

-------------------------------
## Create Content
All your site content resides in folder `content/`
```bash
hugo new <name>.md
```

{{% notice note %}}
Each content file created will have metadata associated known as frontmatter. It gets populated as defined in `archetype/_default.md`
{{% /notice%}}

----------------------
## List vs Single Pages
hugo categorises it's pages into two type. 
+ List pages
+ Single Pages

You can define whole site using these categorisation. All your folders gets list page format while individual files gets single page format.

{{% notice note %}}
List pages are created by default to all root level folders.<br>
But sub level folders you can create list page by adding `dir/_index.md` file.<br >
`_index.md` file can also be used to override the default content which gets populated in list pages.
{{% /notice%}}

---------------------------
## Front Matters
`archetype/_defaul.md` has all frontmatter defined. By default it gets applied to all the content files created.

You can modify or add to it, say to define custom parameters.

Also if you want custom frontmatter definition for specific folders you can create `archetype/<folder_name>.md` file for <foder_name> specific archetype. 

Content inside `<folder_name>` gets these frontmatter applied.

----------------
## Short Codes

{{% notice note %}}
In your content.md file you can include html tags. 
{{% /notice%}}

To keep your Markdown file clean from html tags we can use `Short Codes`
These are handy html code converter which populates actual html tags at compile time
**Syntax:** 
```md
{{</* example-shortcode param1 param2 ... */>}}
```
eg:for youtube iframe
```md
{{</* youtube id_of_video */>}}
```
You can also create custom shortcodes using `layouts/shortcodes/<name>.html` files

{{% notice note %}}
To enable markdown syntax inside shortcodes use `{{%/* **BOLD** */%}}`
{{% /notice%}}

---------------
## Taxonomies
This is a way to organize your site content into logical groups.
hugo by default provides two taxonomies `tags` and `categories`.

You can define in the `front-matter` to which `tags` and `categories` the page belongs.

Also for each `tags` and `categories` a default list page gets created.

**Note:**

- You can also create custom `taxonomy` too.
- Include the new taxonomy into config.yml and then page for that taxonomy also starts to appear.

`config.yaml`
```yaml
taxonomies:
  - tag: tags
  - category: categories
  - mood: moods
# - singular: plural  
```

---------------------
## Layouts or Templates
These are used to change the look and feel of the website.

You can use existing theme templates or customize either of them.

You can completely define your own theme by doing this.

To override the theme supplied templates
- Single Page Template
  + `layouts/_default/single.html`

- List Page Template
  + `layouts/_default/list.html`
- Home page: (is a special type of list page)
  + `layouts/index.html`
---
## Section Template
`layouts/_default` folder applicable to all the pages in the site.

But if you want a custom template for pages in folder `content/dir1` you can do so by defining templates in `layouts/dir1`. 

This is following the same pattern as we do for `archetypes` to control the `frontmatter`.

---
## BaseOf template and Blocks
Baseof as the name says acts as base of whole of your site. This is the best way for modularization your site.


For example if there are common elements in both `list.html` and `single.html` you can define then in `layout/_default/baseof.html`.

For section specific you can also do `layout/section/baseof.html`.

You define blocks in your _single/list_ templates and place them in _baseof_ template.
`baseof.html`
```html
{{ block "header" .}}
    This is baseof header
{{ end}}
{{ block "main" .}}
     This is baseof default content. i.e HTML
{{ end}}
{{ block "header" .}}
     This is baseof footer
{{ end}}
```

`list/single.html`
```html
{{ define "main"}}
       This is overriding list/single page main block.
{{end}}
```

---
## Variables
Variables can be define in frontmatter of each content page: `<key>: <value>`.

These can be access in template files _single/list_ as : `{{ .Params.<key> }}`

You can also define variables in template files: `{{ $key := "value"}}`. Here values can be _string_, _bool_ or _numrical_ types.

You can also use them in conjunction with HTML elements.
```html
{{ $color := "red" }}
<p style="color: {{$color}};"> This will be shown in red</p>
```

Hugo categorizes variables in many ways: https://gohugo.io/variables

---
## Functions
Can only be used in template files
`single.html`
```html
{{ truncate 10 "This is a very long string"}}
{{ add 1 5}}
{{ singularise "dogs"}}
```
`list.html`
```html
{{ range .Pages}}
     {{.Title}}<br>
{{end}}
```

-----------
## Conditionals
Again can only be used in templates
`single.html`
```html
{{ $a :=1}} {{ $b :=2}} {{ $a :=3}}
{{ if and (gt $a $b) (gt $a $c)}}
         A is the greatest
{{ else if (gt $b $c)}}
               B is the greatest
         {{else}}
               C is the greatest
         {{end}}
{{end}}
```

`list.html`
```html
{{ $title := .Title}}
{{ range .Site.Pages}}
        <li style = "{{ if eq .Title @title}} background-color: yellow {{end}} "> {{.Title}}</li>
{{end}}
```

------------
## Data
You can put json, yaml, toml files in `data/` folder and access using variable name `.Site.Data.<file_name>`.

It acts like a input data to your site and **not like a Data Base**

Again only accessible in templates.

-------------------
## Partials
baseof blocks gives highest level of modularity. But partials is even more flexible.

Define common html abstraction templates in folder called `layouts/partials/<name>.html`. Now you can invoke those html partial templates in other templates.

For example define `layouts/partials/header.html` as 
```html
<p>{{.myKey}}</p>
```

Use in `layouts/_default/single.html` as
```html
{{ partials "header" (dict "myKey" "myValue")}}
```

As you can see we have passed named parameter `myKey` to our defined partial and used it. Generated HTML will have `<p>myValue</p>`

--------------------
## Custom Short Codes
For layouts you have baseof, partials, etc for abstraction.

For content.md files you can only have shortcodes.

Define custom shortcodes in `layouts/shortcodes/<name>.html` folder.

ShortCode Examples:
- Type 1 `layouts/shortcodes/highlight.html`. Simple Shortcodes
```html
{{$color:= .Get "color"}}
<p style="{{ if (.Get `color`)}} background-color: $color{{end}}">
         {{.Get "Titile"}}
</p>
```

- Type 2 `layouts/shortcodes/inner.html`. Enclosing Shortcodes
```html
<p style="color: green;">{{ .Inner }}</p>
```

Usage: 

`content/a.md`
```md
This is a.md

{{</* highlight text= "I am using Type 1 Shortcode" color="yellow" */>}}

{{</* inner */>}}
    I am using Type 2 ShortCode. 
{{</* /inner */>}}
```