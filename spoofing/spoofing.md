# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
2. Briefly explain different ways in which vulnerability can be exploited.
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.



1. The spoofing vulnerability in insecure.ts primarily arises from weak session and cookie security, making the application susceptible to cookie theft and Cross-Site Request Forgery (CSRF). Here, session cookies are used for user identification, but with httpOnly set to false, the session cookie (connect.sid) becomes accessible to JavaScript running in the browser. This configuration enables a malicious site or script to capture the cookie if the user unknowingly visits a compromised page. By obtaining the session cookie, an attacker can impersonate the user on insecure.ts and gain unauthorized access to restricted parts of the app.

2. This vulnerability can be exploited mainly through two methods: cookie theft and CSRF. Cookie theft allows a malicious site to programmatically retrieve the session cookie if it’s accessible, as shown with mal-steal-cookie.html. If a user clicks a malicious link or opens a compromised page, the attacker can steal the cookie and use it to impersonate the user, thereby gaining access to potentially sensitive data. In the case of CSRF, the mal-csrf.html file shows how an attacker can create a form that submits requests to insecure.ts on the user’s behalf. When the user’s session is active, these requests bypass authentication, enabling unauthorized actions like logging out or performing sensitive tasks. This is possible because insecure.ts lacks proper request origin validation, leaving it vulnerable to cross-site requests that can trick the application into performing unintended actions.

3. The secure.ts file avoids the spoofing vulnerabilities seen in insecure.ts by adding stronger session protections. First off, secure.ts sets the httpOnly attribute for cookies, which stops JavaScript from accessing the session cookie. This makes it far harder for attackers to steal the cookie using malicious scripts—a key step in preventing cookie theft.

It also enables the sameSite attribute on cookies, which limits the cookie’s use to requests from the same origin only. This means that if a request comes from an external or suspicious site, the browser won’t send the session cookie along with it, reducing the risk of Cross-Site Request Forgery (CSRF). By doing this, secure.ts essentially closes the door on any unauthorized requests that might try and trick the app into performing unintended actions.

So, overall, secure.ts strengthens session security by keeping session data restricted to same-origin requests only, making it much less likely an attacker can exploit the system the way they could in insecure.ts.
