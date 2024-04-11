# SQL-Injection-Attack
This is a lab from my Springboard bootcamp that allowed me to demonstrate a SQL injection attack against WebGoat

<h1>Step 1 - Open OWASP ZAP Attack Proxy</h1>

OWASP ZAP is an attack proxy that will allow us to intercept and alter requests between your browser and the server. This allows you to use many injection attacks you can't perform with the browser alone. ZAP is also able to perform vulnerability scans, fuzzing and other web application tests. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/52042da1-753c-4ddf-9a57-ed4030ca20c6)

You can view the list of tools available by clicking **Tools > Options** 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/4a6ee321-aa08-4215-a8de-639afc8180b2)

In this lab, we utilized Firefox as our browser. In Firefox, you'll enable the ZAP proxy by clicking the FoxyProxy icon in the top right corner of the browser. Select the **ZAP** setting in green. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/47531920-91b3-4221-841b-14393e06b2af)

<h2>Step 2 - Start/Open WebGoat</h2>

WebGoat is an insecure web application created by OWASP to demonstrate application vulnerabilities. Open WebGoat by double-clicking it. It will launch in a terminal and allow it to load completely. You will know when it has finished loading when the message "Browse to http://localhost:8080/WebGoat and happy hacking!" is shown. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/04e9b5eb-be7b-4303-8023-613fa10c58e0)

Return back to Firebox and navigate to localhost:8080/WebGoat. WebGoat provides guest login credentials; go ahead and use the guest account to login.

<h3>Step 3 - Begin the SQL Injection in WebGoat</h3>

In the left pane, click **Injection Flaws** > **Stage 1: String SQL Injection** 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/bb39ecf3-32fc-4f7e-9e58-4619e54e5e7a)


<h4>Step 4 - Launch the SQL Injection Attack</h4>

At the login page, if you click the dropdown menu you can see a list of employee names with a password field underneath. We will use an injection attack to login without a password. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/8985ed45-a354-4bda-9c0b-233236c0f1b8)

Go back to ZAP and click the **Set Break** icon (It's the Green Circle above the second pane)

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/c7e5346a-7d71-402d-98d2-292130f2c320)

ZAP will now intercept all web requests and responses allowing us to examine them. Go back to WebGoat and log in as Neville Bartholomew using the password, "guest". Go back to the ZAP window, and you should receieve this. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/c73e0141-10c8-4c2a-a9e1-73d60b3fbdfb)

Now, we are going to inject our SQL attack by changing the password to = 1' OR '1'='1. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/faeafde8-2e38-4948-ae72-cd36dede4c8c)


At the top of of tools bar on ZAP, press the **Submit and step to next request or response** button twice. This submits the request to the server and then back to WebGoat. Return back to your Firefox window. You should get a notification that states you've made it to Stage 2. Notice we have now logged in as an admin and can see a list of all employees. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/adef5d71-236f-4874-85fe-4efb5519d9c5)

<h5>Step 5 - View Staff Information</h5>

Go back to ZAP and click the **Submit and continue to next break point** icon on the tool bar. This will run through the rest of the process and remove the break point, allowing us to move freely throughout the browser without being intercepted by ZAP.

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/c1c77f31-eb27-4886-b581-9830bab0788a)

Back in Firefox, click any employee's name and you're able to view their profile. We're not only able to view, but can edit their private information and find PII like their Social Security Number and their Credit Card number. 

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/fd351605-1db6-4dad-a08d-8e4a29aaddf7)

<h6>Step 6 - Perform a Command Injection</h6>

Now we will conduct a command injection; this takes advantage of vulnerabilites that allow us to run shell commands on the underlying host. Back in the WebGoat menu, click **Injection Flaws > Command Injection**

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/b298da54-63f1-480d-b11f-5363cb553d58)

Next, go back to ZAP and use the command **Ctrl+B**. This is a keyboard shortcut to set a break in ZAP. Once you've set your break in ZAP, go back to WebGoat and on the dropdown menu next to **Select the lesson plan to view** choose any lesson from the menu and click **View**. You should get the intercepted request with the "Helpfile" field. Here is where we will try our injection to run the shell commands.

![image](https://github.com/amolinaro23/SQL-Injection-Attack/assets/164687651/427f5b9c-20bf-44e3-b49e-07e09cf1aa9e)

In the request, replace the file name with **NoRealFile.help" || netstat -an**. 
