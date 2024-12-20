# NLP
Intelligent Product Search Engine
## Objective 
To develop an intelligent e-commerce search engine leveraging autocorrection, predictive text, and semantic search to enhance product search, user experience, and business outcomes.

## Problem Statement
This project addresses the inefficiencies of traditional e-commerce search engines that rely on exact keyword matching, often failing with queries containing typos, vague descriptions, or mismatched terms. Such limitations result in poor user experiences, product discovery issues, and significant customer attrition. For instance, users searching for "Taiwanese beverage" instead of "bubble tea" often encounter irrelevant or no results. This challenge impacts customer satisfaction and sales.
The goal is to optimize search engines for platforms like Amazon using Natural Language Processing (NLP) techniques. Data for this project comes from the Amazon product dataset by McAuley Lab, characterized by high dimensionality and semi- structured data.

## Success Measures 
Planned metrics include:
• Accuracy in autocorrection, next-word prediction, and search relevance.
• Efficiency in response time and user query resolution.
• Evaluation metrics like cosine similarity scores and Mean Reciprocal Rank
(MRR).

## Background
Poor search functionality in e-commerce affects customer retention, with studies revealing 88% of users abandon websites after a bad search experience. Improved search capabilities like typo correction, semantic matching, and predictive text enhance user satisfaction, reduce frustration, and boost sales. Modern approaches integrate category-based learning and contextual analysis for personalization. Existing work in semantic search engines and NLP advancements like LSTM, Sentence Transformers, and ReLU forms the foundation for this project.

## Impact
This intelligent search engine could transform product search in e-commerce platforms, making it more accurate, user-friendly, and adaptable. Key impacts include:
• Enhanced User Experience: Autocorrection and semantic understanding reduce frustration from typos or incomplete queries.
• Improved Accuracy: LSTM and Sentence Transformers enable context-aware, precise results in extensive datasets.
• Scalability Across Domains: While focused on product search, the approach can extend to healthcare, legal, or academic domains.
• Reduced Manual Effort: The system interprets user intent, eliminating the need for repeated query refinement.
This innovation can redefine e-commerce and other data-intensive fields.

## High Level Design
<img width="432" alt="image" src="https://github.com/user-attachments/assets/8fa61584-4324-4db8-bcaf-71a283f0488d" />

## Methods Explored
In this project, several methods were explored to address the task of next-word prediction and semantic search. These methods included:
Dataset and Selection Process:
The Amazon Reviews dataset [4] from McAuley Lab at UC San Diego was utilized to address challenges in autocorrection, next-word prediction, and semantic search. Due to computational constraints, the Beauty category, comprising 112.6K items, was selected from the 48.19 million items in the dataset. Product titles were extracted to construct a vocabulary for linguistic modeling, demonstrating scalability across other product categories.
Data Pre-processing: The raw data underwent preprocessing to enhance model quality. Common stop-words were removed to eliminate irrelevant terms. A frequency threshold filter discarded overly common words (e.g., "color"). Lemmatization ensured unification of word variations into base forms, improving generalization. Words shorter than three characters and non-alphabetic tokens were excluded. This normalized dataset reduced noise, ensured semantic consistency, and enhanced the models’ ability to understand linguistic patterns.
### Autocorrection System:
The objective was to develop an autocorrection system capable of identifying and correcting misspellings using Levenshtein Distance and morphological transformations to determine the closest match from a vocabulary.
Lemmatization:
Words were normalized to their base forms using a lemmatizer, ensuring variations such as "brushes" and "brush" were unified into root forms.
Plural Handling:
To improve coverage, singular and plural variations were considered. Plural forms were generated by appending 's' to words that did not already include the suffix.
Levenshtein Distance Calculation:
The Levenshtein Distance metric was applied to calculate the minimum number of edits (insertions, deletions, or substitutions) required to transform a misspelled word into a valid vocabulary match.
The system was evaluated by introducing random modifications to a subset of the dataset to simulate errors. Corrected words were compared against actual targets to determine accuracy. 

For instance:
• The erroneous input “lubstuck” was corrected to “lipstick” with an edit distance of 3.

<img width="432" alt="image" src="https://github.com/user-attachments/assets/7bfc1ae5-a7fe-4db5-a199-bc394dbfe4aa" />

• Similarly, “shumpow” was corrected to “shampoo” with an edit distance of 2.

<img width="432" alt="image" src="https://github.com/user-attachments/assets/3065c9eb-52a5-4214-a167-c88f05ce8f6b" />

Data Preparation:
1. Vocabulary Generation:
A vocabulary list was extracted, ensuring words were alphabetic and converted to lowercase.
2. Test Dataset Creation:
A random subset of 100 words was sampled.
Each word was modified using operations such as:
Insertion: A random character inserted at a random position. § Deletion: Removal of a character at a random position.
Replacement: Substitution of a character with a random
alternative.
  
The resulting misspelled words were paired with their correct counterparts to form the test dataset.
Evaluation Metric:
System performance was evaluated based on prediction accuracy, calculated as: Accuracy = Number of correct predictions / Total number of words in test dataset 

### Next Word Prediction with LSTM:
This section outlines the process of building and training a next-word prediction model using LSTM (Long Short-Term Memory) neural networks. The model predicts the next word in a sequence based on a dataset of product titles.

Data Preparation:

A random sample of 10,000 titles was selected to ensure computational efficiency. On average, each title contained 7 words, making large-scale n-gram generation resource-intensive. Titles were tokenized into words, and nouns (POS tags: NN, NNS, NNP, NNPS) were filtered, ensuring they were alphabetic. Nouns were chosen due to their predictive and contextual importance in product titles. The resulting tokens were joined into a sentence-like corpus for tokenization.

Tokenization and Sequence Generation:

The corpus was tokenized by converting words into unique integer indices, forming a vocabulary with word-to-index mappings. N-gram sequences of increasing lengths were generated, and zero padding was applied to standardize sequence lengths for training.

Model Architecture and Training:

Input sequences (x) and corresponding next-word labels (y) were separated, with labels one-hot encoded into categorical format. The architecture consisted of:
• Embedding Layer: Maps word indices to 100-dimensional vectors.
• LSTM Layer: Captures long-term dependencies in sequences.
• Dense Layer: Outputs a probability distribution over the vocabulary.
• Activation: Softmax for probability normalization.

The model was compiled using categorical cross-entropy as the loss function and Adam as the optimizer. Accuracy served as the performance metric. Training was conducted over 50 epochs with verbose output for progress monitoring, enabling the model to learn patterns effectively.

For the input string “shave”, the predicted next word was “cream”.

<img width="394" alt="image" src="https://github.com/user-attachments/assets/40046377-7d86-446f-93bc-98fc5fe57f19" />

For the input string “face”, next word is predicted as “mask”.

<img width="432" alt="image" src="https://github.com/user-attachments/assets/a1414e5f-b0b1-45ed-8e07-6047984f9a12" />

Design of the next word prediction model

The model architecture is designed to predict the next word in a sequence using a combination of embedding, recurrent, and dense layers. It begins with an Embedding Layer that converts each word in the input sequence into a 100-dimensional vector, capturing semantic relationships. This dense representation is passed into an LSTM Layer with 150 units, which effectively processes sequential data by capturing long- term dependencies and contextual information. The output from the LSTM is fed into a Dense Layer with a size equal to the vocabulary, using a SoftMax activation to generate a probability distribution over possible next word. The model is compiled with categorical cross-entropy as the loss function to measure prediction accuracy and Adam as the optimizer for efficient learning. This architecture enables the model to learn complex patterns in sequences and make contextually appropriate word predictions.

<img width="432" alt="image" src="https://github.com/user-attachments/assets/916bfb1a-7696-4f3e-ba75-31669cd62215" />

### Semantic Search with SBERT:

Semantic search enables a system to move beyond exact keyword matching by understanding the context and meaning of queries. Traditional search techniques often rely on exact or partial matches, which may exclude contextually similar results. Semantic search focuses on identifying the intent of a query and retrieving conceptually relevant results.
In this implementation, semantic search is applied to product titles, where queries are matched to titles based on their semantic meaning rather than exact keyword overlaps.
Sentence-BERT (SBERT) Embeddings

Sentence-BERT (SBERT) generates sentence embeddings that capture the semantic meaning of entire sentences. Unlike BERT, which focuses on word-level contexts, SBERT adapts BERT to produce fixed-size vector representations. These embeddings allow similarity measurements between sentences.
The input text is passed through a pre-trained transformer to create a vector representation. For efficiency, the all-MiniLM-L6-v2 model, part of the sentence- transformers library, is utilized. It is compact and optimized for large-scale data like product catalogs or queries without significant performance loss.

Dense Vector Representations

Dense vector representations are compact numerical vectors that encapsulate semantic meaning. Unlike sparse vectors (e.g., one-hot encoding), dense vectors are continuous- valued, enabling better comparisons in high-dimensional spaces. Each dimension captures a different semantic aspect, and the relative positioning of vectors determines similarity. SBERT-generated dense vectors allow efficient comparison and retrieval of semantically similar items.
Retrieving Top-N Titles

Once product titles are converted into dense vectors, they are compared with the query vector to identify the most similar products. Cosine similarity is used as the metric to rank and retrieve the top-N relevant titles.

<img width="326" alt="image" src="https://github.com/user-attachments/assets/7de0e562-7acb-4322-8f30-edb527b21c17" />

### Parameter Choices and Justification

• In the model, the LSTM layer with 150 units performed the best as it likely provided enough neurons to capture complex patterns and long-term dependencies in the data. LSTM units are specifically designed to retain information over extended sequences, making them ideal for tasks like next-word prediction. The higher number of units (150) likely offered a better capacity for learning the intricacies of the dataset, while a smaller number of units (e.g., 100) might have been too restrictive to capture the necessary patterns, leading to lower accuracy.

• The final Dense layer with a SoftMax activation function is crucial for transforming the output of the LSTM layer into a probability distribution over the vocabulary. The SoftMax function ensures that the sum of the predicted probabilities equals one, with each output representing the likelihood of a particular word being the next in the sequence. This probabilistic output is essential for making the model’s predictions interpretable and suitable for tasks such as next-word prediction, where the model must choose the most likely word from the vocabulary.

• To optimize the model, categorical cross-entropy was chosen as the loss function. This loss function is well-suited for multi-class classification tasks, like predicting the next word in a sequence, as it compares the predicted probability distribution with the actual label (the correct next word). Using Adam as the optimizer with a learning rate of 0.01 allowed for adaptive learning, helping the model adjust its weights efficiently during training. Adam is particularly effective for handling noisy data and sparse gradients, making it an appropriate choice for this task.

• The all-MiniLM-L6-v2 model from the sentence-transformers library was selected for its balance between efficiency and performance. This compact model is optimized to generate dense, high-quality sentence embeddings while preserving semantic meaning. It is designed to produce fixed-size vector representations, enabling effective comparison and retrieval of semantically similar product titles. Its lightweight architecture ensures reliable performance, even when processing large- scale datasets such as e-commerce product catalogs. This efficiency is particularly beneficial for real-time applications, including search engines and recommendation systems.

### Methods That Didn't Work

• Bidirectional LSTM (Bi-LSTM): While the Bi-LSTM model provided good accuracy in terms of training performance, it was unable to predict the next word as expected. The model produced outputs that were close to NaN (Not a Number), indicating issues with the model's ability to generalize for next-word prediction. The Bi-LSTM captures context from both directions (past and future), which may have disrupted the learning for next-word prediction and led to overfitting. This behavior helped to understand that bidirectional LSTM may not work as intended for next work prediction tasks.

• N-gram Model: An n-gram model was also tested for word prediction, but it struggled with sparsity issues, especially with a large vocabulary. Despite attempts to optimize smoothing techniques, the n-gram model couldn't provide meaningful predictions compared to the LSTM model, which better captured the context and dependencies between words.

• Fine-tuning was performed to adapt the pre-trained SBERT model (all- MiniLM-L6-v2) to improve product title relevance. Sentence pairs, along with their similarity scores, were used as input. These pairs were converted into training examples, and the model was trained for one epoch using the CosineSimilarityLoss function. A batch size of 16 and 100 warmup steps were used to stabilize learning. During training, the loss decreased steadily over the steps reaching 0.147500, indicating effective learning. Although cosine similarity scores for retrieved results were below 0.70, the outputs remained semantically relevant to the queries. This observation highlights that cosine similarity, as an evaluation metric, may not fully align with human judgment.

### Results
#### Autocorrection
The autocorrection system was evaluated by introducing random misspellings into a subset of words from the dataset. Each misspelled word was paired with its correct version, and the model was assessed on its ability to correct these errors. The system achieved an accuracy of 82%, demonstrating its robustness in handling real-world noisy input and various types of spelling mistakes.

<img width="229" alt="image" src="https://github.com/user-attachments/assets/c087aef2-b2ea-458b-a8b5-f5734c085321" />

#### Next Word Prediction
The next-word prediction model showed consistent improvement throughout the training process. 

Loss Trend:
            The next-word prediction model demonstrated consistent learning progress, with the loss decreasing steadily from an initial value of 6.53 in the first epoch to 0.58 in the final epoch. This decrease in loss indicates that the model's predictions became more accurate over time by adjusting its internal parameters during training.

<img width="208" alt="image" src="https://github.com/user-attachments/assets/52596889-2698-457d-b7c8-66bda5a52652" />

Accuracy Trend:
             The model's accuracy increased progressively from a starting point of 8.02% in epoch 1 to a final accuracy of 84.75% by epoch 50. This indicates a steady improvement in the model’s ability to correctly predict outputs over time, demonstrating effective learning.

<img width="201" alt="image" src="https://github.com/user-attachments/assets/09e3d74f-786b-45ef-b6f5-0a61d3d44e5c" />

#### Semantic Search
The performance of the semantic search was evaluated using Cosine Similarity, Precision at K, and Mean Reciprocal Rank (MRR).

Cosine Similarity: This metric measures the similarity between product titles and search queries. The model achieved an average cosine similarity score of 0.712, indicating good relevance in the retrieved results based on contextual similarity rather than exact keyword matching.

<img width="241" alt="image" src="https://github.com/user-attachments/assets/308b761c-4d6a-464d-ae2e-c7464018e993" />








