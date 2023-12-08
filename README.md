# LLM-personalize-tests

DATA : 
- Data is stored at data-master/gvirgink/BGLobe-filtered.
- All data is stored in a file called combined_data_80k.tsv.
- First 40k entries are used for training, next 20k for validation, final 20k for test.
- First 3 cells in Encoder-BGlobe perform data processing.
- Sentence-transformer model is run on article headlines before model forward pass to save time.

MODEL : 
- Trained with a batch size of 128
- Data is aggregated at the user level in the training step.
- Following steps are run for each time value in the time series and summed together. 
- Each user's data is passed individually into the forward pass of the model to generate their embeddings.
- User embeddings, positive embeddings and negative embeddings are calculated for every user in the batch and stacked.
- Loss is calculated and summed for all users in the batch. 
