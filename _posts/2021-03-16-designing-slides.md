---
layout: post
title: "Applying design guidelines to slides with {xaringanthemer}"
date: 2021-03-16
author: "Katie Jolly"
comments: true
---

At a recent [R-Ladies Seattle](https://rladiesseattle.org/) meetup,
Silvia Canelón gave a [great
presentation](https://silvia.rbind.io/2021-03-16-deploying-xaringan-slides/)
on getting {xaringan} slides set up and deployed with GitHub pages for
easy sharing. We didn’t quite have time for {xaringanthemer} so I wanted
to share a little in a blog post! My hope is that this post will help
you:

-   set up a pre-defined theme
-   edit that theme
-   add custom css code for things that can’t be modified with
    {xaringanthemer}

In order to walk through the process I’ll show you what I would do if I
wanted to use the poster for the meetup event at the design inspiration.

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/twitter_img_march.png" style="width:70.0%" />

Let’s say that you’re working on slides that have to adhere to specific
branding guidelines (colors, fonts, etc.). I’ll show you some tricks for
matching those as closely as possible! I find that’s one of the most
common reasons that people stick with tools like Powerpoint over
{xaringan}. And once you set up a theme you can use it over and over! It
can be included in things like an internal package if everyone on your
team needs to adhere to the same branding.

I wrote up an example of what you might see in a simple design spec
document:

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/design_specs.png" style="width:70.0%" />

From this you can see both the preferred font choices, how to use those
fonts, and the color palette. In the presentation itself I’ll be using
similar-enough fonts that are available on Google Fonts so that you
don’t have to download new fonts to your machine.

# Open a new document

First, make sure {xaringanthemer} is installed on your machine. Once it
is, open a new RMarkdown document and in the dialog box go to
`From Template` &gt; `Ninja Themed Presentation` and then click `OK`.

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/open_doc.png" style="width:40.0%" />

Once you have the document template, delete the filler text you don’t
need and edit the YAML header with your own information (title, name,
etc.).

# Setting theme options

When I look through the themes on the
[xaringanthemer](https://pkg.garrickadenbuie.com/xaringanthemer/index.html)
page I am looking for one that will let me specify two different
background colors based on the slide type. The best two options are:

`style_duo(...)`

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/example_duo.png" style="width:70.0%" />

and `style_duo_accent()`

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/example_duo_accent.png" style="width:70.0%" />

I do not want the white background on any slides so I’ll go with
`style_duo()`.

I’m editing the chunk labeled `xaringan-themer` in the template doc with
the example code from the {xaringanthemer} website:

``` r
style_duo(primary_color = "#1F4257", secondary_color = "#F97B64")
```

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/duo_default.png" style="width:45.0%" /><img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/duo_inverse.png" style="width:45.0%" />

## Specifying fonts

{xaringanthemer} makes it easy to add a font on your machine or through
the [Google Fonts](https://fonts.google.com/) collection. I like using
Google Fonts because other collaborators will more reliably see the same
slide design that you do if they run your code. There are tons of
resources online for choosing good fonts, one that I look is an article
from Typewolf: <https://www.typewolf.com/google-fonts>.

For the header font in these slides I like
[Martel](https://fonts.google.com/specimen/Martel?preview.text_type=custom),
serif, for the body font I like
[Lato](https://fonts.google.com/specimen/Lato?preview.text_type=custom),
sans serif, and [Fira
Mono](https://fonts.google.com/specimen/Fira+Mono?preview.text_type=custom),
monospace, for the inline code.

In the `style_duo()` function, you can add these fonts with
`..._font_google()`.

``` r
style_duo(primary_color = "#1F4257",
          secondary_color = "#F97B64",
          # fonts
          header_font_google = google_font("Martel"),
          text_font_google = google_font("Lato"),
          code_font_google = google_font("Fira Mono"))
```

Now the fonts are closer to what we are looking for!

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/fonts.png" style="width:70.0%" />

## Specifying colors

The background of the slides should be the light yellow color and the
inverse background will be dark yellow. The text should primarily be the
dark gray color with blue as a highlight color. I like to save the
colors as variables to use more easily in multiple places.

``` r
dark_yellow <- "#EFBE43"
light_yellow <- "#FDF7E9"
gray <- "#333333"
blue <- "#4466B0"
```

I’ll set the primary and secondary colors first to assess how the slides
are looking before setting other colors.

``` r
style_duo(
  # colors
  primary_color = light_yellow,
  secondary_color = dark_yellow

  # fonts
  header_font_google = google_font("Martel", "300", "400"),
  text_font_google = google_font("Lato"),
  code_font_google = google_font("Fira Mono")
)
```

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design//bg1.png" style="width:45.0%" />
<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/bg2.png" style="width:45.0%" />

The biggest issue I have with this design is that the contrast between
the yellows is not great. I’ll use blue instead as the highlight color
for links and slide headers and a lightened version of the gray for bold
text and inline code. So now I can edit the design to update the text
colors.

``` r
style_duo(
  # colors
  primary_color = light_yellow,
  secondary_color = dark_yellow,
  header_color = gray,
  text_color = gray,
  code_inline_color = colorspace::lighten(gray),
  text_bold_color = colorspace::lighten(gray),
  link_color = blue,
  title_slide_text_color = blue,

  # fonts
  header_font_google = google_font("Martel", "300", "400"),
  text_font_google = google_font("Lato"),
  code_font_google = google_font("Fira Mono")
)
```

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/bg1_edit.png" style="width:70.0%" />

This is much more readable!

## Custom css to edit the bullet point color

The slides are looking the way I was hoping for, but one of the things
missing from the original poster is the color of the bullet points. The
default is for them to be the same color as the text, but I want the
color to be determined independently so that the the bullets can be
yellow while the text is dark gray.

I only know basic css so I googled “css change bullet point color” and
came upon a page from
[css-tricks](https://css-tricks.com/finally-it-will-be-easy-to-change-the-color-of-list-bullets/).
I can take this example and modify it for this particular task by
changing the assigned color.

I added a new file `custom.css` to the same directory as my slides
document. In that document, I added one of the code chunks from the
article I linked.:

    ul {
      list-style: none;
    }

    li::before {
      content: "• ";
      color: red;
    }

But instead of red, I want to use \#EFBE43 (the dark yellow color):

    ul {
      list-style: none;
    }

    li::before {
      content: "• ";
      color: #EFBE43;
    }

Once I’ve created this file, I’ll add a link to it in the YAML header
for the slides in the already existing css line. Keep the link to the
original `xaringan-themer.css` as well, though!

`css: [xaringan-themer.css, custom.css]`

Now when I render the slides I see the updated bullet point styling

<img src="https://raw.githubusercontent.com/katiejolly/blog/master/assets/slide-design/ul_color.png" style="width:70.0%" />

There are tons of other edits that are possible, but this gives me a
template very close to the original design specs! Templating like this
makes {xaringan} an even better option for presentations that have to
adhere to specific guidelines. If you want to learn more about different
variables available in {xaringanthemer} styles, this reference on the
package website is a great resource:
<https://pkg.garrickadenbuie.com/xaringanthemer/articles/template-variables.html>.
