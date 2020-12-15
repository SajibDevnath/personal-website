+++
date = ""
description = ""
image = ""
tags = []
title = "Contact"

+++
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
<form action="https://examples.webscript.io/contact" method="post">
	<input type="text" name="replyto" />
	<input type="text" name="subject" />
	<textarea name="body" rows="10"></textarea>
	<script src="https://www.google.com/recaptcha/api/challenge?k=<RECAPTCHA PUBLIC KEY>"></script>
	<button type="submit">Send</button>
</form>
