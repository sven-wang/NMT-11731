## File Structure

* `nmt.py`: contains the neural machine translation model and training/testing code.
* `vocab.py`: a script that extracts vocabulary from training data
* `util.py`: contains utility/helper functions
* `attention.py`: attention model
* `encoder.py`: encoder model
* `decoder.py`: decoder model
* `generalRNN.py`: baseCoder model

## Dataset

The IWSLT 2014 dataset has 150K German-English training sentences. The `data/` folder contains a copy of the public release of the dataset. Files with suffix `*.wmixerprep` are pre-processed versions of the dataset from Ranzato et al., 2015, with long sentences chopped and rared words replaced by a special `<unk>` token. You could use the pre-processed training files for training/developing (or come up with your own pre-processing strategy), but for testing you have to use the **original** version of testing files, ie., `test.de-en.(de|en)`.

## Environment

The (pseudo-) template code is written in Python 3.6 using some supporting third-party libraries. We provided a conda environment to install Python 3.6 with required libraries. Simply run

```[bash]
conda env create -f environment.yml
```

## Usage

First, we extract a vocabulary file from the training data using the command:

```[bash]
python vocab.py --train-src=data/train.de-en.de.wmixerprep --train-tgt=data/train.de-en.en.wmixerprep data/vocab.bin
```

This generates a vocabulary file `data/vocab.bin`. The script also has options to control the cutoff frequency and the size of generated vocabulary, which you may play with.

For training and decoding/testing, you may refer to `data/train.sh`. Note that in the training script we set the values of some hyper parameters. They are not guaranteed to be the best hyper-parameters, and you are free to play with them. After training and decoding, we call the official evaluation script `multi-bleu.perl` to compute the corpus-level BLEU score of the decoding results against the gold-standard.


### test run command

en-de
```[bash]
python nmt.py train --train-src=data/train.de-en.en.wmixerprep --train-tgt=data/train.de-en.de.wmixerprep --dev-src=data/valid.de-en.en.wmixerprep --dev-tgt=data/valid.de-en.de.wmixerprep --vocab=data/vocab.bin --batch-size=64
```
de-en
```[bash]
python nmt.py train --train-src=data/train.de-en.de.wmixerprep --train-tgt=data/train.de-en.en.wmixerprep --dev-src=data/valid.de-en.de.wmixerprep --dev-tgt=data/valid.de-en.en.wmixerprep --vocab=data/vocab.bin --batch-size=64
```
