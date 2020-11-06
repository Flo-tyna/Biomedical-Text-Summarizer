# Biomedical-Text-Summarizer
A biomedical text summarizer to extract knowledge from a full-text journal article and generate a summary.
\n
#### Objectives:
●	Build an extractive summarizer 
●	Employ Unsupervised Machine Learning
●	Use Word biomedical word embeddings
●	Evaluate model using Rouge-N

### Background:
The Whitehouse and global teams of leading researchers prepared the COVID-19 Open Research Dataset (CORD-19). It's curated and maintained by the Semantic Scholar team at the Allen Institute for AI to support text mining and NLP research. This dataset contains over 200,000 scholarly articles, including over 100,000 with full text, about COVID-19, SARS-CoV-2, and related coronaviruses. The intent of this dataset is to crowdsource answers against the Covid-19 pandemic.
Data Source:
A subset of data- the bioRxiv was selected from tdocument_parses.tar.gz: A collection of JSON files that contain full text parses of a subset of CORD-19 papers. The biorxiv folder contains 88,000 articles. But I have chosen only the first 600 articles for this project. The Schema of the biorxiv dataset is as follows:
Data Dictionary
- cord_uid	The unique CORD 19 User Identification number assigned to the article	(object)
- title	The title of the article	(object)
- Pmcid	The PMC article unique identification number	(object)
- Abstract	The abstract of the article	(object)
- Authors	The name of the article's authors	(object)
- Journal	The name of the journal, the article was published in	(object)
- Text_body	The article without the abstract, reference and acknowledgement section.	(object)

### Exploratory Data Analysis:
The EDA phase consisted of converting 600 json files into a nested dictionary and finally converting it into a single csv file. Next, the topics across the 600 articles were explored through a topic-modelling process. Finally, feature engineering was used to determine the word count of articles and abstracts, the distribution of the unique words in the text and the reading time was also determined. For a detailed account of the EDA, please refer the jupyter notebook (01_EDA).
Ten broad topic areas were created via topic modelling. Most papers are between 3000-5000 words in length. 98% of the papers are under 10,000 words in length. Similarly, 98% of the papers have ~2500 unique words. It takes 15 minutes to read the average article. Reading time ranges from 3 minutes to almost an hour. 
Data Modelling and Evaluation:
After EDA was completed, 5 summarization modules were tested to determine the best module and algorithm type for summarization. The modules were TextRank-Gensim, NLTK, Gensim-Summarizer, Spacy and BertSum. The working of all the models involved this generalized process model 
•	Splitting the document into sentences
•	Pre-processing each sentence to remove noise. This includes tokenization, stop word removal, and lemmatization
•	Assigning algorithmic specific score to each sentence
•	Sorting the sentences in descending order of the score to retain the top n, where n is a configurable parameter.
•	combining the top n sentences to form the summary. All these algorithms follow the steps mentioned above. However, the scoring logic is different for each algorithm.
### TextRank :
A graph-based ranking model for text processing was used in order to find the most relevant sentences in text and also to find keywords. This model possessed the lowest Rouge metrics because it was not customizable with the word embeddings. 
### Gensim-summarization.summarizer: 
This module was less cumbersome than TextRank. It is a modified version of the text TextRank algorithm. The rouge metrics were much higher than the text rank. However, the process lacked transparency despite the ease of execution. This module has built in capability to generate summaries according to the wordcount and the ratio of the original text. But, just like Text Rank, I was unable to use biomedical word embeddings.
### Spacy : 
A relatively new package for “Industrial strength NLP in Python” developed by Matt Honnibal at explosion.ai. It does not weigh the user down with decisions over what esoteric algorithms to use for common tasks and it’s fast. Incredibly fast (it’s implemented in Cython). It’s reasonably low-level, but very intuitive and performed well.” - Hackermoon
scispaCy en_core_sci_md models are trained on data from a variety of biomedical sources. These embeddings were used in the Spacy model. The speed at which these summaries were processed were the fastest among all the modules tested and its Rouge metrics ranked second. 
### BERT Sum
BERT, which stands for Bidirectional Encoder Representations from Transformers. Bert doesn’t require text preprocessing like tokenization, lemmatization and stop word removal, unlike other modules. BERT is designed to pre-train deep bidirectional representations from unlabeled text by jointly conditioning on both left and right context in all layers. Thus, the pre-trained BERT model can be fine-tuned with just one additional output layer to create state-of-the-art models for summarization, without substantial task-specific architecture modifications. Without the addition of Bio bert embeddings, the model performed well and had the highest rouge metrics BERT’s superior performance is because of its Parallelizing Computation, Reduced number of operations, Long-range dependencies. The usage of this summarization model with SciBert embeddings has yet to be explored. The model works well without any embeddings and has the highest Rouge scores so far. 
SciBert: Allen AI's SciBert has been trained on 1.14 million research papers (18% in the computer science domain, 82% in the biomedical domain), I felt it is the best set of starting weights for this project. Unfortunately, I wasn’t able to implement it due to time constraints. However, I plan on implementing it by demo day.
### Evaluation: Recall-Oriented Understudy for Gisting Evaluation -Rouge
The model was evaluated via the Rouge recall metric. It is measure on the comparison between the machine-generated output and the reference output based on n-grams. An n-gram is a contiguous sequence of n items from a given sample of text or speech. I used the Unigrams and Bigrams score to evaluate the models

The four modules varied in terms of their performance and I found that Spacy modules with Sci Spacy word embeddings were the easiest to implement. Bert was my second favourite because of the amount of computing power it required. The implementation of the BioBert embeddings were not straightforward and easy to use, like the Spacy modules. A reason for this might be the lack of awareness among the bioinformatics community. 

### Findings and Conclusions:
#### Application Summary
Biomedical information in scientific journals are increasing at a rapid pace due to the increase in electronic communication.Currently, three thousand articles are produce everyday and this figure has been projected to double over the next five years. In order to cope with the increasing publications, a summarization tool is the need of the hour. Summarization in the bio-medical field has to be bespoke and retain key information from the original text. This customizable summarization tool will be extremely useful as it will increase productivity and efficiency in the workplace. I have designed this summarization tool with the intent of reducing reading time and producing concise summaries.
#### Future Work:
Many people in the pharma and med-tech industry are faced with the tedious daily task of synthesizing and summarizing biomedical texts from multiple sources. For future work, I intend on building an app to automate this task. Initially, I have embedded the BioBert embeddings in order to fine tune the Bert summary model. The Spacy model works well. However, I have to find a way to automate the process so that I can evaluate at least 100 articles. Once I evaluate and optimize its performance, I will build a streamlit app which uses Spacy and Bert Summaries to summarize full text articles with Name entity recognition. 

