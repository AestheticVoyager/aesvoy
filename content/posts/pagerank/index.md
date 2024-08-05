---
title: "PageRank"
date: 2024-08-02
draft: false
summary: "PageRank, created by Google founders Larry Page and Sergey Brin, changed the web by ranking pages based on the quality and quantity of their links, rather than just keywords. It evaluates a page’s authority through its endorsements, improving the relevance and trustworthiness of search results."
tags: ["Linear Algebra", "PageRank", "Algorithm", "Google", "Search Engine"]
---

# Understanding PageRank: Google's Game-Changing Algorithm

PageRank(PR) is an algorithm that revolutionized how we navigate the internet. Developed by Google and named after one of its co-founders, [Larry Page](https://en.wikipedia.org/wiki/Larry_Page), this algorithm ranks websites in Google's search engine results. PageRank measures the importance of web pages by evaluating the quantity and quality of links pointing to them, based on the principle that more significant websites are likely to attract a higher number of links from other sites.

Although PageRank isn't the only algorithm Google uses to rank search results, it was the first and remains the most well-known.

## How PageRank Works

The PageRank algorithm creates a probability distribution representing the likelihood that a user, randomly clicking on links, will land on any particular page. This can be applied to collections of documents of any size. Initially, the probability distribution is assumed to be evenly divided among all documents in the collection. The computation of PageRank involves multiple iterations through the document collection to adjust the PageRank values progressively, making them more accurate.

### TL;DR Example

[Drawn Explanation of PageRank Algorithm](https://raw.githubusercontent.com/AestheticVoyager/aesvoy/main/content/posts/pagerank/image.png "PageRank")

### The Role of Linear Algebra in PageRank

PageRank leverages several linear algebra techniques, primarily revolving around matrix operations. Here’s a simple summary of the key concepts:

1. **Link Matrix**: The web is represented as a directed graph, with each page as a node and each hyperlink as a directed edge. This is encoded into a stochastic matrix P, known as the link matrix, where each entry P_ij represents the probability of transitioning from page j to page i.

2. **Probability Distribution Vector**: The rank of each page is represented as a probability distribution vector v, with each entry corresponding to the rank of a page. Initially, this vector is usually uniformly distributed.

3. **Power Iteration Method**: To find the steady-state distribution (the PageRank vector), the algorithm uses the power iteration method. This process is iterated until v converges to a steady-state vector.

4. **Teleportation and Damping Factor**: To handle the problem of rank sinks (pages with no outgoing links) and ensure convergence, a damping factor d is introduced. The modified PageRank formula incorporates teleportation, allowing a random jump to any page with a small probability 1 - d.

5. **Convergence**: The process continues until the difference between successive iterations is below a certain threshold, indicating that the algorithm has converged to a stable PageRank vector.

By applying these linear algebra techniques, PageRank effectively computes the relative importance of each web page. This allows Google's search engine to deliver relevant and high-quality search results, fundamentally transforming how we find information online.

## Alternatives to PageRank and Other Search Algorithms

While PageRank has been incredibly influential, several other algorithms and methods are used in search engines today. Here are some notable alternatives and additional algorithms that enhance search capabilities:

### 1. [**HITS (Hyperlink-Induced Topic Search)**](https://en.wikipedia.org/wiki/HITS_algorithm)

Developed around the same time as PageRank, the HITS algorithm, also known as Hubs and Authorities, focuses on identifying two types of web pages: hubs, which are good sources of links to other pages, and authorities, which are pages linked by many hubs. HITS processes the web's link structure to assign two scores to each page, reflecting its value as a hub and as an authority.

### 2. [**TrustRank**](https://en.wikipedia.org/wiki/TrustRank)

TrustRank is designed to combat web spam by propagating trust from a small set of manually verified trustworthy seed pages to other pages. The algorithm assumes that trustworthy sites are less likely to link to spammy ones, thus helping to rank high-quality content higher.

### 3. [**SALSA (Stochastic Approach for Link-Structure Analysis)**](https://en.wikipedia.org/wiki/SALSA_algorithm#:~:text=Stochastic%20Approach%20for%20Link%2DStructure,quantity%20of%20hyperlinks%20among%20them.)

Similar to HITS, SALSA aims to identify authoritative pages and hubs. It combines ideas from both PageRank and HITS by performing random walks on two bipartite graphs formed from the web's link structure, providing a more robust measure of importance in specific contexts.

### 4. [**BM25 (Best Matching 25)**](https://en.wikipedia.org/wiki/Okapi_BM25)

BM25 is a probabilistic information retrieval algorithm used primarily for text search. It ranks documents based on the query terms appearing in each document, considering the term frequency and the length of the document. This approach is particularly effective for matching text-based queries with relevant documents.

### 5. [**Neural Network-Based Algorithms**](https://www.geeksforgeeks.org/neural-architecture-and-search-methods/#:~:text=Neural%20Architecture%20Search%20(NAS)%20falls,learning%20to%20real%2Dworld%20challenges.)

Modern search engines increasingly incorporate neural network-based algorithms. These deep learning models, such as BERT (Bidirectional Encoder Representations from Transformers), understand the context and semantics of search queries better than traditional keyword-based approaches. BERT, for example, allows Google to comprehend the nuances of language, improving the accuracy of search results.

### 6. **Personalization Algorithms**

Personalization algorithms tailor search results to individual users based on their past behavior, preferences, and demographic information. By analyzing user data, these algorithms deliver more relevant and customized search results, enhancing the user experience.


## LLMs vs Search Algorithms

Large Language Models (LLMs) could transform the search experience for everyday users in several impactful ways. Unlike traditional search engines that present a list of links for users to sift through, LLMs can generate concise, contextually relevant answers right away. This means users get straightforward responses to their queries without having to navigate through multiple websites. For example, if someone asks for a recipe or the steps to fix a household issue, an LLM can provide a detailed, step-by-step guide in one go.

Moreover, LLMs can handle more complex, conversational queries. Instead of needing to refine a search through multiple attempts, users can engage in a back-and-forth dialogue with the model. They can ask follow-up questions or request clarifications, making the search process more interactive and personalized. This conversational approach can be especially useful for users with specific needs or those who are exploring a topic in depth.

Another advantage is that LLMs can generate personalized content based on the context of previous interactions. If you’ve asked about travel destinations before, an LLM can tailor its recommendations based on your interests or past inquiries, offering more relevant and customized suggestions.

Overall, LLMs can make the search process faster, more intuitive, and tailored to individual needs, reducing the time spent navigating through search results and providing users with more direct and actionable information.
