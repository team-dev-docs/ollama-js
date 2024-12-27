# Multimodal Support in Ollama-js

Ollama-js provides support for multimodal models, allowing you to use images in generate and chat requests. This document explains how to utilize this functionality with code examples.

## Using Images in Generate Requests

To include images in a generate request, you can pass an array of image paths or image data to the `images` parameter of the `generate` function.

### Example:

```typescript
import ollama from 'ollama'

const imagePath = './path/to/your/image.jpg'
const response = await ollama.generate({
  model: 'llava',
  prompt: 'Describe this image:',
  images: [imagePath],
  stream: true,
})

for await (const part of response) {
  process.stdout.write(part.response)
}
```

In this example, we're using the 'llava' model, which is capable of processing images. The `images` array can contain multiple image paths or image data.

## Using Images in Chat Requests

For chat requests, you can include images in individual messages within the conversation.

### Example:

```typescript
import ollama from 'ollama'

const imagePath = './path/to/your/image.jpg'
const response = await ollama.chat({
  model: 'llava',
  messages: [
    {
      role: 'user',
      content: 'What do you see in this image?',
      images: [imagePath]
    }
  ],
  stream: true,
})

for await (const part of response) {
  process.stdout.write(part.response)
}
```

## Image Encoding

Ollama-js automatically handles image encoding for you. You can provide images in several formats:

1. File paths (as strings)
2. `Uint8Array` or `Buffer` objects containing raw image data
3. Base64 encoded strings

The library will automatically detect the format and encode it appropriately before sending the request to the Ollama server.

## Multimodal Example

For a complete working example of multimodal functionality, refer to the `multimodal.ts` file in the examples directory of the Ollama-js repository. This example demonstrates how to use the 'llava' model to generate a description of an image.

Here's a snippet from the multimodal example:

```typescript
import ollama from 'ollama'

const imagePath = './examples/multimodal/cat.jpg'
const response = await ollama.generate({
  model: 'llava',
  prompt: 'describe this image:',
  images: [imagePath],
  stream: true,
})
for await (const part of response) {
  process.stdout.write(part.response)
}
```

This example loads an image of a cat and asks the 'llava' model to describe it. The response is streamed, with each part of the description written to the console as it's generated.

## Important Notes

- Ensure you're using a model that supports multimodal inputs, such as 'llava'.
- The `images` parameter accepts an array, allowing you to include multiple images in a single request if the model supports it.
- When using file paths, make sure they are accessible from where your script is running.
- Streaming responses (`stream: true`) can be useful for real-time output, especially with longer descriptions or analyses.

By leveraging these multimodal capabilities, you can create more interactive and visually-aware applications with Ollama-js.