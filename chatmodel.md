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
