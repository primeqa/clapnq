# :clap: CLAP NQ: Cohesive Long-form Answers from Passages in Natural Questions

**[Description](#description) | [Paper](#paper) | [Data](#data) | [Results](#results) | [Contact](#contact)**

Retrieval Augmented Generation (RAG) has become a popular application for large language models. It is preferable that successful RAG systems provide accurate answers that are supported by being grounded in a passage without any hallucinations. While considerable work is required for building a full RAG pipeline, being able to benchmark performance is also necessary.  We present CLAP NQ, a benchmark Long-form Question Answering dataset for the full RAG pipeline. CLAP NQ includes long answers with grounded gold passages from Natural Questions (NQ) and a corpus to perform either retrieval, generation, or the full RAG pipeline. The CLAP NQ answers are *concise*, 3x smaller than the full passage, and *cohesive*, with multiple pieces of the passage that are not contiguous. 

# Description

CLAP NQ is created from the subset of [Natural Questions](https://ai.google.com/research/NaturalQuestions) (NQ) that have a long answer but no short answer. NQ consists of ~380k examples. There are ~30k questions that are long answers without short answers excluding tables and lists. To increases the likelihood of longer answers we only explored ones that have more than 5 sentences in the passage. The subset that was annotated consists of ~12k examples. All examples where cohesion of non-consecutive sentences was required for the answer were annotated a second time. The final dataset is made up of all data that went through two rounds of annotation. (We provide the single round annotations as well - it is only training data) An equal amount of unanswerable questions have also been added from the original NQ train/dev sets. Details about the annotation task and unanswerables can be found [here](annotated_data).

# Paper

The paper is available on Arxiv: 

# Data 

We provide the following data:

Data | Description
--- | ---
[CLAPNQ Annotations](annotated_data)   | The CLAP NQ annotated training and dev data
[Original Documents](original_documents) | The original Wikipedia documents for each CLAP NQ question
[Retrieval](retrieval) | The data in retrieval format for ingesting

## Data Stats

Split | No. Questions | Answerable | Source | Unanswerable | Source
--- | --- | --- | --- | --- | --- 
Train | 3745 | 1954 | NQ Train | 1791 | NQ Train
Dev | 600 | 300 | NQ Train | 300 | NQ Dev 
Test | 601 | 301 | NQ Train + NQ dev (67) | 300 | NQ Dev
Total | 4946 | 2555 | | 2391 | 

Note 1 : All answerable questions are non-consecutive except for 13 questions in the dev set.

Note 2: We include training examples in the dev and test sets because there were only 67 non-consecutive examples in the dev set.  

## Test Data

We are keeping the test data hidden. If you feel you should have access to the test data please contact us with your request.

## Data Format

The data is available in jsonl format, with one question on each line. The output follows the KILT-ELI5 format, with additional metadata. Each line consists of the following:

```
id: original NQ id
input: question
passages: title, text (gold passage), and sentences (gold passages split into sentences)
output:  answer, selected_sentences (used to make the answer. This is a subset of the sentences above), 
meta information: 
  list of annotators who gave the answer, 
  non_consecutive: True if non-consecutive selected sentences, otherwise False.
  round: Whether the annotation is from round 1 or 2 (if both rounds are the same it will say round 2)
```

### Answerable Example

```
{
  "id": "138713725038644067",
  "input": "where does the last name mercado come from",
  "passages": [
    {
      "title": "De Mercado",
      "text": "De Mercado is a Spanish surname . It is believed to have first appeared around the Spanish provinces of Segovia and Valladolid . Its roots are most likely in Old Castile or Andalusia. ( 1 ) Some variants of the name are Mercado , Mercaddo , Meradoo , Mercados , Mercadors , Mercadons , de Mercado , deMercado , Demercado , and DeMercado . The name means ' market ' in Spanish and goes back to Latin mercatus , with the same meaning . Although not a Portuguese surname , the de Mercado name can also be found in Portugal to a limited extent , as it was brought over there from Spain generations ago . Some of the first settlers of this family name or some of its variants were among the early explorers of the New World were many who settled in the Caribbean and Central America . They included Gutierre De Mercado who came to the Spanish Main in 1534 and Gabriel de Mercado who arrived in New Spain in 1578 . The name was brought into England in the wake of the Norman Invasion of 1066 , and Roger Marcand , recorded in the year 1202 in County Berkshire , appears to be the first of the name on record . Roger Mauchaunt appears in Yorkshire in 1219 , and Ranulph le Marchand was documented in 1240 in County Essex . The associated coat of arms is recorded in Cronistas Reyes de Armas de España . The heraldic office dates back to the 16th century . They have judicial powers in matters of nobiliary titles , and also serve as a registration office for pedigrees and grants of arms . ",
      "sentences": [
        "De Mercado is a Spanish surname .",
        "It is believed to have first appeared around the Spanish provinces of Segovia and Valladolid .",
        "Its roots are most likely in Old Castile or Andalusia.",
        "( 1 ) Some variants of the name are Mercado , Mercaddo , Meradoo , Mercados , Mercadors , Mercadons , de Mercado , deMercado , Demercado , and DeMercado .",
        "The name means ' market ' in Spanish and goes back to Latin mercatus , with the same meaning .",
        "Although not a Portuguese surname , the de Mercado name can also be found in Portugal to a limited extent , as it was brought over there from Spain generations ago .",
        "Some of the first settlers of this family name or some of its variants were among the early explorers of the New World were many who settled in the Caribbean and Central America .",
        "They included Gutierre De Mercado who came to the Spanish Main in 1534 and Gabriel de Mercado who arrived in New Spain in 1578 .",
        "The name was brought into England in the wake of the Norman Invasion of 1066 , and Roger Marcand , recorded in the year 1202 in County Berkshire , appears to be the first of the name on record .",
        "Roger Mauchaunt appears in Yorkshire in 1219 , and Ranulph le Marchand was documented in 1240 in County Essex .",
        "The associated coat of arms is recorded in Cronistas Reyes de Armas de España .",
        "The heraldic office dates back to the 16th century .",
        "They have judicial powers in matters of nobiliary titles , and also serve as a registration office for pedigrees and grants of arms ."
      ]
    }
  ],
  "output": [
    {
      "answer": "De Mercado has first appeared around the Spanish provinces of Segovia and Valladolid. Its roots are most likely in Old Castile or Andalusia. The name means ' market ' in Spanish and goes back to Latin mercatus , with the same meaning.",
      "selected_sentences": [
        "It is believed to have first appeared around the Spanish provinces of Segovia and Valladolid .&",
        "Its roots are most likely in Old Castile or Andalusia.&",
        "The name means ' market ' in Spanish and goes back to Latin mercatus , with the same meaning .&"
      ],
      "meta": {
        "annotator": [
          47200615,
          46373812
        ],
        "has_minimal_answer": false,
        "non_consecutive": true,
    	"round": 2
      }
    }
  ]
}
```

### Unanswerable Example
```
{
  "id": 2769033134349167616,
  "input": "where does the hakuna matata symbol come from",
  "passages": [
    {
      "title": "Hakuna matata",
      "text": "In the mid-1980s , the saying appeared in the Swedish comic book Bamse by Rune Andréasson . The first words of Brumma , the baby daughter of Bamse the bear , are `` Hakuna matata '' , which no one understands except the tortoise Skalman . He later made it his and Brumma 's secret motto , and the phrase has reappeared several times in the cartoon . Skalman gave readers several clues as to what language the phrase came from but never said directly that it was Swahili .",
      "sentences": [
        "In the mid-1980s , the saying appeared in the Swedish comic book Bamse by Rune Andréasson .",
        "The first words of Brumma , the baby daughter of Bamse the bear , are `` Hakuna matata '' , which no one understands except the tortoise Skalman .",
        "He later made it his and Brumma 's secret motto , and the phrase has reappeared several times in the cartoon .",
        "Skalman gave readers several clues as to what language the phrase came from but never said directly that it was Swahili ."
      ]
    }
  ],
  "output": [
    {
      "answer": null,
      "meta": null
    }
  ]
}
```

# Results

Please refer to the paper for baseline experiments.

# References 

Natural Questions: a Benchmark for Question Answering Research. Tom Kwiatkowski Jennimaria Palomaki Olivia Redfield Michael Collins Ankur Parikh Chris Alberti Danielle Epstein Illia Polosukhin Matthew Kelcey Jacob Devlin Kenton Lee Kristina N. Toutanova Llion Jones Ming-Wei Chang Andrew Dai Jakob Uszkoreit Quoc Le Slav Petrov. TACL 2019

KILT-ELI5: https://paperswithcode.com/sota/open-domain-question-answering-on-kilt-eli5

# Contact

For questions/issues/usage ideas about this dataset, please post an issue or contact us:

Sara Rosenthal: sjrosenthal@us.ibm.com

Avi Sil: avi@us.ibm.com

# Acknowledgements

I'd like to thank Mohamed Nasr and the annotation team for their effort in annotating the CLAP NQ dataset: Chie Ugumori, Eva Maria Wolfe, Hee Dong Lee, Joekie Gurski, Marina Variano, and Roxana Passaro.

Thank you to Arafat Sultan and Salim Roukos for early annotations during the pilot phase.
