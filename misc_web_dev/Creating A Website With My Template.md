## Set Up
### Design
I am starting by making a very simple design document with only a couple type faces and a basic color palette. I used [Google Fonts](https://fonts.google.com/) and a website I found called [Coolers](https://coolors.co) to find a color palette I liked.
### View Engine
I use EJS as my view engine of choice for Node.js projects. EJS works great with HTMX and I prefer writing HTML. Set up the view engine in `server.ts`:
```typescript
app.set("views" , "./views");
app.set("view engine", "ejs");
```