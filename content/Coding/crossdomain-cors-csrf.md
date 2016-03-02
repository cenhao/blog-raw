Title: Cross-domain, CORS and CSRF
Tags: web, security
Slug: cross-domain-cors-csrf
Date: 2016-02-29 11:00

A month ago I was working on something that requires communication between two domains. Let's say domain <font color="#DA1D1B">product.com</font> contains javascript that will send AJAX requests to APIs hosted in domain <font color="#DA1D1B">api.com</font>, and we own both <font color="#DA1D1B">product.com</font> and <font color="#DA1D1B">api.com</font>. Finally a domain <font color="#DA1D1B">login.com</font> is used by both <font color="#DA1D1B">product.com</font> and <font color="#DA1D1B">api.com</font> to authenticate users, it's owned by the company's account team and we barely have any access to it.

Here're the requirements:

1. As we own all three domains, we want to achieve SSO (**S**ingle **S**ign **O**n) experience, i.e., after users goes to <font color="#DA1D1B">product.com</font> and finishes sign in, they won't need to sign in to <font color="#DA1D1B">api.com</font> again with the same account and password.

2. <font color="#DA1D1B">api.com</font> accepts two types of authentication: *oauth token* or *cookie*. <font color="#DA1D1B">login.com</font> provides public *oauth* service to get the *token* of a user, but users will be asked to provide their consent -- they will see a pop-up windows asking them if they allow the *app* to access their data, like those you will see when you install an app in facebook. We would like to avoid this.

I'll give the quick answer for meeting these requirements: after user logins in to <font color="#DA1D1B">product.com</font>, we embed a **hidden iframe** pointing to <font color="#DA1D1B">api.com</font>. As the two domains use <font color="#DA1D1B">login.com</font> to sign users in, after user signs in to one of them, he won't need to sign in to the other again. Then we can send **cross domain** AJAX requests to <font color="#DA1D1B">api.com</font> with <font color="#DA1D1B">api.com</font>'s cookies.

Of course things are a lot more complex then this, there're a ton of details to be addressed.

First we need to understand how <font color="#DA1D1B">login.com</font> performs the authentication:

		+-----------+                                     +---------+
		|product.com|    --not sign in, redirect to-->    |login.com|
		+-----------+                                     +---------+

		                                                       |
		                                                user enter pwd
		                                           set cookie for login.com*
		                                          redirect (with a pass key)
		                                                       |
		                                                       V

		+-----------+                                 +-----------------+
		|product.com|  <--set cookie, redirect to--   |product.com/login|
		+-----------+                                 +-----------------+

The cookie authentication flow for <font color="#DA1D1B">api.com</font> is more or less the same. Please note that I just call it <font color="#DA1D1B">api.com</font> for simplicty, in fact it's more like <font color="#DA1D1B">product2.com</font>, it's just the APIs are hosted under this domain, <font color="#DA1D1B">api.com</font> also hosts other services as well, and some of its services use the cookie to call the APIs.

But how does a users get SSO for <font color="#DA1D1B">api.com</font> after he signs in to <font color="#DA1D1B">product.com</font>? Here's the illustration:

		+-------+                                         +---------+
		|api.com|      --not sign in, redirect to-->      |login.com|
		+-------+                                         +---------+

		                                                       |
		                                          found cookie for login.com**
		                                          redirect (with a pass key)
		                                                       |
		                                                       V

		+-------+                                     +-----------------+
		|api.com|     <--set cookie, redirect to--    |product.com/login|
		+-------+                                     +-----------------+

The key is the two lines marked with `*` and `**`. After first time sign in, <font color="#DA1D1B">login.com</font> will set cookie for itself, so next time the user sign in, he won't need to enter his credentials again as <font color="#DA1D1B">api.com</font> can use the cookie. This provides the user SSO experience.

The second key is how we send cross domain request from <font color="#DA1D1B">product.com</font> to <font color="#DA1D1B">api.com</font>, with cookies of <font color="#DA1D1B">api.com</font>. The *same origin policy* is imposed on modern browsers, there's no way for javascript to access cookies from a different domain (we can even restrict javascrpit's access to the cookie in the same domain with **HttpOnly**).

We don't really need to access the cookies, We need to understand, for cross domain request, how can we tell the browser to attach the cookie for us. If it's a simple HTTP request like **GET** or **POST**, the browser will attach the cookies: if there's a form or hyper link targeting another domain in the HTML page, when user triggers it, the cookie for that domain (if any) will be attached to the HTTP request.

On the other hand, for an AJAX request, the cross domain cookies are not attached by default. But most modern browsers implement AJAX with [**XMLHttpRequest**](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest), the browser will attach the cross domain cookies if the **withCredentials** property is set to **true**. (This can be easily done in *jQuery* too).

So far, candies, rainbows and unicorns. After we embeded the iframe pointing to <font color="#DA1D1B">api.com</font>, after <font color="#DA1D1B">api.com</font> is properly loaded, we will have the cookies stored in the browser, all we need is sending AJAX request with the setting to tell the browser to attach the cookie for us. It seems the problem is solved.

##Well, not really.

We haven't talked about the security concern for our own services and the security mechanism imposed by the browser.

I'll talk about [**CORS**(**C**ross **O**rigin **R**esource **S**haring)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) first. Though we can send cross domain AJAX with the cookies, the browser won't allow the javascript to read the service's response unless the service explicitly allow this.

More precisely, in the response, it must at least contain the `Access-Control-Allow-Origin` header telling the browser the *origin* is allowed to request the service(<font color="#DA1D1B">api.com</font>) with AJAX. Here're a few more things to be noted about the CORS:

1. For **GET**, **POST** requests with certain headers, they're considered *simple*, that only 1 HTTP request is required to perform this action. Cross site requests other than that are called **preflighted** requests. 2 HTTP requests are involved in a **preflighted** request, the first one is an HTTP **OPTIONS** request, and the second one is the actual request.

2. There're more different headers for controlling access of cross site AJAX, like `Access-Control-Allow-Credentials`.

3. Javascrip is not allowed to modify the `Origin` header, and this header is always set for cross site AJAX (some older browser does not set this header for same origin requests).

4. IE under version 10 has a very poor and non-standard support for cross site AJAX.

After talking about the security mechanism from the browser, we also need to review the service so that we won't be vulnerable to [**CSRF**(**C**ross **S**ite **R**equest **F**orgery)](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attack.

If we implemented the service as above, any malicious site could put a link pointing to <font color="#DA1D1B">api.com</font>in the page, and deceive the visitor to click (or whatsoever), so that the malicious site can send request to <font color="#DA1D1B">api.com</font> with the user's cookies. If we have no protection against this, the user's data is basically exposed to all those malicious sites.

One very common way for CRSF protection is using something call **CSRF token**. The idea is, to request the service, we must send a special header defined by the developer(s) with value returned from the service before, like the value of the cookie, the vaule of the a tag of the page. In short, this value(token) can only be retrieved by visiting the service before sending the request, and must be send in the request in a header that won't be set automatically by the browser. Due to the *same origin policy*, those malicious sites won't be able to access the token, so they have no way of sending it.

However, if we use this approach, it means we have to set our custom header. But setting custom header will make the request a **preflighted** one, which will make the <font color="#DA1D1B">product.com</font> slower. As we own both domains, we can add logic on <font color="#DA1D1B">api.com</font> to make it check the `Origin` header to see if this request is from <font color="#DA1D1B">product.com</font>. It's kinda like using the `Origin` header as a token, but simpler and less generic.

The last thing to mention is about the **third party cookie**. A third party cookie is set by sites that are not directly visited by the user, like from the **iframe**. Most browsers have the option to block **third party cookies**, and some browsers even block those cookies by default. As mention we use **iframe** pointing to <font color="#DA1D1B">api.com</font> to provide **sso** experience, so if **third party cookies** are blocked, we won't be able to talk to <font color="#DA1D1B">api.com</font> from <font color="#DA1D1B">product.com</font>, but so far we haven't found a solution for this.
