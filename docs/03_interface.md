# Interface Web 

**app**
├── **embeddings**
│   ├── embeddings_FT_corpus_movie
│   └── embeddings_corpus_movie
├── **models**
│   ├── sentence_similarity_model
│   └── sentence_similarity_model_FT
├── **src**
│   ├── database.db
│   ├── database.py
│   ├── main.py
│   ├── models.py
│   ├── schemas.py
│   ├── **scripts**
│   │   ├── finetune_first_model.py
│   │   ├── finetuning.py
│   │   ├── preprocessing.py
│   │   ├── similarity.py
│   │   └── utils.py
│   └── services.py
├── **static**
│   ├── LICENSE.txt
│   ├── README.txt
│   ├── **assets**
│   │   ├── css
│   │   ├── js
│   │   ├── sass
│   │   └── webfonts
│   ├── **images**
│   └── index.html
└── **templates**
    └── result_table.html.jinja