<!-- WEASEL: AUTO-GENERATED DOCS START (do not remove) -->

# 🪐 Weasel Project: Training a spaCy Coref Model

This project trains a coreference model for spaCy using OntoNotes.

To run this pipeline in GPU you will need at least 20GB GPU.


## 📋 project.yml

The [`project.yml`](project.yml) defines the data assets required by the
project, as well as the available commands and workflows. For details, see the
[Weasel documentation](https://github.com/explosion/weasel).

### ⏯ Commands

The following commands are defined by the project. They
can be executed using [`weasel run [name]`](https://github.com/explosion/weasel/tree/main/docs/cli.md#rocket-run).
Commands are only re-run if their inputs have changed.

| Command | Description |
| --- | --- |
| `uncompress` |  Uncompress dataset | 
| `preprocess` | Convert the data to spaCy's format |
| `train-cluster` | Train the clustering component |
| `prep-span-data` | Prepare data for the span resolver component. |
| `train-span-resolver` | Train the span resolver component. |
| `assemble` | Assemble all parts into a complete coref pipeline. |
| `eval` | Evaluate model on the test set. |

### ⏭ Workflows

The following workflows are defined by the project. They
can be executed using [`weasel run [name]`](https://github.com/explosion/weasel/tree/main/docs/cli.md#rocket-run)
and will run the specified commands in order. Commands are only re-run if their
inputs have changed.

| Workflow | Steps |
| --- | --- |
| `prep` | `uncompress` &rarr; `preprocess` |
| `train` | `train-cluster` &rarr; `prep-span-data` &rarr; `train-span-resolver` &rarr; `assemble` |
| `all` | `uncompress` &rarr; `preprocess` &rarr; `train-cluster` &rarr; `prep-span-data` &rarr; `train-span-resolver` &rarr; `assemble` &rarr; `eval` |

### 🗂 Assets

The following assets are defined by the project. They can
be fetched by running [`weasel assets`](https://github.com/explosion/weasel/tree/main/docs/cli.md#open_file_folder-assets)
in the project directory.

| File | Source | Description |
| --- | --- | --- |
| `assets/` | Git | Dataset for training, generated from ontonotes 5 |

<!-- WEASEL: AUTO-GENERATED DOCS END (do not remove) -->

# Getting Started

Before using this project:

1. install spaCy with GPU support - see the [install widget](https://spacy.io/usage)
2. run `pip install -r requirements.txt`
3. modify `project.yml` to set your GPU ID and OntoNotes path (see [Data Preparation](#data-preparation) for details)

After that you can just run `spacy project run all`.

Note that during training you will probably see a warning like `Token indices
sequence length is longer than ...`. This is a rare condition that
`spacy-transformers` handles internally, and it's safe to ignore if it
happens occasionally. For more details see [this
thread](https://github.com/explosion/spaCy/discussions/9277#discussioncomment-1374226).


## Using the Trained Pipeline

After training the pipeline (or downloading a pretrained version), you can load and use it like this:

```
import spacy
nlp = spacy.load("training/coref")

doc = nlp("John Smith called from New York, he says it's raining in the city.")
# check the word clusters
print("=== word clusters ===")
word_clusters = [val for key, val in doc.spans.items() if key.startswith("coref_head")]
for cluster in word_clusters:
    print(cluster)
# check the expanded clusters
print("=== full clusters ===")
full_clusters = [val for key, val in doc.spans.items() if key.startswith("coref_cluster")]
for cluster in full_clusters:
    print(cluster)
```

The prefixes used here are a user setting, so you can customize them for your
own pipelines.
