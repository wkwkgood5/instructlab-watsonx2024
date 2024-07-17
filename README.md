# Chat Models

Language models that use a sequence of messages as inputs and return chat messages as outputs (as opposed to using plain text). These are traditionally newer models (older models are generally LLMs, see below). Chat models support the assignment of distinct roles to conversation messages, helping to distinguish messages from the AI, users, and instructions such as system messages.

Although the underlying models are messages in, message out, the LangChain wrappers also allow these models to take a string as input. This means you can easily use chat models in place of LLMs.

When a string is passed in as input, it is converted to a `HumanMessage` and then passed to the underlying model.

LangChain does not host any Chat Models, rather we rely on third party integrations.

We have some standardized parameters when constructing ChatModels:

- **model:** the name of the model
- **temperature:** the sampling temperature
- **timeout:** request timeout
- **max_tokens:** max tokens to generate
- **stop:** default stop sequences
- **max_retries:** max number of times to retry requests
- **api_key:** API key for the model provider
- **base_url:** endpoint to send requests to

## Important Notes

- Standard params only apply to model providers that expose parameters with the intended functionality. For example, some providers do not expose a configuration for maximum output tokens, so `max_tokens` can't be supported on these.
- Standard params are currently only enforced on integrations that have their own integration packages (e.g. `langchain-openai`, `langchain-anthropic`, etc.), they're not enforced on models in `langchain-community`.
- ChatModels also accept other parameters that are specific to that integration. To find all the parameters supported by a ChatModel, head to the API reference for that model.

### Tool Calling

Some chat models have been fine-tuned for tool calling and provide a dedicated API for tool calling. Generally, such models are better at tool calling than non-fine-tuned models, and are recommended for use cases that require tool calling. Please see the tool calling section for more information.

For specifics on how to use chat models, see the relevant how-to guides [here](#).

## Multimodality

Some chat models are multimodal, accepting images, audio, and even video as inputs. These are still less common, meaning model providers haven't standardized on the "best" way to define the API. Multimodal outputs are even less common. As such, we've kept our multimodal abstractions fairly lightweight and plan to further


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

# Prompt Templates

Prompt templates help to translate user input and parameters into instructions for a language model. This can be used to guide a model's response, helping it understand the context and generate relevant and coherent language-based output.

Prompt Templates take as input a dictionary, where each key represents a variable in the prompt template to fill in.

Prompt Templates output a `PromptValue`. This `PromptValue` can be passed to an LLM or a ChatModel, and can also be cast to a string or a list of messages. The reason this `PromptValue` exists is to make it easy to switch between strings and messages.

There are a few different types of prompt templates:

## String PromptTemplates

These prompt templates are used to format a single string and generally are used for simpler inputs. For example, a common way to construct and use a `PromptTemplate` is as follows:

```python
from langchain_core.prompts import PromptTemplate

prompt_template = PromptTemplate.from_template("Tell me a joke about {topic}")

prompt_template.invoke({"topic": "cats"})
```

# Example Selectors

One common prompting technique for achieving better performance is to include examples as part of the prompt. This gives the language model concrete examples of how it should behave. Sometimes these examples are hardcoded into the prompt, but for more advanced situations, it may be nice to dynamically select them. Example Selectors are classes responsible for selecting and then formatting examples into prompts.

For specifics on how to use example selectors, see the relevant how-to guides [here](#).

# Output Parsers

> **NOTE**  
> The information here refers to parsers that take a text output from a model and try to parse it into a more structured representation. More and more models are supporting function (or tool) calling, which handles this automatically. It is recommended to use function/tool calling rather than output parsing. See documentation for that [here](#).

Output parsers are responsible for taking the output of a model and transforming it into a more suitable format for downstream tasks. This is useful when you are using LLMs to generate structured data or to normalize output from chat models and LLMs.

LangChain supports various types of output parsers. The table below has various pieces of information:

- **Name:** The name of the output parser
- **Supports Streaming:** Whether the output parser supports streaming.
- **Has Format Instructions:** Whether the output parser has format instructions. This is generally available except when (a) the desired schema is not specified in the prompt but rather in other parameters (like OpenAI function calling), or (b) when the OutputParser wraps another OutputParser.
- **Calls LLM:** Whether this output parser itself calls an LLM. This is usually only done by output parsers that attempt to correct misformatted output.
- **Input Type:** Expected input type. Most output parsers work on both strings and messages, but some (like OpenAI Functions) need a message with specific kwargs.
- **Output Type:** The output type of the object returned by the parser.
- **Description:** Our commentary on this output parser and when to use it.

# Chat History

Most LLM applications have a conversational interface. An essential component of a conversation is being able to refer to information introduced earlier in the conversation. At a bare minimum, a conversational system should be able to access some window of past messages directly.

The concept of `ChatHistory` refers to a class in LangChain which can be used to wrap an arbitrary chain. This `ChatHistory` will keep track of inputs and outputs of the underlying chain and append them as messages to a message database. Future interactions will then load those messages and pass them into the chain as part of the input.

# Documents

A `Document` object in LangChain contains information about some data. It has two attributes:

- **page_content**: `str` - The content of this document. Currently, it is only a string.
- **metadata**: `dict` - Arbitrary metadata associated with this document. Can track the document id, file name, etc.

# Document Loaders

These classes load `Document` objects. LangChain has hundreds of integrations with various data sources to load data from: Slack, Notion, Google Drive, etc.

Each `DocumentLoader` has its own specific parameters, but they can all be invoked in the same way with the `.load` method. An example use case is as follows:

```python
from langchain_community.document_loaders.csv_loader import CSVLoader

loader = CSVLoader(
    ...  # <-- Integration specific parameters here
)
data = loader.load()
```

# Text Splitters

Once you've loaded documents, you'll often want to transform them to better suit your application. The simplest example is you may want to split a long document into smaller chunks that can fit into your model's context window. LangChain has a number of built-in document transformers that make it easy to split, combine, filter, and otherwise manipulate documents.

When you want to deal with long pieces of text, it is necessary to split up that text into chunks. As simple as this sounds, there is a lot of potential complexity here. Ideally, you want to keep the semantically related pieces of text together. What "semantically related" means could depend on the type of text. This notebook showcases several ways to do that.

At a high level, text splitters work as follows:

1. Split the text up into small, semantically meaningful chunks (often sentences).
2. Start combining these small chunks into a larger chunk until you reach a certain size (as measured by some function).
3. Once you reach that size, make that chunk its own piece of text and then start creating a new chunk of text with some overlap (to keep context between chunks).

That means there are two different axes along which you can customize your text splitter:

- **How the text is split**
- **How the chunk size is measured**

For specifics on how to use text splitters, see the relevant how-to guides [here](#).

# Embedding Models

Embedding models create a vector representation of a piece of text. You can think of a vector as an array of numbers that captures the semantic meaning of the text. By representing the text in this way, you can perform mathematical operations that allow you to do things like search for other pieces of text that are most similar in meaning. These natural language search capabilities underpin many types of context retrieval, where we provide an LLM with the relevant data it needs to effectively respond to a query.

The `Embeddings` class is designed for interfacing with text embedding models. There are many different embedding model providers (OpenAI, Cohere, Hugging Face, etc.) and local models, and this class is designed to provide a standard interface for all of them.

The base `Embeddings` class in LangChain provides two methods: one for embedding documents and one for embedding a query. The former takes as input multiple texts, while the latter takes a single text. The reason for having these as two separate methods is that some embedding providers have different embedding methods for documents (to be searched over) vs. queries (the search query itself).

For specifics on how to use embedding models, see the relevant how-to guides [here](#).

# Vector Stores

One of the most common ways to store and search over unstructured data is to embed it and store the resulting embedding vectors, and then at query time to embed the unstructured query and retrieve the embedding vectors that are 'most similar' to the embedded query. A vector store takes care of storing embedded data and performing vector search for you.

Most vector stores can also store metadata about embedded vectors and support filtering on that metadata before similarity search, allowing you more control over returned documents.

Vector stores can be converted to the retriever interface by doing:

```python
vectorstore = MyVectorStore()
retriever = vectorstore.as_retriever()
```

# Tools

Tools are interfaces that an agent, a chain, or a chat model/LLM can use to interact with the world.

A tool consists of the following components:

- The name of the tool
- A description of what the tool does
- JSON schema of what the inputs to the tool are
- The function to call
- Whether the result of a tool should be returned directly to the user (only relevant for agents)

The name, description, and JSON schema are provided as context to the LLM, allowing the LLM to determine how to use the tool appropriately.

Given a list of available tools and a prompt, an LLM can request that one or more tools be invoked with appropriate arguments.

Generally, when designing tools to be used by a chat model or LLM, it is important to keep in mind the following:

- Chat models that have been fine-tuned for tool calling will be better at tool calling than non-fine-tuned models.
- Non fine-tuned models may not be able to use tools at all, especially if the tools are complex or require multiple tool calls.
- Models will perform better if the tools have well-chosen names, descriptions, and JSON schemas.
- Simpler tools are generally easier for models to use than more complex tools.

For specifics on how to use tools, see the relevant how-to guides [here](#).

To use an existing pre-built tool, see [here](#) for a list of pre-built tools.

# Toolkits

Toolkits are collections of tools that are designed to be used together for specific tasks. They have convenient loading methods.

All Toolkits expose a `get_tools` method which returns a list of tools. You can therefore do:

```python
# Initialize a toolkit
toolkit = ExampleToolkit(...)

# Get list of tools
tools = toolkit.get_tools()
```

# Agents

By themselves, language models can't take actions - they just output text. A big use case for LangChain is creating agents. Agents are systems that use an LLM as a reasoning engine to determine which actions to take and what the inputs to those actions should be. The results of those actions can then be fed back into the agent and it determines whether more actions are needed, or whether it is okay to finish.

LangGraph is an extension of LangChain specifically aimed at creating highly controllable and customizable agents. Please check out that documentation for a more in-depth overview of agent concepts.

There is a legacy agent concept in LangChain that we are moving towards deprecating: `AgentExecutor`. `AgentExecutor` was essentially a runtime for agents. It was a great place to get started; however, it was not flexible enough as you started to have more customized agents. In order to solve that, we built LangGraph to be this flexible, highly-controllable runtime.

If you are still using `AgentExecutor`, do not fear: we still have a guide on how to use `AgentExecutor`. It is recommended, however, that you start to transition to LangGraph. In order to assist in this, we have put together a transition guide on how to do so.

## ReAct Agents

One popular architecture for building agents is ReAct. ReAct combines reasoning and acting in an iterative process - in fact, the name "ReAct" stands for "Reason" and "Act".

The general flow looks like this:

1. The model will "think" about what step to take in response to an input and any previous observations.
2. The model will then choose an action from available tools (or choose to respond to the user).
3. The model will generate arguments for that tool.
4. The agent runtime (executor) will parse out the chosen tool and call it with the generated arguments.
5. The executor will return the results of the tool call back to the model as an observation.
6. This process repeats until the agent chooses to respond.

There are general prompting-based implementations that do not require any model-specific features, but the most reliable implementations use features like tool calling to reliably format outputs and reduce variance.


