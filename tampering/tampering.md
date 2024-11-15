# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

1. The insecure.ts file has some security issues, especially around how it handles sessions and input. It doesn’t use the httpOnly or sameSite options on cookies, so the session cookie can be accessed by JavaScript in the browser, making it vulnerable to cross-site requests and theft. This leaves the app open to cookie theft or CSRF attacks, where an attacker could use the session data or perform unauthorized actions. Another problem is that insecure.ts doesn’t sanitize the user input when setting the session username, meaning the app could be at risk of XSS (Cross-Site Scripting) attacks if someone enters malicious code into a form.

2. Attackers could exploit these weaknesses in a few different ways. First, with the session cookie being accessible in JavaScript, a malicious script could steal it to impersonate the user on the app. If the user visits a page with harmful scripts, those scripts could grab and send the cookie to an attacker. Another attack would be CSRF, where the attacker could embed forms or links on their own site, making requests to the app on behalf of the user. This lets them perform actions like setting sensitive data without the user knowing. Finally, without input sanitization, an attacker could enter scripts into form fields, allowing them to control parts of the page or access session data if those scripts are run.

3. secure.ts addresses these vulnerabilities through stronger security practices. By setting httpOnly and sameSite: 'strict' on cookies, secure.ts prevents JavaScript from accessing the session cookie and stops it from being sent in cross-site requests, making cookie theft and CSRF much harder. Plus, secure.ts sanitizes input before saving it to the session, which helps to prevent XSS by ensuring any special characters are encoded safely. These measures together make session data secure and protect against potential attacks through user input. It sanitizes with:

function escapeHTML(input: string): string {
  return input
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#39;");
}
