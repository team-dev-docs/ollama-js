# Using Embeddings in Ollama-js

Ollama-js provides two methods for generating embeddings from text: `embed` and `embeddings`. These allow you to convert text into vector representations that can be used for semantic search, clustering, and other natural language processing tasks.

## The `embed` Method

The `embed` method allows you to generate embeddings for one or more text inputs.

### Usage

```javascript
const ollama = new Ollama();

const response = await ollama.embed({
  model: "llama2", 
  input: "Hello world"
});
```

You can also pass an array of strings to embed multiple inputs:

```javascript
const response = await ollama.embed({
  model: "llama2",
  input: ["Hello world", "How are you?"]
});
```

### Parameters

- `model`: The name of the model to use for embedding (required)
- `input`: A string or array of strings to embed (required)
- `truncate`: Whether to truncate the input to the model's maximum context length (optional)
- `keep_alive`: Duration to keep the model loaded in memory (optional)
- `options`: Additional model options (optional)

### Response Format

The `embed` method returns a promise that resolves to an `EmbedResponse` object:

```typescript
interface EmbedResponse {
  model: string;
  embeddings: number[][];
}
```

- `model`: The name of the model used
- `embeddings`: An array of embedding vectors, where each vector is an array of numbers

## The `embeddings` Method

The `embeddings` method is similar to `embed`, but is designed for generating a single embedding.

### Usage

```javascript
const ollama = new Ollama();

const response = await ollama.embeddings({
  model: "llama2",
  prompt: "Hello world"
});
```

### Parameters

- `model`: The name of the model to use for embedding (required)
- `prompt`: The text to embed (required)
- `keep_alive`: Duration to keep the model loaded in memory (optional)
- `options`: Additional model options (optional)

### Response Format

The `embeddings` method returns a promise that resolves to an `EmbeddingsResponse` object:

```typescript
interface EmbeddingsResponse {
  embedding: number[];
}
```

- `embedding`: A single embedding vector represented as an array of numbers

## Differences Between `embed` and `embeddings`

While both methods generate embeddings, there are some key differences:

1. `embed` can handle multiple inputs, while `embeddings` is for a single input.
2. `embed` uses the parameter name `input`, while `embeddings` uses `prompt`.
3. The response formats differ slightly, with `embed` returning an array of embeddings and `embeddings` returning a single embedding.

Choose the method that best fits your use case and input format.