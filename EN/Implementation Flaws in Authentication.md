# Implementation Flaws in Authentication

Implementation flaws tend to be more subtle and harder to detect than design defects such as poor-quality passwords and brute- forcibility. For this reason, they are often a fruitful target for attacks against the most security-critical applications, where numerous threat models and penetration tests are likely to have claimed any low-hanging fruit.

## **Fail-Open Login Mechanisms**
It's a less common vulnerability but it stills remains today. It is about incorrect implementaion on authentications. Take the code below as an example:

```
public Response checkLogin(Session session) {
      try {
          String uname = session.getParameter(“username”);
          String passwd = session.getParameter(“password”);
          User user = db.getUser(uname, passwd);
          if (user == null) {
              // invalid credentials
              session.setMessage(“Login failed. “);
              return doLogin(session);
	} 
}
      catch (Exception e) {}
      // valid user
      session.setMessage(“Login successful. “);
      return doMainMenu(session);
}
```

You can see if the user is not passed, or the code throws an exception, the login still succeeds. In the field, you would not expect code like this to pass even the most cursory security review. But it is important to have in mind.

## **Defects in Multistage Login Mechanisms**

Some applications use elaborate login mechanisms involving multiple stages, such as the following:

- Entry of a username and password
- A challenge for specific digits from a PIN or a memorable word
- The submission of a value displayed on a changing physical token

It is often assumed that multistage login mechanisms are less prone to security bypasses than standard username/password authentication. This belief is mistaken. Performing several authentication checks may add considerable security to the mechanism. But counterbalancing this, the process is more prone to flaws in implementation. In several cases where a combination of flaws is present, it can even result in a solution that is less secure than a nor- mal login based on username and password.

There can be a numerous of security flaws in the checks presented between the stages:

- The application didn't consider the user between the stages, meaning that an attacker can use the credentials know by him and on the final stage manipulate the parameters to login into another account.
- Each stage has to check if all the previous ones had their challenges approved. By doing that the application can assure the user didn't jump between the stages.

Pay close attention to what is presented in the response/request during the multi-step login and also in the HTML. Answers might be present on these places.

## **Insecure Storage of Credentials**
It is common to encounter web applications in which user credentials are stored insecurely within the database. This may involve passwords being stored in cleartext. But if passwords are being hashed using a standard algorithm such as MD5 or SHA-1, this still allows an attacker to simply look up observed hashes against a precomputed database of hash values.
