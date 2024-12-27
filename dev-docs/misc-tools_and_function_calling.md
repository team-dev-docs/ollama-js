# Using Tools and Function Calling in Ollama-js

Ollama-js supports tools and function calling capabilities, allowing you to define custom functions that can be called by the AI model during chat interactions. This enables more complex and interactive conversations, where the model can request specific actions or calculations to be performed.

## Defining Tools

Tools are defined as JavaScript objects that specify the function name, description, and parameters. Here's an example of defining two calculator tools:

```typescript
const addTwoNumbersTool = {
  type: 'function',
  function: {
    name: 'addTwoNumbers',
    description: 'Add two numbers together',
    parameters: {
      type: 'object',
      required: ['a', 'b'],
      properties: {
        a: { type: 'number', description: 'The first number' },
        b: { type: 'number', description: 'The second number' }
      }
    }
  }
};

const subtractTwoNumbersTool = {
  type: 'function',
  function: {
    name: 'subtractTwoNumbers',
    description: 'Subtract two numbers',
    parameters: {
      type: 'object',
      required: ['a', 'b'],
      properties: {
        a: { type: 'number', description: 'The first number' },
        b: { type: 'number', description: 'The second number' }
      }
    }
  }
};
```

## Implementing Tool Functions

You'll need to implement the actual functions that correspond to your tool definitions:

```typescript
function addTwoNumbers(args: { a: number, b: number }): number {
  return args.a + args.b;
}

function subtractTwoNumbers(args: { a: number, b: number }): number {
  return args.a - args.b;
}

const availableFunctions = {
  addTwoNumbers: addTwoNumbers,
  subtractTwoNumbers: subtractTwoNumbers
};
```

## Making Requests with Tools

When making a chat request, include the tools in the request options:

```typescript
const response = await ollama.chat({
  model: 'llama3.1:8b',
  messages: [{ role: 'user', content: 'What is three minus one?' }],
  tools: [addTwoNumbersTool, subtractTwoNumbersTool]
});
```

## Handling Tool Calls in Responses

After receiving a response, check if the model has requested any tool calls:

```typescript
if (response.message.tool_calls) {
  for (const tool of response.message.tool_calls) {
    const functionToCall = availableFunctions[tool.function.name];
    if (functionToCall) {
      console.log('Calling function:', tool.function.name);
      console.log('Arguments:', tool.function.arguments);
      const output = functionToCall(tool.function.arguments);
      console.log('Function output:', output);

      // Add the function response to messages for the model to use
      messages.push(response.message);
      messages.push({
        role: 'tool',
        content: output.toString(),
      });
    } else {
      console.log('Function', tool.function.name, 'not found');
    }
  }

  // Get final response from model with function outputs
  const finalResponse = await ollama.chat({
    model: 'llama3.1:8b',
    messages: messages
  });
  console.log('Final response:', finalResponse.message.content);
}
```

## Full Example: Calculator

Here's a complete example demonstrating the use of tools for a simple calculator:

```typescript
import ollama from 'ollama';

// Tool definitions and function implementations
// ... (as shown in previous sections)

async function run(model: string) {
  const messages = [{ role: 'user', content: 'What is three minus one?' }];
  console.log('Prompt:', messages[0].content);

  const response = await ollama.chat({
    model: model,
    messages: messages,
    tools: [addTwoNumbersTool, subtractTwoNumbersTool]
  });

  if (response.message.tool_calls) {
    // Process tool calls from the response
    for (const tool of response.message.tool_calls) {
      const functionToCall = availableFunctions[tool.function.name];
      if (functionToCall) {
        console.log('Calling function:', tool.function.name);
        console.log('Arguments:', tool.function.arguments);
        const output = functionToCall(tool.function.arguments);
        console.log('Function output:', output);

        // Add the function response to messages for the model to use
        messages.push(response.message);
        messages.push({
          role: 'tool',
          content: output.toString(),
        });
      } else {
        console.log('Function', tool.function.name, 'not found');
      }
    }

    // Get final response from model with function outputs
    const finalResponse = await ollama.chat({
      model: model,
      messages: messages
    });
    console.log('Final response:', finalResponse.message.content);
  } else {
    console.log('No tool calls returned from model');
  }
}

run('llama3.1:8b').catch(error => console.error("An error occurred:", error));
```

This example shows how to define tools, make requests with tools, and handle tool calls in responses. The model can use these tools to perform calculations and provide more accurate and interactive responses.