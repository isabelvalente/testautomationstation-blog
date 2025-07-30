---
title: "How to Easily Add Allure Reporting to Your Java Selenium Framework in 2025"
date: 2025-07-30
tags: [selenium, java, allure, testautomation, junit, maven]
---

What is testing without reporting? It is bread without butter. It is a beach with no sand. Without clean reports, bugs could slide, priorities could slip and test teams could literally fall apart.

In this post, we’ll add **Allure reporting** to the Selenium automation framework we built in [our previous post](../javaautomationframeworkintenminutes.md/). Allure provides **clean, visual, interactive reports** that help you understand your test results quickly and clearly.

---

## What You’ll Need

- Java 17+
- Maven
- JUnit 5 (already in our framework)
- Allure Maven plugin
- IntelliJ or any Java IDE
- Git (optional)

---

## Step 1: Add Allure Dependencies

In your `pom.xml`, add the following dependencies and plugin:

```xml
<dependencies>
  <!-- Add this inside the <dependencies> block -->
  <dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-junit5</artifactId>
    <version>2.25.0</version>
  </dependency>
</dependencies>

<build>
  <plugins>
    <!-- Add this inside the <plugins> block -->
    <plugin>
      <groupId>io.qameta.allure</groupId>
      <artifactId>allure-maven</artifactId>
      <version>2.11.2</version>
    </plugin>
  </plugins>
</build>
```
> `allure-junit5` integrates with your JUnit test lifecycle. The `allure-maven` plugin helps generate reports directly from the CLI.

---

##  Step 2: Annotate Your Test Classes

Allure uses annotations to provide more meaningful output. You can add these to your test classes and methods.

```java
import io.qameta.allure.*;

@Epic("Login Feature")
@Feature("Login with valid credentials")
@Story("User logs in successfully with correct username and password")
@Severity(SeverityLevel.CRITICAL)
@Owner("Isabel Valente")
@Test
public void successfulLoginTest() {
    // Your test code here
}
```

> You can also use `@Step` on helper methods to log actions step-by-step.

---

## Step 3: Run Tests and Generate Report

### First, run your tests as usual:

```bash
mvn clean test
```

### Then, generate the report:

```bash
mvn allure:report
```

### Finally, open the report in your browser:

```bash
mvn allure:serve
```

This will start a local server (usually at `http://localhost:port`) and open the report in your browser automatically.

---

## What You’ll See

The Allure report includes:

- Test history with pass/fail trends
- Step-by-step actions
- Screenshots (optional if added)
- Tags like epic, feature, severity, and owner

---

## Optional: Add Screenshots on Failure

If you want to attach screenshots to failed tests, you can enhance your `@AfterEach` method like this:

```java
@AfterEach
public void tearDown(TestInfo testInfo) {
    if (driver != null) {
        // Take screenshot logic here
        Allure.addAttachment("Screenshot", new ByteArrayInputStream(takeScreenshot()));
        driver.quit();
    }
}

public byte[] takeScreenshot() {
    return ((TakesScreenshot) driver).getScreenshotAs(OutputType.BYTES);
}
```

---

## Tip: Add Allure to Your GitHub Actions Workflow

If you're running your tests on GitHub Actions, you can add Allure to the workflow and even upload the generated HTML report as an artifact.


---

## Summary

With just a few dependencies and some annotations, Allure adds **professional-grade reporting** to your Java Selenium framework. It's a simple upgrade that makes your test results more accessible and insightful.

---

## Coming Next

In the next post, we’ll look at **adding screenshot and video recording** to failed Selenium tests. This is perfect for debugging and the video recording especially, is an often missed feature in test teams.

---

Thanks for reading! 

Follow me for more automation tips and Java tutorials.


