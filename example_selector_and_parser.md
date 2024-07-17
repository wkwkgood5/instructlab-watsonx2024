# Example Selectors

One common prompting technique for achieving better performance is to include examples as part of the prompt. This gives the language model concrete examples of how it should behave. Sometimes these examples are hardcoded into the prompt, but for more advanced situations, it may be nice to dynamically select them. Example Selectors are classes responsible for selecting and then formatting examples into prompts.

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
