# Design Flaws in Authentication Mechanisms

## **Weak password rules not enforced**

It's required to every application to enforce password to meet an standard. If not, an attacker can leverage an attack against any user in the application because the SysAdmin can not guarantee the quality of the password.

## **Brute-forcible Login**

Is another policy that has to be respected. Login functionality presents an open invitation for an attacker to try to guess usernames and passwords and therefore gain unauthorized access to the application. If the application allows an attacker to make repeated login attempts with different passwords until he guesses the correct one, it is highly vulnerable even to an amateur attacker who manually enters some common usernames and passwords into his browser.

The brute-force has to:

1. Block multiple attempts of password guessing
2. Do not base the re-attempt on parameters, such as cookies, or on client-side controls.
3. Any new attempt, on lock-out accounts, have to be blocked in the same way for correctly on wrongly passwords. It can not offer different lengths as response for correct password compared with wrong ones. This means that an attacker can complete his password-guessing attack even though the targeted account is locked out.

## **Verbose failure messages**

Do not give attackers info!!!! If the login fails, because either username or password, do not tell the attacker. If a login failed, it has failed. Period. Given what is wrong, username or password, the attacker can guess a list of valid usernames and try to guess the password by using the huge number of leaks that happen every month. And this is not about login forms, it is also about any functionality that requires authentication mechanisms. One location where username enumeration is commonly found is the user registration function. If the application allows new users to register and specify their own usernames, username enumeration is virtually impossible to prevent if the application is to prevent duplicate usernames from being registered. Even if an application’s responses to login attempts containing valid and invalid usernames are identical in every intrinsic respect, it may still be possible to enumerate usernames based on the time taken for the application to respond to the login request. Applications often perform very different back-end processing on a login request, depending on whether it contains a valid username. For example, when a valid username is submitted, the application may retrieve user details from a back-end database, perform various processing on these details (for example, checking whether the account is expired), and then validate the password (which may involve a resource-intensive hash algorithm) before returning a generic message if the password is incorrect. The timing difference between the two responses may be too subtle to detect when working with only a browser, but an automated tool may be able to discriminate between them.

## **Vulnerable Transmission of Credentials**

When passing credentials through the network, is required to encrypt the data. Avoid, for example, sending username and password through query parameters as they can be read by any attacker present on the network or be saw in the browser's history. This will avoid man-in-the-middle attacks.

## **Password Change Functionality**

It is important for every well-designed application because it ensures that vulnerable accounts due to password leaks, for example, can be protected. Also it reduces the windows in which given password can be targeted in a guessing attack.**Although it is a necessary part of an effective authentication mechanism, password change functionality is often vulnerable by design.** Many web applications’ password change functions are accessible without authentication and do the following:

- Provide a verbose error message indicating whether the requested username is valid.
- Allow unrestricted guesses of the “existing password” field.
- Check whether the “new password” and “confirm new password” fields have the same value only after validating the existing password, thereby allowing an attack to succeed in discovering the existing password non-invasively.

## **Forgotten Password Functionality**

It can introduce, for example, user enumeration. And when it's careful about it also sends an email tied to its username in the query parameters. This type of implementation can offer to an attacker ways to reset any account's password by changing only the query parameter. **Even if the application does not provide a non-screen field for you to provide an e-mail address to receive the recovery URL, the application may transmit the address via a hidden form field or cookie. This presents a double opportunity: you can discover the e-mail address of the user you have compromised, and you can modify its value to receive the recovery URL at an address of your choosing.**

## Remember Me functionality

Is often insecure by design since it sends on the request a persistent cookie to the server to unique identify the user and bypass the login procedure.

Possible usage:

- Sends the username through the cookie parameter to identify the user
- Sends an simple integer representing the user id to identify the user
- Even if it’s encrypted it can also be vulnerable by using cross-site scripting technique.

## **User Impersonation Functionality**

It's often used for system admins or helpdesks to perform operations to help users. By itself it may contain vulnerabilities since it's kind of a backdoor.

In most of the cases is implemented as a hidden function which is not subject to proper access controls. If the application allows administrative accounts to be impersonate it may result in a vertical privilege escalation. Rather than simply gaining access to other ordinary user's data, an attacker may gain full control of the application.

## **Nonunique Usernames**

Despite being not common in modern applications it may still out there. Non unique usernames not enforced may lead to both account take overs or password and username disclosure. During the design of the application every username should be unique in order to prevent these type of attacks. And when it's enforced it also help attackers to enumerate usernames.

## **Predictable Usernames and Passwords**

Some applications in order to make user's life a little easier generates usernames during their sign up(cust5331, cust5332, and so on). However this may end-up badly since most users are careless about most of the personalizations provided in the applications. So an attacker can notice a pattern during the username generation and attempt to bruteforce password to a specific username and gain privileges into their accounts. In the same way we can apply the same logic to password. While attemping to exploit this vulnerability an attacker can see a pattern in passwords generated and thus try to access accounts that didn't change the default password.

## **Insecure Distribution of Credentials**
Sometimes credentials, or even procedures for account creation are distributed in out-of-band ways(e.g.: email, SMS, etc.) in order to confirm that the information provided belongs to the user. For example URL for account activation. In some cases these informations are not protected against reuse, for example. Time limit for use is not enforced. An attacker can make use of the pattern generated by the distribution and takeover accounts, change passwords, change emails, and so on.
