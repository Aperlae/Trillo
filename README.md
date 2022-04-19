# Trillo

[Trillo - Your all-in-one booking app](https://aperlae.github.io/Trillo/)
This is the user-interface of a mockup booking app built with just HTML and CSS for the [Advanced CSS and SASS Course | Jonas Schmedtmann | Udemy](https://www.udemy.com/course/advanced-css-and-sass/)

## Table of Contents

- [Overview](#overview)
  - [Features](#features)
  - [Screenshots](#screenshots)
- [The Process](#the-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued Development](#continued-development)
  - [Useful Resources](#useful-resources)
- [Author and Acknowledgment](#author-and-acknowledgment)

## Overview

### Features

- Fully Responsive with the use of Flexbox and Media Queries
- Custom CSS Properties
- BEM (Block Element Modifier) Naming Convention
- Masks
- Icon SVGs
- Animations

### Screenshots

#### Desktop

![Screenshot-desktop.png](https://github.com/Aperlae/Trillo/blob/main/assets/Screenshot-desktop.png)
![User-nav.gif](https://github.com/Aperlae/Trillo/blob/main/assets/User-nav.gif)
![Gallery.gif](https://github.com/Aperlae/Trillo/blob/main/assets/Gallery.gif)
![Side-nav.gif](https://github.com/Aperlae/Trillo/blob/main/assets/Side-nav.gif)

#### Tablet

![Screenshot-tablet-landscape.png](https://github.com/Aperlae/Trillo/blob/main/assets/Screenshot-tablet-landscape.png) iPad Air 1180x820
![Screenshot-tablet-portrait.png](https://github.com/Aperlae/Trillo/blob/main/assets/Screenshot-tablet-portrait.png) iPad Air 820x1180

#### Mobile

![Screenshot-mobile.png](https://github.com/Aperlae/Trillo/blob/main/assets/Screenshot-mobile.png)

## The Process

### Built with

- Some CSS Custom Properties
- SASS Features:
  - SCSS syntax
  - Variables
  - Mixins
  - nesting
  - @use instead of @import
  - npm scripts and dependencies

### What I learned

This project focused mostly on flexbox, I would have built it mobile first but I followed on with Jonas's lessons and built it desktop first.  There were a few cool css tricks that I will be using in my future projects.

- Using *scale* and multiple transition properties with different settings, to   create a cool hovering effect.

```SCSS
  .user-nav {
  
    &__item::before {
      content: '';
      position: absolute;
      top: 100%;
      right: 0;
      height: .25rem;
      width: .25rem;
      background-color: var(--color-primary-light);
      transform: scaleX(0);
      transform-origin: left;
      transition: transform cubic-bezier(1,0,0,1) .5s;
    }
  
    &__item:hover::before {
      transform: scaleX(1);
      width: 100%;
    }
  }
```

- Knowing how and why to use the *currentColor* CSS variable

```SCSS
.user-nav {
  
  &__link:link,
  &__link:visited {
    display: block;
    color: var(--color-grey-dark-2);
    font-size: 1.6rem;
    text-decoration: none;
    padding: 1.5rem 3rem;
    @include mixins.flexCenter;
    transition: color .1s;

    &__link-icon {
      width: 2rem;
      height: 2rem;
      margin-right: 1rem;
      fill: currentColor; //inherits parent's font-color
    }
  }  
}
```

- Creating an infinite animation

```SCSS
.btn-inline {
  color: var(--color-primary);
  font-size: inherit;
  border: none;
  border-bottom: 1px solid currentColor;
  padding-bottom: 2px;
  display: inline-block;
  background-color: transparent;
  cursor: pointer;
  transition: all .2s;

  & span {
    margin-left: .3rem;
    transition: margin-left .2s;
  }

  &:hover {
    color: var(--color-grey-dark-1);

    span {
      margin-left: .8rem;
    }
  }

  &:focus {
    outline: none;
    animation: pulsate 1s infinite;
  }
}

@keyframes pulsate {
  0% {
    transform: scale(1);
    box-shadow: none;
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 1rem 4rem rgba(0, 0, 0, .25);
  }
  100% {
    transform: scale(1);
    box-shadow: none;
  }
}
```

- Using CSS masks with *mask-image* and *mask-size*

```SCSS
.list {
  &__item::before {
    content: '';
    display: inline-block;
    width: 1rem;
    height: 1rem;
    margin-right: .7rem;

    //Older browsers
    background-image: url(../assets/chevron-thin-right.svg);
    background-size: cover;

    //Mask for New Browsers
    @supports (-webkit-mask-image: url()) or (mask-image: url()) {
      background-color: var(--color-primary);
      -webkit-mask-image: url(../assets/chevron-thin-right.svg);
      mask-image: url(../assets/chevron-thin-right.svg);
      -webkit-mask-size: cover;
      mask-size: cover;
      background-image: none;
    }
  }
}
```

- And last but not least, I finally learned how to compile SASS without using an extension.  These are the npm scripts and dependencies I used in my package.json file.

```JSON
{
  "name": "trillo",
  "version": "0.1.0",
  "description": "SASS compile|autoprefix|minimize and live-reload dev server using Browsersync for static HTML",
  "main": "index.html",
  "author": "Aperlae",
  "scripts": {
    "build:sass": "sass  --no-source-map src/sass:public/css",
    "copy:assets": "copyfiles -u 1 ./src/assets/**/* public",
    "copy:html": "copyfiles -u 1 ./src/*.html public",
    "copy": "npm-run-all --parallel copy:*",
    "watch:assets": "onchange \"/src/assets/**/*\" -- npm run copy:assets",
    "watch:html": "onchange \"src/*.html\" -- npm run copy:html",
    "watch:sass": "sass  --no-source-map --watch src/sass:public/css",
    "watch": "npm-run-all --parallel watch:*",
    "serve": "browser-sync start --server public --files public",
    "start": "npm-run-all copy --parallel watch serve",
    "build": "npm-run-all copy:html build:*",
    "postbuild": "postcss public/css/*.css -u autoprefixer cssnano -r --no-map"
  },
  "dependencies": {
    "autoprefixer": "^10.4.4",
    "browser-sync": "^2.27.7",
    "copyfiles": "^2.4.1",
    "cssnano": "^5.0.17",
    "npm-run-all": "^4.1.5",
    "onchange": "^7.1.0",
    "postcss-cli": "^9.1.0",
    "sass": "^1.49.8"
  }
}
```

### Continued Development

I did not build this project with accessibility in mind and going back over it I realised that there's loads I could have done to make it a bit more user friendly so I need to work harder at that on my next project right from the start.

### Useful Resources

- [Jonas'Resources](https://codingheroes.io/resources/) - Plenty of handy resources here including tools, icons, fonts, colors etc
- [Kevin Powell Sass Compile](https://www.youtube.com/watch?v=o4cECvhrBo8) - Used this video and Eckles script below to help me get set up with compiling Sass in my environment
- [Stephanie Eckles Minimum Static Site Setup with Sass](https://thinkdobecreate.com/articles/minimum-static-site-sass-setup/)
- [Kevin Powell Stop using @import with Sass](https://youtu.be/CR-a8upNjJ0) - I came across this while browsing and decided I should start getting comfortable with it from now

## Author and Acknowledgment

Full design credits go to [Jonas Schmedtmann "Advanced CSS and SASS: Flexbox, Grid, animations and more" course on Udemy](https://www.udemy.com/course/advanced-css-and-sass/) with some minor tweaks of my own.
