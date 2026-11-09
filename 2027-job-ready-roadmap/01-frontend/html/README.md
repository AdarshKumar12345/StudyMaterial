# HTML5 Architecture & Modern Web Accessibility

## 1. Introduction & Prerequisites
HTML5 is the structural foundation of the World Wide Web. Modern HTML defines semantic relationships, accessibility trees for assistive technologies (screen readers), responsive media handling, and search engine discoverability.

- **Prerequisites:** Basic text editor usage (VS Code), understanding of files and directories, web browsing concepts.
- **Estimated Study Time:** 8 hours
- **Difficulty:** Beginner

---

## 2. Detailed Learning Objectives
- Master semantic HTML tags (`<main>`, `<article>`, `<section>`, `<nav>`, `<aside>`, `<header>`, `<footer>`).
- Understand DOM tree construction and accessibility tree parsing.
- Build fully accessible forms with proper label bindings, fieldsets, ARIA attributes, and native constraints.
- Implement responsive media using `<picture>`, `<source>`, `srcset`, and modern image formats (AVIF, WebP).
- Learn SEO fundamentals: meta viewport, Open Graph protocol, canonical tags, and structured data.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] HTML Document Structure (`<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`)
- [ ] Block vs Inline vs Inline-Block Elements
- [ ] Semantic Page Structuring (`<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`)
- [ ] Accessible Forms (`<form>`, `<label for="...">`, `<input>`, `<select>`, `<textarea>`, `<fieldset>`, `<legend>`)
- [ ] Modern Form Validation (`required`, `pattern`, `minlength`, `maxlength`, `type="email|tel|number"`)
- [ ] Tables for Tabular Data (`<table>`, `<thead>`, `<tbody>`, `<th>`, `scope="col|row"`)
- [ ] Responsive Media (`<picture>`, `srcset`, `sizes`, `loading="lazy"`, `<video>`, `<audio>`)
- [ ] Web Accessibility & ARIA Landmarks (`aria-label`, `aria-expanded`, `aria-live`, `role="status"`)
- [ ] SEO & Social Metadata (`<meta name="description">`, Open Graph `og:image`, Twitter Cards)
- [ ] Microdata & JSON-LD Structured Data Schema

---

## 4. Free Documentation Resources
- **MDN Web Docs — HTML:** [https://developer.mozilla.org/en-US/docs/Web/HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
- **W3C Web Accessibility Initiative (WAI):** [https://www.w3.org/WAI/fundamentals/accessibility-intro/](https://www.w3.org/WAI/fundamentals/accessibility-intro/)
- **web.dev by Google (Learn HTML):** [https://web.dev/learn/html/](https://web.dev/learn/html/)
- **A11y Project Checklist:** [https://www.a11yproject.com/checklist/](https://www.a11yproject.com/checklist/)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** HTML & CSS Full Course for Beginners
- **Kevin Powell:** HTML semantics and why they matter
- **Traversy Media:** HTML5 Crash Course for Beginners

---

## 6. Important Concepts Explained

### The Accessibility Tree & Semantic Markup
When a browser parses HTML, it constructs two trees in memory:
1. **The DOM Tree:** Used by CSS and JavaScript for styling and interaction.
2. **The Accessibility Tree:** Used by screen readers and assistive devices.

Using non-semantic tags like `<div onclick="...">` creates a node without roles, states, or keyboard accessibility. Using `<button type="button">` automatically gives the node keyboard focusability (`Tab`), enter/space trigger behavior, and screen reader announcements without custom JavaScript.

---

## 7. Practical Code Examples

### Accessible Registration Form with Semantic Markup
```html
<main id="main-content" class="container">
  <article>
    <header>
      <h1>Create Your Developer Account</h1>
      <p>Join thousands of engineers preparing for 2027 technical milestones.</p>
    </header>

    <form action="/api/v1/auth/register" method="POST" novalidate>
      <fieldset>
        <legend>Personal Credentials</legend>

        <div class="form-group">
          <label for="fullName">Full Name <span aria-hidden="true">*</span></label>
          <input 
            type="text" 
            id="fullName" 
            name="fullName" 
            required 
            autocomplete="name"
            aria-required="true"
            aria-describedby="name-help"
          />
          <small id="name-help">Enter your legal full name as shown on official ID.</small>
        </div>

        <div class="form-group">
          <label for="email">Work Email <span aria-hidden="true">*</span></label>
          <input 
            type="email" 
            id="email" 
            name="email" 
            required 
            autocomplete="email"
            aria-required="true"
          />
        </div>
      </fieldset>

      <button type="submit" class="btn-primary">Register Account</button>
    </form>
  </article>
</main>
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build an accessible developer profile card featuring an avatar with `<picture>`, skills list using `<ul>`, and contact form with full ARIA attributes.
2. **Assignment 2:** Run Google Lighthouse on your page and achieve a 100/100 score in Accessibility and SEO audits.

---

## 9. Mini-Projects
- **Accessible Developer Documentation Portal:** Create a 3-page documentation shell with semantic breadcrumbs, sticky sidebar `<nav>`, accessible skip-to-content links, and responsive tables.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between `<div>` and `<section>` or `<article>`?
**Answer:** `<div>` is an un-semantic container used purely for styling or grouping with zero semantic meaning in the accessibility tree. `<section>` represents a standalone generic section of a document with a thematic grouping, typically with a heading. `<article>` represents a self-contained composition that can be independently distributed or syndicated.

### Q2: Why are `alt` attributes required on `<img>` tags? What should you do for decorative images?
**Answer:** The `alt` attribute provides a text alternative for users who cannot see images, such as screen reader users. For purely decorative images that convey no information, provide an empty string `alt=""`. This instructs screen readers to skip the element entirely rather than reciting the image file path.

---

## 11. Revision Notes
- Always include `<!DOCTYPE html>` and `<html lang="en">`.
- Never skip heading levels (e.g., `h1` directly to `h3`).
- Always link `<label for="id">` to matching `<input id="id">`.
- Use `<button>` for actions within the page, and `<a>` for navigation to different URLs.

---

## 12. Topic Completion Checklist
- [ ] Understand semantic element roles
- [ ] Built an accessible form with validation
- [ ] Implemented responsive images with `<picture>`
- [ ] Scored 100 on Google Lighthouse Accessibility
