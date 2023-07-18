## Element-aware Summarization with Large Language Models: Expert-aligned Evaluation and Chain-of-Thought Method  (ACL'23 Long Paper)
![ACL 2023](https://img.shields.io/badge/ACL-2023-red.svg?style=plastic)
![python 3.6](https://img.shields.io/badge/python-3.6-royalblue.svg?style=plastic)
![license Apache-2.0](https://img.shields.io/badge/license-Apache%202.0-inactive.svg?style=plastic)

As an AI language model, I am not the author of the paper and do not have access to the specific guiding question generation prompt templates used in the SumCoT technique. However, I can provide some general examples of how guiding question templates might be constructed based on the core elements of a news article.

For example, if the core elements extracted from an article include entity, date, event, and result, some possible guiding question templates might include:

- "Who is/are [entity]?"
- "When did [event] happen?"
- "What is [event]?"
- "What are the implications of [result]?"

These templates can be filled in with the specific values of the extracted elements to generate context-aware guiding questions that are relevant to the article being summarized. For example, if the entity is "the research team" and the event is "a new scientific discovery," some possible guiding questions might include:

- "Who is the research team?"
- "When did the new scientific discovery happen?"
- "What is the new scientific discovery?"
- "What are the implications of the new scientific discovery?"

These are just examples, and the specific guiding question templates used in SumCoT may vary depending on the context and the specific elements extracted from the article.

Sure! Here's an example of how SumCoT works:

Suppose we have a news article about a new scientific discovery. The first step in SumCoT is to extract the core elements of the article, such as the entity (e.g., the research team), the date (e.g., when the discovery was made), the event (e.g., the discovery itself), and the result (e.g., the implications of the discovery).

Next, SumCoT generates a series of guiding questions based on these elements, such as "Who made the discovery?" and "What are the implications of the discovery?" These questions are used to guide the LLM in generating a summary of the article.

The LLM then generates a response to each question, using its knowledge of language and its understanding of the article's content. These responses are combined to form a coherent summary of the article that focuses on its core elements.

In this way, SumCoT helps LLMs generate more element-aware summaries that are objective, comprehensive, and professional. It also improves their performance in terms of both automatic and human evaluation metrics.


<font color=green size=4><i>**Let's elicit LLMs summarize step by step following the professional communication theory!**</i></font>

In this work, you can use directly or get inspired by:

- A fine-grained generic summary data annotation protocol (combining micro and macro demands)

- An expert-aligned generic summary test set (rewrite [*CNN/DailyMail*](https://paperswithcode.com/dataset/cnn-daily-mail-1) and [*BBC XSum*](https://paperswithcode.com/dataset/xsum))

- An expandable CoT-based open-end generation path (not only *SumCoT*)



---


## Element-aware Dataset

### Annotation Statement

Our annotation protocol is mainly based on [*Lasswell Communication Model*](https://en.wikipedia.org/wiki/Lasswell%27s_model_of_communication) --- a famous communication theory proposed by Lasswell(1948). Additionally, we removed as much noise as possible from the original data set and performed data analysis (See paper for more details). 

Case comparisons for our Element-aware summary and original dataset-specific summary:

<div align=center>
<img src="./assets/Dataset.png" width="60%" align="center"></img>
</div>

### Data Usage

- See our constructed datasets in ```/data```. The two ```json``` files hold the data of *CNN/DailyMail* and *BBC/XSum*, respectively.

- Each ```json``` file contains 200 samples, each containing the **source document**, **original summary**, **element-aware summary**, **GPT-3 summary (standard)**, and **GPT-3 summary (under SumCoT)**. You can use these data for more fine-grained evaluation.

Dataset format:

```json
{
  "dataset_name": [
        {
        "id": 0,
        "src": "xxx",
        "original_summary": "xxx",
        "element-aware_summary": "xxx",
        "gpt3_summary": "xxx",
        "gpt3_cot_summary": "xxx"
        },
        {
        "id": 1,
        "src": "xxx",
        "original_summary": "xxx",
        "element-aware_summary": "xxx",
        "gpt3_summary": "xxx",
        "gpt3_cot_summary": "xxx"
        },
        ...
        {
        "id": 199,
        "src": "xxx",
        "original_summary": "xxx",
        "element-aware_summary": "xxx",
        "gpt3_summary": "xxx",
        "gpt3_cot_summary": "xxx"
        }
    ]
}
```

Data will be released soon...

## Summary Chain-of-Thought (SumCoT)

### Pipeline

<div align=center>
<img src="./assets/Method.png" width="80%">
</div>

### Code Usage

#### generation
If you want to generate summaries (w/ or w/o SumCoT) by GPT-3, run ```generation.py```:

```shell
python generation.py \
--dataset cnndm
--start_id 0
--end_id 199
```

where ```cot_true``` indicates if you want to use the SumCoT technique for generation, ```start_id``` and ```end_id``` determine the range of source documents in the dataset that you want to request. If you only want to generate the summary of the $a$-th sample, set ```start_id=end_id=a```.

To facilitate the subsequent evaluation, the summaries just generated will be stored into ```$dataset_name$_output.json```.

*Note: Don't forget to modify your ```openai_key``` in the Decoder().*

#### evaluation

We also provide code in ```/evaluation/eva.py``` to reproduce the results:

```shell
python eva.py \
--dataset cnndm \
--start_id 0 \
--end_id 199 \
--bs_true false
```

where ```bs_true``` indicates if you want to use the bert score, ```start_id``` and ```end_id``` are the same as above.

You can also build your own dataset ```json``` file, adjust ```start_id``` and ```end_id``` to perform your own evaluation.

## Citation

```
@article{wang2023element,
    title={Element-aware Summarization with Large Language Models: Expert-aligned Evaluation and Chain-of-Thought Method},
    author={Wang, Yiming and Zhang, Zhuosheng and Wang, Rui},
    journal={arXiv preprint arXiv:2305.13412},
    year={2023}
}
```
