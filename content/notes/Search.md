---
title: "Search"
tags:
- software
- nlp
enableToc: true
---

> Search has become a topic of interest of mine. In my opinion, it is a very powerful tool that has been largely stuck in the past. I think there is a lot of room for improvement in the search space. Here, I will just be documenting notes on the topic.

## How Search Engines Work
First, there needs to be some form of data for search engines to search through. This can be a database of documents, a database of images, a database of videos, etc. 

The search engine will then need to be able to "index" the data. This basically means that the search engine needs to parse the data and store it in a way that makes it easy to search through. An example of this is using inverted indices for text documents. This indexing system maps each significant word ever used in a document to a list of all the documents that contain that word. This allows for fast full-text searches.

Then, the actual search engine algorithm is used to retrieve the most relevant results for a given query. There are many different algorithms to do this, and it usually depends on how the data has been indexed. For example, if the data is indexed using inverted indices, then the search engine algorithm will likely be some form of Term Frequency-Inverse Document Frequency (TF-IDF) algorithm. This algorithm will rank documents based on how many times the query terms appear in the document, and how rare the query terms are in the entire corpus of documents. This algorithm will take the query, and generate a TF-IDF score for each document in the corpus. The documents with the highest scores will be returned as the results. To make the algorithm more advanced, you can account for typos using fuzzy matching, you can account for common stemming using lemmatization, etc.

These are the three main components of search engine in a nutshell. There are many more components that can be added, and there are many more ways to make each of these components more advanced. For example, we can account for semantic meaning by indexing using word/document embeddings.

## Types of Search
In my opinion, there are a few types of search that search engines can be used for:
- Exploratory Search
- Known-Item Search
- Question-Answering Search

Applications that utilize search are typically specialized for one of these types of search, but there's no reason why a search engine can't be created to be able to offer all three types of search.

### Exploratory Search
Exploratory search is when a user is searching a topic that they are not very familiar with for most likely the first time. There usually isn't a specific answer or outcome a user is looking for. They are just trying to learn more about the topic. An example is if a programmer were to search "What is a bloom filter?". This is what search engines such as Google and Bing search are used for.

#### Current Problems
There are a few problems with current search engines. 

One main thing is that current search engines are dominated by ads, as companies pay to have their websites show up first in the search results. This means that the search results are not necessarily the most relevant to the user, but rather the most relevant to the company that paid the most money, which makes user exploration harder.

[This](https://twitter.com/marcinignac/status/1400806180797231104?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1400806180797231104%7Ctwgr%5Ee613450bba35b3aa27932d794a9b626bb51bf7ae%7Ctwcon%5Es1_c10&ref_url=https%3A%2F%2Fwww.apoorva-srinivasan.com%2Fan-experiment-in-interfaces-to-explore-not-search%2F) Twitter thread by Marcin Ignac highlights another problem with current exploratory search. Current search engines do not necessarily _enable_ users to explore. They simply give users a list of links to click on, and the onus is on the user to explore. 

A cool example I saw to combad this can be found in [this](https://www.apoorva-srinivasan.com/an-experiment-in-interfaces-to-explore-not-search/) blog post.

#### Priorities for Exploratory Search
Data neutrality is a big priority for exploratory search. Users should be able to explore without being influenced by ads or other external factors. This is why I think it is important to have a search engine that is not dominated by ads.

It is also important to reemphasize that in exploratory search, user's do not know exactly what they are looking for. This means semantic search is important, which requires indexing using [document embeddings](CS224n.md#word2vec-overview). This allows search engines to return results that are semantically similar to the query. Additionally, it allows search engines to discover similarities and relationships between documents, which can be used to help users explore.

Speed is also important for exploratory search, though not as important as it is for known-item search. Users are innately impatient, so if a search engine takes too long to return results, users will likely leave the search engine. This is why it is important to have a fast search engine.

I attempted to build a search engine for exploring courses at The University of Texas at Austin [here](-course-search-5e5ddpxplq-uc.a.run.app). It utilizes semantic search, and tries to provide fast results.

### Known-Item Search
Known-item search occurs when a user knows exactly what they are looking for and just wants the search engine to take them to the item. An example of this is performing a file-search on your computer, or searching for a tab you have open in your browser. My favorite example of this is [Raycast](https://www.raycast.com/). I also like using Chrome's tab search feature because of all the tabs I have open.

#### Current Problems
I don't necessarily see many glaring problems with known-item search. However, I do think it has a lot of potential to be improved. Using known-item search can be a great productivity booster by basically creating a text-based interface for your computer, similar to how terminal text editors like Vim and Emacs can be insanely productive for those who are experts at it.

It would also be great web browsers allowed for more advanced known-item search based on my history. There are many times I search for the same thing repeatedly, and it would be great if in the search bar, I could type a few keywords from the contents of the website I am looking for, and the website would show up as a result. For example, I always need to search for how to create a new conda environment. I know exactly the website I need to get to, but I don't know the exact URL. It would be great if I could just type "conda environment" in the search bar, and the website would show up as a result. In general, better known-item search in web browsers would be great.

#### Priorities for Known-Item Search
I believe that speed is the most important priority for known-item search. If users know exactly what they want, they want to reach it pretty much immediately. 

I also think instant accessiblity is important. This means that the search engine should be accessible from anywhere with something like a keyboard shortcut. In general, I should not have to take my hands off of the keyboard to access it or to navigate it.


### Question-Answering Search
Question-answering search is when a user has a specific question they want answered. An example of this is searching "What is the capital of Texas?". This is what search engines such as Wolfram Alpha and Google's "People also ask" feature are used for. This is also basically how developers search for solutions to bugs.

Nowadays, users are more frequently using Foundational Language Models such as ChatGPT and Bard for these tasks, mainly because users don't need to find the right website to get the answer.

#### Current Problems
Currently, with the rise of Large Language Model chat applications, people have started to treat these models as oracles of truth. This can be a problem because of multiple reasons.

First, these models are prone to hallucinations. Models don't know what they don't know, so they can generate answers that are completely wrong with high confidence. 

Second, these models are only trained on information up to a certain date. If this model is prompted with a question about information after the date it was trained on, it would not be equipped to answer the question correctly, and with hallucinations, it could generate an answer that is completely wrong.

Lastly, using these models gives us a more lossy way of accessing information. All information that we try to access will go through another layer of processing, which can lead to information loss and potentially misinterpretation of the information.

One way to combat this to some extent is the use Retrieval-Augmented Generation (RAG). This is a technique that takes a query, retrieves relevant documents to the query, and provides the documents as context to the language model to generate an answer. This allows the model to be more informed about the query, and it allows the model to generate answers that are more grounded in reality. 

However, this still does not solve the problem of information loss, as the information still needs to go through processing in the language model. This can be mitigated if the language model also provided the documents that it used to generate the answer. However, this is not scalable. For a general purpose search engine, this would require an infinite amount of documents of various sizes to be stored, which is not feasible. Creating a balance between these foundational models and retrieving relevant documents for users to explore seems to still be an open problem.
 