# Messages

Some language models take a list of messages as input and return a message. There are a few different types of messages. All messages have a role, content, and `response_metadata` property.

The role describes WHO is saying the message. LangChain has different message classes for different roles.

The content property describes the content of the message. This can be a few different things:

- A string (most models deal with this type of content)
- A List of dictionaries (this is used for multimodal input, where the dictionary contains information about that input type and that input location)

## HumanMessage

This represents a message from the user.

## AIMessage

This represents a message from the model. In addition to the content property, these messages also have:

- `response_metadata`

### Response Metadata

The `response_metadata` property contains additional metadata about the response. The data here is often specific to each model provider. This is where information like log-probs and token usage may be stored.

### Tool Calls

These represent a decision from a language model to call a tool. They are included as part of an `AIMessage` output. They can be accessed from there with the `.tool_calls` property.

This property returns a list of dictionaries. Each dictionary has the following keys:

- **name:** The name of the tool that should be called.
- **args:** The arguments to that tool.
- **id:** The id of that tool call.

## SystemMessage

This represents a system message, which tells the model how to behave. Not every model provider supports this.

## FunctionMessage

This represents the result of a function call. In addition to role and content, this message has a `name` parameter which conveys the name of the function that was called to produce this result.

## ToolMessage

This represents the result of a tool call. This is distinct from a `FunctionMessage` in order to match OpenAI's function and tool message types. In addition to role and content, this message has a `tool_call_id` parameter which conveys the id of the call to the tool that was called to produce this result.
