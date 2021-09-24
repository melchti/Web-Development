# Web-Development

In this unit, I learned about web application architecture and the web client-server model HTTP communication process.

•	On Day 1 covered the HTTP communication process in the web client-server model. I then looked at how sessions are managed by web applications using cookies.

•	On Day 2 covered modern web application architecture, web application software "stacks," how to deploy web application container sets, and how to create SQL queries to manage data in databases.



Bonus Challenge Overview: The Cookie Jar

For this challenge, you'll once again be using curl, but this time to manage and swap sessions.

⚠ Heads Up: You'll need to have WordPress set up from the Swapping Sessions activity from Day 1 of this unit. If you have not done it or it is improperly set up, please refer to the Day 1 student guide and the Swapping Sessions activity file.

If you recall, on Day 1 of this unit you used Google Chrome's Cookie-Editor extension to swap sessions and cookies. For this homework challenge, we'll be using the command-line tool curl to practice swapping cookie and sessions within the WordPress app.

It is important for cybersecurity professionals to know how to manage cookies with curl:

•	Web application security engineers need to regularly ensure cookies are both functional and safe from tampering.

o	For example, you might need to request a cookie from a webpage and then test various HTTP responses using that cookie. Doing this over and over through the browser is tedious, but can be automated with scripts.

•	The same concept applies for penetration testers and hackers: curl is used to quickly save a cookie in order to test various exploits.

o	For example, an HTTP server may be configured so that, in order to POST data to specific pages, clients need to have cookies or authentication information set in their request headers, which the server will verify.


Revisiting curl

Recall that you used curl to craft different kinds of requests for your curl activity, and that you saw how to use the Chrome extension Cookie-Editor to export and import cookies and swap sessions.

There will be many systems in which you will need to test requests and cookies that will not connect to a browser or browser extension.
curl not only allows users to look through headers, send data, and authenticate to servers, but also to save and send cookies through two curl options: --cookie-jar and --cookie.

These two options work exactly like Cookie-Editor, but on the command line.

•	--cookie-jar allows a curl user to save the cookies set within a response header into a text file.

•	--cookie allows a user to specify a text file where a cookie is saved, in order to send a request with the cookies embedded in the request header.


Let's look at how we can create a curl command that will log into a web page with a supplied username and password, and also save the server's response that should contain a cookie.

Logging In and Saving Cookies with Curl

If we want to use the curl command to log into an account, Amanda, with the password password, we use the following curl options:

•	curl --cookie-jar ./amandacookies.txt --form "log=Amanda" --form "pwd=password" http://localhost:8080/wp-login.php --verbose

•	curl: The tool that we are using.

•	--cookie-jar: Specifies where we will save the cookies.

•	./amandacookies.txt: Location and file where the cookies will be saved.

•	--form: Lets us pick the login username and password forms that we set in our user info earlier. In this case it's our username.

•	log=Amanda: How WordPress understands and accepts usernames.

•	--form: Lets us pick the login username and password forms that we set in our user info earlier. In this case it's our password.

•	pwd=password: How WordPress understands and accepts passwords.

•	http://localhost:8080/wp-login.php: Our WordPress login page.

•	--verbose: Outputs more specific description about the actions the command is taking.

Run the command: curl --cookie-jar ./amandacookies.txt --form "log=Amanda" --form "pwd=password" http://localhost:8080/wp-login.php --verbose

If the site confirms our credentials, it will give us a cookie in return, which curl will save in the cookie jar file ./amandacookies.txt.

Now let's look at how to use that saved cookie on a page that requires us to be logged in.

Using a Saved Cookie

To use a saved cookie, we use the following curl syntax:

•	curl --cookie ./amandacookies.txt http://localhost:8080/wp-admin/users.php 

o	curl: The tool that we are using.

o	--cookie: Precedes the location of our saved cookie that we want to use.

o	./amandacookies.txt: Location and file where the cookies are saved.

o	http://localhost:8080/wp-admin/users.php: A page that requires authentication to see properly. Note that we are not going to the login page, because supplying a cookie in this instance assumes that we are already logged in.


Now that we know how to use the curl cookie jar, let's look at what we need to do for this challenge.

________________________________________

Bonus Challenge Instructions: The Cookie Jar

First, using Docker Compose, navigate to the Day 1 WordPress activity directory and bring up the container set:

•	/home/sysadmin/Documents/docker_files

Using curl, you will do the following for the Ryan user:

•	Log into WordPress and save the user's cookies to a cookie jar.

•	Test a WordPress page by using a cookie from the cookie jar.

•	Pipe the output from the cookie with grep to check for authenticated page access.

•	Attempt to access a privileged WordPress admin page.

Step 1: Set Up

Create two new users: Amanda and Ryan.

1.	Navigate to localhost:8080/wp-admin/
2.	On the left-hand toolbar, hover over Users and click Add New.
3.	Enter the following information to create the new user named Amanda.

o	Username: Amanda 

o	Email: amanda@email.com 

4.	Skip down to password:

o	Password: password 

o	Confirm Password: Check the box to confirm use of weak password.

o	Role: Administrator 

5.	Create another user named Ryan.

o	Username: Ryan 

o	Email: ryan@email.com 

6.	Skip down to password:

o	Password: 123456 

o	Confirm Password: Check the box to confirm use of weak password.

o	Role: Editor 

7.	Log out and log in with the following credentials:

o	Username: Amanda 

o	Password: password 

Step 2: Baselining

For these "baselining" steps, you'll want to log into two different types of accounts to see how the WordPress site looks at the localhost:8080/wp-admin/users.php page. We want to see how the Users page looks from the perspective of an administrator, vs. a regular user.

1.	Using your browser, log into your WordPress site as your sysadmin account and navigate to localhost:8080/wp-admin/users.php, where we previously created the user Ryan. Examine this page briefly. Log out.
2.	Using your browser, log into your Ryan account and attempt to navigate to localhost:8080/wp-admin/index.php. Note the wording on your Dashboard.
3.	Attempt to navigate to localhost:8080/wp-admin/users.php. Note what you see now.

Log out in the browser.

Step 3: Using Forms and a Cookie Jar

Navigate to ~/Documents in a terminal to save your cookies.

1.	Construct a curl request that enters two forms: "log={username}" and "pwd={password}" and goes to http://localhost:8080/wp-login.php. Enter Ryan's credentials where there are placeholders.

2.	Construct the same curl request, but this time add the option and path to save your cookie: --cookie-jar ./ryancookies.txt. This option tells curl to save the cookies to the ryancookies.txt text file.

3.	Read the contents of the ryancookies.txt file.

Note that each one of these is a cookie that was granted to Ryan after logging in.

Step 4: Log in Using Cookies
1.	Craft a new curl command that now uses the --cookie option, followed by the path to your cookies file. For the URL, use http://localhost:8080/wp-admin/index.php.
2.	Press the up arrow on your keyboard to run the same command, but this time, pipe | grep Dashboard to the end of your command to return all instances of the word Dashboard on the page.

Step 5: Test the Users.php Page
1.	Finally, write a curl command using the same --cookie ryancookies.txt option, but attempt to access http://localhost:8080/wp-admin/users.php.


