---
title: 'UI Fundamentals'
last_modified_at: 2023-05-09T23:39
categories:
  - frontend
tags:
  - UI/UX
  - design
toc: true
toc_sticky: true
---


## Intro
I've been working as a front-end since last year, and I started to question UI/UX and wanted to learn about the subject. 
Searching for UI/UX-related courses online, I found a free course at Scrimba.
The 'UI Design Fundamentals'[^fn1] is taught by UI/UX designer Gary Simon. 
This page contains notes I took while taking the course. 


## White Space
- the empty spsace between the elements in UI
- primary CSS properties include `padding` and `margin` 

## Alignment 
- the process of ensuring that every element is positioned correctly in relation to other elemtns.
- ex) when a page contains logo, title, and content, the logo, title, it looks much better if the elements start at the same x coordinate. If they all start at different x levels, the page looks much less unified.  

## Contrast 
- being in a 'strikingly' different state from something else. 
- every UI element by nature has certain amount of contrast based on a background that is behind it. 
  - examples include a button text that is positioned behind its button background, a button background that is positioned on a card, and a card that is positioned on a layout.
- WCAG(Web Content Accessibility Guideline) 2.0 contrast guidelines
  - minimum(requirement)
    - text and images of text has a contrast ratio of at least `4.5:1`, except for large text which should have a contrast ratio of at least `3:1`.[^fn2] 
  - enhanced(recommended)
    - text and images of text: at least `7:1`
    - large text: at least `4.5:1`
- there are contrast checking tools that tell the contrast ratios
  - browser plugins
  - websites
  - UI design application plugins (Sketch, Figma, etc)

## Scale 
- the size of every UI element must be carefully considered. 

## Typography 
- good typography requires the understanding of other fundamentals, along with a few special considerations
- considerations
  - font choice(s)
    - don't use more than 2 font families
    - most of the time, 1 would be sufficient 
  - visual hierarchy 
    - a way to establish the order of importance. 
  - font size (scale)
  - alignment
  - letter spacing & line height 
  - font styles (weight, italics, etc)
  - color & contrast


## Color 
- the first UI design fundamental that shapes a user's experience
- color psychology
  - each color has meaning to certain group of people
  - green: wealth, nature, growth
  - black: luxury, sophistication, elegance 
  - this depends on audience's culture. 
- things to avoid 
  - using a bunch of color s
  - using colors that don't complement each other well 
- can use 
  - slight variances of the same hue
  - brightness values to emphasize blocks of layout 


## Visual Hierarchy 
- every element on a UI has a level of importance. 
  - some elements are more important than others. 
- visual hierarchy is how we establish this importance. 


## Recap 
Besides explaining the concepts, the course goes over code examples and some challenges. 
I learned that in UI design, applying some of the fundamentals carries subjectivity. 
Like coding, there is no one big answer for the design, but there are some guidelines we could follow. 


# References
[^fn1]: [Scrimba: learn UI Design Fundamentals](https://scrimba.com/learn/design){:target="_blank"}
[^fn2]: [WCAG Contrast(Minimum) - Level AA](https://www.w3.org/WAI/WCAG21/quickref/#contrast-minimum){:target="_blank"}