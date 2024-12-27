# API Request Handling in Ollama-js

Ollama-js is designed to efficiently handle API requests to the Ollama server, providing a robust interface for various operations such as generating responses, chatting, and managing models. This document explains the key aspects of how Ollama-js processes API requests, including the use of fetch, error handling, and streaming responses.

## Fetch Usage

Ollama-js utilizes the `fetch` API for making HTTP requests to the Ollama server. The library supports both browser environments and Node.js, adapting its fetch implementation accordingly:

- In browser environments, it uses the native `fetch` function.
- For Node.js, it relies on a fetch polyfill or a custom fetch implementation provided during configuration.

The `fetchWithHeaders` function in `utils.ts` wraps the fetch calls, adding default headers such as `Content-Type`, `Accept`, and a custom `User-Agent` that includes the Ollama-js version and platform information.

## Request Processing

The main request processing logic is implemented in the `processStreamableRequest` method of the `Ollama` class. This method handles both streamed and non-streamed requests:

1. For non-streamed requests, it sends a POST request and returns the JSON response.
2. For streamed requests, it creates an `AbortableAsyncIterator` that yields response messages as they arrive.

## Error Handling

Error handling in Ollama-js is primarily managed through the `checkOk` function in `utils.ts`. This function:

1. Checks if the response status is OK (200-299 range).
2. If not, it attempts to parse the error message from the response body.
3. Throws a `ResponseError` with the error message and status code.

The `ResponseError` class extends the native `Error` class, providing additional context about the API error.

## Streaming Responses

Ollama-js supports streaming responses for certain operations like generate, chat, create, pull, and push. The streaming functionality is implemented using the `AbortableAsyncIterator` class, which:

1. Wraps an async generator that parses the response stream.
2. Provides an abort mechanism to cancel ongoing requests.
3. Yields parsed JSON objects from the stream.

The `parseJSON` function in `utils.ts` is responsible for parsing the `ReadableStream` of `Uint8Array` into JSON objects, handling potential parsing errors gracefully.

## Role of utils.ts

The `utils.ts` file plays a crucial role in request processing:

1. **HTTP Methods**: It provides wrapper functions (`get`, `head`, `post`, `del`) for HTTP methods, adding default headers and error checking.

2. **Stream Parsing**: The `parseJSON` function handles parsing of streamed responses.

3. **Error Handling**: The `checkOk` function and `ResponseError` class manage error detection and formatting.

4. **Host Formatting**: The `formatHost` function ensures proper formatting of the Ollama server host URL.

5. **Platform Detection**: The `getPlatform` function determines the current runtime environment for user agent information.

By centralizing these utilities, `utils.ts` promotes code reuse and maintains consistency in request handling across different API endpoints in Ollama-js.