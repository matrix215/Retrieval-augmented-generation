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
