# Adding a Tensorboard report to LlamaIndex Finetuning 
<p align = "center">
<img src = "https://github.com/SwamiKannan/SentenceTransformer-with-Tensorboard/blob/main/cover.png">
</p>

## Introduction
LlamaIndex is a really great open-source data framework that connects custom data sources to large language models (LLMs) for document Q&A, data augmented chatbots, and structured analytics. 
However, apart from the standard data ingestion, vector store and database integration tools, LlamaIndex also has a pretty good library for finetuning embeddings - <a href="https://github.com/run-llama/llama_index/tree/main/llama-index-finetuning">llamaindex.finetune() </a>

However, the finetune library calls <a href="https://www.sbert.net/">sentence-transformers </a> to do the actual training. And sentence transformers has been in the process of developing a framework for reporting for quite some time. Hence, currently there is no official repo / library to monitor training on sentence-transformers and hence llama-index.finetuning using <a href="https://www.tensorflow.org/tensorboard">Tensorboard </a>

Hence, I got a little impatient and built this repo to track finetuning using Tensorboard

#### Note1: 
Some of the code in this repo is based on <a href="https://github.com/Mogady">Mogady's </a> <a href="https://github.com/UKPLab/sentence-transformers/pull/1532/files#diff-b85567d4fdaffe34a3ccd8fe6cd1fcb15a986ebd34af373c71f1f5cf5efff021">PR </a> for Sentence Transformers. Huge credits to him for this submission.

#### Note2:
I have already submitted a <a href="https://github.com/run-llama/llama_index/pull/11568">PR </a> to Llamaindex based on changes to the repo. However, this is a standalone repo with no changes to the main sentence-transformers/ llamaindex library. However, those packages do need to be installed.

## Requirements:
1. llama-index finetune library
    ```
    pip install llama-index-finetuning
   ```
2. sentence-transformers
   ```
   pip install sentence-transformers
   ```

## Usage:
1. Get into the src directory
    ```
    cd src
    ```
2. Run the code below updating details for the input json file, model_output_path, model_id, epochs and the path where the logs should be stored (writer_path)
    ```
    from llama_index.core.evaluation import EmbeddingQAFinetuneDataset
    from llama_index.finetuning import SentenceTransformersFinetuneEngine
    from sftb import TBSTFE

    train_dataset = EmbeddingQAFinetuneDataset.from_json(json_file)
    finetuner = TBSTFE(
      dataset=train_dataset,
      model_id=model_id,
      model_output_path=model_output_path,
      epochs=epochs,
      writer_path=w_path
    )
    finetuner.finetune()
    finetuned_model = finetuner.get_finetuned_model()
    finetuned_model.to_json()
    ```
