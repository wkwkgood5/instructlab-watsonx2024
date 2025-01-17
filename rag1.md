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
