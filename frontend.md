***
title: FrontEnd
date: 2020-04-03 20:30:50+01:00
author: Gaxpert
***

# FrontEnd

marking up --> taking plain text and turning it into html
hypertext -> when its loaded onto the browser
parsing --> taking text file containing markup and turning into trre-like representation
rendering --> taking the tree and showing it to the user

use compatibility scripts so it can work across several browsers/ old

##HTML
<>   tags

###element
`\<tag> bla \</tag>`

###atribute
\<p class=Bla
common atributes
id= unique identifier
class

\<p> \<em> child element \</em> \</p> 

self closing tags --> Cant have children. Ex: \<br>

lists
Unordereder --> \<ul>
Ordered --> \<ol>
\<li> elemt1\</li>

Emphasis
\<em>
\<strong>

Neutral elements
\<div> --> block level
\<span> --> inline element

Links 
\<a href= >  --> links to be interacted

Images
\<img src="" alt="">

Objects
\<object data="" type="">

href --> somewhere the user can go
src --> something a browser should fetch

Inline frames
\<iframe> embedded browser window


##HTML notes
hgroup --> group h1,h2....
aside --> sidebar
article/section --> similar
nav --> navigaetion bar
time --> specific for time and specify datetime
figura --> has caption

microdata
item, itemtype, itemprp

HTML4--> 3 types of inputs (input, select, field-set)
required field

output element
progress / meter
placeholder (default value)
autofocus

location...

iframe --> indepenten browser window inside widget. Data is private and independent
if done with JS --> all data is accessible

    CSS
    
css rule
p { color:blue; }
p --> selector
color:blue; --> declaration
color --> property

 #element --> id selector
.class --> class selector
we can have several classes for same element



The more specific rule will be used

###Pseudoclasses
p:hover {color:blue;}

Most used

Borders
border-width:5px;
border-style:solid;
border-color:blue;

Backgrounds
background-color
background-image
background-repeat
background-position
background-attachement (scroll,fixed)

Layout
elements have:
width, height
padding
border
margin

display: (block, inline)
    sass

& --> parent selector

Mixin rules
@mixin BLA --> Extract rules
@include BLA --> includes rules

validation
input:valid {}
input:invalid {}
required/ optional

pseudo-elements vs pseudo-classes
pseudo-elements create virtual elements in your document -->  ::
pseudo-classes rely on properties of the document -->  :

##Css Combinators

id
p em {color:bla} : space is the descendent combinator
child combinator
body > footer --> selects only DIRECT children of body
h1 + p {} --> adjacent siblin combinator.Selects any paragraph element immediately that immediatly follows a h1 element
h1 ~ p {} --> general sibling combinator. not necessaraily directly adjacent

li:first-child
li:last-child
li:nth-child(3)
accepts patterns
li:nth-child(2n)
li:nth-child(odd)  (or even)
li:nth-last-child(n)

li:first-of-type{}
li:last-of-type{}

[itemscope] {} Existence selector

External links: a[href^="http://"]
Specific URLs: a[href="/home"]
File downloads: a[href$=".pdf

CSS dynamic pseudo classes
input
:enabled
:disabled
:valid
:invalid
:required
:optional
:target


Addapt to screen
@media screen and (max-width:800px and orientation: portrait/landscape) {}

##Javascript

How it fits in html
1. inline in a script element
2. linked in a separate file
3. inline in a event handler

check input validity
d.getElementById('A').checkValidity()
