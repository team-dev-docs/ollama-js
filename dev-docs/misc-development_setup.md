# Setting Up the Ollama-js Development Environment

This guide will walk you through the process of setting up your development environment for Ollama-js, including installing dependencies, building the project, and running tests.

## Prerequisites

Before you begin, ensure you have the following installed on your system:
- Node.js (preferably the latest LTS version)
- npm (usually comes with Node.js)

## Clone the Repository

First, clone the Ollama-js repository:

```bash
git clone https://github.com/ollama/ollama-js.git
cd ollama-js
```

## Install Dependencies

Install the project dependencies using npm:

```bash
npm install
```

This will install all the dependencies listed in the `package.json` file, including development dependencies like TypeScript, Jest, ESLint, and Prettier.

## Build Process

To build the project, run:

```bash
npm run build
```

This command uses the `unbuild` package to create distribution files in the `dist` directory. The build process is defined in the `build` script in `package.json`.

## Running Tests

To run the test suite, use:

```bash
npm test
```

This command runs Jest with the configuration specified in `jest.config.cjs` and executes all test files in the `test` directory.

## Additional Scripts

The `package.json` file defines several other useful scripts:

- **Format code**: 
  ```bash
  npm run format
  ```
  This uses Prettier to format all files in the project.

- **Lint code**:
  ```bash
  npm run lint
  ```
  This runs ESLint on all TypeScript files in the `src` directory.

- **Prepare for publishing**:
  ```bash
  npm run prepublishOnly
  ```
  This script runs the build process before publishing the package.

## TypeScript Configuration

The project uses TypeScript, and its configuration is defined in `tsconfig.json`. Key settings include:
- Target: ES6
- Module: ES2022
- Strict type checking enabled
- Declaration files generated

## Development Workflow

1. Make changes to the source files in the `src` directory.
2. Run `npm run lint` to check for any linting issues.
3. Run `npm run format` to ensure consistent code formatting.
4. Run `npm test` to make sure all tests pass.
5. Run `npm run build` to create the distribution files.

By following these steps, you'll have a fully set up development environment for Ollama-js, ready for you to start contributing to the project.