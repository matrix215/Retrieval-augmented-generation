## Overview

### What is RAG?

RAG is a technique for augmenting LLM knowledge with additional, often private or real-time, data.

LLMs can reason about wide-ranging topics, but their knowledge is limited to the public data up to a specific point in time that they were trained on. If you want to build AI applications that can reason about private data or data introduced after a model's cutoff date, you need to augment the knowledge of the model with the specific information it needs. The process of bringing the appropriate information and inserting it into the model prompt is known as Retrieval Augmented Generation (RAG).

### What's in this guide?

LangChain has a number of components specifically designed to help build RAG applications. To familiarize ourselves with these, we'll build a simple question-answering application over a text data source. Specifically, we'll build a QA bot over the [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) blog post by Lilian Weng. Along the way we'll go over a typical QA architecture, discuss the relevant LangChain components, and highlight additional resources for more advanced QA techniques. We'll also see how LangSmith can help us trace and understand our application. LangSmith will become increasingly helpful as our application grows in complexity.

**Note**
Here we focus on RAG for unstructured data. Two RAG use cases which we cover elsewhere are:
- [QA over structured data](/docs/use_cases/qa_structured/sql) (e.g., SQL)
- [QA over code](/docs/use_cases/question_answering/code_understanding) (e.g., Python)


## Architecture
A typical RAG application has two main components:

**Indexing**: a pipeline for ingesting data from a source and indexing it. *This usually happens offline.*

**Retrieval and generation**: the actual RAG chain, which takes the user query at run time and retrieves the relevant data from the index, then passes that to the model.

The most common full sequence from raw data to answer looks like this:

#### Indexing
1. **Load**: First we need to load our data. We'll use [DocumentLoaders](/docs/modules/data_connection/document_loaders/) for this.
2. **Split**: [Text splitters](/docs/modules/data_connection/document_transformers/) break large `Documents` into smaller chunks. This is useful both for indexing data and for passing it in to a model, since large chunks are harder to search over and won't in a model's finite context window.
3. **Store**: We need somewhere to store and index our splits so that they can later be searched over. This is often done using a [VectorStore](/docs/modules/data_connection/vectorstores/) and [Embeddings](/docs/modules/data_connection/text_embedding/) model.

![image](https://github.com/matrix215/Retrieval-augmented-generation/assets/101815603/bde07e45-0cd0-4b8e-8eff-bc38e453cb46)


#### Retrieval and Generation
4. **Retrieve**: Given a user input, relevant splits are retrieved from storage using a [Retriever](/docs/modules/data_connection/retrievers/).
5. **Generate**: A [ChatModel](/docs/modules/model_io/chat_models) / [LLM](/docs/modules/model_io/llms/) produces an answer using a prompt that includes the question and the retrieved data

![image](https://github.com/matrix215/Retrieval-augmented-generation/assets/101815603/6d20a339-9e36-4874-8dbd-99127b88efae)

