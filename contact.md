---
layout: page
title: Contact us
permalink: /contact/
banner_image: mail.jpg
banner_image_alt: Contact us
---

<!-- Form settings: https://getsimpleform.com/forms/18875cf0213a46388b9dd9e2286102f2/edit -->

<form action='https://getsimpleform.com/messages?form_api_token=36c118daf6350ed43595947c6f1fa3fa' method='post'>
  <!-- the redirect_to is optional, the form will redirect to the referrer on submission -->
  <input type='hidden' name='redirect_to' value='/contact-success' />

  <!-- all your input fields here.... -->

  <div class="form-group">
	  <label for="name" class="col-sm-2 control-label">Name</label>
	  <div class="col-sm-10">
    	<input type='text' name='name' placeholder='Example: Foo Bar' required class='form-control' />
      </div>
  </div>

  <div class="form-group">
	  <label for="email" class="col-sm-2 control-label">Email</label>
	  <div class="col-sm-10">
		<input type='email' name='email' placeholder='Example: foo@bar.com' required class='form-control' />
      </div>
  </div>

  <div class="form-group">
	  <label for="subject" class="col-sm-2 control-label">Subject</label>
	  <div class="col-sm-10">
		<input type='text' name='subject' placeholder='Subject' required class='form-control' />
      </div>
  </div>

  <div class="form-group">
	  <label for="message" class="col-sm-2 control-label">Message</label>
	  <div class="col-sm-10">
		<textarea name='message' rows='5' placeholder='Message' class='form-control'></textarea>
      </div>
  </div>

  <div class="form-group">
	<div class="col-sm-offset-2 col-sm-10">
  	  <input type='submit' value='Send' class='bt bt-default' />
	</div>
  </div>

</form>

---

{% include social.html %}
