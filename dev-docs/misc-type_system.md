# Ollama-js Type System

Ollama-js utilizes TypeScript to provide a robust type system, enhancing code reliability and developer experience. This document focuses on the interfaces defined in `interfaces.ts` and how they are used throughout the codebase.

## Core Interfaces

### Config

The `Config` interface defines the configuration options for the Ollama client:

```typescript
export interface Config {
  host: string
  fetch?: Fetch
  proxy?: boolean
  headers?: HeadersInit
}
```

This interface is used in the `Ollama` class constructor to set up the client configuration.

### Request Interfaces

Several interfaces are defined for different types of requests to the Ollama API:

- `GenerateRequest`
- `ChatRequest`
- `PullRequest`
- `PushRequest`
- `CreateRequest`
- `DeleteRequest`
- `CopyRequest`
- `ShowRequest`
- `EmbedRequest`
- `EmbeddingsRequest`

These interfaces ensure that the correct parameters are provided when making API calls. For example, the `GenerateRequest` interface:

```typescript
export interface GenerateRequest {
  model: string
  prompt: string
  suffix?: string
  system?: string
  template?: string
  context?: number[]
  stream?: boolean
  raw?: boolean
  format?: string | object
  images?: Uint8Array[] | string[]
  keep_alive?: string | number
  options?: Partial<Options>
}
```

### Response Interfaces

Corresponding response interfaces are defined for each request type:

- `GenerateResponse`
- `ChatResponse`
- `EmbedResponse`
- `EmbeddingsResponse`
- `ProgressResponse`
- `ModelResponse`
- `ShowResponse`
- `ListResponse`
- `ErrorResponse`
- `StatusResponse`

These interfaces define the structure of the data returned by the API, allowing for type-safe handling of responses.

## Usage in the Codebase

The interfaces defined in `interfaces.ts` are used extensively throughout the Ollama-js codebase to ensure type safety and improve developer experience.

### Method Signatures

In the `Ollama` class (`browser.ts` and `index.ts`), method signatures use these interfaces to define input parameters and return types. For example:

```typescript
async generate(
  request: GenerateRequest
): Promise<GenerateResponse | AbortableAsyncIterator<GenerateResponse>> {
  // Implementation
}
```

This ensures that the correct parameters are passed to the `generate` method and that the return type is properly handled.

### Type Overloading

The codebase uses TypeScript's function overloading to provide more specific type information based on the `stream` parameter:

```typescript
generate(
  request: GenerateRequest & { stream: true }
): Promise<AbortableAsyncIterator<GenerateResponse>>
generate(request: GenerateRequest & { stream?: false }): Promise<GenerateResponse>
```

This allows for more precise type checking and better IDE support when using the `generate` method.

### Generic Types

The `processStreamableRequest` method uses generic types to handle different response types:

```typescript
protected async processStreamableRequest<T extends object>(
  endpoint: string,
  request: { stream?: boolean } & Record<string, any>
): Promise<T | AbortableAsyncIterator<T>> {
  // Implementation
}
```

This allows the method to be used with different response types while maintaining type safety.

## Benefits

The type system in Ollama-js provides several benefits:

1. **Compile-time error checking**: TypeScript can catch type-related errors before runtime.
2. **Improved IDE support**: Developers get better autocomplete and type information while coding.
3. **Self-documenting code**: The interfaces serve as documentation for the expected structure of requests and responses.
4. **Refactoring support**: TypeScript makes it easier to refactor code safely by catching type-related issues.

## Conclusion

The type system in Ollama-js, centered around the interfaces defined in `interfaces.ts`, provides a solid foundation for type-safe development. By leveraging TypeScript's features, the codebase ensures consistency in API interactions and improves the overall developer experience when working with the Ollama client library.