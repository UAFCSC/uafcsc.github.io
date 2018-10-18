---
layout: post
title: Web Development and Security
featured-img: webdev
categories: [Lectures]
---

#### Lab Repository: [https://github.com/RyanStonebraker/hackThisPage](https://github.com/RyanStonebraker/hackThisPage)

## Exploits:

### SQL Injection
SQL Injections are a form of exploit where SQL code can be maliciously run by abusing user input. A SQL vulnerability typically takes the form of finishing the SQL statement that the server was planning on executing, arbitrarily adding SQL to do malicious things, and then ending the user input by rewriting the left half of the initial SQL statement being run. This can be seen in the diagram below.

![](readme_media/sql_injection.svg)

In this case, the server written SQL and the user input would combine to form 3 SQL statements:

  1. SELECT * FROM cats WHERE name LIKE '%%';
  2. INSERT into cats (name, picture, price, description) VALUES ("Evil Cat", "http://doubtfulnews.com/wp-content/uploads/2012/07/Evil_cat.jpg", 1000000, "Mwahaha Im evil kat. I hakz ur databaz.");
  3. SELECT * FROM cats WHERE name LIKE '%%';

If you notice, statements 1 and 2 are the same. This is because if we ignored the SQL the server wrote, we would just get a broken query. So instead, we use the server's SQL to our advantage and write the right half of the statement for the first one and the  left half of the same statement for the third one.

#### Examples for this demo

#####  Add a cat without credentials:
```sql
%'; INSERT into cats (name, picture, price, description) VALUES ("Evil Cat","http://doubtfulnews.com/wp-content/uploads/2012/07/Evil_cat.jpg", 1000000, "Mwahaha Im evil kat. I hakz ur databaz."); SELECT * FROM cats WHERE name LIKE '%Koala
```
#####  Get a user's password:
```sql
%'; USE users; SELECT username, sess_id, password FROM credentials WHERE username LIKE '%admin
```
#####  Get the session id for a user named 'admin':
```sql
%'; USE users; SELECT * FROM credentials WHERE username LIKE '%admin
```

### Session Hijacking
Session hijacking can take the form of a really simple exploit, or of one with a much more complex nature. The idea of session hijacking is pretty simple. Server's already have a hard enough time uniquely identifying users, so if you completely recreated a user's computer environment on your computer, then how could the server tell the difference? It turns out this can actually be done without too much work (in some cases).

In 2010 for example, a major exploit called [Firesheep](https://en.wikipedia.org/wiki/Firesheep) made waves. All Firesheep did though was intercept authenticated user's cookies on a network and duplicate them for the attacker. This exploit was packaged in a nice Firefox plugin with a GUI and made stealing credentials easier than ever for people even without any technical experience. This demo replicates an environment similar to which Firesheep was ideal for. There are only two cookies stored and used for authentication, `USERNAME` and `SIMPLE_SESSID`. These cookies can be easily manipulated in javascript. If you open up the developer tools in the web browser of your choosing and go to the console, you can type:
```js
document.cookie;
```
This will list all cookies you currently have stored. To change your cookies, you can just set the cookie string equal to the new value. Note though that only one cookie can be declared per assignment.
```js
document.cookie = "USERNAME = admin";
document.cookie = "SIMPLE_SESSID = 123";
```
If you enter the above code in your browser and reload the page, you'll notice your name at the top right changes to whatever is in the USERNAME field of the cookie. However, if you go to `Post-A-Cat`, you'll notice that the authentication still fails. This is because the `SIMPLE_SESSID` is a unique identifier that is randomly generated when a user logs in. So in order to hijack a user's session, the attacker in this case would have a small window to get their hands on the user's cookies and paste them as their own in order to gain access. However, the `SIMPLE_SESSID` in this case really is just a simple session id. It is a randomly generated number between 0 and 100,000. If an attacker was really motivated to gain access, all they would have to do is set the USERNAME to their target, and then write a script to automate incrementing the SIMPLE_SESSID and checking the `Post-A-Cat` page to check for authentication.

In the real world, sessions are a bit more complex. They often involve using things like the user's IP address and have long session ids that are encrypted with a secret key. Furthermore, these session ids are often made to change on every page load and expire shortly after inactivity. This makes it increasingly hard for a hacker to replicate a user's information in this small window. Furthermore, most sites today don't write session authentication themselves. Instead, they rely on proven libraries that make this task easy for the site to implement and hard for the attacker to break. Nevertheless, this type of attack is not unheard of, even today, and it is important to keep this stuff in mind when developing secure web apps.

### PHP Reverse Shells
PHP Reverse shells are perhaps the simplest exploit out of the three that this website was prepared for. However, they are also by far the most powerful. The idea behind a reverse shell is that you get something uploaded to the server that communicates back to the attacker and executes whatever the attacker sends it. This sounds complicated, but in reality, this can be done a numerous ways. Perhaps one of the most trivial ways and the way that this demo uses is through an insecure file uploader. If the file uploader does not do any sort of sanitizing on the files uploaded and the server happens to allow PHP, then all an attacker has to do is upload a simple PHP script (which could range from a single line long to more complex ones that ensure stable and persistent connections). Examples of PHP Reverse Shells can be found [here](https://highon.coffee/blog/reverse-shell-cheat-sheet/).
