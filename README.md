
# Semantic NLP Filtering for Deep Learning Research in Virology and Epidemiology

This project is focused on filtering, categorizing, and extracting information from research papers that apply deep learning in the fields of virology and epidemiology, using the data [(Virology and Epidemiology AI Research Collection)](https://github.com/jd-coderepos/virology-ai-papers/). Through a series of NLP tasks, I identify relevant papers, classify the methods they use, and extract specific details about these methods. This README provides an overview of each task, along with the approaches and models I used.

---

## Project Overview

The project includes three main tasks:
1. **Filtering**: Identifying papers that focus on deep learning in virology or epidemiology.
2. **Classification**: Categorizing the methods used in the relevant papers.
3. **Extraction**: Extracting the specific method names mentioned in each paper.

Each task takes as input a combination of each paper’s **title** and **abstract** (the paper’s summary), concatenated into a single text body.

---

## Task 1: Filtering Out Irrelevant Papers

In Task 1, I filter out papers that don’t discuss **deep learning** applications in **virology** or **epidemiology**.

### Approaches

#### Approach 1: Zero-Shot Classification

- **Method**: I used zero-shot classification to identify relevant papers.
- **Model**: I chose [**MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli**](https://huggingface.co/MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli), a model with 184 million parameters, which can handle multi-label classification.
- **Labels and Filtering Criteria**:
  - I used the following labels:
    - **"deep learning in virology"**
    - **"deep learning in epidemiology"**
  - Each paper received a score for these labels. Papers with a score of **0.5 or higher** for either label were considered relevant, while those scoring below were excluded.

#### Approach 2: Semantic Similarity

- **Method**: I applied **semantic similarity** using [**Sentence Transformers**](https://sbert.net/) to compare each paper’s title and abstract to a search query.
- **Model used**: I used the [**msmarco-distilbert-base-v4**](sentence-transformers/msmarco-distilbert-base-v4) model, which is trained to find passages relevant to a search query, such as keywords or a search phrase.
- **Criteria**: Papers with a similarity score above a set threshold were kept.
- **Advantages**:
  - Captures variations in wording, making it more adaptable to different phrasing.
  - Fast, with embeddings that can be reused across different queries.
- **Limitations**:
  - The similarity threshold may require adjustment, as it can vary based on text length.
- **Improvements**:
  - I split the text into sentences and scored each sentence individually against the query, then assigned the highest score to the paper.
  - This approach is more accurate than scoring the entire text block but is more memory-intensive.

---

## Task 2: Classifying Methods in Relevant Papers

This task involves categorizing the relevant papers (from Task 1) by the type of deep learning method they use. The categories are:
- **Text mining**
- **Computer vision**
- **Both**
- **Other**

### Approach

- **Multi-Label Classification**: I used the same [**MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli**](https://huggingface.co/MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli) model for multi-label classification.
- **Labeling Criteria**:
  - The labels used were:
    - **"computer vision"**
    - **"text mining"**
  - Papers with a score of **0.5 or higher** for both labels were categorized as **"both"**. Papers scoring above **0.5** for only one label were categorized as either **"computer vision"** or **"text mining"**. Papers that didn’t meet these scores were labeled **"other"**.

---

## Task 3: Extracting Method Names

In this final task, I extract the specific deep learning methods mentioned in each relevant paper.

### Approach

- **Extractive Question Answering (QA)**: For this task, an extractive QA model was used because it is well-suited for extracting text directly from the input rather than generating new content.
- **Model**: I used [**deepset/roberta-base-squad2**](https://huggingface.co/deepset/roberta-base-squad2), a relatively lightweight QA model with 124 million parameters.
- **Question Prompt**: The model was prompted with the question **"What method is used in this research paper?"**, with the combined title and abstract as input.

---

## Summary of Techniques Used

- **Zero-Shot Text Classification**: Enables classification without labeled data, offering a better understanding than simple keyword matching.
- **Semantic Similarity Search**: Helps find relevant papers by recognizing similar meanings even when words differ, improving accuracy.
- **Extractive QA**: Ensures accurate method names are extracted directly from the text, rather than generating new information.

---

## Key Advantages of the Approach

This approach uses advanced NLP techniques to go beyond simple keyword matching, offering:

1. **Zero-Shot Text Classification**: Quickly classifies papers without pre-labeled data, saving time on manual labeling.
2. **Semantic Similarity Search**: Recognizes different wordings, making it more effective at finding relevant papers.
3. **Extractive QA**: Reliably extracts specific method names from text, which increases accuracy.

## Statistics
### Zero-Shot Classification Approach
| Relevant Papers  | Text Mining | Computer Vision | Both | Other |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 5832  | 2657  | 239  | 918  | 2018 |

### Semantic Similarity (sentences) Approach 
| Relevant Papers  | Text Mining | Computer Vision | Both | Other |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 4959  | 1333  | 861  | 1211  | 1554 |

