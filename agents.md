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
