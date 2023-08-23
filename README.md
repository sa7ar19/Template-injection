# Template-injection


**Be on the lookout for the use of AngularJS and test out fields using the Angular
syntax {{ }}. To make your life easier, get the Firefox plugin Wappalyzer - it will
show you what software a site is using, including the use of AngularJS.**



**template injection vulnerabilities can
sometimes lead to remote code execution.**

**#############**

- **the presentation logic**, which is concerned with handling user interaction and updating the view of the application as presented to the user;
- **the application logic**, which is concerned with the detailed application-specific processing associated with the application (also referred to as the business logic);
- **the data logic**, which is concerned with the persistent storage of the application, typically in a database management system.

#############

# Server Side Template Injections {SSTI}

a template engines are usually associated with specific
programming languages, when an injection occurs, it may be possible to execute arbitrary code from that language

Since the syntax isn’t uniform across all templating engines, it’s important to determine what software was used to build the site being tested. Tools like **Wappalyzer** or **BuiltWith** are specifically designed to help do this so I recommend you use either of them. Once you’ve identified the software, use that syntax to submit the payload 7*7.

**(be it web framework, front end rendering engine, etc.)**

```jsx
[https://developer.uber.com/docs/deep-linking?q=wrtz{{(_="".sub).call.call({}[$="constructor"].getOwnPropertyDescriptor(_.__proto__,$).value,0,"alert(1)](https://developer.uber.com/docs/deep-linking?q=wrtz%7B%7B(_=%22%22.sub).call.call(%7B%7D%5B$=%22constructor%22%5D.getOwnPropertyDescriptor(_.__proto__,$).value,0,%22alert(1))")()}}zzzz
```

### ****Exploring SSTI In Flask/Jinja2****

- https://nvisium.com/blog/2016/03/09/exploring-ssti-in-flask-jinja2 • https://nvisium.com/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii

### ****Rails Dynamic Render To RCE (CVE-2016-0752)****

[Rails Dynamic Render to RCE (CVE-2016-0752)](https://blog.nvisium.com/rails-dynamic-render-to-rce-cve-2016-0752)

**##############**

# Clint side Template Injection {CSTI}

⇒ typically written in JavaScript

⇒ Popular client template engines include AngularJS developed by Google and ReactJS developed by Facebook

⇒ most injections can typically only be exploited to achieve cross-site scripting (XSS) and not remote code execution

⇒ AngularJS versions before 1.6 include a Sandbox intended to limit access to some JavaScript functions and thereby protect against XSS

**(you can confirm the version of AngularJS being used by opening the developer console in your browser and entering** angular.version**)**

⇒ ethical hackers routinely found and released Angular sandbox bypasses. A popular bypass used for the Sandbox in versions 1.3.0-1.5.7 that you can submit when a Angular injection is found is:

```jsx
{{a=toString().constructor.proptotypr;a.charAt=a.trim;$eval('a,alert(1),a')}}
```

⇒ Angular Sandbox escapes at https://pastebin.com/xMXwsm0N
and https://jsfiddle.net/89aj1n7m/.

⇒ I found a CSTI by using the payload {{4+4}} which returned 8 on a site using AngularJS. However, when I used {{4*4}}, the text {{44}} was returned because the site sanitized the input by removing the asterisk. The field also removed special characters like () and [] and only allowed a maximum of 30 characters. All this combined effectively rendered the CSTI useless.
