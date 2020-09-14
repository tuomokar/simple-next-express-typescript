# Error reproduction for Next.js with TypeScript and custom server

This is a fork from https://github.com/tiptopcoder/simple-next-express-typescript, which demonstrates a simple working
example using Next.js with TypeScript and Express. A nice medium article about it can be found 
[here](https://levelup.gitconnected.com/set-up-next-js-with-a-custom-express-server-typescript-9096d819da1c]). 

However, I noticed that if you upgrade the Next.js version to its latest version, you get a server error. This fork is to reproduce the issue.

To see the issue after cloning this:
  1. Run `npm i` in the project's root
  2. Run `npm run dev`
  3. Go to localhost:3000 in the browser
  
In the browser, you should see something like "Internal server error". In the command line, you should get an error `RangeError: Invalid array length`.

The error seems to be caused by the spread done [here](https://github.com/vercel/next.js/blob/7203f500916d336f4e1cbcd162baff624c9cd969/packages/next/pages/_document.tsx#L64),
specifically by the attempted spread of the Set. This breaks because by default TypeScript does not support iterables on arrays.

The project uses the default tsconfig created by Next.js.

A couple of ways to fix the issue would be:
  - Add `downlevelIteration` to the tsconfig
  - Change that line of code to `Array.from(new Set([...sharedFiles, ...pageFiles]))` (this is transpiled a little differently by TypeScript, compared to simply using spread)
