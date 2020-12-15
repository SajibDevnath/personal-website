+++
date = ""
description = ""
image = ""
tags = []
title = "Contact"

+++
```css
<style>
body {font-family: Arial, Helvetica, sans-serif;}
* {box-sizing: border-box;}

input[type=text], input[type=email], select, textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 6px;
  margin-bottom: 16px;
  resize: vertical;
  align:left;
}

input[type=submit] {
  background-color: #34495E;
  color: white;
  padding: 12px 20px;
  border: none;
  cursor: pointer;
  align:left;
}

input[type=submit]:hover {
  background-color: #4C7393;
}

.new-form {
  border-radius: 5px;
  background-color: #ffffff;
  padding: 20px;
}
</style>
```
````html
<div class="new-form">
  <form name="contact" method="POST" data-netlify="true" action="/thank-you/">
    <label for="fname">Name</label><br>
    <input type="text" id="fname" name="name" placeholder="Your name">

    <label for="lname">Email</label><br>
    <input type="email" id="lname" name="email" placeholder="Your Email">

    <label for="subject">Message</label><br>
    <textarea id="subject" name="message" placeholder="Write something.." style="height:200px"></textarea>
    <br>
    <input type="submit" value="Submit">
  </form>
</div>
````