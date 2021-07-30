---
title: Windows Forms development for a Node + React developer
date: 2021-06-17T13:02
thumb: gatsby_netlify.png
tags: 
    - React
    - NodeJS
    - Windows
    - Visual Basic
    - .NET
---

I am going to let you in on a secret. I really dislike Windows.

However, sometimes work calls for using technologies one does not like, and right now is a time that I need to suck it up and deal with it. I am working on some projects written in Visual Basic as Windows Forms. Here are some of the things I have learned that may prove useful to you.

1. When you are editing a Visual Basic Windows Forms application, open the `.vbproj` file, not the entire folder (you are probably used to opening directories in Visual Studio Code).
2. `.dll` files are like compiled libraries that you can import. To import a library, go to the `Solution Explorer` in Visual Studio, right click on `References`, and then `Add reference`. (This took me way too long to figure out; I was working on a project and looked __everywhere__ for the mysterious `WebUtils` code that a retired employee wrote. Turns out, it was in a `.dll` file.)
3. When you are editing the format of a Windows Form, you add code to a component by double clicking on it. Visual Studio will then take you to the code that corresponds with the button that you just double clicked.
4. Before you get started with Forms, write a console application. It will be an easier way to get your mind around the syntax of Visual Basic.