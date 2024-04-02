# Data Files

A description of each data file is provided below. Details regarding the creation and annotation of the dataset follows.

data | split | answerable | round | count
--- | --- | --- | --- | ---
[ClapNQ_train_answerable.jsonl](train/clapnq_train_answerable.jsonl) | train | Y | 2 | 1954
[ClapNQ_train_unanswerable.jsonl](train/clapnq_train_unanswerable.jsonl) | train | N | 2 | 1791
[ClapNQ_train_single_unanswerable.jsonl](train/clapnq_train_single_answerable.jsonl) | train | Y | 1 | 12657
[ClapNQ_dev_answerable.jsonl](dev/clapnq_dev_answerable.jsonl) | dev | Y | 2 | 300
[ClapNQ_dev_unanswerable.jsonl](dev/clapnq_dev_unanswerable.jsonl) | dev | N | 2 | 300

# Data Creation and Annotation

## Original NQ 

Each NQ training example is annotated by one person. The following are stats about the full NQ training set of 307k examples:

{'la': 27027, 'boolean': 3798, 'table': 11778, 'list': 2619, 'sa': 106926, 'na': 155225}

Each NQ dev example is annotated by 5 people. We only explore the examples where the majority of the 5 annotators indicated it was a long answer without a short answer.

## Annotation Task

### Round 1: 

After some initial training and pilots (~100 questions) each of these questions were annotated by one trained annotator, this task is called Single Round 1. 

Single Round 1 Task: The annotators were provided the question, title, and long answer paragraph from NQ divided into sentences. The annotators had to select the sentences relevant to the answer and then write a concise answer in their own words with some copy/pasting allowed. The annotators were instructed to write the answer using the selected sentences and that it should make sense, be concise, and grammatically correct. The question could also be skipped.

### Round 2: 

All answers from round 1 that were made up of two or more selected sentences that were not consecutive (meaning there was at least one unselected sentence between them) were annotated a second time by another annotator. This means that these answers required cohesion. This is called Round 2. These questions were selected as they are more likely to have complex answers. The annotators saw the answer from the first round and could choose to keep the same answer or modify it. Therefore, the second round answers are likely to be of higher quality, however due to human subjectivity both answers could still be good. In some cases the round 2 annotator skipped the question.

## Final Data

The final data consists of all answers that have been annotated by more than one person. The majority of these are the questions with non-consecutive answers from Round 2, but a few are high-quality from the initial pilot rounds. We provide the annotations from both rounds if they were different. The round and type of answer can be found in the meta output format (see below). 

The stats for the dataset that have been annotated are below. These are from the 27k NQ long answers without short answers excluding tables and lists where there are more than 5 sentences in the passage (12k have more than 5 sentences):

## Data stats from original NQ per round

Round 1 (Single Annotation):

Full 12022
Train 11638
Dev 384

Round 2 (Two Annotations each - non-consecutive examples from Round 1):

Full 2391
Train 2458
Dev 67

The round 2 data has been divided into train/dev/test portions. 

## Unanswerable

An equal amount of unanswerable questions have been added from the original NQ train/dev sets (In the NQ training set there is only one annotations, in the NQ dev set all 5 annotators must have said it was unanswerable.). The unanswerable questions were randomly chosen from examples that had more than 5 sentences in the passage by matching the first word distribution of ClapNQ (e.g. 26.6% "What", 20.5% "Where" etc… In contrast "Who" is the most common first word in NQ no answers (19.1%) and short extractive answers (35.6%). We randomly choose a passage from the document as the candidate passage that is > 5 sentences and <=30 sentences as unanswerable questions to not have associated passages.
