---
title: "How to Quickly Build a Selenium Automation Framework in Java using IntelliJ IDEA"
date: 2025-07-13
tags: ["selenium", "java", "automation testing", "intellij", "test automation", "maven"]
categories: ["Test Automation"]
draft: false
description: "Learn how to build a simple and scalable Selenium test automation framework in Java using IntelliJ IDEA, Maven, and TestNG. Ideal for beginners and testers transitioning into automation."
---

When I was first starting out with automation testing, I had the privilege to attend a live class where I built an automation framework. About a year later when I was thrown head first into my first automation project, I found myself absolutely flabbergasted at the beast of a framework I was looking at. 

It was scary, imposter syndrome kicked into overdrive.

I write this post today as redemption for that fresh-faced automation tester. 


If you're starting out in test automation or want to create a lightweight Selenium framework from scratch, this guide is for you. I’ll walk you through how to build a basic yet scalable automation framework in **Java** using **IntelliJ IDEA** — explaining the **what** *and* the **why** at each step.

By the end, you’ll have a working test setup you can build on with features like logging, reporting, and CI/CD.

---

## Tools & Technologies Used

We’re keeping it simple and industry-relevant:

- **Java** – Popular for enterprise-level testing
- **IntelliJ IDEA** – A powerful IDE for Java development
- **Maven** – For managing project dependencies
- **Selenium WebDriver** – The core automation library
- **TestNG** – A testing framework for organizing and running tests

---

## Step 1: Set Up Your Project in IntelliJ IDEA

> **Why?** Starting from scratch gives you full control and understanding. Using Maven simplifies dependency management.

1. Open IntelliJ and select **New Project**.
2. Choose **Maven**.
3. Set your JDK (Java 17+ recommended), and name your project (e.g., `SeleniumFramework`).
4. Click **Finish** and let IntelliJ build the structure.

---

## Step 2: Add Dependencies to `pom.xml`

> **Why?** Maven lets you manage libraries without manually downloading JAR files.

Add the following inside your `<dependencies>` tag:

```xml
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>4.20.0</version>
</dependency>
<dependency>
  <groupId>org.testng</groupId>
  <artifactId>testng</artifactId>
  <version>7.10.1</version>
  <scope>test</scope>
</dependency>
```
Right-click your project and select Maven > Reload Project.

---

## Step 3: Create the folder structure

Organize your files like this:

```bash
src
├── main
│   └── java
│       └── framework
│           ├── base       → base setup classes like DriverManager
│           └── utils      → reusable helpers (optional)
├── test
│   └── java
│       └── tests          → actual test classes
```
---

## Step 4: Set up the WebDriver

The WebDriver (driver) is the start of the show, literally the driver of the automation, you could call it the "bot" behind the automation framework
With that being said, it will be used in every single test, and it needs to be set up. Imagine having a whole suite of hundreds of tests... Do you get where this is going?
We need to have one central location where we set up the Driver once and apply it to every test in our suite. Let's do it like this:

File: ``` src/main/java/framework/base/DriverManager.java ```

```java
package framework.base;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class DriverManager {
private static WebDriver driver;

    public static void initDriver() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    public static WebDriver getDriver() {
        return driver;
    }

    public static void quitDriver() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```
Windows Tip: Make sure chromedriver.exe is in your system PATH, or specify it like this: 

```java
System.setProperty("webdriver.chrome.driver", "C:\\path\\to\\chromedriver.exe");
```

---

## Step 5: Create a Base Test Class

This ensures that setup and teardown logic (like opening and closing the browser) is automatically applied to all test classes.

File: ```src/main/java/framework/base/BaseTest.java```

```java
package framework.base;

import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

public class BaseTest {

    @BeforeMethod
    public void setUp() {
        DriverManager.initDriver();
    }

    @AfterMethod
    public void tearDown() {
        DriverManager.quitDriver();
    }
}

```

Any test class that extends BaseTest will automatically open and close the browser.

---

## Step 6: Write Your First Test

Just like that we've done enough set up to get us started on our first test. 

File: ``` src/test/java/tests/GoogleSearchTest.java ```

```java
package tests;

import framework.base.BaseTest;
import framework.base.DriverManager;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.testng.annotations.Test;

public class GoogleSearchTest extends BaseTest {

    @Test
    public void testGoogleSearch() {
        WebDriver driver = DriverManager.getDriver();
        driver.get("https://www.google.com");
        driver.findElement(By.name("q")).sendKeys("Selenium WebDriver");
        driver.findElement(By.name("q")).submit();
        System.out.println("Page title is: " + driver.getTitle());
    }
}

```
---

## Step 7: Run Your Test

The moment we've all been waiting for. To see your masterpiece in action you have two options:

1. Right-click the test file in IntelliJ and click 'Run'

2. Create a ```testng.xml``` file to organize and execute suites:
```xml
<suite name="AutomationSuite">
    <test name="GoogleTests">
        <classes>
            <class name="tests.GoogleSearchTest"/>
        </classes>
    </test>
</suite>

```

To execute, right-click the ```testng.xml```, all included tests will run.

---


