# Implementation Flaws in Authentication

## **Fail-Open Login Mechanisms**

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


## **Defects in Multistage Login Mechanisms**


## **Insecure Storage of Credentials**
