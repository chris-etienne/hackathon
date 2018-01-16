# Graphviz
## What is Graphviz?

Graphviz is open source graph visualization software. Graph visualization is a way of representing structural information as diagrams of abstract graphs and networks. It has important applications in networking, bioinformatics,  software engineering, database and web design, machine learning, and in visual interfaces for other technical domains. 

[Documentation](http://graphviz.org/) can be found here.

### Why do I need this?

Graphviz is a tool that will allow you to rapidly create diagrams that you can use in your microservice design process. 


## Instructions for Setup

1. Install the Dot Language.

You'll need to install this first.  It is a prerequite.  The extension for Visual Studio code requires it.

First you'll need to download it from [graphviz.org](http://graphviz.org/download):

![image](/resources/8-downloading=dot.PNG)

2. Install Visual Studio Code

3. Install the [Graphviz (dot) language support for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Stephanvs.dot)

![image](/resources/5-GraphViz-extension.PNG)


4. Install the [Graphviz Preview Extension](https://marketplace.visualstudio.com/items?itemName=EFanZh.graphviz-preview)

![image](/resources/6-adding-preview-ext.PNG)

## Frequently Asked Questions

### I Started Up Visual Studio Code and it cant find GIT

You are probably seeing something that looks like this:

![image](/resources/git-not-installed.PNG)

If your screen looks like this, you need to install git:

![image](/resources/install-git.PNG)
![image](/resources/installing-git.PNG)

### What do I do if Visual Studio can't find DOT.exe?

You can tell if its not installed if you see a message like this:

![image](/resources/7-no-dot-installed.PNG)

First you should find the location of dot.exe. It will likely be located here:

![image](/resources/Location-of-dot-exe.PNG)

Next you'll need to fix the missing reference in the Visual Studio configuration:

![image](/resources/setting-dot-path.PNG)