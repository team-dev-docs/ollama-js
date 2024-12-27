# Using Chat and Generate in Ollama-js

Ollama-js provides two main methods for interacting with language models: `chat()` and `generate()`. This guide will explain how to use these features, including code examples, available options, and handling responses.

## Chat

The `chat()` method allows you to have a conversational interaction with the model.

### Basic Usage

```typescript
import ollama from 'ollama'

const response = await ollama.chat({
  model: 'llama2',
  messages: [
    {
      role: 'user',
      content: 'Hello, how are you?'
    }
  ]
})

console.log(response.message.content)
```

### Options

The `chat()` method accepts a `ChatRequest` object with the following properties:

- `model`: (string, required) The name of the model to use
- `messages`: (array of Message objects, required) The conversation history
- `stream`: (boolean, optional) Whether to stream the response
- `format`: (string or object, optional) The desired output format
- `keep_alive`: (string or number, optional) How long to keep the model loaded
- `tools`: (array of Tool objects, optional) Available tools for the model to use
- `options`: (object, optional) Additional model-specific options

### Streaming Responses

To stream responses, set `stream: true` in the request:

```typescript
const stream = await ollama.chat({
  model: 'llama2',
  messages: [{ role: 'user', content: 'Tell me a story' }],
  stream: true
})

for await (const part of stream) {
  process.stdout.write(part.message.content)
}
```

### Handling Images

The `chat()` method supports including images in messages:

```typescript
const response = await ollama.chat({
  model: 'llava',
  messages: [
    {
      role: 'user',
      content: 'What's in this image?',
      images: ['./path/to/image.jpg']
    }
  ]
})
```

Images can be provided as file paths, base64-encoded strings, or `Uint8Array` objects.

## Generate

The `generate()` method is used for one-shot text generation tasks.

### Basic Usage

```typescript
import ollama from 'ollama'

const response = await ollama.generate({
  model: 'llama2',
  prompt: 'Write a haiku about programming'
})

console.log(response.response)
```

### Options

The `generate()` method accepts a `GenerateRequest` object with these key properties:

- `model`: (string, required) The name of the model to use
- `prompt`: (string, required) The input prompt
- `system`: (string, optional) A system message to set context
- `template`: (string, optional) A custom prompt template
- `context`: (array of numbers, optional) Previous context to continue generation
- `stream`: (boolean, optional) Whether to stream the response
- `raw`: (boolean, optional) Whether to return raw response
- `format`: (string or object, optional) The desired output format
- `images`: (array of Uint8Array or string, optional) Images to include with the prompt
- `options`: (object, optional) Additional model-specific options

### Streaming Responses

Similar to `chat()`, you can stream responses by setting `stream: true`:

```typescript
const stream = await ollama.generate({
  model: 'llama2',
  prompt: 'Write a short story',
  stream: true
})

for await (const part of stream) {
  process.stdout.write(part.response)
}
```

### Handling Images

The `generate()` method also supports including images:

```typescript
const response = await ollama.generate({
  model: 'llava',
  prompt: 'Describe this image:',
  images: ['./path/to/image.jpg']
})
```

## Response Handling

Both `chat()` and `generate()` return response objects with similar structures:

- For `chat()`: `ChatResponse`
- For `generate()`: `GenerateResponse`

These responses include properties such as:

- `model`: The name of the model used
- `created_at`: Timestamp of when the response was created
- `response` (for `generate()`) or `message` (for `chat()`): The generated content
- `done`: Boolean indicating if the response is complete
- `context`: Token context for continuation (in `generate()`)
- Various timing and evaluation count properties

When streaming, each chunk of the response will be a partial object of the same type.

## Error Handling

Both methods may throw errors, which should be handled appropriately:

```typescript
try {
  const response = await ollama.generate({
    model: 'nonexistent-model',
    prompt: 'Hello'
  })
} catch (error) {
  console.error('An error occurred:', error)
}
```

## Conclusion

The `chat()` and `generate()` methods in Ollama-js provide powerful and flexible ways to interact with language models. By understanding their options and how to handle responses, you can build sophisticated applications leveraging these capabilities.