# Java Spring MVC web application!

### Step 1: Create a new Maven project in Eclipse IDE by selecting File > New > Project > Maven Project. Choose an archetype from the list or create a simple webapp archetype. Enter a Group ID and Artifact ID and click Finish.

### Step 2: Once the project is created, you can observe the following folder structure:

```
project
|-- src/main/java
|   |-- com.example.calculator
|   |   |-- controller
|   |   |   `-- CalculatorController.java
|   |   |-- service
|   |   |   `-- CalculatorService.java
|   |   `-- Application.java
|-- src/main/resources
|   |-- application.properties
|   `-- templates
|       `-- calculator.html
|-- src/test/java
|   `-- com.example.calculator.service
|       `-- CalculatorServiceTest.java
|-- pom.xml
`-- README.md
```

### Step 3: Create a web page named `calculator.html` under the `src/main/resources/templates` folder. This page contains a simple form with two input fields to input two numbers and a submit button to submit the form.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Calculator</title>
</head>
<body>
	<h1>Calculator</h1>
	<form action="/calculate" method="post">
		<input type="number" name="num1" placeholder="Enter first number"><br>
		<input type="number" name="num2" placeholder="Enter second number"><br>
		<select name="operation">
			<option value="addition">Addition</option>
			<option value="subtraction">Subtraction</option>
			<option value="multiplication">Multiplication</option>
			<option value="division">Division</option>
		</select><br>
		<input type="submit" value="Calculate">
	</form>
</body>
</html>
```

### Step 4: Create a Controller class named `CalculatorController` under the `src/main/java/com.example.calculator.controller` package. This class contains a method that maps to the form submission and forwards the request to a method defined in the Service or business logic class.

```
package com.example.calculator.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.example.calculator.service.CalculatorService;

@Controller
public class CalculatorController {

    private final CalculatorService calculatorService;

    public CalculatorController(CalculatorService calculatorService) {
        this.calculatorService = calculatorService;
    }

    @PostMapping("/calculate")
    public String calculate(@RequestParam("num1") double num1,
                             @RequestParam("num2") double num2,
                             @RequestParam("operation") String operation,
                             Model model) {
        double result = 0;
        switch (operation) {
            case "addition":
                result = calculatorService.add(num1, num2);
                break;
            case "subtraction":
                result = calculatorService.subtract(num1, num2);
                break;
            case "multiplication":
                result = calculatorService.multiply(num1, num2);
                break;
            case "division":
                result = calculatorService.divide(num1, num2);
                break;
        }
        model.addAttribute("result", result);
        return "calculator";
    }
}
```


### Step 5: Create a Service class named `CalculatorService` under the `src/main/java/com.example.calculator.service`

### Step 6: Implement the methods in the `CalculatorService` class to perform the required calculator operations.

```
package com.example.calculator.service;

import org.springframework.stereotype.Service;

@Service
public class CalculatorService {

    public double add(double num1, double num2) {
        return num1 + num2;
    }

    public double subtract(double num1, double num2) {
        return num1 - num2;
    }

    public double multiply(double num1, double num2) {
        return num1 * num2;
    }

    public double divide(double num1, double num2) {
        if (num2 == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return num1 / num2;
    }
}
```

### Step 7: Configure the application properties by creating a `application.properties` file under the `src/main/resources` folder. Add the following properties to it:

```
spring.mvc.view.prefix=/templates/
spring.mvc.view.suffix=.html
```

### Step 8: Create a main class named Application under the `src/main/java/com.example.calculator` package. This class contains the main method that starts the Spring application.

```
package com.example.calculator;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Step 9: Create a unit test class named `CalculatorServiceTest` under the `src/test/java/com.example.calculator.service` package. This class contains test methods for the calculator operations.

```
package com.example.calculator.service;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

class CalculatorServiceTest {

    private CalculatorService calculatorService;

    @BeforeEach
    void setUp() {
        calculatorService = new CalculatorService();
    }

    @Test
    void testAdd() {
        double result = calculatorService.add(2, 3);
        Assertions.assertEquals(5, result);
    }

    @Test
    void testSubtract() {
        double result = calculatorService.subtract(5, 3);
        Assertions.assertEquals(2, result);
    }

    @Test
    void testMultiply() {
        double result = calculatorService.multiply(2, 3);
        Assertions.assertEquals(6, result);
    }

    @Test
    void testDivide() {
        double result = calculatorService.divide(6, 3);
        Assertions.assertEquals(2, result);
        Assertions.assertThrows(ArithmeticException.class, () -> calculatorService.divide(6, 0));
    }
}
```

### Step 10: Run the application by right-clicking on the `Application` class and selecting Run As > Java Application. The application will start on the embedded Tomcat server.

### Step 11: Open a web browser and go to `http://localhost:8080/` to see the calculator web page. Enter two numbers and select an operation, then click the Calculate button. The result will be displayed on the same page.

### Step 12: To package and host the application on Tomcat without running it from Eclipse, run the following Maven command in the project directory:

```
mvn clean package
```

### This will create a WAR file under the `target` folder. Copy the WAR file to the `webapps` folder of your Tomcat installation, then start Tomcat by running the `bin/startup.bat` script (on Windows) or bin/startup.sh script (on Linux).

### Step 13: To debug a Controller method, add a breakpoint in the method, then right-click on the Debug As > Debug on Server option. Select your Tomcat server and click on the OK button. This will start the Tomcat server in debug mode and attach the Eclipse debugger to it.

### Step 14: In Eclipse, open the `Debug` perspective by clicking on the Window > Perspective > Open Perspective > Debug menu option. Select the Controller class that you want to debug, then add a breakpoint in the method that you want to debug.

### Step 15: Run the application in debug mode by clicking on the Debug button or selecting Debug As > Debug on Server. When the application reaches the breakpoint, the debugger will stop and allow you to step through the code line by line using the F6 (step over), F5 (step into), and F8 (resume) keys.

### Step 16: To add more unit test cases for the service class methods, add new test methods to the `CalculatorServiceTest` class and use the `Assertions` class to verify the expected results.

#### For example:

```
@Test
void testAddNegativeNumbers() {
    double result = calculatorService.add(-2, -3);
    Assertions.assertEquals(-5, result);
}

@Test
void testSubtractNegativeNumbers() {
    double result = calculatorService.subtract(-5, -3);
    Assertions.assertEquals(-2, result);
}

@Test
void testMultiplyNegativeNumbers() {
    double result = calculatorService.multiply(-2, -3);
    Assertions.assertEquals(6, result);
}

@Test
void testDivideNegativeNumbers() {
    double result = calculatorService.divide(-6, -3);
    Assertions.assertEquals(2, result);
}
```

### Step 17: To run the unit test cases, right-click on the `CalculatorServiceTest` class and select Run As > JUnit Test. The test runner will execute all the test methods in the class and report the results.


### Step 18: Once you have completed building and testing your application, it is time to package it into a WAR file and deploy it to a Tomcat server. To do this, first, right-click on your project in Eclipse and select Export > WAR File. Follow the prompts to export the WAR file to a location on your local machine.

### Step 19: Next, copy the WAR file to the `webapps` directory of your Tomcat server. This directory is typically located at `Tomcat_Home/webapps`.

### Step 20: Start your Tomcat server by running the `startup.bat` (Windows) or `startup.sh` (Linux/Mac) file in the `bin` directory of your Tomcat installation.

### Step 21: Open a web browser and navigate to `http://localhost:8080/your-application-name` where `your-application-name` is the name of your deployed WAR file. If everything is working correctly, you should see your calculator web application running on the Tomcat server.

### Step 22: To stop the Tomcat server, run the `shutdown.bat` (Windows) or `shutdown.sh` (Linux/Mac) file in the `bin` directory of your Tomcat installation.

## Congratulations, you have successfully built and deployed a Java Spring MVC web application with a calculator feature using Maven and Tomcat!
