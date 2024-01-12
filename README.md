# LLM-personalize-tests

DATA : 
- Data is stored at /home/gvirgink/articles/combined_data_80k.tsv.
- All data is stored in a file called combined_data_80k.tsv.
- Entries from 2017 are used for training, first half 2018 for validation, second half 2018 for test. Users who had impressions in both 2017 and 2018 may appear in both train and validation/test.
- First 40k entries are used for training, next 20k for validation, final 20k for test.
- Data is read from the CSV and split into three filtered_data arrays - one for train, one for validation, one for test. This data is stored in the format user_id, headline, time.
- Sentence-transformer model is run on all article headlines before model forward pass to save time.
- Data is split into an array of user_ids, a 2D array of headlines, and an array of times. Every article consumed on the same day is stored in one array.

MODEL : 
- Data is aggregated at the user level in the training step.
- Following steps are run for each time value in the time series and summed together :

- Each user's data is passed individually into the forward pass of the model to generate their embeddings.
- Weights are generated for each user at time T based on the date they read each article. Each article embedding is multipled by the corresponding weight.
- The sum of all positive weights is passed through an MLP and is split into consistent and transiet embeddings. 
- User embeddings, positive embeddings and negative embeddings are calculated for every user in the batch.
- Loss is calculated and summed for all users in the batch (Data is passed through the LSTM here to get P(t+1)).
- After the loss is calculated, the model is run on the validation data to find the validation loss. 
