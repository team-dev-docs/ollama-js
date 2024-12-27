# Model Management in Ollama-js

Ollama-js provides several methods for managing models, including creating, pulling, pushing, and deleting models. This guide explains how to use these features with code examples.

## Creating Models

You can create a new model using the `create` method. This allows you to create a model from a Modelfile or by specifying a path to an existing model.

### API Endpoint

`POST /api/create`

### Method Signature

```typescript
create(request: CreateRequest): Promise<ProgressResponse | AbortableAsyncIterator<ProgressResponse>>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.create({
  model: 'my-new-model',
  path: '/path/to/model',
  stream: true
})

for await (const chunk of response) {
  console.log(chunk.status)
}
```

## Pulling Models

To download a model from the Ollama registry, use the `pull` method.

### API Endpoint

`POST /api/pull`

### Method Signature

```typescript
pull(request: PullRequest): Promise<ProgressResponse | AbortableAsyncIterator<ProgressResponse>>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.pull({
  model: 'llama2',
  stream: true
})

for await (const chunk of response) {
  console.log(`${chunk.status}: ${chunk.completed} / ${chunk.total}`)
}
```

## Pushing Models

You can push a local model to the Ollama registry using the `push` method.

### API Endpoint

`POST /api/push`

### Method Signature

```typescript
push(request: PushRequest): Promise<ProgressResponse | AbortableAsyncIterator<ProgressResponse>>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.push({
  model: 'my-custom-model',
  stream: true
})

for await (const chunk of response) {
  console.log(`${chunk.status}: ${chunk.completed} / ${chunk.total}`)
}
```

## Deleting Models

To remove a model from your local Ollama instance, use the `delete` method.

### API Endpoint

`DELETE /api/delete`

### Method Signature

```typescript
delete(request: DeleteRequest): Promise<StatusResponse>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.delete({
  model: 'model-to-delete'
})

console.log(response.status) // Should output 'success'
```

## Listing Models

You can get a list of all available models using the `list` method.

### API Endpoint

`GET /api/tags`

### Method Signature

```typescript
list(): Promise<ListResponse>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.list()
console.log(response.models)
```

## Showing Model Details

To get detailed information about a specific model, use the `show` method.

### API Endpoint

`POST /api/show`

### Method Signature

```typescript
show(request: ShowRequest): Promise<ShowResponse>
```

### Example

```javascript
import ollama from 'ollama'

const response = await ollama.show({ model: 'llama2' })
console.log(response.modelfile)
console.log(response.template)
console.log(response.parameters)
```

These methods provide comprehensive model management capabilities in Ollama-js, allowing you to create, download, upload, delete, list, and inspect models with ease.