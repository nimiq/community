---
layout: post
section_id: academy
title:  "Beginner Programmer Guide"
date:   2017-12-08 10:20:00
excerpt: With this course you will learn the necessary skills to start programming on top of the Nimiq Blockchain
type: course
skills:
  - Get information from Nimiq APIs
  - Use the most simple application on top of Nimiq
  - Running a NodeJS Nimiq Node
  - Build a simple web application using Nimiq
course: nimiqprogrammerbasic
courseimg: images/academy/nimiqprogrammer.jpg
author: Richy
images: 
  - "images/academy/nimiqprogrammer.jpg"
  - "images/academy/nodejscode.jpg"
---

<div class='links'>
  <h3>Lessons</h3>
  <ul>
    {% for post in site.posts %}
      {% if forloop.index > 10 %}
        {% break %}
      {% endif %}
     {% if post.type=="lesson" and post.course==page.course and post.title == page.title%}
       <li><a href="{{post.url}}" style="color:#f7b747;">{{post.title | capitalize}}</a></li>
     {% elsif post.type=="lesson" and post.course==page.course and post.title != page.title %}
       <li><a href="{{post.url}}">{{post.title | capitalize}}</a></li>
     {% endif %}
     {% endfor %}
  </ul>
</div>

