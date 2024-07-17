# Chat History

Most LLM applications have a conversational interface. An essential component of a conversation is being able to refer to information introduced earlier in the conversation. At a bare minimum, a conversational system should be able to access some window of past messages directly.

The concept of `ChatHistory` refers to a class in LangChain which can be used to wrap an arbitrary chain. This `ChatHistory` will keep track of inputs and outputs of the underlying chain and append them as messages to a message database. Future interactions will then load those messages and pass them into the chain as part of the input.
