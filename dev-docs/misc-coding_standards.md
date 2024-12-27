# Ollama-js Contribution Guidelines

This document outlines the coding standards and best practices for contributing to Ollama-js. Following these guidelines helps maintain consistency and quality across the codebase.

## TypeScript Usage

Ollama-js is written in TypeScript. When contributing, please adhere to the following TypeScript best practices:

1. Use strict type checking. The `tsconfig.json` file should have `"strict": true`.

2. Prefer interfaces over types for object shapes:

```typescript
// Good
interface Config {
  host: string;
  fetch?: Fetch;
  proxy?: boolean;
  headers?: HeadersInit;
}

// Avoid
type Config = {
  host: string;
  fetch?: Fetch;
  proxy?: boolean;
  headers?: HeadersInit;
};
```

3. Use union types for variables that can be multiple types:

```typescript
async encodeImage(image: Uint8Array | Buffer | string): Promise<string> {
  // Implementation
}
```

4. Utilize TypeScript's built-in utility types when appropriate (e.g., `Partial<T>`, `Pick<T>`, `Omit<T>`).

5. Use type inference where possible, but include explicit return types for functions:

```typescript
async function createBlob(path: string): Promise<string> {
  // Implementation
}
```

## Error Handling

Proper error handling is crucial for maintaining a robust and user-friendly library. Follow these guidelines:

1. Use custom error classes for specific error types:

```typescript
class ResponseError extends Error {
  constructor(
    public error: string,
    public status_code: number,
  ) {
    super(error);
    this.name = 'ResponseError';

    if (Error.captureStackTrace) {
      Error.captureStackTrace(this, ResponseError);
    }
  }
}
```

2. Catch and handle errors appropriately. Avoid swallowing errors without logging or reporting:

```typescript
try {
  // Attempt an operation
} catch (error) {
  console.error('Operation failed:', error);
  // Handle the error or rethrow if necessary
}
```

3. Use async/await with try/catch for asynchronous error handling:

```typescript
async function someAsyncFunction() {
  try {
    const result = await someAsyncOperation();
    return result;
  } catch (error) {
    console.error('Async operation failed:', error);
    throw error; // or handle it appropriately
  }
}
```

## Documentation Practices

Good documentation is essential for maintainability and usability. Follow these documentation practices:

1. Use JSDoc comments for classes, interfaces, and functions:

```typescript
/**
 * Generates a response from a text prompt.
 * @param request {GenerateRequest} - The request object.
 * @returns {Promise<GenerateResponse | AbortableAsyncIterator<GenerateResponse>>} - The response object or
 * an AbortableAsyncIterator that yields response messages.
 */
async generate(
  request: GenerateRequest,
): Promise<GenerateResponse | AbortableAsyncIterator<GenerateResponse>> {
  // Implementation
}
```

2. Include inline comments for complex logic or non-obvious code:

```typescript
// Filter out default headers from custom headers
const customHeaders = Object.fromEntries(
  Object.entries(options.headers).filter(([key]) => !Object.keys(defaultHeaders).some(defaultKey => defaultKey.toLowerCase() === key.toLowerCase()))
);
```

3. Keep README.md and other documentation files up-to-date with changes to the API or functionality.

4. Use markdown for documentation files to ensure readability both in text editors and when rendered.

## Code Style and Formatting

1. Use a consistent code style throughout the project. Consider using tools like Prettier or ESLint to enforce style rules.

2. Use meaningful variable and function names that describe their purpose.

3. Keep functions small and focused on a single task.

4. Use consistent indentation (preferably 2 spaces).

5. Limit line length to improve readability (typically 80-120 characters).

## Version Control Practices

1. Write clear, concise commit messages that describe the changes made.

2. Use feature branches for new features or significant changes.

3. Keep pull requests focused on a single feature or fix.

4. Ensure all tests pass before submitting a pull request.

By following these guidelines, we can maintain a high-quality, consistent, and well-documented codebase for Ollama-js. Thank you for your contributions!