# Context in Natural Language Processing

This project aims to answer a question that I have: how do LLMs know the context in the questions that users ask? To answer the question, I first had to understand the underlying knowledge where the words are first transformed to numerical representations called **tokens**. Thus, this led to the project being divided into the following phases:
1. Text Representation
2. Static Word Embeddings
3. Contextual Embeddings
4. Building Self-Attention from Scratch
5. 

## **1. Text Representation**
In this phase we seek to answer the question: How can text be converted to numerical representations that allow a computer to compare documents?

First, we loaded the sentences that we wanted to use. The sentences represent the documents that are usually loaded to LLMs. Next, we split the sentences into tokens which are the bite-sized units of data that a large language model (LLM) reads and generates. I then built a vocabulary from the generated tokens in order to build a corpus of unique words that are occuring in our sentences.

Using the corpus, we assign a token(number) to each of the words and then create a **Bag of Words** which converts raw text into numerical vectors by recording word counts while completely ignoring word order, grammar and sentence structure. In this, the presence of a word is represented by a 1 while an absence is represented by a 0. 

Since the Bag of Words treats all words as having equal importance, we now implement **Term Frequency - Inverse Document Frequency (TF-IDF).** TF-IDF improves BoW by assigning higher importance to words that occur frequently in a document but rarely across the entire corpus. For example, "The man visited the bank", in this sentence "the" is assigned lesser importance than "bank" because it occurs frequently compared to "bank".

We also use **cosine similarity** which is used to quantify semantic similarity between documents. It is calculated by measuring the angle between vectors. If the value is close to 1, then the documents are very similar while if the value is close to 0, then the documents are not similar.

The following were the findings in this phase. We can:
- Convert text into numerical vectors.
- Measure word frequency
- Identify important words
- Compare documents using mathematical similarity

The limitations were as follows:

- They do not understand semantic 
- They treat identical words as identical concepts
- They ignore deeper context
- They struggle with synonyms
- They cannot distinguish different meanings of ambiguous words. 
*For example, **"The bank approved the loan"** and **"They sat on the bank of the river"**, both contain the same token "bank", but TF-IDF cannot determine that the word has different meanings in each context.*

So in this phase we understand that text is converted to numerical representations by converting the words into tokens, creating a Bag of Words, measuring word frequency and comaring mathematical similarity.

## **2. Static Word Embeddings**
In this phase, we seek to answer the question how can a computer represent the meaning/relationships of words as vectors?

An embedding is a vector representing an object, in this case, a word. Here, the model learns numerical representations that are useful for predicting relationships in language. The embeddings are better than a Bag of Words because in a BoW, the vectors do not tell us that a cat is equally closer to a dog, rather they are different positins in a vocabulary. With embeddings, we get different vectors and that way a cat and a dog might be mathematically closer.

For this, we will use **GloVe embeddings**(Global Vectors for Word Representation). We realise that using static embeddings, each word is assigned a single representation regardless of the sentence.

Visualising the embedding space by reducing the 50 dimensions that we used for our model to 2 dimensions using PCA(Principal Component Analysis), we see groups emerging with animals clustering around the same region and vehicles in another cluster.

We can also see that the embedding model captures some relationships. For example, querying the model for a female version of the king and we get the queen. This is a demonstration of **vector semantics**; they represent word meanings as points(vectors) in a multidimensional space, where words with similar meanings or contexts are located close to each other. This simply means that the training process has produced a vector space where certain linguistic relationships can be represented approximately through geometric relationships.

So in this phase, we find that we can represent words as embeddings and that with GloVe we can see that a word has the exact same vector regardless of the sentence meaning because the model refers to the same vector for both sentences.

## **3. Contextual Embeddings**
In this phase, we want to answer how can we make the representation of a word change depending on the words around it?

In the previous phase, we were using static embeddings but we realised that they have a fundamental problem; they assign only one vector to a word regardless of the context. For example, bank is assigned one vector regardless of whether we are talking about money or a river.

We will use **Bidirectional Encoder Representations from Transformers**(BERT) which reads and analyzes words in asentence from both the left and right sides simultaneously, capturing deep contextual meanings, that is, distinguishing the different meanings of the word based on surrounding text. In this, every token receives a 768-dimensional contextual representation.

For this experiment, we will use two sentences: "The bank approved my loan." and "The fisherman sat on the bank of the river." The word we are investigationg is bank. We will then calculate the cosine similarity and compare the two vectors. The number is closer to zero meaning that they are unrelated but the similarity value is not extremely low because they are still the same word, so BERT's representation will retain some similarity. This proves that BERT captures the surrounding context which influences the representation.

So in this phase we learn that BERT accomplishes creating contextual embeddings by considering the surrounding tokens, so its representation incorporates contextual information. Think of its working as "a token representation together with contextual information to get a new representation."

