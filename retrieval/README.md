# Retrieval 

The retrieval data can be used to build an index for querying ClapNQ in a retrieval setting. It is built using all the passages in the documents from the ClapNQ dataset.

## Notes
 
Some documents are used by more than one question, only one document is kept. In addition, in some cases there were slightly different versions of the same document. We only kept one in such cases and ensured that there was high rouge match between the differing passages if they were a gold passage to a ClapNQ question. All gold answerable passages are included in the corpus.

# Results

Please refer to the paper for baseline retrieval experiments.