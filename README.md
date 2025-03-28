# 🎯 CSS Specificity Guide

Understanding **CSS specificity** is essential for writing clean, maintainable, and predictable styles. This guide breaks down how specificity works and how to manage it effectively.

---

## 📐 What Is Specificity?

**Specificity** is the algorithm browsers use to determine which CSS rule applies when multiple rules target the same element.

The specificity is calculated as:

- **ID selectors** (e.g., `#header`) → `1-0-0`
- **Class, attribute, pseudo-class** (e.g., `.btn`, `[type="text"]`, `:hover`) → `0-1-0`
- **Type & pseudo-elements** (e.g., `div`, `h1`, `::before`) → `0-0-1`

---

## 🧮 Specificity Calculation

| Selector | Specificity |
|----------|-------------|
| `#main-nav` | `1-0-0` |
| `.menu-item:hover` | `0-2-0` |
| `nav li a` | `0-0-3` |
| `input[type="text"]:focus` | `0-2-1` |
| `style="color: red"` | `1-0-0-0` (inline style) |

---

## ⛔ What Doesn't Affect Specificity?

- The universal selector `*`
- Combinators (`>`, `+`, `~`, space)
- `:where()` (always 0-0-0)
- `:not()`, `:is()`, `:has()` → These don't count, but their **contents do**

```css
/* 0-0-1 */
p,
:is(p) {
  color: blue;
}
```

---

## 🚨 !important & Inline Styles

| Rule           | Priority                                   |
|----------------|--------------------------------------------|
| Inline styles  | Highest (unless overridden by `!important`) |
| `!important`   | Overrides all other declarations            |

> Avoid using `!important` unless absolutely necessary.

---

## 💡 Best Practices

✅ Use low-specificity selectors:

```css
:where(.btn) {
  color: blue;
}
```

✅ Use cascade layers:

```css
@layer theme {
  button {
    background: white;
  }
}
```

❌ Avoid over-specific selectors:

```css
html body div.container ul li a span {
  color: red;
}
```

---

## 🧰 Tips for Managing Specificity

- Use attribute selectors (`[id="something"]`) instead of `#something` to lower specificity.
- Use `:where()` to zero out specificity.
- Repeat class or ID selectors **only when needed** to increase specificity:

```css
.btn.btn.btn {
  color: red; /* 0-3-0 */
}
```

---

## 🔄 Specificity Examples

```css
/* 1-1-1 */
#myElement input.myClass {
  color: red;
}

/* 0-2-1 */
input[type="password"]:required {
  color: blue;
}

/* 0-0-4 */
html body main input {
  color: green;
}
```

**Result:** Red wins — highest specificity (`1-1-1`)

---

## 📦 Overriding Third-party CSS

If you're using CSS from a library (like Bootstrap or Tailwind), consider:

1. Importing it into a cascade layer:

```css
@import "thirdparty.css" layer(thirdparty);
```

2. Writing overrides outside the layer or in a later layer.

---

## 🧪 Test It Out

Use tools like:

- [Specificity Calculator](https://specificity.keegan.st)
- [CSS Cascade Visualizer](https://csscascade.dev)

---

## 📝 License

MIT License. Feel free to use and contribute.
