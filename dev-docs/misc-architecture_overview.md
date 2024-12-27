# Ollama-js Architecture Overview

Ollama-js is a JavaScript/TypeScript library for interacting with the Ollama API. It provides a convenient interface for generating text, chatting with models, and managing Ollama models in both browser and Node.js environments.

## Main Classes

### Ollama

The `Ollama` class is the core of the library, providing methods to interact with the Ollama API. It is implemented differently for browser and Node.js environments:

- **Browser**: Defined in `browser.ts`
- **Node.js**: Extends the browser version in `index.ts` with additional functionality

### OllamaBrowser

`OllamaBrowser` is an alias for the browser version of the `Ollama` class. It provides the base implementation that works in both browser and Node.js environments.

## Key Interfaces

The library defines several interfaces in `interfaces.ts`:

- `Config`: Configuration options for the Ollama client
- `GenerateRequest`, `ChatRequest`, `EmbedRequest`, etc.: Request interfaces for various API endpoints
- `GenerateResponse`, `ChatResponse`, `EmbedResponse`, etc.: Response interfaces for API calls
- `Message`: Represents a chat message
- `ModelResponse`: Contains information about a model
- `ErrorResponse`: Represents an error from the API

## Interacting with the Ollama API

The library interacts with the Ollama API through HTTP requests. Key aspects of this interaction include:

1. **Request Processing**: The `processStreamableRequest` method handles both streaming and non-streaming requests to the API.

2. **Fetch Wrapper**: Custom `get`, `post`, and `del` functions in `utils.ts` wrap the fetch API to add default headers and error handling.

3. **Streaming Responses**: The `AbortableAsyncIterator` class manages streamed responses, allowing for cancellation of ongoing requests.

4. **JSON Parsing**: The `parseJSON` function in `utils.ts` handles parsing of streamed JSON responses.

## Browser vs Node.js Differences

While the core functionality is similar, there are some key differences between the browser and Node.js versions:

### Browser Version (`browser.ts`)

- Uses the browser's built-in `fetch` API
- Limited to in-memory operations
- Cannot directly access the file system

### Node.js Version (`index.ts`)

- Extends the browser version with additional features
- Can access the file system (e.g., for reading model files)
- Implements `encodeImage` to handle file paths and convert images to base64
- Provides additional model management features (e.g., `parseModelfile`, `createBlob`)

## Key Features

1. **Text Generation**: Using the `generate` method
2. **Chat Interactions**: Via the `chat` method
3. **Model Management**: Methods like `create`, `pull`, `push`, `delete`, `copy`, `list`, and `show`
4. **Embeddings**: Generate embeddings using `embed` and `embeddings` methods
5. **Streaming Support**: Most methods support streaming responses for real-time processing

## Extensibility

The architecture allows for easy extension and customization:

- The `Config` interface allows for custom headers and fetch implementations
- The `Ollama` class can be extended to add or modify functionality
- The use of TypeScript interfaces enables strong typing and easy integration with other tools and frameworks

This architecture provides a flexible and powerful way to interact with Ollama models, suitable for a wide range of applications in both browser and Node.js environments.