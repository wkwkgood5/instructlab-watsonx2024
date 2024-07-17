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
