[![LinkedIn][linkedin-shield]][linkedin-url]

## :jigsaw: Objective

- Pick language traslation dataset from opus_books
- Perform data analysis
- Optimize training loop based on data analysis
- Use One Cycle Policy and Automatic Mixed Precision to reduce training time
- Use Lion optimizer
- Train encoder-decoder transformer model for language translation task

## Prerequisites
* [![Python][Python.py]][python-url]
* [![Pytorch][PyTorch.tensor]][torch-url]
* [![Hugging face][HuggingFace.transformers]][huggingface-url]

## :open_file_folder: Files
- [**config_file.py**](config_file.py)
    - File to configure dataset, model and training parameters
- [**data_analysis.ipynb**](data_analysis.ipynb)
    - Data analysis of opus_books translation dataset
- [**dataset.py**](dataset.py)
    - This file contains dataset class for translation task
- [**model.py**](model.py)
    - This file contains all the building blocks require to make Encoder-Decoder transformet model
- [**train.py**](train.py)
    - This is the main file of this project
    - It uses functions available in `dataset.py` and `model.py`.
    - Most of the opimization techniques are imaplimented in this file 

## :building_construction: Model Architecture
The model is implemented based on Attention Is All You Need paper. The transformer architecture is
structured with N Encoder-Decoder blocks, each incorporating multi-head attention mechanisms. Our
specific model consists of 6 such blocks. Tokens undergo embedding into 512-dimensional vectors
(d_model), and each block employs multi-head attention with 8 heads (h), succeeded by feed-forward
networks with a size of 128 (d_ff). Additionally, positional encodings are integrated to account for
sequence context, and the decoder's output is projected onto the target vocabulary for translation.

**Model Dimensions:**

- Embedding Dimension (d_model): Size of the embedding vectors (Default=512).
- Feed-Forward Dimension (d_ff): Size of the internal layers in the feed-forward network of encoder
 and decoder blocks (Default=2048).
- Attention Heads (h): Number of Attention Heads for Multi-Head Attention (Default=8).

## :golfing: Training Optimization

To reduce training time from 20 minutes per batch to 3 minutes per batch on same GPU.

- Ratio of Tokenizer lenght: Remove sentences where ratio between lenght of source and target 
sentence is less than 0.5 or greater than 2. This indicates traslation might be wrong. 
- Split longer sentences: As per data analysis only 1.95% sentences have token lenght greater
than 100. We can split those sentances and remove longer sentences.
- Sort dataset: Sort dataset by lenght so each batch have sentences with almost equal lenghts.
- Combine inputs: Split sorted dataset into half, reverse one of them and combine them. This will
go through data faster at cost of slow iteration. This will help model understad about longer
contexts better. 
- Random batch selection: Reorder dataset by selecting batches randomly.
- Dynamic Padding: Limit the number of added token based on lenght of longest sequence in each mini 
batch.
- Automatic Mixed Precision: Matrix Multiplication and convolutions are moved to F16 calculations,
and backprop is still kept at F32.
- One Cycle Policy: Use super convergence to train model faster.


## Installation

1. Clone the repo
```
git clone https://github.com/AkashDataScience/language_translation_optimization
```
2. Go inside folder
```
 cd language_translation_optimization
```
3. Install dependencies
```
pip install -r requirements.txt
```

## Training

```
# Start training with:
python train.py

```

## Usage 
Please refer to [ERA V2 Session 18](https://github.com/AkashDataScience/ERA-V2/tree/master/Week-18)

## Contact

Akash Shah - akashmshah19@gmail.com  
Project Link - [ERA V2](https://github.com/AkashDataScience/ERA-V2/tree/master)



[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/akash-m-shah/
[Python.py]:https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54
[python-url]: https://www.python.org/
[PyTorch.tensor]: https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white
[torch-url]: https://pytorch.org/
[HuggingFace.transformers]: https://img.shields.io/badge/%F0%9F%A4%97-Hugging%20Face-orange
[huggingface-url]: https://huggingface.co/