# Generating Words from Embeddings
This is the code for my blog post on [Generating Words from Embeddings](https://rajatvd.github.io/Generating-Words-From-Embeddings/). It uses a character level decoder RNN to convert a word embedding (which represents a meaning) into a word by sampling one character at a time.

To get straight into sampling words, run these commands:

```
conda create -y -n word_generator python=3.6  
source activate word_generator
git clone https://github.com/rajatvd/WordGenerator.git  
cd WordGenerator  
pip install -r requirements.txt   
python preprocess_data.py  
python sampling.py with word=musical sigma=0.2
```

This works only for Linux systems. For Windows, you have to manually install pytorch 0.4.1 before running the scripts. Use this command:

`pip3 install http://download.pytorch.org/whl/cu90/torch-0.4.1-cp36-cp36m-win_amd64.whl`


# Requirements
`python 3.6`  
`pytorch 0.4.1`

Also needs the following packages:  
* `pytorch-nlp` - for getting word embeddings [link](https://github.com/PetrochukM/PyTorch-NLP)
* `sacred` - keeping track of configs of training runs and easily writing scripts [link](https://github.com/IDSIA/sacred)
* `visdom` - live dynamic loss plots [link](https://github.com/facebookresearch/visdom)
* `pytorch-utils` - for easily writing training code in pytorch [link](https://github.com/rajatvd/PytorchUtils)
* `visdom-observer` - interface between `sacred` and `visdom` [link](https://github.com/rajatvd/VisdomObserver)

Install these using:

`pip install -r requirements.txt`

All the scripts are `sacred` experiments, so they can be run as

`python <script>.py with <config updates>`

# Word embeddings

First, get the GloVe vectors and preprocess them by running

`python preprocess_data.py`

This will download the GloVe word vectors and pickle them to be used for training and inference.

# Sampling
If you want to directly sample words from a pretrained network, just go ahead and run

`python sampling.py with word=musical sigma=0.2`

You can change the word and _sigma_ to sample for different embeddings. The sampling script also has other parameters like start characters and beam size.

The `sampling.py` script is used to generate words from a trained model. A pretrained set of weights are present in the `trained_model/` directory, along with the config used to train it.

Run `python sampling.py print_config` to see the different sampling parameters.

# Train your own model
Run `python train.py print_config` to get a list of config options for training your own model.

To train your own model, make sure to have a `visdom` server running in the background at port `8097`. Just run `visdom` in a separate terminal before running the train script to start the server.

To train the same model I used for generating the words in the post, run this command:

`python train.py with trained_model/config.json`






