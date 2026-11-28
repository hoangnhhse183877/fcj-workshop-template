---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Amazon S3 Vectors Hybrid Search – Optimizing Search and RAG Systems on AWS

| Information | Details |
|---|---|
| Author | Đan Huy (Team Leader) |
| Publication date | November 27, 2026 |
| Status | Approved and published |
| Platform | [AWS Study Group VN](https://www.facebook.com/groups/awsstudygroupfcj) |
| Post URL | [View the original post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347637312667894) |

This article was written and published by Đan Huy, my team leader. It is included as a contribution from our team; the article below retains the original author's perspective.

## 1. Introduction

Are you building a RAG application and still having to set up a separate vector database? AWS has introduced an interesting update that may make us reconsider this approach.

In AI projects that use RAG, I have often seen people use Pinecone, Qdrant, pgvector, or OpenSearch to store embeddings and perform semantic search.

## 2. Amazon S3 Vectors Overview

One AWS service that I have found particularly noteworthy recently is Amazon S3 Vectors.

I previously knew Amazon S3 mainly as a service for storing images, videos, documents, or JSON data. Since AWS introduced S3 Vectors, however, we can store and query vector embeddings without necessarily deploying a separate vector database ourselves.

In particular, on November 16, 2026, AWS added Hybrid Search to S3 Vectors.

## 3. Hybrid Search, BM25, and RRF

This addresses an interesting problem in AI-powered search systems.

Suppose a user searches for "iPhone 16 Pro battery drain after an update."

With keyword search alone, the system may miss comments such as "The phone's battery is draining unusually quickly after the update."

Conversely, with Vector Search alone, the system may find semantically similar comments but may not always prioritize the exact product name or error code that the user is looking for.

Hybrid Search combines both approaches:

- Vector Search enables semantic search and recognizes different ways of describing the same issue.
- Full-text Search (BM25) helps prioritize specific keywords such as product names, error codes, or technical terms.

AWS then uses Reciprocal Rank Fusion (RRF) to combine the results and return the most relevant ranked list.

A useful feature is that both search methods can be performed on the same S3 Vectors index through the QueryVectors API.

## 4. Application in BrandTracker

I am exploring how to apply this approach to BrandTracker, a system that aggregates and analyzes user feedback from social media.

For example, posts and comments are collected from Reddit and YouTube, then sent through Amazon SQS for asynchronous processing. AWS Lambda cleans and chunks the content and uses an embedding model on Amazon Bedrock.

Lambda then stores the vectors and metadata in Amazon S3 Vectors.

When users want to find issues that customers are complaining about regarding a product, the backend can combine semantic search and keyword search to retrieve relevant feedback, together with links to the original posts.

Instead of simply identifying whether customers mention particular keywords, the system can find feedback with the same meaning even when it is expressed in entirely different ways.

## 5. System Architecture

I have drawn a simple architecture illustrating the processing flow using SQS, Lambda, Bedrock, and S3 Vectors to make the approach easier to visualize.


![Figure 1. Proposed BrandTracker architecture: ingestion through Amazon SQS and AWS Lambda, embedding generation with Amazon Bedrock, and hybrid search queries on Amazon S3 Vectors.](/images/blogs/blog3/brandtracker-s3-vectors-architecture.png)

*Figure 1. Proposed BrandTracker architecture: ingestion through Amazon SQS and AWS Lambda, embedding generation with Amazon Bedrock, and hybrid search queries on Amazon S3 Vectors.*


The diagram describes an application approach under investigation, rather than evidence of a successfully deployed system.

## 6. Cost Optimization

Another noteworthy point is AWS's claim that S3 Vectors can reduce vector storage and query costs by up to 90% compared with certain dedicated vector solutions. This is a maximum figure reported by AWS, and actual savings depend on data volume, query frequency, and latency requirements.

## 7. Limitations and Technical Considerations

S3 Vectors is not yet a complete replacement for pgvector, Qdrant, or OpenSearch. For systems requiring extremely fast queries, high query frequency, or complex search capabilities, dedicated solutions are still worth considering.

One technical consideration is that Hybrid Search requires a vector index in ENHANCED mode. When S3 Vectors is used through Bedrock Knowledge Bases, that integration currently remains limited to semantic-only search, so the new Hybrid Search feature can be accessed directly using the QueryVectors API.

## 8. Conclusion

I think this is an approach worth exploring for RAG projects, AI agents, data analysis systems, or semantic search applications that handle large volumes of data but do not necessarily need to maintain a dedicated vector database.

If you were building a real-world AI system, would you choose S3 Vectors, pgvector, or OpenSearch? Under what circumstances would you use each solution?

## 9. References

1. [AWS – Amazon S3 Vectors now supports hybrid search (16/11/2026)](https://aws.amazon.com/about-aws/whats-new/2026/11/s3-vectors-hybrid-search/)
2. [InfoWorld – AWS looks to cut storage costs for LLM embeddings with Amazon S3 Vectors](https://www.infoworld.com/article/4023401/aws-looks-to-cut-storage-costs-for-llm-embeddings-with-amazon-s3-vectors.html)
3. [DevelopersIO – I Tried Hybrid Search (Vector + Full-Text) with Amazon S3 Vectors (17/11/2026)](https://dev.classmethod.jp/en/articles/s3-vectors-hybrid-search-try/)

### Additional technical documentation for verification

- [Hybrid search with Amazon S3 Vectors](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-hybrid-search.html)
- [Configuring full-text search](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-configuring-full-text-search.html)
- [Using S3 Vectors with Amazon Bedrock Knowledge Bases](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-bedrock-kb.html)

#AWS #AmazonS3 #RAG #VectorSearch #GenerativeAI #CloudComputing #SoftwareEngineering #FCAJ

## Published Article Evidence


![Figure 2. Đan Huy's article in AWS Study Group VN, November 27, 2026.](/images/blogs/blog3/blog3-publication-proof.png)

*Figure 2. Đan Huy's article in AWS Study Group VN, November 27, 2026.*


The article was confirmed to have been approved and published on November 27, 2026. [View the original post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347637312667894).
