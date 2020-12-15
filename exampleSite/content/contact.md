+++
date = ""
description = ""
image = ""
tags = []
title = "Contact"

+++
<form action="https://examples.webscript.io/contact" method="post">
	<input type="text" name="replyto" />
	<input type="text" name="subject" />
	<textarea name="body" rows="10"></textarea>
	<script src="https://www.google.com/recaptcha/api/challenge?k=<RECAPTCHA PUBLIC KEY>"></script>
	<button type="submit">Send</button>
</form>