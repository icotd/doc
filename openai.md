# OpenAI TypeScript API Library

## Installation

```sh
bun add openai
```

## OpenAI Responses API

OpenAI's most advanced interface for generating model responses. Supports text and image inputs, and text outputs. Create stateful interactions with the model, using the output of previous responses as input. Extend the model's capabilities with built-in tools for file search, web search, computer use, and more. Allow the model access to external systems and data using function calling.


```ts
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: process.env['OPENAI_API_KEY'], // This is the default and can be omitted
});

const response = await client.responses.create({
  model: 'gpt-4o',
  instructions: 'You are a coding assistant that talks like a pirate',
  input: 'Are semicolons optional in JavaScript?',
});

console.log(response.output_text);
```

## Streaming responses

We provide support for streaming responses using Server Sent Events (SSE).

```ts
import OpenAI from 'openai';

const client = new OpenAI();

const stream = await client.responses.create({
  model: 'gpt-4o',
  input: 'Say "Sheep sleep deep" ten times fast!',
  stream: true,
});

for await (const event of stream) {
  console.log(event);
}
```

## File uploads

Request parameters that correspond to file uploads can be passed in many different forms:

- `File` (or an object with the same structure)
- a `fetch` `Response` (or an object with the same structure)
- an `fs.ReadStream`
- the return value of our `toFile` helper

```ts
import fs from 'fs';
import fetch from 'node-fetch';
import OpenAI, { toFile } from 'openai';

const client = new OpenAI();

// If you have access to Node `fs` we recommend using `fs.createReadStream()`:
await client.files.create({ file: fs.createReadStream('input.jsonl'), purpose: 'fine-tune' });

// Or if you have the web `File` API you can pass a `File` instance:
await client.files.create({ file: new File(['my bytes'], 'input.jsonl'), purpose: 'fine-tune' });

// You can also pass a `fetch` `Response`:
await client.files.create({ file: await fetch('https://somesite/input.jsonl'), purpose: 'fine-tune' });

// Finally, if none of the above are convenient, you can use our `toFile` helper:
await client.files.create({
  file: await toFile(Buffer.from('my bytes'), 'input.jsonl'),
  purpose: 'fine-tune',
});
await client.files.create({
  file: await toFile(new Uint8Array([0, 1, 2]), 'input.jsonl'),
  purpose: 'fine-tune',
});
```

## Handling errors

When the library is unable to connect to the API,
or if the API returns a non-success status code (i.e., 4xx or 5xx response),
a subclass of `APIError` will be thrown:

<!-- prettier-ignore -->
```ts
async function main() {
  const job = await client.fineTuning.jobs
    .create({ model: 'gpt-4o', training_file: 'file-abc123' })
    .catch(async (err) => {
      if (err instanceof OpenAI.APIError) {
        console.log(err.request_id);
        console.log(err.status); // 400
        console.log(err.name); // BadRequestError
        console.log(err.headers); // {server: 'nginx', ...}
      } else {
        throw err;
      }
    });
}

main();
```

Error codes are as followed:

| Status Code | Error Type                 |
| ----------- | -------------------------- |
| 400         | `BadRequestError`          |
| 401         | `AuthenticationError`      |
| 403         | `PermissionDeniedError`    |
| 404         | `NotFoundError`            |
| 422         | `UnprocessableEntityError` |
| 429         | `RateLimitError`           |
| >=500       | `InternalServerError`      |
| N/A         | `APIConnectionError`       |

### Retries

Certain errors will be automatically retried 2 times by default, with a short exponential backoff.
Connection errors (for example, due to a network connectivity problem), 408 Request Timeout, 409 Conflict,
429 Rate Limit, and >=500 Internal errors will all be retried by default.

You can use the `maxRetries` option to configure or disable this:
 
```js
// Configure the default for all requests:
const client = new OpenAI({
  maxRetries: 0, // default is 2
});

// Or, configure per-request:
await client.chat.completions.create({ messages: [{ role: 'user', content: 'How can I get the name of the current day in JavaScript?' }], model: 'gpt-4o' }, {
  maxRetries: 5,
});
```

### Timeouts

Requests time out after 10 minutes by default. You can configure this with a `timeout` option:

 
```ts
// Configure the default for all requests:
const client = new OpenAI({
  timeout: 20 * 1000, // 20 seconds (default is 10 minutes)
});

// Override per-request:
await client.chat.completions.create({ messages: [{ role: 'user', content: 'How can I list all files in a directory using Python?' }], model: 'gpt-4o' }, {
  timeout: 5 * 1000,
});
```

On timeout, an `APIConnectionTimeoutError` is thrown.

Note that requests which time out will be [retried twice by default](#retries).

## Request IDs

> For more information on debugging requests, see [these docs](https://platform.openai.com/docs/api-reference/debugging-requests)

All object responses in the SDK provide a `_request_id` property which is added from the `x-request-id` response header so that you can quickly log failing requests and report them back to OpenAI.

```ts
const response = await client.responses.create({ model: 'gpt-4o', input: 'testing 123' });
console.log(response._request_id) // req_123
```

You can also access the Request ID using the `.withResponse()` method:

```ts
const { data: stream, request_id } = await openai.responses
  .create({
    model: 'gpt-4o',
    input: 'Say this is a test',
    stream: true,
  })
  .withResponse();
```

## Auto-pagination

List methods in the OpenAI API are paginated.
You can use the `for await … of` syntax to iterate through items across all pages:

```ts
async function fetchAllFineTuningJobs(params) {
  const allFineTuningJobs = [];
  // Automatically fetches more pages as needed.
  for await (const fineTuningJob of client.fineTuning.jobs.list({ limit: 20 })) {
    allFineTuningJobs.push(fineTuningJob);
  }
  return allFineTuningJobs;
}
```

Alternatively, you can request a single page at a time:

```ts
let page = await client.fineTuning.jobs.list({ limit: 20 });
for (const fineTuningJob of page.data) {
  console.log(fineTuningJob);
}

// Convenience methods are provided for manually paginating:
while (page.hasNextPage()) {
  page = await page.getNextPage();
  // ...
}
```

## CODE EXAMPLES


### STRUCTURED OUTPUTS

```ts
import { OpenAI } from 'openai';
import { zodTextFormat } from 'openai/helpers/zod';
import { z } from 'zod';

const Step = z.object({
  explanation: z.string(),
  output: z.string(),
});

const MathResponse = z.object({
  steps: z.array(Step),
  final_answer: z.string(),
});

const client = new OpenAI();

async function main() {
  const rsp = await client.responses.parse({
    input: 'solve 8x + 31 = 2',
    model: 'gpt-4o-2024-08-06',
    text: {
      format: zodTextFormat(MathResponse, 'math_response'),
    },
  });

  console.log(rsp.output_parsed);
  console.log('answer: ', rsp.output_parsed?.final_answer);
}

main().catch(console.error);
```

### structured outputs tools
```ts
import { OpenAI } from 'openai';
import { zodResponsesFunction } from 'openai/helpers/zod';
import { z } from 'zod';

const Table = z.enum(['orders', 'customers', 'products']);
const Column = z.enum([
  'id',
  'status',
  'expected_delivery_date',
  'delivered_at',
  'shipped_at',
  'ordered_at',
  'canceled_at',
]);
const Operator = z.enum(['=', '>', '<', '<=', '>=', '!=']);
const OrderBy = z.enum(['asc', 'desc']);
const DynamicValue = z.object({
  column_name: Column,
});

const Condition = z.object({
  column: Column,
  operator: Operator,
  value: z.union([z.string(), z.number(), DynamicValue]),
});

const Query = z.object({
  table_name: Table,
  columns: z.array(Column),
  conditions: z.array(Condition),
  order_by: OrderBy,
});

async function main() {
  const client = new OpenAI();

  const tool = zodResponsesFunction({ name: 'query', parameters: Query });

  const rsp = await client.responses.parse({
    model: 'gpt-4o-2024-08-06',
    input: 'look up all my orders in november of last year that were fulfilled but not delivered on time',
    tools: [tool],
  });

  console.log(rsp);

  const functionCall = rsp.output[0]!;

  if (functionCall.type !== 'function_call') {
    throw new Error('Expected function call');
  }

  const query = functionCall.parsed_arguments;

  console.log(query);
}

main();
```

### STREAM

```ts
import OpenAI from 'openai';

const openai = new OpenAI();

async function main() {
  const runner = openai.responses
    .stream({
      model: 'gpt-4o-2024-08-06',
      input: 'solve 8x + 31 = 2',
    })
    .on('event', (event) => console.log(event))
    .on('response.output_text.delta', (diff) => process.stdout.write(diff.delta));

  for await (const event of runner) {
    console.log('event', event);
  }

  const result = await runner.finalResponse();
  console.log(result);
}

main();
```

### STREAMING TOOLS

```ts
import { OpenAI } from 'openai';
import { zodResponsesFunction } from 'openai/helpers/zod';
import { z } from 'zod';

const Table = z.enum(['orders', 'customers', 'products']);
const Column = z.enum([
  'id',
  'status',
  'expected_delivery_date',
  'delivered_at',
  'shipped_at',
  'ordered_at',
  'canceled_at',
]);
const Operator = z.enum(['=', '>', '<', '<=', '>=', '!=']);
const OrderBy = z.enum(['asc', 'desc']);
const DynamicValue = z.object({
  column_name: Column,
});

const Condition = z.object({
  column: Column,
  operator: Operator,
  value: z.union([z.string(), z.number(), DynamicValue]),
});

const Query = z.object({
  table_name: Table,
  columns: z.array(Column),
  conditions: z.array(Condition),
  order_by: OrderBy,
});

async function main() {
  const client = new OpenAI();

  const tool = zodResponsesFunction({ name: 'query', parameters: Query });

  const stream = client.responses.stream({
    model: 'gpt-4o-2024-08-06',
    input: 'look up all my orders in november of last year that were fulfilled but not delivered on time',
    tools: [tool],
  });

  for await (const event of stream) {
    console.dir(event, { depth: 10 });
  }
}

main();
```

## Responses Types

- <code>ComputerTool</code>
- <code>EasyInputMessage</code>
- <code>FileSearchTool</code>
- <code>FunctionTool</code>
- <code>Response</code>
- <code>ResponseAudioDeltaEvent</code>
- <code>ResponseAudioDoneEvent</code>
- <code>ResponseAudioTranscriptDeltaEvent</code>
- <code>ResponseAudioTranscriptDoneEvent</code>
- <code>ResponseCodeInterpreterCallCodeDeltaEvent</code>
- <code>ResponseCodeInterpreterCallCodeDoneEvent</code>
- <code>ResponseCodeInterpreterCallCompletedEvent</code>
- <code>ResponseCodeInterpreterCallInProgressEvent</code>
- <code>ResponseCodeInterpreterCallInterpretingEvent</code>
- <code>ResponseCodeInterpreterToolCall</code>
- <code>ResponseCompletedEvent</code>
- <code>ResponseComputerToolCall</code>
- <code>ResponseComputerToolCallOutputItem</code>
- <code>ResponseComputerToolCallOutputScreenshot</code>
- <code>ResponseContent</code>
- <code>ResponseContentPartAddedEvent</code>
- <code>ResponseContentPartDoneEvent</code>
- <code>ResponseCreatedEvent</code>
- <code>ResponseError</code>
- <code>ResponseErrorEvent</code>
- <code>ResponseFailedEvent</code>
- <code>ResponseFileSearchCallCompletedEvent</code>
- <code>ResponseFileSearchCallInProgressEvent</code>
- <code>ResponseFileSearchCallSearchingEvent</code>
- <code>ResponseFileSearchToolCall</code>
- <code>ResponseFormatTextConfig</code>
- <code>ResponseFormatTextJSONSchemaConfig</code>
- <code>ResponseFunctionCallArgumentsDeltaEvent</code>
- <code>ResponseFunctionCallArgumentsDoneEvent</code>
- <code>ResponseFunctionToolCall</code>
- <code>ResponseFunctionToolCallItem</code>
- <code>ResponseFunctionToolCallOutputItem</code>
- <code>ResponseFunctionWebSearch</code>
- <code>ResponseInProgressEvent</code>
- <code>ResponseIncludable</code>
- <code>ResponseIncompleteEvent</code>
- <code>ResponseInput</code>
- <code>ResponseInputAudio</code>
- <code>ResponseInputContent</code>
- <code>ResponseInputFile</code>
- <code>ResponseInputImage</code>
- <code>ResponseInputItem</code>
- <code>ResponseInputMessageContentList</code>
- <code>ResponseInputMessageItem</code>
- <code>ResponseInputText</code>
- <code>ResponseItem</code>
- <code>ResponseOutputAudio</code>
- <code>ResponseOutputItem</code>
- <code>ResponseOutputItemAddedEvent</code>
- <code>ResponseOutputItemDoneEvent</code>
- <code>ResponseOutputMessage</code>
- <code>ResponseOutputRefusal</code>
- <code>ResponseOutputText</code>
- <code>ResponseReasoningItem</code>
- <code>ResponseRefusalDeltaEvent</code>
- <code>ResponseRefusalDoneEvent</code>
- <code>ResponseStatus</code>
- <code>ResponseStreamEvent</code>
- <code>ResponseTextAnnotationDeltaEvent</code>
- <code>ResponseTextConfig</code>
- <code>ResponseTextDeltaEvent</code>
- <code>ResponseTextDoneEvent</code>
- <code>ResponseUsage</code>
- <code>ResponseWebSearchCallCompletedEvent</code>
- <code>ResponseWebSearchCallInProgressEvent</code>
- <code>ResponseWebSearchCallSearchingEvent</code>
- <code>Tool</code>
- <code>ToolChoiceFunction</code>
- <code>ToolChoiceOptions</code>
- <code>ToolChoiceTypes</code>
- <code>WebSearchTool</code>

Methods:

- <code title="post /responses">client.responses.create({ ...params }) -> Response</code>
- <code title="get /responses/{response_id}">client.responses.retrieve(responseId, { ...params }) -> Response</code>
- <code title="delete /responses/{response_id}">client.responses.del(responseId) -> void</code>

## InputItems

Types:

- <code>>ResponseItemList</code>

Methods:

- <code title="get /responses/{response_id}/input_items">client.responses.inputItems.list(responseId, { ...params }) -> ResponseItemsPage</code>
