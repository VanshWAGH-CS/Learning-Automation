# 🎯 CSS Selectors in Selenium — Revision Notes

---

## 📌 What is CSS?

**CSS** = **Cascading Style Sheets**

Used by web developers to design web pages — applying colors, sizes, alignment, fonts, etc.

In Selenium, **CSS Selector** is a *locator strategy* that lets us find web elements using CSS-style expressions.

> 💡 CSS Selector is a **customized locator** — we *create our own* using attributes from the HTML.

---

## 🔑 Key Symbols to Remember

| Symbol | Represents | Example |
|--------|-----------|---------|
| `#` | ID attribute | `input#myId` |
| `.` | Class attribute | `input.myClass` |
| `[ ]` | Any other attribute | `input[name='q']` |

---

## 🗂️ 4 Types of CSS Selector Combinations

```
┌─────────────────────────────────────────────────────────────┐
│                    CSS SELECTOR TYPES                       │
├─────────────────────┬───────────────────────────────────────┤
│  Type               │  Syntax                               │
├─────────────────────┼───────────────────────────────────────┤
│  Tag + ID           │  tagName#idValue                      │
│  Tag + Class        │  tagName.className                    │
│  Tag + Attribute    │  tagName[attribute='value']           │
│  Tag + Class + Attr │  tagName.className[attribute='value'] │
└─────────────────────┴───────────────────────────────────────┘
```

> ⚠️ **Tag name is OPTIONAL** in all combinations — but the symbol (`#`, `.`, `[`) must remain.

---

## 🔵 Type 1 — Tag + ID

**Syntax:**
```
tagName#idValue
```
or without tag:
```
#idValue
```

**Selenium Code:**
```java
driver.findElement(By.cssSelector("input#small-searchterms")).sendKeys("t-shirts");
// Without tag name:
driver.findElement(By.cssSelector("#small-searchterms")).sendKeys("t-shirts");
```

**When to use:** When `id` attribute is available for the element.

> 📝 ID is always **unique** — no two elements share the same ID on a page.

---

## 🟢 Type 2 — Tag + Class

**Syntax:**
```
tagName.className
```
or without tag:
```
.className
```

**Selenium Code:**
```java
driver.findElement(By.cssSelector("input.search-box-text")).sendKeys("t-shirts");
// Without tag name:
driver.findElement(By.cssSelector(".search-box-text")).sendKeys("t-shirts");
```

**When to use:** When `class` attribute is available.

> ⚠️ If class name contains spaces (multiple classes), use only the **first part** before the space. Including spaces in CSS class causes matching failure.

**Example:**
```
class="search-box ion-text ui-autocomplete-input"
          ✅ Use this part only
```

---

## 🟡 Type 3 — Tag + Attribute

**Syntax:**
```
tagName[attribute='value']
```
or without tag:
```
[attribute='value']
```

**Selenium Code:**
```java
driver.findElement(By.cssSelector("input[placeholder='Search store...']")).sendKeys("t-shirts");
// or using name attribute:
driver.findElement(By.cssSelector("input[name='q']")).sendKeys("t-shirts");
```

**When to use:** When ID and class are not available, or you want to use any other attribute like `placeholder`, `name`, `type`, `href`, etc.

> 💡 Inside the `[ ]`, you can use **single quotes** `' '` or **double quotes** `" "`.  
> In Java, prefer **single quotes** inside the CSS string to avoid escaping issues.

---

## 🔴 Type 4 — Tag + Class + Attribute

**Syntax:**
```
tagName.className[attribute='value']
```
or without tag:
```
.className[attribute='value']
```

**Selenium Code:**
```java
driver.findElement(By.cssSelector("input.search-box-text[name='q']")).sendKeys("t-shirts");
```

**When to use:** When tag + class alone matches **multiple elements** and you need to pinpoint one specific element.

---

## 🔄 Flow: Choosing the Right CSS Selector

```
                    Need to locate an element?
                            │
                    Does it have an ID?
                    ┌───────┴───────┐
                   YES             NO
                    │               │
            Use Tag + ID    Does it have a Class?
                            ┌───────┴───────┐
                           YES             NO
                            │               │
                    Use Tag + Class   Use Tag + Attribute
                            │
                Does Tag+Class match only 1 element?
                    ┌───────┴───────┐
                   YES             NO
                    │               │
                  Done!    Add extra attribute →
                           Use Tag + Class + Attribute
```

---

## 🖥️ Maximizing Browser Window

Always add this after launching the browser:

```java
driver.manage().window().maximize();
```

**Method chaining explained:**

```
driver.manage()        → returns Options object
       .window()       → returns Window object
       .maximize()     → maximizes the browser window
```

---

## ✅ Verifying CSS Selectors Before Automation

Before using a CSS selector in code, verify it in the browser:

1. Open **DevTools** → Press `Ctrl + F` (or `Cmd + F` on Mac)
2. Type your CSS selector in the search box
3. It highlights the matching element(s) on the page
4. Number shown = how many elements match

```
Example: input#small-searchterms → 1 of 1 ✅ (unique match)
         li.nav-item             → 7 of 7 ⚠️ (matches multiple)
```

---

## 🛠️ Selector Hub — Auto-Generate Locators

**Selector Hub** is a browser extension that automatically generates all types of locators.

### How to Install:
1. Go to Chrome Web Store
2. Search **"SelectorHub"**
3. Click **Add to Chrome** → **Add Extension**

### How to Use:

| Method | Steps |
|--------|-------|
| **From Plugin Icon** | Click the SelectorHub icon in toolbar → Use inspect arrow → Click any element |
| **From DevTools** | `F12` → Go to DevTools bottom panel → Find "SelectorHub" tab |
| **Right-click shortcut** | Right-click element → SelectorHub menu → Copy CSS / XPath directly |

### What Selector Hub Shows:
- Relative CSS Selector
- Relative XPath
- Absolute XPath
- ID, Name, Class, Tag Name
- How many elements match each locator

> 🌟 **Advantage over DevTools**: Works on hidden elements, Shadow DOM, iframe elements — DevTools often fails on these.

---

## ⚠️ Common Mistakes

| Mistake | Fix |
|---------|-----|
| Including full class with spaces: `.search-box ion-text` | Use only first class: `.search-box` |
| Using double quotes inside Java string: `"input[name="q"]"` | Use single quotes inside: `"input[name='q']"` |
| Forgetting `#` when removing tag for ID: `small-searchterms` | Always keep `#`: `#small-searchterms` |
| Forgetting `.` when removing tag for class: `search-box` | Always keep `.`: `.search-box` |

---

## 📊 Quick Reference Table

| Combination | Symbol | Syntax | Tag Optional? |
|-------------|--------|--------|:---:|
| Tag + ID | `#` | `input#myId` | ✅ |
| Tag + Class | `.` | `input.myClass` | ✅ |
| Tag + Attribute | `[ ]` | `input[name='q']` | ✅ |
| Tag + Class + Attr | `. []` | `input.myClass[name='q']` | ✅ |

---

## 🧪 Assignment

Practice on **[OpenCart Demo](https://demo.opencart.com)** — write CSS selectors covering **all 4 combinations**:

- [ ] Locate search box using **Tag + ID**
- [ ] Locate an element using **Tag + Class**
- [ ] Locate an element using **Tag + Attribute** (placeholder / name)
- [ ] Locate a specific element using **Tag + Class + Attribute**
- [ ] Verify each locator using `Ctrl+F` in DevTools before using in Selenium

---

## 📝 Key Takeaways

1. CSS Selector is a **customized locator** — formed by combining tag + attributes from HTML.
2. **Tag name is always optional** in all combinations.
3. `#` = ID, `.` = Class, `[ ]` = Any attribute — memorize these 3 symbols.
4. When a locator matches **multiple elements**, add more attributes to narrow it down.
5. Always **verify your CSS selector** in DevTools (`Ctrl+F`) before writing automation code.
6. Use **Selector Hub** extension to auto-generate and verify all locator types quickly.
7. Next topic: **XPath** — used 80–90% of the time in real projects; handles dynamic attributes.

---
