# Silero-VAD Dataset

> The dataset was created with the support of the Innovation Assistance Fund as part of the federal project "Artificial Intelligence" of the national program "Digital Economy of the Russian Federation".

Below are links to `.feather` files containing annotated open audio datasets using Silero VAD, as well as a brief description of each dataset with loading examples. `.feather` files can be opened using the `pandas` library:

```python
import pandas as pd
dataframe = pd.read_feather(PATH_TO_FEATHER_FILE)
```

Each `.feather` file with annotations contains the following columns:

- `speech_timings` - annotation of the given audio. This is a list containing dictionaries of the form `{'start': START_SECOND, 'end': END_SECOND}`, where `START_SECOND` and `END_SECOND` are the start and end times of speech in seconds. The number of these dictionaries equals the number of speech audio segments found in the given audio;
- `language` - ISO code of the language of the given audio.

Columns containing information about the audio file download vary and are described for each dataset below.

**All data is annotated with a temporal discretization of ~30 milliseconds (`num_samples` - 512)**

| Name                     | Number of Hours | Number of Languages | Link                                               | License                               | md5sum                           |
| ------------------------ | --------------- | ------------------- | -------------------------------------------------- | ------------------------------------- | -------------------------------- |
| **Bible.is**             | 53,138          | 1,596               | [URL](https://live.bible.is/)                      | [Unique](https://live.bible.is/terms) | ea404eeaf2cd283b8223f63002be11f9 |
| **globalrecordings.net** | 9,743           | 6,171[^1]           | [URL](https://globalrecordings.net/en)             | CC BY-NC-SA 4.0                       | 3c5c0f31b0abd9fe94ddbe8b1e2eb326 |
| **VoxLingua107**         | 6,628           | 107                 | [URL](https://bark.phon.ioc.ee/voxlingua107/)      | CC BY 4.0                             | 5dfef33b4d091b6d399cfaf3d05f2140 |
| **Common Voice**         | 30,329          | 120                 | [URL](https://commonvoice.mozilla.org/en/datasets) | CC0                                   | 5e30a85126adf74a5fd1496e6ac8695d |
| **MLS**                  | 50,709          | 8                   | [URL](https://www.openslr.org/94/)                 | CC BY 4.0                             | a339d0e94bdf41bba3c003756254ac4e |
| **Total**                | **150,547**     | **6,171+**          |                                                    |                                       |                                  |

## Bible.is

[Link to the annotated `.feather` file](https://models.silero.ai/vad_datasets/BibleIs.feather)

- The `audio_link` column contains links to specific audio files.

## globalrecordings.net

[Link to the annotated `.feather` file](https://models.silero.ai/vad_datasets/globalrecordings.feather)

- The `folder_link` column contains links to download a `.zip` archive for a specific language. Note! Links to archives are duplicated, as each archive may contain multiple audio files.
- The `audio_path` column contains paths to specific audio files after unpacking the corresponding archive from the `folder_link` column.

`The number of unique ISO codes in this dataset does not match the actual number of languages represented, as some closely related languages may be encoded with the same ISO code.`

## VoxLingua107

[Link to the annotated `.feather` file](https://models.silero.ai/vad_datasets/VoxLingua107.feather)

- The `folder_link` column contains links to download a `.zip` archive for a specific language. Note! Links to archives are duplicated, as each archive may contain multiple audio files.
- The `audio_path` column contains paths to specific audio files after unpacking the corresponding archive from the `folder_link` column.

## Common Voice

[Link to the annotated `.feather` file](https://models.silero.ai/vad_datasets/common_voice.feather)

This dataset cannot be downloaded via static links. To download, you need to go to [the link](https://commonvoice.mozilla.org/en/datasets) and, after gaining access through the appropriate form, download the archives for each available language. Note! The provided annotation is relevant for the original dataset version `Common Voice Corpus 16.1`.

- The `audio_path` column contains unique names of `.mp3` files obtained after downloading the corresponding dataset.

## MLS

[Link to the annotated `.feather` file](https://models.silero.ai/vad_datasets/MLS.feather)

- The `folder_link` column contains links to download a `.zip` archive for a specific language. Note! Links to archives are duplicated, as each archive may contain multiple audio files.
- The `audio_path` column contains paths to specific audio files after unpacking the corresponding archive from the `folder_link` column.

## License

This dataset is distributed under the [license](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en) `CC BY-NC-SA 4.0`.

## Citation

```
@misc{Silero VAD Dataset,
  author = {Silero Team},
  title = {Silero-VAD Dataset: a large public Internet-scale dataset for voice activity detection for 6000+ languages},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/snakers4/silero-vad/datasets/README.md}},
  email = {hello@silero.ai}
}
```

[^1]: `The number of unique ISO codes in this dataset does not match the actual number of languages represented, as some closely related languages may be encoded with the same ISO code.`
