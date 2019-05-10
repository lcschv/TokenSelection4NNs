## TokenSelection4NNs
Do we really need all tokens? Rethinking token selection in neuralnetwork for NLP


## Dependent environment
* Python3.0+
* pip install stanfordcorenlp, keras, numpy, pickle, argparse, nltk, etc.
* you need to download english "stopword" for nltk if you did not. 
* you need to download stanford CoreNLP, You can find in: https://stanfordnlp.github.io/CoreNLP/download.html the version we used is: "stanford-corenlp-full-2018-10-05", put the file path in configuration file (refer to configuration section).


## running steps
**Step1:**: Prepare the data (generate the token selection files).
* Python File: "token_selection.py".
* Add your data into path as follows: prepared/your_dataset_name/your_file_name => e.g. prepared/IMBD/train.csv ; prepared/IMBD/test.csv ; etc..
* Change the parameter in __main__ , part of the code is shown below:
-------------------------------------------------------
	nlp = StanfordCoreNLP(params.corenlp_root)
	## below is where you can set your dataset and file_name
	token_select.token_selection_preparation(nlp = nlp, dataset="IMDB",file_name="train.csv")
	token_select.token_selection_preparation(nlp = nlp, dataset="IMDB",file_name="test.csv")
	nlp.close() # Do not forget to close! The backend server will consume a lot memery.

**Step2:**: run the neural model.
* Python File: "main.py".
* you need to change the parameters in the function of train_model(), especially for the line below:
-----------------------------
	# strategy can be: fulltext, stopword, random, POS, dependency, 
	train = token_select.get_train(dataset="IMDB",file_name="train.csv",stragety="stopword",POS_category="Noun")

* where you need to specify the dataset and file_name, and strategy, and then you can run your code. Note that if you did not set stragety="POS", the POS_category won't be used. 
* If you set strategy="POS", POS_category can be: "Noun", "Verb", "Adjective", "Noun_Verb", "Noun_Adjective", "Verb_Adjective", "Noun_Verb_Adjective". 


## configuration
* Configuration File:"config/config.ini"
* Basically, you just need to put the standford CoreNLP file path for "corenlp_root"; 
* And Glove embedding files for "GLOVE_DIR"

Our config.ini looks like below:

	[COMMON]
	MAX_SEQUENCE_LENGTH = 150
	MAX_SEQUENCE_LENGTH_contatenate = 150 
	MAX_NB_WORDS = 20000   
	EMBEDDING_DIM = 100
	VALIDATION_SPLIT = 0.1
	batch_size = 64
	epoch_num = 100
	dropout_rate = 0.2
	hidden_unit_num = 100
	hidden_unit_num_second = 100
	cell_type = gru
	contatenate = 1
	lr= 0.001
	corenlp_root=/home/dongsheng/data/resources/stanford-corenlp-full-2018-10-05
	GLOVE_DIR = /home/dongsheng/data/resources/glove
	dataset_dir = input/dataset
	model= bilstm2
