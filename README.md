# Metric learning for user-defined KWS
This repository contains the official code for Metric learning for user-defined Keyword spotting. Our code is based on the code [voxceleb trainer](https://github.com/clovaai/voxceleb_trainer), which is implemented for the speaker recognition task.

<img width="1024" alt="image" src="https://github.com/kaistmm/Metric-UD-KWS/assets/71073008/8c798afe-8641-48eb-a5e2-33f261e2993f">

Paper: [METRIC LEARNING FOR USER-DEFINED KEYWORD SPOTTING](https://arxiv.org/pdf/2211.00439.pdf)

[Project page](https://mm.kaist.ac.kr/projects/kws/)

If you find our paper useful in your research, please use the following BibTeX entry for citation.
```BibTeX
@inproceedings{jung2023metric,
  title={Metric Learning for User-Defined Keyword Spotting},
  author={Jung, Jaemin and Kim, Youkyum and Park, Jihwan and Lim, Youshin and Kim, Byeong-Yeol and Jang, Youngjoon and Chung, Joon Son},
  booktitle={ICASSP 2023-2023 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  pages={1--5},
  year={2023},
  organization={IEEE}
}
```

---
### Data preparation
This code is implemented for mono (single-channel) audios in 16kHz. If you need to resample your audios, please utilize [ffmpeg](https://ffmpeg.org).
#### LibriSpeech Keywords
The dataset size is large, so we share the dataset creation code.
(https://github.com/jjm5811/Keyword_FA.git)
<!-- Please find the LibriSpeech Keyworkds(LSK) [here](). -->
#### Google Speech Commands
The Google Speech Commands datasets are used for these experiments. Follow the instructions on [this page](https://www.tensorflow.org/datasets/catalog/speech_commands?hl=ko) to download and prepare the data for training. We used Speech Commands v0.02 (35 keywords in total) for our baseline.

---
### Customize your target keywords
Default target keywords are ['zero', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine'].
If you don't set the target keywords, default keywords are treated as the target keywords.
```
python trainKeywordNet.py --target_keys bed bird sheila ...
```
---
### Train a new model
#### Dependencies
```sh
conda create --file requirements.txt -n [env_name] -c pytorch -c conda-forge
```

#### Training examples
- Pre-training (Softmax, AM-Softmax)
```
python trainKeywordNet.py --save_path [save_path] --augment True --batch_size [batch_size] --trainfunc [trainfunc] --model [ResNet15, ResNet26]
```
- Pre-training (Prototypical, Angular Prototypical)
```
python trainKeywordNet.py --save_path [save_path] --augment True --metric_batch_size [metric_batch_size] --batch_size 1 --trainfunc [trainfunc] --model [ResNet15, ResNet26]
```

- Fine-tuning (Softmax, AM-Softmax)
```
python trainKeywordNet.py --fine_tuning --save_path [save_path] --augment True --batch_size [batch_size] --trainfunc [trainfunc] --model [ResNet15, ResNet26] --initial_model [model.pt] --lr 0.00001
```
- Fine-tuning (Prototypical, Angular Prototypical)
```
python trainKeywordNet.py --fine_tuning --save_path [save_path] --augment True --metric_batch_size [metric_batch_size] --batch_size 1 --trainfunc [trainfunc] --model [ResNet15, ResNet26] --initial_model [model.pt] --lr 0.00001
```
#### Testing Examples
```
python trainKeywordNet.py --eval --save_path [save_path] --initial_model [initial_model] --enroll_num [1, 5, 10 (# of shots)] --enroll_list [/path/to/enroll_list.txt] --test_acc_list [/path/to/test_acc_list.txt]
```

#### Implemented loss functions
```
Softmax (softmax)
Additive Margin Softmax (amsoftmax)
Prototypical (proto)
Angular Prototypical (angleproto)
```

#### Implemented models
For the model, res15 from *"deep residual learning for small-footprint keyword spotting", R. Tang & J. Lin, 2018, ICASSP* is used. Code for the model is based on [Honk: CNNs for Keyword Spotting](https://github.com/castorini/honk).
```
ResNet15
ResNet26
ConvMixer
```
