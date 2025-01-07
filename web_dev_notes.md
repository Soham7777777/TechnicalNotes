# Module imports in javascript:

- in the `import "module-name"` syntax, the "module-name" is the **urlpath** to a file which has valid javascript code and served with "text/javascript" mime types. The ".js" extension don't matter on web because they are resolved by the "host", The browsers only care about valid code and mime type.

- For example a vanilla-js vite starter app has following line in its main.js file: `import "./style.css"`, when you open it up in browser host(vite) converts that to `import /src/style.css` which browser interprets as "localhost:5173/src/style.css" which is a valid javascript code! again converted by the vite.

- That's how vite handles static assets inside javascript code.
