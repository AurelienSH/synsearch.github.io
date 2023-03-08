# Système 

Cette section décrit le système utiliser pour faire foncitonner :clapper: *SynSearch*

utilisation des sentences transformers pour créer des embeddings de documents (exploration de doc2vec et classification également)

Toutes les informations utilisées proviennent de [Hugging Face :hugs:](https://huggingface.co/) et de [SBERT](https://www.sbert.net/index.html).

## Modèle choisi 

Nous avons choisi d'utiliser des Sentences Transformers pour avoir une représentation en vecteur de nos données. Nous voulions pouvoir calculer la distance entre la requête d'un utilisateur et le synopsis d'un film. Pour cela, avoir des embeddings de nos documents nous sembler être la meilleure option. Les Sentences Transformers sont une référence dans l'état de l'art pour les embeddings de documents, c'est pour cette raison que nous avons choisi des les utiliser. 

Une fois les documents et la requête de l'utilisateur transformer en embeddings, nous avons pu calculer la distance cosinus pour trouver les meilleurs films à recommender. Plus la distance est faible (c'est à dire se rapproche de 0), plus la requête et le synopsis sont proches. 

Ainsi, on suggère les cinq films les plus proches de la requête à l'utilisateur. 

## Fine tuning 

Bien que notre modèle soit déjà performant sans *fine-tuning*, nous avons décidé de tenter de l'améliorer. Plusieurs pistes étaient possibles, nous en avons retenu deux :

- L'utilisation de requête (*queries*) 
- Le crowdsourcing

### Avec des requêtes (*queries*)

Les requêtes des utilisateurs ne sont pas vraiment semblables au synopsis de films. Nous avons donc [augmenter nos données](data.md#augmentation-des-données) en ajoutant manuellement à 50 synopsis une requête. Voici un exemple de données :

>
> :label: *title* : Findind Nemo <br>
> :page_facing_up: *synopsis* : Nemo, an adventurous young clownfish, is unexpectedly taken from his Great Barrier Reef home to a dentist's office aquarium. It's up to his worrisome father Marlin and a friendly but forgetful fish Dory to bring Nemo home -- meeting vegetarian sharks, surfer dude turtles, hypnotic jellyfish, hungry seagulls, and more along the way. <br>
>:question: *query* : I want to watch a heartwarming animated movie suitable for the whole family.


En suivant la documentation, nous avons utiliser la [*Triplet Loss*](https://www.sbert.net/docs/package_reference/losses.html#tripletloss) pour fine-tuner notre modèle. 

Cette méthode a permis d'améliorer les suggestions en rapprochant les requêtes et les synopsis déjà proches et en éloignant les moins proches. 


### Avec le crowdsourcing 

Nous avons également décidé d'utiliser le crowdsourcing pour améliorer notre modèle. L'idée est d'utiliser l'avis des utilisateurs pour fine-tuner le modèle : on rapproche les synospsis et les requêtes que l'utilisateur jugent similaires et inversement. 

Pour cela, on utilise la [*Cosine Similarity Loss*](https://www.sbert.net/docs/package_reference/losses.html#cosinesimilarityloss). 
On récupère l'information des utilisateurs grâces aux avis ( :+1: et :-1: ), ce qui nous donne des informations ayant la structure suivante : 

```python
(['synopsis', 'query'], label)
```

où le label dépend de l'avis de l'utilisateur. Nous avons arbitrairement choisi de fixer les labels suivants : 
- :+1: = 0.4
- :-1: = 1.6 <br>
Ces valeurs ont été choisi après observation des données, pour que les labels aient des valeurs légérement en dessus et en dessous des maximum et des minimums du calcul de la distance cosinus. 

>[!Warning]
> Néanmoins, nous n'avons pas trouver de moyen de lancer automatiquement ce fine-tuning. Il faut qu'il soit activé par une personne. Cela fait partie des pistes d'amélioration évoqué [ici](methodologie.md#et-après)

