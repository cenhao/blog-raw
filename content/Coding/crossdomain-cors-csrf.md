Title: Cross-domain, CORS and CSRF
Tag: web, security
Slug: cross-domain-cors-csrf
Date: 2016-02-29 11:00

A month ago I was working on something that requires communication between two domains. Let's say domain `product.com` contains JavaScript that will send AJAX requests to APIs hosted in domain `api.com`, we own both `product.com` and `api.com`. Finally a domain `login.com` is used by both `product.com` and `api.com` to authenticate users, it's owned by the company's account team and we barely have any access to it.

Here're the requirements:

1. As we own all three domains, we want to achieve SSO (**S**ingle **S**ign **O**n) experience, i.e., after users goes to `product.com` finishes sign in, they won't need to sign in to `api.com` again with the same account and password.

2. `api.com` accepts two types of authentication: *oauth token* or *cookie*. `login.com` provides public *oauth* service to get the *token* of a user, but users will be asked to provide their consent -- they will see a pop-up windows asking them if the allow the *app* to access their data, like those you will see when you install an app in facebook. We would like to avoid this.
