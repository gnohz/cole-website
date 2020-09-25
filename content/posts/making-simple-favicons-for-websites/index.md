---
title: "Making Simple Favicons for Websites"
date: 2020-06-13T01:01:00-04:00
draft: false
tags: ["web-dev", "imagemagick"]
categories: ["Software-Dev"]
---

# Favicon Time

I'm working on a [side project](https://artif.ai) and it came the time to find a favicon (at least a temporary one so I could stop looking at the create-react-app favicon).

The process wasn't particularly complex, but it would have helped me out to have the following presented so I figured I'd share it here.

# Making the Favicon

Early on, you can't go wrong with [favicon.io](https://favicon.io) for all your favicon needs. It won't give you anything very sophisticated, but definitely enough to get off the ground. That being said, be weary of the following saying:

<blockquote>
Nothing is more permanent than a temporary solution.
</blockquote>

## favicon.io

favicon.io basically takes input text, fits it into a shape of your choosing, and makes it easy to save the result in a variety of favicon formats.

{{< image src="favicon-generator.png" caption="favicon.io" >}}

Make sure to realize that you can leave the "Background Color" blank for a transparent background.

Note that it is pretty intimidating to try and scroll through the lengthy list of potential font families. Thankfully they point out that they support most every Google Font which makes the viewing experience much nicer.

{{< image src="google-fonts.png" >}}

Personally I like to opt in for just the "Display" type fonts as they seem to give the most spicy favicons.

# Image Magick

Now that you've downloaded your favicons, you're really trying to avoid opening up a photo editor in order to make conversions between various image formats and sizes. This is where [`imagemagick`](https://imagemagick.org/index.php) saves the day!

`create-react-app` uses `logo512.png`, `logo192.png`, and `favicon.ico` by default. Generating images of these types from the output of favicon.io is as easy as the following commands!

{{< highlight bash >}}
mkdir logos
mv favicon_io.zip logos
unzip logos
mv android-chrome-512x512.png logo512.png
convert logo512.png -resize 192 logo192.png
{{< /highlight >}}

Now your good to plug these images into your web app and get back to coding.


