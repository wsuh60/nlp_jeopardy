# NLP Vectorization Techniques to Group Jeopardy Questions

Capstone Project for General Assembly Data Science Immersive Program
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/Jeopardy.jpg">

October 22, 2018

# Executive Summary

The objective of my capstone is to group Jeopardy questions based on keywords. Jeopardy questions already have category titles but they are often "tongue-in-cheek" titles that offer contextual clues for answers.
Thus, using cateogry titles to categorize questions is not the most optimal way to study a subject.

Therefore, I wanted to build an unsupervised model that would cluster Jeopardy questions by their true topic based on keywords and topics. People who are training for Jeopardy can optimize their studies
by grouping similar topical questions together instead of relying on category titles provided by Jeopardy. 

Currently, the Doc2Vec model can provide top 10 most similar questions based on the original question the user chooses.
The LDA model groups questions by class and outputs all questions belonging to the same class.

Eventually, instead of using only Jeopardy questions, I have a vision where it can be applied as a general purpose learning tool for students.

# Data Source

The dataset was downloaded as a CSV file from this link: https://drive.google.com/file/d/0BwT5wj_P7BKXUl9tOUJWYzVvUjA/view

The data was originally scraped from a website that archives Jeopardy questions: http://j-archive.com

# EDA

The dataset had 216,930 questions and seven features.
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/Screen%20Shot%202018-10-20%20at%202.41.08%20PM.png">

During the EDA phase, I saw some of the Jeopardy categories were used frequently. Also, the same answers appeared many times as well. All of the popular answers were place names.
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/Question%20Categories.png">
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/Answer%20Distribution.png">

Some of the questions were audio and video clues and I decided to drop those observations. Furthermore, there were two questions that had null values so I dropped those as well.

# Model Evaluation Metric

For evaluating how well my models were clustering questions, I relied on heuristic/qualitative measurements instead of quantiative metrics (like inertia and silhouette score).
Although heuristic evaluation could be subjective to interpretation, I believe it was the best method for evaluating how the Jeopardy questions were clustering together.

# Results

**1) KMeans with 10, 50, and 100 centroids with CountVec and TFIDF feature extraction:**

KMeans clustering did not perform very well. Visually, the clusters were too general with 10 and 50 centroids. I thought 100 centroids might provide better clusters but when I checked the clusters heuristically, there were some questions that were similar but a lot of unrelated questions were present.
Cluster groups were assigned by the nearest centroid by Euclidean distance. 

**2) DBSCAN with CountVec and TFIDF feature extraction:**

DBSCAN performed more poorly than KMeans-- especially when the features were extracted with TFIDF. Even when the epsilon was set really low (.005), the clusterings were unsatisfactory.

**3) Doc2Vec:**

Doc2Vec performed really well (as expected). Using Doc2Vec's '.most_similar' method, I got similarity scores and the index of questions that were most alike to the original question. I then outputed the top 10 most similar questions. It's not perfect but it performs pretty well. I attached an example cluster for your review.

<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/doc2vec.png">

**4) LDA with t-SNE:**

LDA performed surpsingly well too. Visually checking, the clusters were bleeding into each other a little bit so I thought the model wouldn't work as well as Doc2Vec. 
However, after checking the question clusters heuristically, I was surprised by how well some groups related topically.
Some groups clustered better than others (due to repetitive keywords) but overall, I considered it rather successful. 

Keywords:

<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/similar_output.png">

Questions pertaining to pop culture/TV shows:
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/similar_output_pop_culture.png">

LDA visualization:

<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/lda%20tsne.png">

5) **KMeans on top of t-SNE:**

I applied KMeans clustering on top the LDA/t-SNE model above to see if it would cluster better. The model became more transparent but the question clusters began to lose some relevancy.
<img src = "https://git.generalassemb.ly/suhw/NLP_Jeopardy/blob/master/assets/kmeans%20tsne.png">

# Limitations

The Doc2Vec model's major limitation is the relative small size of the corpus. There were only 216,887 questions in the Jeopardy dataset and most documents contained less than 100 words. 
The model would have performed better if there were more questions to train Jeopardy specific vocabulary to the pretrained Doc2Vec library. 

Furthermore, it is hard to visually graph the Doc2Vec model's clusters and it isn't as transparent as KMeans or DBSCAN's clusters.

# Presentation
Please refer to presentation.pptx for further information.

# Next Steps

I want to scrape the Jeopardy archive website myself to get the most up-to-date dataset and train the Doc2Vec model again with a larger corpus. 

Also, I wish to use non-negative matrix factorization (NMF) to see what keywords belong to each topic. This could help aspiring contestants study by using keywords as hints.

# Directory

**Jeopardy.ipynb:**

Note book containing clustering models

**doc2vec_data.txt:**

processed questions that are in a list form to be used with Doc2Vec

**jeopardy.txt:**

Processed questions (using BeautifulSoup, lemmatization, lower-cased, and regex'd)

**JEOPARDY_CSV.csv**

Original CSV file

**assets**

Screenshots of visualizations/datasets

# Sources

<a href = "http://www.emilyinamillion.me/blog/2016/7/13/visualizing-supreme-court-topics-over-time">Visualizing Supreme Court Topics Over Time</a>

<a href = "https://towardsdatascience.com/topic-modeling-and-latent-dirichlet-allocation-in-python-9bf156893c24">LDA Topic Modeling</a>

<a href = "https://medium.com/ml2vec/topic-modeling-is-an-unsupervised-learning-approach-to-clustering-documents-to-discover-topics-fdfbf30e27df">Unsupervised Learning Approach to Clustering Documnets to Discover Topics</a>

<a href = "https://github.com/skipgram/modern-nlp-in-python/blob/master/executable/Modern_NLP_in_Python.ipynb">Modern NLP in Python</a>

<a href = "https://shuaiw.github.io/2016/12/22/topic-modeling-and-tsne-visualzation.html">Topic Modeling and t-SNE Visualization</a>
