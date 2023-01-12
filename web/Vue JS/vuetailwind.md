# Vue and tailwind

## Tailwind Setup

### Setup

Install from NPM

```bash
npm install -D tailwindcss postcss autoprefixer
```

Init Tailwind Config

```bash
npx tailwindcss init -p
```

Setting Tailwind Config Json

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

Add Tailwind directives to CSS of file ./src/style.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Enjoy Coding
