# Selenium WebDriver – Revision Notes
### Day 21: Introduction, Environment Setup & First Automation Script

---

## 1. What is Selenium?

Selenium is a **suite of three components**:
- **Selenium IDE** – record/playback tool
- **Selenium WebDriver** – main automation component for web apps
- **Selenium Grid** – for parallel/distributed test execution

---

## 2. Three Definitions of WebDriver

### Definition 1 – Component of Selenium
WebDriver is the primary component in the Selenium suite used to **automate web applications**.

### Definition 2 – Java Interface
WebDriver is designed as a **Java interface**, which means:
- It contains public, static, abstract, and default methods
- It cannot be instantiated directly
- It must be **implemented by a class**

**Hierarchy:**
```
<<interface>> WebDriver
      ↑ (also extends)
<<interface>> SearchContext   ← root interface
      ↑
<<class>> RemoteWebDriver     ← directly implements WebDriver
      ↑
<<class>> ChromiumDriver      ← intermediate class (Selenium 4+)
      ↑              ↑
ChromeDriver      EdgeDriver  ← final browser-specific classes
      
<<class>> FirefoxDriver       ← also extends RemoteWebDriver
```

> **Key point:** A `WebDriver` variable can hold objects of `ChromeDriver`, `EdgeDriver`, `FirefoxDriver`, etc., because they are all child classes (upcasting).

### Definition 3 – API (Application Programming Interface)
WebDriver acts as an **API** because it sits between two layers:
- **Client layer** → Java program (calls WebDriver methods)
- **Backend layer** → Browser (where actions are performed)

Just as a waiter (API) takes orders (requests) from a customer (client) and brings food (response) from the kitchen (backend), WebDriver bridges Java code and the browser.

> From **Selenium 4 onwards**, the W3C protocol replaced the older JSON Wire Protocol for browser communication.

---

## 3. Environment Setup

### Approach 1 – Manual (Not Recommended)
1. Create a **Java Project** in Eclipse
2. Download Selenium JAR files (zip) from [selenium.dev](https://selenium.dev)
3. Extract the zip
4. Right-click project → **Properties** → **Java Build Path** → **Libraries** → **Add External JARs**
5. Add all JARs including those inside the `/libs` folder

**Problem:** Upgrading versions requires manually removing old JARs and repeating all steps.

---

### Approach 2 – Maven Project (Recommended ✅)

**Steps:**
1. Create a **Maven Project** in Eclipse (File → New → Maven Project → check "Create a simple project")
2. Provide **Group ID** and **Artifact ID** (e.g., `selenium-web-driver`)
3. Open `pom.xml` and add the Selenium dependency inside a `<dependencies>` tag

```xml
<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.8.1</version>
    </dependency>
</dependencies>
```

4. Right-click project → **Maven** → **Update Project** (check "Force update of Snapshots/Releases")
5. All JARs are automatically downloaded and attached under **Maven Dependencies**

**To upgrade version:** Just change the version number in `pom.xml` and update the project — old JARs are removed and new ones downloaded automatically.

> Find all dependencies at: [mvnrepository.com](https://mvnrepository.com)

---

### Default Maven Project Structure

| Folder | Purpose |
|---|---|
| `src/main/java` | Developer code |
| `src/main/resources` | Developer resource files |
| `src/test/java` | **Automation test scripts** ← use this |
| `src/test/resources` | Test resource files (Excel, properties, etc.) |

---

## 4. Browsers Supported by Selenium

| Browser | Driver Class |
|---|---|
| Chrome | `ChromeDriver` |
| Edge | `EdgeDriver` |
| Firefox | `FirefoxDriver` |
| Safari | `SafariDriver` |
| Opera | `OperaDriver` |

> **Note (Selenium 4+):** Browser-specific drivers (like `chromedriver.exe`) are now **bundled** within the Selenium JARs. No separate download needed.

**Supported OS:** Windows, Mac, Linux  
**Supported Languages:** Java, Python, JavaScript, Ruby, C#

---

## 5. Writing Your First Automation Script

### Test Case (Manual Steps):
1. Launch Chrome browser
2. Open URL: `http://tutorialsninja.com/demo/`
3. Verify title of the page is **"Your Store"**
4. Close the browser

### Automation Script:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class FirstTestCase {
    public static void main(String[] args) {

        // Step 1: Launch Chrome browser
        WebDriver driver = new ChromeDriver();

        // Step 2: Open URL
        driver.get("http://tutorialsninja.com/demo/");

        // Step 3: Validate the title
        String actualTitle = driver.getTitle();
        if (actualTitle.equals("Your Store")) {
            System.out.println("Test Passed");
        } else {
            System.out.println("Test Failed");
        }

        // Step 4: Close the browser
        driver.close(); // or driver.quit()
    }
}
```

---

## 6. Key WebDriver Methods Used

| Method | Purpose |
|---|---|
| `driver.get("url")` | Opens the given URL in the browser |
| `driver.getTitle()` | Returns the title of the current page |
| `driver.close()` | Closes the current browser window |
| `driver.quit()` | Closes all browser windows |

---

## 7. Switching Browsers – Just Change the Object

```java
// Chrome
WebDriver driver = new ChromeDriver();

// Edge
WebDriver driver = new EdgeDriver();

// Firefox
WebDriver driver = new FirefoxDriver();
```

> All other code (methods) stays the same — only the driver object changes.

**Prerequisite:** The respective browser must be **installed** on your system and kept **up to date**.

---

## 8. Important Concepts Summary

| Concept | Detail |
|---|---|
| WebDriver | Java Interface → implemented by RemoteWebDriver |
| RemoteWebDriver | Parent class of ChromiumDriver, FirefoxDriver, etc. |
| ChromiumDriver | Parent of ChromeDriver and EdgeDriver (Selenium 4+) |
| SearchContext | Root interface that WebDriver extends |
| pom.xml | Central Maven config file; controls all dependencies |
| Maven Dependency | XML snippet that auto-downloads required JARs |
| `driver.get()` | Opens URL — takes String parameter |
| `driver.getTitle()` | Captures page title — returns String |
| Web Element | Any visible UI item: button, input, link, checkbox, dropdown |

---

## 9. Coming Up Next

- **Locating Web Elements** (2–3 sessions):
  - By ID, Name, XPath, CSS Selector, LinkText, etc.
- **Performing Actions on Elements**:
  - `click()`, `sendKeys()`, `select()`, etc.
- **TestNG Integration**:
  - Reports, parallel testing, parameterization

---

## Assignment

Write an automation script to:
1. Launch Chrome browser
2. Open a URL of your choice
3. Verify the title of the page
4. Close the browser

> Practice the Maven project setup + the class example first, then attempt the assignment.
