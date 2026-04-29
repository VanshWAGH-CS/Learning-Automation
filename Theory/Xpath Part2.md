# 🧭 XPath Locators in Selenium — Revision Notes

---

## 📌 What is XPath?

- **XPath** = **XML Path** (derived from XML — Extensible Markup Language)
- XPath is the **address of an element** on a web page
- XPath works based on the **DOM** (Document Object Model)

### What is DOM?
> **DOM** = **Document Object Model** — a tree structure loaded in the browser at runtime when you open any web page. Every HTML tag is a **node** in this tree.

```
When you open a URL → Browser loads the DOM → XPath reads from the DOM → Element is located
```

> ⚠️ If the DOM is not fully loaded, even a correct XPath will fail to find the element.

---

## 🌿 Two Types of XPath

```
                        XPath
                    ┌────┴────┐
                    │         │
              Absolute       Relative
              XPath          XPath
           (Full XPath)   (Partial XPath)
```

---

## 🔵 Absolute XPath (Full XPath)

- Starts from the **root node** (`html`) and navigates through **every node** down to the target element
- Uses **only tag names** — no attributes
- Uses **single slash `/`** between nodes
- Single `/` = navigate only **one level** at a time

**Example:**
```
/html/body/header/div/div[2]/div/form/input
```

**How to get it:** Right-click element → Inspect → Right-click the HTML tag → Copy → **Copy Full XPath**

### ❌ Why Absolute XPath is NOT preferred:

```
HTML
 └── body
      └── header
           └── div
                └── div[2]       ← If dev adds/moves an element here...
                     └── form
                          └── input  ← ...this path BREAKS!
```

When developers add, remove, or reposition elements, the hierarchy changes and **Absolute XPath breaks**.

---

## 🟢 Relative XPath (Partial XPath) ✅ PREFERRED

- Starts with **double slash `//`** — directly jumps to the element
- Uses **attributes** of the element to locate it
- `//` = can jump **multiple levels** at once
- NOT affected by changes in page hierarchy

**Basic Syntax:**
```
//tagName[@attribute='value']
```
or if tag name is unknown:
```
//*[@attribute='value']
```

> `*` = wildcard, represents any tag name

**How to get it:** Right-click element → Inspect → Right-click HTML tag → Copy → **Copy XPath**  
Or use **SelectorHub** → Relative XPath is auto-generated.

---

## ⚖️ Absolute vs Relative XPath — Key Differences

| Feature | Absolute XPath | Relative XPath |
|---------|---------------|----------------|
| Starts with | Single slash `/` | Double slash `//` |
| Uses attributes? | ❌ No — only tag names | ✅ Yes — uses attributes |
| Navigation | Traverses every node from root | Jumps directly to element |
| Impact of UI changes | ❌ Breaks easily | ✅ Mostly unaffected |
| Preferred? | ❌ Not preferred | ✅ Always preferred |

> 💡 **Interview answer:** Relative XPath is preferred **not because it's shorter**, but because **UI changes don't break it** — it uses attributes, not hierarchy.

---

## 🛠️ How to Write Relative XPath — Syntax

```
//tagName[@attributeName='attributeValue']
```

| Part | Description |
|------|-------------|
| `//` | Start of relative XPath |
| `tagName` | HTML tag (e.g., `input`, `a`, `div`) — or `*` for any tag |
| `@` | Symbol before attribute name |
| `attributeName` | Name of the attribute (e.g., `id`, `name`, `placeholder`) |
| `'value'` | Value of the attribute (use single or double quotes) |

> ⚠️ In Java, the entire XPath goes inside `"double quotes"`, so use `'single quotes'` for attribute values inside.

---

## 📚 Flavors of Relative XPath

### 1️⃣ XPath with Single Attribute

```java
driver.findElement(By.xpath("//input[@name='search']")).sendKeys("t-shirts");
```

---

### 2️⃣ XPath with Multiple Attributes

Specify more than one attribute — all must be specified in separate `[ ]` brackets:

```java
driver.findElement(By.xpath("//input[@name='search'][@placeholder='Search store...']")).sendKeys("t-shirts");
```

> Adding more attributes makes the XPath **stronger and more unique**.

---

### 3️⃣ XPath with `and` / `or` Operators

Use `and` / `or` inside a **single bracket** to combine multiple conditions:

**`and` operator** — BOTH attributes must match:
```java
driver.findElement(By.xpath("//input[@name='search' and @placeholder='Search store...']")).sendKeys("t-shirts");
```

**`or` operator** — AT LEAST ONE attribute must match:
```java
driver.findElement(By.xpath("//input[@name='search' or @placeholder='Search store...']")).sendKeys("t-shirts");
```

| Operator | Condition | Use When |
|----------|-----------|----------|
| `and` | Both attributes must be true | You want very specific matching |
| `or` | At least one must be true | Attributes change dynamically |

---

### 4️⃣ XPath with `text()` Method

Use when an element has **inner text** and you don't want to use any attributes:

```
//tagName[text()='exactText']
```

```java
driver.findElement(By.xpath("//a[text()='MacBook']")).click();
driver.findElement(By.xpath("//h3[text()='Featured']")).getText();
```

> **Inner text** = the visible text inside an element tag  
> **Link text** can be inner text, but **not all inner text is a link**.

```
<a href="...">Click Me</a>   → inner text ✅ + link text ✅
<div>Welcome</div>           → inner text ✅ | link text ❌
<input placeholder="...">   → inner text ❌ (no text method possible)
```

---

### 5️⃣ XPath with `contains()` Method

Use when you only know a **partial value** of an attribute or text:

```
//tagName[contains(@attribute, 'partialValue')]
```

```java
driver.findElement(By.xpath("//input[contains(@placeholder, 'Searc')]")).sendKeys("t-shirts");
```

- `contains()` takes **two parameters**: attribute name + partial value
- The partial value can appear **anywhere** in the full string (beginning, middle, or end)

---

### 6️⃣ XPath with `starts-with()` Method

Use when the attribute value **starts with** a known string:

```
//tagName[starts-with(@attribute, 'startValue')]
```

```java
driver.findElement(By.xpath("//input[starts-with(@placeholder, 'Searc')]")).sendKeys("t-shirts");
```

> ⚠️ Note: It's `starts-with` (with hyphen), **not** `start-with`.

---

## 🔄 `contains()` vs `starts-with()` — Key Difference

| Method | Where partial value can appear | Example |
|--------|-------------------------------|---------|
| `contains()` | **Anywhere** in the string | `"XYZ"` found in `"001XYZ"` ✅ |
| `starts-with()` | **Only at the beginning** | `"XYZ"` starts `"XYZ001"` ✅ but NOT `"001XYZ"` ❌ |

---

## 🔥 Handling Dynamic Attributes (Very Important!)

When attribute values **change at runtime** (e.g., ID changes on each page load), static XPath breaks. Use `contains()` or `starts-with()` to handle them.

**Scenario:** A button toggles between "Start" and "Stop". ID changes from `start` to `stop`.

```
ID = "start"  →  button shows "Start"
ID = "stop"   →  button shows "Stop"
```

**Solutions:**

```xpath
<!-- Using or operator -->
//*[@id='start' or @id='stop']

<!-- Using contains (common part = "st") -->
//*[contains(@id, 'st')]

<!-- Using starts-with -->
//*[starts-with(@id, 'st')]
```

**Another Scenario:** `name` attribute changes like `XYZ001`, `XYZ002`, `XYZ003`...

```xpath
<!-- contains works (XYZ is common anywhere) -->
//*[contains(@name, 'XYZ')]

<!-- starts-with works (XYZ is at beginning) -->
//*[starts-with(@name, 'XYZ')]
```

**Scenario:** `name` changes like `001XYZ`, `002XYZ`, `003XYZ`...

```xpath
<!-- contains works (XYZ is common) -->
//*[contains(@name, 'XYZ')]

<!-- starts-with CANNOT use XYZ (it's at end, not beginning) -->
<!-- starts-with CAN use '00' since all start with '00' -->
//*[starts-with(@name, '00')]
```

---

## ⛓️ Chained XPath

Use when the **target element has no attributes** — go to the parent that has attributes, then navigate down to the child.

```
//parentTag[@attribute='value']/childTag/grandChildTag
```

**Example:** `img` has no attributes → its parent `a` has no attributes → grandparent `div` has `id="logo"`

```java
driver.findElement(By.xpath("//div[@id='logo']/a/img")).isDisplayed();
```

```
This is a COMBINATION of:
  //div[@id='logo']   ← Relative XPath (uses attribute)
  /a/img              ← Absolute-style navigation (tag names only)
```

> 💡 If the immediate parent has no attribute, go one more level up. Keep going until you find a parent with an attribute.

---

## 📊 XPath Flavors — Quick Reference

| XPath Type | Syntax | Use Case |
|------------|--------|----------|
| Single attribute | `//tag[@attr='val']` | Basic element location |
| Multiple attributes | `//tag[@a='v'][@b='v']` | Stronger, more unique XPath |
| `and` operator | `//tag[@a='v' and @b='v']` | Both conditions must be true |
| `or` operator | `//tag[@a='v' or @b='v']` | At least one must be true |
| `text()` | `//tag[text()='value']` | Element with inner text, no attributes |
| `contains()` | `//tag[contains(@attr,'part')]` | Partial attribute value, dynamic attributes |
| `starts-with()` | `//tag[starts-with(@attr,'part')]` | Attribute starts with known string |
| Chained | `//parent[@attr='v']/child/grandchild` | Target has no attributes |

---

## ⚡ XPath vs CSS Selector — Key Difference

| Feature | CSS Selector | XPath |
|---------|-------------|-------|
| Direction of traversal | **Top → Down only** (one direction) | **All directions** (top-down, bottom-up, sibling) |
| Can go from child to parent? | ❌ No | ✅ Yes (via XPath Axes) |
| Complexity | Simpler syntax | More powerful |

> XPath can navigate in **all directions** across the DOM using **XPath Axes** (following, preceding, parent, child, sibling — covered in next class).

---

## 🛠️ Verifying XPath in Browser

1. Open DevTools → Press `Ctrl + F`
2. Paste your XPath in the search box
3. Check how many elements match
4. Or use **SelectorHub** → paste XPath → press Enter → see match count

---

## 📝 Key Takeaways

1. XPath is the **address of an element**, based on the **DOM**.
2. Always prefer **Relative XPath** — UI changes don't break it.
3. Absolute starts with `/`, Relative starts with `//`.
4. Absolute uses only **tag names**; Relative uses **attributes**.
5. Use `contains()` or `starts-with()` for **dynamic attributes**.
6. `and` = both must match; `or` = at least one must match.
7. Use `text()` for elements with **inner text** but no attributes.
8. Use **Chained XPath** when target element has **no attributes** — borrow from parent.
9. XPath is **multi-directional**; CSS is only **top-down**.
10. **XPath Axes** (next class) enable navigation in all DOM directions.

---

*Next Class → XPath Axes: following, preceding, parent, child, sibling*
