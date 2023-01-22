# Setup Vite TS React

## generate vite + react + typescript App
   1. Create and install all dependencies
      ```
      npm create vite@latest
      npm i
      ```


## clean up vite setup files
1. index.css: remove content
2. app.css: delete
3. react/vite logo: delete
4. App.tsx: remove all current and format new App component
5. Remove favicon

## Lint setup
1. Run
   ```
   npm i -D eslint
   ```
2. Init Options
   ```
   npx eslint --init
   ```
   1. check syntax and find problems > JavaScript modules (import/export) > React > Typescript > Browser > JavaScript > Install all recommended dependencies > npm
   
3. Install Airbnb eslint (https://www.npmjs.com/package/eslint-config-airbnb)
   ```
   npx install-peerdeps --dev eslint-config-airbnb
   ```
4. Add Airbnb eslint TS support (https://www.npmjs.com/package/eslint-config-airbnb-typescript)
   ```
   npm i -D eslint-config-airbnb-typescript
   ```
5. ESLint config file: 
   1. Extends add and remove (prettier added in earlier than needed)
   ```json
   extends: [
    "airbnb",
    "airbnb-typescript",
    "airbnb/hooks",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"],

   ```
   2. ParserOptions add

   ```json
   parserOptions: { project: './tsconfig.json'}
   ```
6. tsconfig.json add (vite config added ahead of time)
   ```json
   "include": ["vite.config.ts",".eslintrc.cjs","src"],
   ```
7. Edit ESLint rules
   1. Check which rule is affected by disabling it first and getting the rule name
   2. go to .eslintrc.cjs and put name under (sample below)
   ```json
      rules: {
      'react/react-in-jsx-scope': 0,
      'jsx-filename-extension': 0,
      'react/jsx-filename-extension': 0,
      'jsx-quotes': 0,
   },
   ```
8. Prettier
   1. install
      1. npm i -D prettier eslint-config-prettier eslint-plugin-prettier
   2. configuration: .prettierrc.cjs
      1. https://prettier.io/docs/en/configuration.html
   ```json
   module.exports = {
      trailingComma: "es5",
      tabWidth: 2,
      semi: true,
      singleQuote: true,
      };
   ```
      2. Add to .eslintrc.cjs plugins
   ```json
   plugins: ['react', '@typescript-eslint', 'prettier'],
   ```

## Testing
1. Install (https://vitest.dev/guide/)
   ```
   npm install -D vitest
   ```
2. Vite file: Configuration (https://github.com/vitest-dev/vitest/blob/main/examples/react-testing-lib-msw/vite.config.ts)
   1. Ignore vite import error for entire file, the error says that vite should not be a devDependency, but that's is incorrect (it should be devDep).
```javascript
/// <reference types="vitest" />
/// <reference types="vite/client" />

import react from '@vitejs/plugin-react'
import { defineConfig } from 'vite'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/setupTests.ts'],
  },
})
```
3. Setup Tests
   1. Create src/setupTests.ts file
   2. Testing Library (https://testing-library.com/docs/react-testing-library/intro) & Jest DOM (https://github.com/testing-library/jest-dom)
   ```
   npm i -D @testing-library/react
   npm i -D @testing-library/jest-dom
   ```
4. Set up Jest DOM with Vitest (https://markus.oberlehner.net/blog/using-testing-library-jest-dom-with-vitest/)

   ```javascript
   //Add the following to src/setupTests.js file

   /* eslint-disable import/no-extraneous-dependencies */
   import matchers from '@testing-library/jest-dom/matchers';
   import { expect } from 'vitest';

   expect.extend(matchers);
   ```

5. Create first test file:
   ```javascript
   /* eslint-disable import/no-extraneous-dependencies */
   import { describe, it } from 'vitest';
   import { render, screen } from '@testing-library/react';
   import App from './App';

   describe('App', () => {
   it('Renders app', () => {
      // ARRANGE: GET TEST SETUP
      render(<App />);
      // ACT: DO WHAT USERS DO
      // EXPECT: WHAT HAPPENS AFTER USER INTERACTS
      const heading = screen.getByRole('heading', { level: 1 });
      expect(heading).toHaveTextContent('Hello world');
   });
   });
   ```
6. Create test script in package.json
   ```json
   "scripts": {
      "dev": "vite",
      "build": "tsc && vite build",
      "preview": "vite preview",
      "test": "vitest --coverage"
   },
   ```
7. Testing priority (https://testing-library.com/docs/queries/about/#priority)

## Routing (React Router)
1. Install
   ```
   npm i react-router-dom
   ```
2. App.tsx
   ```javascript
   import { HashRouter } from 'react-router-dom';

   function App() {
   return (
      <div className="bg-black text-white min-h-screen">
         <h1>Hello world</h1>
         <Routes>
         <Route path="/" />
         </Routes>
      </div>
   );
   }

   function WrappedApp() {
   return (
      <HashRouter>
         <App />
      </HashRouter>
   );
   }

   export default WrappedApp;

   ```
3. Add /src/pages directory
   