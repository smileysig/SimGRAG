# SimGRAG

The is the repository for the paper "SimGraphRAG: Leveraging Similar Subgraphs for Knowledge Graphs Driven Retrieval-Augmented Generation".
SimGRAG is a KG-driven RAG approach that can support various KG based tasks, such as question answering and fact verification.

## Prerequisites

It supports plug-and-play usability with the following three components:
- Large language model: For generation.
- Embedding model: For node and relation embedding.
- Vector database: store the embedding of the nodes and relations in the knowledge graph, supporting efficient similarity search.

This repository is built on open-source solutions of these components:
- Ollama for runing the large language model of Llama 3 70B
- Nomic embedding model for node and relation embedding
- Milvus for vector database

You can replace the components with your own preference, all you need is to prepare the APIs.
Next, we provide the preparation steps for the components we used.

### Ollama

Please visit the [Ollama](https://ollama.com/) website to install Ollama on your local environment.
After installation, you can use the following command to run the Llama 3 70B model:
```
ollama run llama3:70b
```
Then, you can use the following command to start the service needed by SimGRAG:
```
bash ollama_server.sh
```

### Nomic Embedding Model

You can clone the model from [here](https://huggingface.co/nomic-ai/nomic-embed-text-v1) with the following command:
```
mkdir -p data/raw
cd data/raw
git clone https://huggingface.co/nomic-ai/nomic-embed-text-v1
```

### Milvus

Please visit the [Milvus](https://milvus.io/) website to install Milvus on your local environment.
After installation, you can follow its documentation to start the service needed by SimGRAG.

## Data preparation

### MetaQA
Please download the MetaQA dataset following the url in the [repository](https://github.com/yuyuz/MetaQA) and put it in the `data/raw` folder.

### FactKG
Please download the FactKG dataset following the url in the [repository](https://github.com/jiho283/FactKG) and put it in the `data/raw` folder.

### Directonary structure
After preparation, the directories should be organized as follows:
```
SimGraphRAG
├── data
│   └── raw
│       ├── nomic-embed-text-v1
│       ├── MetaQA
│       └── FactKG
├── configs
├── pipeline
├── prompts
└── src
```

## Configuration

You can find the configuration files in the `configs` folder. You can modify the configuration files to fit your needs.

## Runing the pipeline

For MetaQA, you can run the following command:
```
cd pipeline
python metaQA_index.py
python metaQA_query1hop.py
python metaQA_query2hop.py
python metaQA_query3hop.py
```

For FactKG, you can run the following command:
```
cd pipeline
python factKG_index.py
python factKG_query.py
```

The results can be found in the file that assigned to the "output_filename" in the configuration file. For example, "results/FactKG_query.txt".
Each line of the result file is a dictionary, in which the key "correct" presents the correctness of the final answer.
