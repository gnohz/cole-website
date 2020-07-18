---
title: "Converting Webflow Site to React With Appfairy - Ultimate Guide"
date: 2020-07-14T20:38:44-04:00
draft: false
tags: ["react", "webflow", "appfairy"]
categories: ["Technical"]
---

# The Idea

I'm working on a side project. We are using material-ui and are having a great time with their React components. However, we were looking to really spice up our index page. This is where webflow comes in. It let's you create crazy animations that would take a million years to pull together.

Check it out for yourself here: [Artifai](https://artif.ai).

# The problem

We went into webflow thinking it would be easy to move it over into the react project. We were very wrong. Luckily someone wrote [Appfairy](https://github.com/DAB0mB/Appfairy) which makes the process way easier. Thank you [Eytan](https://github.com/DAB0mB/Appfairy)! There were still a few quirks though.

# Walkthrough

## Prereqs

Make sure to read the Appfairy README.md, and watch this [video](https://www.youtube.com/watch?v=6hJe6pZld0o) to get an overview of how the process works.

This guide assumes some familiarity with React and Create React App.

## Initial setup

Export your webflow site as a zip.

For the purpose of this walkthrough i'll be starting over from create-react-app, but you could just as easily follow along from inside your own react project.

create-react-app appfairy-walkthrough

Copy the unzipped contents of your zip to .appfairy, and perform the following very difficult step!

Run `appfairy`.

Phew. The hardest part is behind us :)

In order to see your sight on the screen, you have to import your view!

In index.js make sure to add the following imports:

{{< highlight javascript >}}
import "./scripts.js";
import "./styles.css"
import IndexView from "./views/IndexView";
{{</ highlight >}}

and replace `<App />` with `<IndexView />`

Great! Now by running `npm start` you'll see your webflow sight running locally on your computer! However, if you navigate around you'll notice that some of the animations are amiss. Follow the next section to get your site to full power!

## Modifications

### js and css

Make sure to add webflow.js and your css files to index.html

- If you don't add a jquery script tag to index.html, it will load after the webflow.js script which will cause problems because webflow relies on javascript.
- If you don't add css to index.html, the page will do a first paint without css which doesn't look very aesthetic.

### Webflow Page Id and Site Id

In implementing this and the next modification I had a lot of help from [this](https://github.com/DAB0mB/Appfairy/issues/7) github issue. I recommend that you read through it.

In order for webflow.js to function properly on your react site, you need to add the necessary meta data to index.html. Thanks to [rolandocpontes](https://github.com/ronaldocpontes) for the below code which makes this process easy.

Get the meta data by running the following commands on the console from a deployed webflow site:

{{< highlight javascript >}}
document.getElementsByTagName("html")[0].getAttribute('data-wf-page')
document.getElementsByTagName("html")[0].getAttribute('data-wf-site')
{{</ highlight >}}

Then put them into your site by adding the following to `componentDidMount` in `App.js`.

{{< highlight javascript >}}
componentDidMount() {
    const WEBFLOW_PAGE_ID = 'your page id'
    const WEBFLOW_SITE_ID = 'your site id'

    var doc = document.getElementsByTagName("html")[0]
    doc.setAttribute('data-wf-page', WEBFLOW_PAGE_ID)
    doc.setAttribute('data-wf-site', WEBFLOW_SITE_ID)
  };
}
{{</ highlight >}}

Now your ready to get the animations working. 

### Page Trigger Interactions

The idea is that while Appfairy prepends all the webflow element classes with "af-class-" in order to prevent conflicts, it doesn't make the relevant changes in webflow.js that are necessary for the animations to work. Luckily there is a quick solution to this issue.

Thanks to [rolandocpontes](https://github.com/ronaldocpontes) for the following node script which makes the necessary changes to the animation json in webflow.js:

{{< highlight javascript >}}
const fs = require('fs');

let rawdata = fs.readFileSync('events.json');
let events = JSON.parse(rawdata);

function traverse(object, convertFunction) {
    if (typeof convertFunction != "function")
        throw "convertFunction must be a function(key,valye) that returns a value for all keys"

    if (object !== null && typeof object == "object") {
        Object.entries(object).forEach(([key, value]) => {
            object[key] = convertFunction(key, value)
            traverse(value, convertFunction);
        });
    }
}

traverse(events, (key, value) => (key == "selector") ? value.split('.').join('.af-class-') : value)

fs.writeFileSync('appfairy-events.json', JSON.stringify(events))
{{</ highlight >}}

I wrote a short bash script that automates the process of applying rolandocpontes' script (make sure to only run it once. The script is not idempotent).

{{< highlight bash >}}
#!/usr/bin/env bash

# Save json from webflow.js to events.json
tail -n2 webflow.js | head -n1 > events.json

# Delete last two lines of webflow.js
sed -i '$ d' webflow.js
sed -i '$ d' webflow.js

# Generate appfairy-events.json
node converter.js

# Write appfairy-events.json to webflow.js
cat appfairy-events.json >> webflow.js

# Add closing parenthesis
echo >> webflow.js
echo ");" >> webflow.js
{{</ highlight >}}

Copy both of the above scripts into your `public/js` folder as `converter.js` and `apply-converter.sh` respectively, run `bash apply-converter.sh`, and you're good to go!

### On Enter page animations

Unfortunately, "On Enter" page transitions don't transfer over very easily to React applications from Webflow. It's possible to get them to work on initial page load, but when leaving and returning to a page using react router, the animation may not retrigger.

The easiest way to get around this is to change from using an "On Page Load" trigger to a "Button Click" trigger. Assign the button click trigger to a button which you then set `display: none`, and `id=animation-button`.

Now from inside the useEffect (or componentDidMount for a React component) function of the view controller you can add in the following in order to trigger the animation the right way everytime!

{{< highlight javascript >}}
useEffect(() => {
  document.getElementById("animation-button").click();
  setTimeout(() => {
    document.getElementById("animation-button").click();
  }, 1000);
}
{{</ highlight >}}

## Conclusion

I hope these steps help you save a day of debugging. Appfairy is an awesome tool thank you [Eytan Manor](https://github.com/DAB0mB/Appfairy)! Let me know in the comments if you have any questions or if I should elaborate more on this guide.
