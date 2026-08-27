## 1. FlexBox vs Grid

`Flexbox` is a one‑dimensional layout system used to align items in a row or column, while `Grid` is a two‑dimensional system that arranges items in rows and columns; `Flexbox` is best for content alignment like navbars or buttons, `Grid` is best for full page structures like dashboards, and in practice we often combine them — `Grid` for overall layout and `Flexbox` for fine‑tuning inside sections.

## 2. 🧱 CSS Grid Properties

1. `display: grid` → Turns an element into a grid container.
2. `grid-template-areas` → Names and maps sections of the layout (header, sidebar, content, footer).
3. `grid-template-columns` → Defines column widths (e.g., 1fr, repeat(2, 1fr), 250px 1fr).
4. `grid-template-rows` → Defines row heights (e.g., auto, minmax(100px, auto)).
5. `gap` → Adds spacing between rows and columns.
6. `grid-auto-rows` → Sets default row height for items not explicitly placed.
7. `justify-items` → Aligns items horizontally inside their grid cell.
8. `align-items` → Aligns items vertically inside their grid cell.
9. `place-items` → Shorthand for `justify-items + align-items`.
10. `grid-area` → Assigns an element to a named area defined in grid-template-areas.

## 3. 🧩 FlexBox Properties

1. `display: flex` → Turns an element into a flex container.
2. `flex-direction` → Defines the main axis (row, column, row-reverse, column-reverse).
3. `justify-content` → Aligns items along the main axis (start, center, space-between, space-around).
4. `align-items` → Aligns items along the cross axis (top, center, bottom).
5. `align-content` → Aligns multiple rows of flex items.
6. `flex-wrap` → Allows items to wrap onto multiple lines.
7. `flex` → Shorthand for flex-grow, flex-shrink, and flex-basis.
8. `order` → Controls the order of items regardless of HTML structure.
9. `gap` → Adds spacing between flex items.

## 4. Dashboard Example

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello, World!</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
  <div class="dashboard">
  <header>
    <div class="logo">Logo</div>
    <nav class="links">
      <a href="#">Home</a>
      <a href="#">About</a>
      <a href="#">Services</a>
    </nav>
    <div class="logout">Logout</div>
  </header>

  <aside class="sidebar">
    <a href="#">Dashboard</a>
    <a href="#">Reports</a>
    <a href="#">Settings</a>
    <a href="#">Profile</a>
  </aside>

  <main class="content">
    <div>Box 1</div>
    <div>Box 2</div>
    <div>Box 3</div>
    <div>Box 4</div>
    <div>Box 5</div>
    <div>Box 6</div>
    <div>Box 7</div>
    <div>Box 8</div>
    <div>Box 9</div>
    <div>Box 10</div>
    <div>Box 11</div>
    <div>Box 12</div>
  </main>

  <footer>
    <div class="copy">© 2026 MyCompany</div>
    <div class="date">27 Aug 2026</div>
  </footer>
</div>
  </body>
</html>
```

```css
/* GRID CONTAINER */
.dashboard {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar content"
    "footer footer";
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  gap: 10px;
  min-height: 100vh;
}

/* HEADER (Flexbox) */
header {
  grid-area: header;
  display: flex;
  justify-content: space-between; /* logo left, links center, logout right */
  align-items: center;
  background: #0288d1;
  color: white;
  padding: 10px 20px;
}
header .links {
  display: flex;
  justify-content: center;
  gap: 20px;
}
header .links a {
  color: white;
  text-decoration: none;
  font-weight: bold;
}

/* SIDEBAR (Flexbox column) */
.sidebar {
  grid-area: sidebar;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  gap: 10px;
  background: #81d4fa;
  padding: 20px;
}
.sidebar a {
  text-decoration: none;
  color: #01579b;
  font-weight: 600;
}

/* MAIN CONTENT (Grid inside Grid) */
.content {
  grid-area: content;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(100px, auto);
  gap: 10px;
  background: #e1f5fe;
  padding: 20px;
}
.content div {
  background: #03a9f4;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
}

/* FOOTER (Flexbox) */
footer {
  grid-area: footer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #0288d1;
  color: white;
  padding: 10px 20px;
}

/* RESPONSIVE DESIGN */
@media (max-width: 768px) {
  .dashboard {
    grid-template-areas:
      "header"
      "sidebar"
      "content"
      "footer";
    grid-template-columns: 1fr;
  }
  .content {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 480px) {
  header .links {
    flex-direction: column;
    gap: 10px;
  }
  .content {
    grid-template-columns: 1fr;
  }
}

```