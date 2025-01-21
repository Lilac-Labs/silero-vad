# Tuning the Silero-VAD Model

> The tuning code was created with the support of the Innovation Assistance Fund as part of the federal project "Artificial Intelligence" of the national program "Digital Economy of the Russian Federation."

Tuning is used to improve the speech detection quality of the Silero-VAD model on custom data.

## Dependencies

The following dependencies are used for tuning the VAD model:

- `torchaudio>=0.12.0`
- `omegaconf>=2.3.0`
- `sklearn>=1.2.0`
- `torch>=1.12.0`
- `pandas>=2.2.2`
- `tqdm`

## Data Preparation

Dataframes for tuning must be prepared and saved in the `.feather` format. The following columns in the `.feather` files for training and validation are mandatory:

- **audio_path** - the absolute path to the audio file in the file system. Audio files should be `PCM` data, preferably in `.wav` or `.opus` formats (other popular audio formats are also supported). To speed up the training process, it is recommended to resample the audio files (change the sampling rate) to 16000 Hz beforehand;
- **speech_ts** - annotation for the corresponding audio file. A list consisting of dictionaries in the format `{'start': START_SEC, 'end': 'END_SEC'}`, where `START_SEC` and `END_SEC` are the start and end times of the speech segment in seconds, respectively. For quality training, it is recommended to use annotations with an accuracy of up to 30 milliseconds.

The more data used during the training phase, the more effectively the adapted model performs in the target domain. The length of the audio is not limited, as each audio will be trimmed to `max_train_length_sec` seconds before being fed into the neural network. Long audio should preferably be cut into pieces of `max_train_length_sec` length.

An example of a `.feather` dataframe can be found in the file `example_dataframe.feather`.

## Configuration File `config.yml`

The configuration file `config.yml` contains paths to the training and validation datasets, as well as training parameters:

- `train_dataset_path` - the absolute path to the training dataframe in `.feather` format. It must contain the columns `audio_path` and `speech_ts`, described in the "Data Preparation" section. An example of the dataframe structure can be found in `example_dataframe.feather`;
- `val_dataset_path` - the absolute path to the validation dataframe in `.feather` format. It must contain the columns `audio_path` and `speech_ts`, described in the "Data Preparation" section. An example of the dataframe structure can be found in `example_dataframe.feather`;
- `jit_model_path` - the absolute path to the Silero-VAD model in `.jit` format. If this field is left empty, the model will be loaded from the repository depending on the value of the `use_torchhub` field;
- `use_torchhub` - If `True`, the model for training will be loaded using torch.hub. If `False`, the model for training will be loaded using the silero-vad library (must be pre-installed with the command `pip install silero-vad`);
- `tune_8k` - this parameter determines which head of the Silero-VAD to train. If `True`, the head with an 8000 Hz sampling rate will be trained, otherwise with 16000 Hz;
- `model_save_path` - the path to save the trained model;
- `noise_loss` - the loss coefficient applied to non-speech windows of audio;
- `max_train_length_sec` - the maximum length of audio in seconds during training. Longer audio will be trimmed to this value;
- `aug_prob` - the probability of applying augmentations to the audio file during training;
- `learning_rate` - the training rate;
- `batch_size` - the batch size during training and validation;
- `num_workers` - the number of threads used for data loading;
- `num_epochs` - the number of training epochs. All training data is processed in one epoch;
- `device` - `cpu` or `cuda`.

## Training

Training is started with the command

`python tune.py`

It lasts for `num_epochs`, and the best checkpoint based on the ROC-AUC metric on the validation set will be saved in `model_save_path` in jit format.

## Threshold Search

Input and output thresholds can be selected using the command

`python search_thresholds`

This script uses the configuration file described above. The model specified in the configuration will be used to find optimal thresholds on the validation dataset.

## Citation

```
@misc{Silero VAD,
  author = {Silero Team},
  title = {Silero VAD: pre-trained enterprise-grade Voice Activity Detector (VAD), Number Detector and Language Classifier},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/snakers4/silero-vad}},
  commit = {insert_some_commit_here},
  email = {hello@silero.ai}
}
```
