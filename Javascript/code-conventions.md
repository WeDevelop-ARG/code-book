# Code conventions for Javascript (ES6)

## Table of Contents

  1. [Asynchronous code](#Asynchronous-code)
  1. [Imports](#Imports)
  1. [Exports](#Exports)
  1. [Naming](#Naming)
  1. [Conditionals](#Conditionals)
  1. [Where to put blank lines](#Where-to-put-blank-lines)
  1. [Coding](#Coding)

## Asynchronous code

  - Use `async/await` with `try/catch` instead of `Promise.then().catch()`

## Imports

  Use `import` instead of `require` when possible.

  1. Initialization, dotenv ES6, and so on.
  1. npm packages and libraries
  1. root imports
      - ([**babel root import**](https://www.npmjs.com/package/babel-plugin-root-import))
  1. local imports
      - e.g. local components
  1. assets
      - PNGs, SVGs, etc.
  1. constants (If defined in external files)
  1. styles or classNames

## Exports

- For pages and components use default export

- For mixins, functions, services use individual exports

- Prefer not to use only-exports `index.js` (files that only imports and re-exports) as they need to be maintained. If needed, use a platform specific way to export all existing code (so that a new file addition gets automatically exported).
    - For example, this is the code to export all `.graphql` files from the training program

      ```jsx
      import path from 'path'
      import fs from 'fs'
      import { gql } from 'apollo-server-express'

      const typeDefs = []

      fs
        .readdirSync(__dirname)
        .filter(file => file.endsWith('.graphql'))
        .forEach((file) => {
          typeDefs.push(
            gql(fs.readFileSync(path.join(__dirname, file), 'utf-8'))
          )
        })

      module.exports = typeDefs
      ```

## Naming

  - `camelCase` for variables, functions and methods
  - `UPPERCASE_NAMES` for constants and `.env` variables
  - Use context naming.
      - For example for a path `src/pages` and a `SignUp` page, the resulting filename should not be `SignUpPage`, as the context communicates this is the concrete page and not a component

## Conditionals

  - For `if/then` if the block is composed of only one instruction, then do not use `{}`

    ```jsx
      // not good
      if (myBool) {
        process()
      }

      // good
      if (myBool) process()
    ```

  - For ternary conditionals with more than one instruction in their blocks

    (`(conditional) ? <instructionsTrue> : <instructionsFalse>`)

    treat the instructions as a block that needs to be indented

    ```jsx
    (myBool) ? {
      instruction1
      instruction2
    } : {
      instruction3
      instruction4
    }
    ```

    Which is common to appear in **React** as instructions could be components instead.

 - For ternary which instructions are only a `return`, this is valid:

    ```jsx
    const result = (myBool) ? val1 : val2

    // or

    return (myBool) ? val1 : val2
    ```

## Where to put blank lines

  - Blank line at end of file [VSCode extension](https://marketplace.visualstudio.com/items?itemName=riccardoNovaglia.missinglineendoffile)
  - After all imports
  - Before `function` definitions
  - Before `export`
  - Before `return`
  - As a separation for Logical Groupings
      - A logical grouping is a group of instructions that are binded by functionality (for example processing some data) or by thematic (for example, variable definitions)

## Coding

  - Do not use intermediate variables **unless**:
      - It serves as a storage for a long computation or async call, and will be referenced more than once in the code.

        ```jsx
        // not good
          if (longComputation() > THRESHOLD) {
            const double = longComputation() * 2
          }
        ```

        **Note: If the computation is not expensive, it's sometimes more readable to duplicate the call.**

      - It's an explaining variable.

      ```jsx
          // will require multiple reads to know what is happening

          const offset = multiply(interpolate(progress, {
          inputRange: [0, 1],
          outputRange: [0, 2 * Math.PI]
          }), radius)


          // Preferable, provided I don't know what interpolate does, I know we are establishing an offset with an angle and a radius

          const angle = interpolate(progress, {
              inputRange: [0, 1],
              outputRange: [0, 2 * Math.PI]
            })

          const offset = multiply(angle, radius)
      ```

  - Use `faulty` and `truthy` constructions
    ```jsx
        // good
        const DEFAULT_JWT_LIFESPAN = process.env.JWT_LIFESPAN || '10d'
    ```

  - Use auto-named properties in objects when possible

  ```jsx
      // not good
      const obj = {
        property: property
      }

      // good

      const obj = {
        property
      }
  ```
