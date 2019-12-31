---
layout: post
title:  "Email BackEnd with SMTP Gmail"
date:   2019-12-12 01:38:15 +0700
categories: [python, django]
---
Add this configurations in your `settings.py`

This configurations is if you work with `smtp.gmail.com`, other smtp is similiar with configurations.

* Unlock Captha: [https://accounts.google.com/DisplayUnlockCaptcha](https://accounts.google.com/DisplayUnlockCaptcha)
* Change to active: [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps)

```
EMAIL_HOST          = 'smtp.gmail.com'
EMAIL_PORT          = 587
EMAIL_HOST_USER     = 'your_gmail@gmail.com'
EMAIL_HOST_PASSWORD = 'your_password'
EMAIL_USE_TLS       = True
DEFAULT_FROM_EMAIL  = EMAIL_HOST_USER
EMAIL_BACKEND       = 'django.core.mail.backends.smtp.EmailBackend'
```

<div id="gitmentContainer"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
var gitment = new Gitment({
    owner: '1906244925@qq.com',
    repo: 'joker-j-o',
    oauth: {
        client_id: '02d65d2354511830d4ae',
        client_secret: '5f7ed7ebfc8c4941233321d932219cd576333c29',
    },
});
gitment.render('gitmentContainer');
</script>