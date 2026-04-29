# Selenium WebDriver – Revision Notes
### Day 22: Locators & Actions

---

## 1. The Two Pillars of Automation

Every automation task boils down to just **two things**:

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│   STEP 1: LOCATE the element on the web page        │
│              ↓                                      │
│   STEP 2: PERFORM an action on that element         │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 2. Types of Locators

```
                        LOCATORS
                           │
           ┌───────────────┴───────────────┐
           │                               │
     BASIC LOCATORS                CUSTOMIZED LOCATORS
  (found directly in HTML)       (user-generated / dynamic)
           │                               │
   ┌───────┴────────┐              ┌───────┴───────┐
   │                │              │               │
  ID              Name           XPath            CSS
  Link Text       Class Name     Selector
  Partial         Tag Name
  Link Text
```

| Feature | Basic Locators | Customized Locators |
|---|---|---|
| Found in HTML? | ✅ Yes, directly | ❌ No, must be written/generated |
| Examples | `id`, `name`, `linkText`, `className`, `tagName` | `xpath`, `cssSelector` |
| Flexibility | Fixed / static | Dynamic, can handle changing attributes |
| User-defined? | No | Yes |

---

## 3. Basic HTML Concepts You Need to Know

Before using locators, you must be able to read HTML:

```html
<input type="text" id="search" name="search" class="form-input" placeholder="Search...">
  │         │           │           │               │                    │
 TAG    ATTRIBUTE    VALUE       ATTRIBUTE        ATTRIBUTE           ATTRIBUTE
 NAME   (type)      (text)      (id)             (name)              (class)
```

- **Tag name** → the element type (`input`, `a`, `div`, `img`, `button`)
- **Attribute** → property of the element (`id`, `name`, `class`, `href`, `type`)
- **Value** → the actual value of that attribute

> 💡 To inspect HTML: Right-click on any element on the web page → **Inspect** → **Elements** tab

---

## 4. Basic Locators – When to Use What

### 4.1 ID Locator
- Used when the element has a unique `id` attribute
- **Most reliable** basic locator

```java
WebElement logo = driver.findElement(By.id("logo"));
boolean isDisplayed = logo.isDisplayed();
System.out.println(isDisplayed); // true or false
```

```html
<!-- HTML -->
<div id="logo"><img src="..."/></div>
```

---

### 4.2 Name Locator
- Used when `name` attribute is present

```java
WebElement searchBox = driver.findElement(By.name("search"));
searchBox.sendKeys("mac");
```

```html
<!-- HTML -->
<input type="text" name="search" ...>
```

---

### 4.3 Link Text vs Partial Link Text

> These locators work **ONLY for link elements** (`<a>` tags with `href` attribute)

**What is Link Text?**
```html
<a href="/tablets">Tablets</a>
         ↑ href = target page    ↑ this is the LINK TEXT
```

| Locator | What you pass | Example |
|---|---|---|
| `linkText` | Full text of the link | `"Tablets"` |
| `partialLinkText` | Any portion of the link text | `"Tab"` or `"ablet"` |

```java
// Link Text – pass full text
driver.findElement(By.linkText("Tablets")).click();

// Partial Link Text – pass partial text
driver.findElement(By.partialLinkText("Tab")).click();
```

**Which one to prefer?**

```
┌─────────────────────────────────────────────────────────────┐
│  Always prefer LINK TEXT over PARTIAL LINK TEXT             │
│                                                             │
│  Reason: Partial link text may match MULTIPLE elements      │
│  on the same page → ambiguous → unreliable                  │
│                                                             │
│  Use Partial Link Text ONLY when:                           │
│  → There is exactly ONE link containing that text           │
│  → No other link shares that partial text                   │
└─────────────────────────────────────────────────────────────┘
```

**Example of the conflict problem:**

```
Web page links: [Tablets]  [Tables]

Using By.partialLinkText("tab") → matches BOTH ❌
Using By.linkText("Tablets")   → matches only ONE ✅
```

---

### 4.4 Class Name & Tag Name – For Groups of Elements

These two locators are used to **capture multiple / group web elements**, not single ones.

**Why?** Because the same class and tag name is shared across many elements:

```
All <img> tags   → same tag name: "img"
All <a> tags     → same tag name: "a"
All checkboxes   → same class name value
All dropdowns    → same class name value
```

**Use Case: Count all links on a page**
```java
List<WebElement> allLinks = driver.findElements(By.tagName("a"));
System.out.println("Total links: " + allLinks.size());
```

**Use Case: Count all images on a page**
```java
List<WebElement> allImages = driver.findElements(By.tagName("img"));
System.out.println("Total images: " + allImages.size());
```

**Use Case: Count specific group elements using class name**
```java
List<WebElement> headerLinks = driver.findElements(By.className("list-inline-item"));
System.out.println("Header links: " + headerLinks.size());
```

---

## 5. `findElement` vs `findElements`

### Basic Difference

| Method | Returns | Return Type |
|---|---|---|
| `findElement` | Single web element | `WebElement` |
| `findElements` | Group of web elements | `List<WebElement>` |

> 📌 `findElements` returns a **List** (not a Set) because duplicate elements are possible.

---

### Scenario Comparison Table

| Scenario | Method Used | Locator Matches | Result | Return Type |
|---|---|---|---|---|
| 1 | `findElement` | 1 element | Returns that 1 element | `WebElement` |
| 2 | `findElements` | 1 element | Returns list with 1 element | `List<WebElement>` |
| 3 | `findElement` | Multiple elements | Returns **first** element on page | `WebElement` |
| 4 | `findElements` | Multiple elements | Returns all matching elements | `List<WebElement>` |
| 5 | `findElement` | **No match** | ❌ Throws **NoSuchElementException** | — |
| 6 | `findElements` | **No match** | ✅ Returns **empty list** (size = 0), no exception | `List<WebElement>` |

---

### Key Scenario – No Element Found

```
Locator doesn't match any element
             │
    ┌────────┴────────┐
    │                 │
findElement      findElements
    │                 │
    ▼                 ▼
Throws          Returns empty list
NoSuchElement   size() = 0
Exception ❌    No Exception ✅
```

> 💡 **Interview Tip:** The most important difference is **Scenario 5 vs 6** — exception vs empty list when no element is found.

---

## 6. Quick Locator Cheat Sheet

| Locator | `By` Method | Best Used For | Single or Group? |
|---|---|---|---|
| ID | `By.id("value")` | Unique elements | Single |
| Name | `By.name("value")` | Form inputs | Single |
| Link Text | `By.linkText("value")` | Anchor/link elements | Single |
| Partial Link Text | `By.partialLinkText("value")` | Links (when unique) | Single |
| Class Name | `By.className("value")` | Group of same-category elements | Group |
| Tag Name | `By.tagName("value")` | All elements of same type | Group |
| XPath | `By.xpath("expression")` | Complex / dynamic elements | Both |
| CSS Selector | `By.cssSelector("selector")` | Complex / dynamic elements | Both |

---

## 7. Full Code Example – Locators in Action

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import java.util.List;

public class LocatorsDemo {
    public static void main(String[] args) {

        // Setup
        WebDriver driver = new ChromeDriver();
        driver.get("https://demo.opencart.com/");
        driver.manage().window().maximize();

        // 1. NAME locator – enter text in search box
        WebElement searchBox = driver.findElement(By.name("search"));
        searchBox.sendKeys("mac");

        // 2. ID locator – check if logo is displayed
        WebElement logo = driver.findElement(By.id("logo"));
        boolean logoDisplayed = logo.isDisplayed();
        System.out.println("Logo displayed: " + logoDisplayed);

        // 3. LINK TEXT – click on a navigation link
        driver.findElement(By.linkText("Tablets")).click();

        // 4. TAG NAME – count all links on the page
        List<WebElement> allLinks = driver.findElements(By.tagName("a"));
        System.out.println("Total links: " + allLinks.size());

        // 5. TAG NAME – count all images on the page
        List<WebElement> allImages = driver.findElements(By.tagName("img"));
        System.out.println("Total images: " + allImages.size());

        // 6. CLASS NAME – count specific group of elements
        List<WebElement> headerLinks = driver.findElements(By.className("list-inline-item"));
        System.out.println("Header links: " + headerLinks.size());

        driver.quit();
    }
}
```

---

## 8. Common Actions on Web Elements

| Element Type | Common Action | Method |
|---|---|---|
| Input box | Type text | `.sendKeys("text")` |
| Button / Link | Click | `.click()` |
| Any element | Check visibility | `.isDisplayed()` |
| Any element | Get text content | `.getText()` |

---

## 9. Key Differences Summary

### Link Text vs Partial Link Text
```
Link Text        → Full text   → Preferred ✅ → Unique match
Partial Link Text → Partial text → Risky ⚠️  → May match multiple elements
```

### findElement vs findElements
```
findElement  → returns WebElement        → throws exception if not found
findElements → returns List<WebElement>  → returns empty list if not found
```

### Basic vs Customized Locators
```
Basic (ID, Name, LinkText...)  → found in HTML  → may fail if attrs change
Customized (XPath, CSS)        → user-written    → dynamic, more reliable
```

---

## 10. Coming Up Next

- **XPath** – writing custom paths to elements (static & dynamic)
- **CSS Selectors** – another powerful customized locator
- **Handling different element types** – dropdowns, checkboxes, radio buttons, alerts

---

## Assignment

Using the application `https://demo.opencart.com/`:
1. Find **total number of links** on the homepage and print it
2. Find **total number of images** on the homepage and print it
3. Click on any product link using **`linkText`**
4. Click on another link using **`partialLinkText`**
