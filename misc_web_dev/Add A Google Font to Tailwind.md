Find a google font and copy the @import code for the web. At the top of the input.css file add the import statement exactly as written. Then add it to @theme section of the css file. Example of adding Fira Code and Fira Sans Condensed in a tailwind css project:
```css
@import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300..700&family=Fira+Sans+Condensed:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
@import "tailwindcss";

@theme {
  --font-code: "Fira Code", sans-serif;
  --font-fira-condensed: "Fira Sans Condensed";
}
```
Then use the fonts in your html like this:
```html
<h1 class="font-code">Hello World!</h1>
```