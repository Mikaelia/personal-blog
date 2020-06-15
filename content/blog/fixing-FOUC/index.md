---
title: Removing FOUC in a Netlify-deployed Gatsby Project
date: "2020-06-07"
description: "Fixing that flicker of unstyled content"
---

As I've been working more with Gatsby and styled-components, I've run across an issue with flashes of unstyled content showing on page load and refresh in an application deployed to Netlify. Squashing this issue required a bit of exploration.


Here are the two steps that eventually fixed my problem:

1. Install the `gatsby-plugin-styled-components` `styled-components` `babel-plugin-styled-components` plugins and update the `gatsby-config.js` file (example below). I had only imported `styled-components` library, not knowing the other packages were needed.

This corrected the majority of issues I was having on page refresh. However, the unstyled font flicker was a little tricker to fix. After some experimentation I finally discovered that the `gatsby-plugin-typograpy` package did the trick.


2. Install the `gatsby-plugin-typography`, `react-typography` and `typography` packages and update the `gatsby-config.js` (also below) in order to fix font issues.

This involves creating a `src/utils` folder, with a `typography.js` file. In order to use Google fonts, the typography file required some configurations.


  Here's mine:


`typography.js`
```


import Typography from "typography"

const typography = new Typography({
  baseFontSize: "18px",
  baseLineHeight: 1.666,
  googleFonts: [
    { name: "Bai Jamjuree", styles: [700] },
    { name: "Muli", styles: [400, 700, 800] },
  ],
  headerFontFamily: ["Muli", "sans-serif"],
  bodyFontFamily: ["Muli", "serif"],
})

export default typography

```
&nbsp;

Despite not setting the `Bai Jamjuree` font to either `headerFontFamily` or `bodyFontFamily`, I was able to use it elsewhere in my application.

Here's my `gatsby-config.js` file:


```
// gatsby-config.js

  plugins: [
    {
      resolve: `gatsby-plugin-styled-components`,
      options: {
        // Add any options here
      },
    },
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    }
  ]
```
&nbsp;

Hope this helps!
