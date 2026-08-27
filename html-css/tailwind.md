
`TailwindCSS` is a utility-first CSS framework packed with single-purpose classes like flex, pt-4, and text-center that you compose directly in your HTML markup to build custom designs.

## 1. 🧩 FlexBox Classes
1. `flex` → Makes container a FlexBox.
2. `flex-row / flex-col` → Direction of items (row or column).
3. `justify-start / justify-center / justify-between` → Align items along main axis.
4. `items-start / items-center / items-end` → Align items along cross axis.
5. `flex-wrap` → Allow items to wrap.
6. `gap-x-* / gap-y-*` → Spacing between flex items.
7. `Responsive` → Prefix with breakpoints: `sm:flex-col`, `md:flex-row`, `lg:justify-between`.

## 2. 🧱 Grid Classes
1. `grid` → Makes container a grid.
2. `grid-cols-*` → Number of columns (grid-cols-1, grid-cols-2, grid-cols-4).
3. `grid-rows-*` → Number of rows (grid-rows-3).
4. `col-span-* / row-span-*` → Span across multiple columns/rows.
5. `gap-*` → Space between grid cells.
6. `place-items-center` → Center items both horizontally and vertically.
7. `Responsive` → sm:grid-cols-1, md:grid-cols-2, lg:grid-cols-4.

- Use `FlexBox` classes for alignment inside sections (navbars, footers, toolbars).
- Use `Grid` classes for overall structure (dashboards, galleries, multi‑column layouts).
- Combine them with responsive prefixes (sm:, md:, lg:) to adapt layouts across devices.

## 3. Dashboard Example

```html
<div class="grid min-h-screen gap-2 grid-rows-[auto_1fr_auto] grid-cols-1 md:grid-cols-[250px_1fr] md:grid-areas-[header_header_'sidebar_content'_footer_footer]">
  
  <!-- Header -->
  <header class="flex justify-between items-center bg-blue-700 text-white p-4 col-span-full">
    <div class="font-bold">Logo</div>
    <nav class="flex gap-4 justify-center">
      <a href="#" class="hover:underline">Home</a>
      <a href="#" class="hover:underline">About</a>
      <a href="#" class="hover:underline">Services</a>
    </nav>
    <div class="cursor-pointer">Logout</div>
  </header>

  <!-- Sidebar -->
  <aside class="flex flex-col gap-2 bg-blue-300 p-4">
    <a href="#" class="hover:underline">Dashboard</a>
    <a href="#" class="hover:underline">Reports</a>
    <a href="#" class="hover:underline">Settings</a>
    <a href="#" class="hover:underline">Profile</a>
  </aside>

  <!-- Main Content -->
  <main class="grid grid-cols-2 md:grid-cols-4 gap-2 bg-blue-100 p-4">
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 1</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 2</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 3</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 4</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 5</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 6</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 7</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 8</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 9</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 10</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 11</div>
    <div class="flex items-center justify-center bg-blue-500 text-white p-6">Box 12</div>
  </main>

  <!-- Footer -->
  <footer class="flex justify-between items-center bg-blue-700 text-white p-4 col-span-full">
    <div>© 2026 MyCompany</div>
    <div>27 Aug 2026</div>
  </footer>
</div>
```