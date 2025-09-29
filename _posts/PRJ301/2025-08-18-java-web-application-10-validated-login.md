---
title: Epoch 10 – Validated login form
description: >-
  Implement Login with proper validation in MVC style.
author: [shandy]
date: 2025-08-18
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 110
# pin: true
# media_subpath: '/posts/01'
---

## 1. Password Validation Requirements

### 1.1 Client-Side (JavaScript in login.jsp)

- Not Empty: Password field cannot be left blank.
- Minimum Length: At least 6–8 characters (configurable).
- Optional Strong Policy (if required):
  - At least 1 uppercase letter
  - At least 1 lowercase letter
  - At least 1 number
  - At least 1 special character (@#$%^&*!?)

### 1.2. Server-Side (Servlet)

- Check again that the password is not empty and meets minimum length.
- Forward back to login.jsp with an error message if invalid.
- Example error messages:
  - "Password cannot be empty."
  - "Password must be at least 6 characters."

## Resolve requirements

### Create a Password Validator

+ **Utils/PasswordValidator.java**

```java
package utils;

import java.util.regex.Pattern;

public class PasswordValidator {

    // Regex rule: At least 8 chars, one upper, one lower, one digit, one special char
    private static final String PASSWORD_PATTERN =
            "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@#$%^&*!?])[A-Za-z\\d@#$%^&*!?]{8,}$";

    private static final Pattern pattern = Pattern.compile(PASSWORD_PATTERN);

    /**
     * Validate password strength
     * @param password the password input
     * @return true if valid, false otherwise
     */
    public static boolean isValid(String password) {
        if (password == null || password.trim().isEmpty()) {
            return false;
        }
        return pattern.matcher(password).matches();
    }

    /**
     * Validate only basic length (if you want a simpler rule)
     */
    public static boolean isMinLength(String password, int length) {
        return password != null && password.length() >= length;
    }
}
```

> **Note: Regular expression (Regex)**

```
    ^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&*!?])[A-Za-z\d@#$%^&*!?]{8,}$
```

This is a Java-style regex (works with String.matches() or Pattern.compile()), commonly used for password validation.
It ensures:
- `(?=.*[a-z])`: at least one lowercase letter
- `(?=.*[A-Z])`: at least one uppercase letter
- `(?=.*\d)`: at least one digit
- `(?=.*[@#$%^&*!?])`: at least one special character
- `[A-Za-z\d@#$%^&*!?]{8,}`: only allowed characters, length ≥ 8

- Update **LoginServlet**:

```java
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // Validate password format first
        // if (!PasswordValidator.isMinLength(password, 8)) {
        //     request.setAttribute("loginError", "Password must be at least 8 characters long.");

        //     request.setAttribute("contentPage", "login_form_content.jsp");
        //     request.getRequestDispatcher("views/pages/login.jsp").forward(request, response);
        //     return;
        // }
        
        if (!PasswordValidator.isValid(password)) {
            request.setAttribute("loginError", "Password must contain uppercase, lowercase, number, special char, and be at least 8 characters.");
            request.getRequestDispatcher("/views/login.jsp").forward(request,response);
            return;
        }
        
        UserDAO users = new UserDAO();
        String uName = users.CheckUser(username, password);
        if (uName != null) {
            Cookie cookie1 = new Cookie("username", uName);
            cookie1.setMaxAge(60 * 60); // 1 hour
            response.addCookie(cookie1);

            response.sendRedirect(request.getContextPath() + "/home");
        } else {
            request.setAttribute("loginError", "Username or password are failed!");
            request.getRequestDispatcher("/views/login.jsp").forward(request,response);
        }
    }

```

- Update **Login form (login_form_content.jsp):**

```jsp

    <!-- ... -->
    
    <button type="submit" class="btn btn-primary">Login</button>
    
    <p style="color:red;"><%= request.getAttribute("loginError") != null ? request.getAttribute("loginError") : "" %></p>
    
    <!-- ... -->
```

### Update password of Users

```sql
    UPDATE Users
    SET Password = '123456x@X'
    WHERE Username = 'admin';
```