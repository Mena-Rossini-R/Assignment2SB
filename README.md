
# Spring Boot MVC Assignment II - User Registration with Validation

##  Aim
To create a Spring Boot MVC application that validates a user registration form using server-side validation annotations and displays field-specific error messages in Thymeleaf.

---

##  Technologies Used
- Java
- Spring Boot
- Spring MVC
- Thymeleaf
- Maven
- Bean Validation (`jakarta.validation`)

---

##  Dependencies in `pom.xml`
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
```

---

## Model: `User.java`
```java
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

public class User {

    @NotBlank(message = "Name is required")
    private String name;

    @Email(message = "Email is invalid")
    @NotBlank(message = "Email is required")
    private String email;

    @Size(min = 6, message = "Password must be at least 6 characters")
    private String password;

    @NotBlank(message = "Confirm Password is required")
    private String confirmPassword;

    // Getters and Setters
}
```

---

## Controller: `RegistrationController.java`
```java
import jakarta.validation.Valid;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class RegistrationController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@Valid @ModelAttribute("user") User user,
                             BindingResult result,
                             Model model) {
        if (!user.getPassword().equals(user.getConfirmPassword())) {
            result.rejectValue("confirmPassword", "error.confirmPassword", "Passwords do not match");
        }

        if (result.hasErrors()) {
            return "register";
        }

        model.addAttribute("name", user.getName());
        return "success";
    }
}
```

---

## Template: `register.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head><title>Register</title></head>
<body>
<h2>Register</h2>
<form th:action="@{/register}" th:object="${user}" method="post">
    Name: <input type="text" th:field="*{name}" /><br/>
    <p th:if="${#fields.hasErrors('name')}" th:errors="*{name}"></p>

    Email: <input type="email" th:field="*{email}" /><br/>
    <p th:if="${#fields.hasErrors('email')}" th:errors="*{email}"></p>

    Password: <input type="password" th:field="*{password}" /><br/>
    <p th:if="${#fields.hasErrors('password')}" th:errors="*{password}"></p>

    Confirm Password: <input type="password" th:field="*{confirmPassword}" /><br/>
    <p th:if="${#fields.hasErrors('confirmPassword')}" th:errors="*{confirmPassword}"></p>

    <button type="submit">Register</button>
</form>
</body>
</html>
```

---

##  Template: `success.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head><title>Success</title></head>
<body>
<h2>Registration Successful!</h2>
<p>Welcome, <span th:text="${name}"></span></p>
</body>
</html>
```

---

##  How to Run

1. Create a new Spring Boot project using [Spring Initializr](https://start.spring.io/)
2. Add the dependencies as shown above.
3. Implement the model, controller, and templates.
4. Run the application and navigate to `http://localhost:8080/register`

---

## Output
### when given invalid password:

<img width="1920" height="1080" alt="Screenshot from 2025-07-29 14-32-32" src="https://github.com/user-attachments/assets/373ddfc6-f56d-401e-a819-332feb8cc232" />

<img width="1920" height="1080" alt="Screenshot from 2025-07-29 14-32-41" src="https://github.com/user-attachments/assets/6d0e61a0-e5d0-4b0a-bae7-a8b4f40f7c6d" />

### when given valid inputs:

<img width="1920" height="1080" alt="Screenshot from 2025-07-29 14-32-54" src="https://github.com/user-attachments/assets/a6dd8798-8f59-4c79-a519-7485407dbf8b" />

<img width="1920" height="1080" alt="Screenshot from 2025-07-29 14-33-02" src="https://github.com/user-attachments/assets/3cf06d5b-df3d-4b82-b65c-7b8ca7195612" />



