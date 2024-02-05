# Classificazione di tweet in base al linguaggio d'odio e offensivo
Settembre 2023 - Progetto di Apprendimento Automatico

Questo progetto si propone di classificare dei tweet in tre categorie: hate speech, offensive language e neither, usando un modello di deep learning. Il progetto è stato realizzato per il corso di Machine Learning presso l'Università di Bologna.

## Installazione e uso

Si può eseguire il progetto su colab.
Vengono utilizzate le seguenti librerie:

- numpy
- pandas
- torch
- transformers
- sklearn
- matplotlib

Il progetto consiste in un unico notebook Jupyter che contiene tutte le istruzioni e i commenti necessari per seguire il flusso di lavoro del progetto.

## Dataset

Il dataset usato per il progetto è il seguente:

Davidson, T., Warmsley, D., Macy, M., & Weber, I. (2017, May). Automated hate speech detection and the problem of offensive language. In Eleventh international aaai conference on web and social media.

Il dataset contiene 24783 tweet, etichettati in base alla presenza di linguaggio d'odio o offensivo. Per ogni tweet, ci sono 7 campi:

*   id: identificatore numerico
*   count: numero delle parole rilevanti ai fini della classificazione (questo è pari alla somma dei tre numeri successivi)
*   hate_speech: numero di associate con "hate speech"
*   offensive_language: numero di parole associate con "offensive language"
*   neither: numero di parole prese in considerazione ma non associate ale classi precedenti
*   class: la categoria a cui appartiene la sentenza (ground truth)
*   tweet: la sentenza da classificare

Per il progetto, si sono usati solo i campi class e tweet. Il dataset è stato diviso in train, validation e test set, con le seguenti proporzioni:

- train: 70%
- validation: 15%
- test: 15%

Il dataset è stato sottoposto a un preprocessing che ha incluso le seguenti operazioni:

- rimozione degli username, degli hashtag e degli URL
- rimozione della punteggiatura e dei caratteri non alfanumerici
- conversione in minuscolo
- sostituzione delle ripetizioni di lettere con una sola lettera (es. cooool -> cool)
- rimozione delle stopwords
- lemmatizzazione

Per la tokenizzazione, si è usato il tokenizer preallenato `bert-base-uncased`, fornito dalla libreria `transformers`. Si è scelto di usare questo tokenizer perché è compatibile con il modello usato per la classificazione e perché gestisce in modo efficace il vocabolario inglese.

## Modello

Il modello usato per il progetto è il seguente:

Devlin, J., Chang, M. W., Lee, K., & Toutanova, K. (2018). Bert: Pre-training of deep bidirectional transformers for language understanding. arXiv preprint arXiv:1810.04805.

Si tratta di un modello di deep learning basato sull'architettura Transformer, che sfrutta la rappresentazione bidirezionale del contesto per apprendere le caratteristiche semantiche e sintattiche del testo. Il modello è stato preallenato su due grandi corpus di testo inglese: Wikipedia e BookCorpus, usando due compiti di apprendimento non supervisionato: la predizione di parole mancanti e la classificazione di coppie di frasi.

Per il progetto, si è usata la versione `bert-base-uncased` del modello, che ha 12 livelli di encoder, 768 unità nascoste e 12 teste di attenzione. Il modello è stato adattato al compito di classificazione dei tweet, aggiungendo uno strato lineare finale che prende in input l'output del modello per il token `[CLS]` e restituisce un vettore di 3 elementi, corrispondenti alle probabilità delle tre classi. Il modello è stato allenato usando il train set, con i seguenti iperparametri:

- batch size: 32
- learning rate: 2e-5
- numero di epoche: 4
- criterio di ottimizzazione: Adam
- funzione di perdita: cross-entropy

Il modello è stato valutato usando il validation set, monitorando la metrica macro-f1. Il modello ha raggiunto una macro-f1 di 0.81 sul validation set.

## Risultati

Il modello è stato testato usando il test set, ottenendo i seguenti risultati:

- macro-f1: 0.82
- f1-accuracy per le singole classi:

| Classe | f1-accuracy |
|--------|-------------|
| hate speech | 0.59 |
| offensive language | 0.88 |
| neither | 0.79 |

- confusion matrix:

| Classe | hate speech | offensive language | neither |
|--------|-------------|--------------------|---------|
| hate speech | 98 | 29 | 17 |
| offensive language | 18 | 1827 | 63 |
| neither | 9 | 90 | 346 |

I risultati mostrano che il modello ha una buona capacità di classificare i tweet, soprattutto per le classi offensive language e neither, che sono le più frequenti nel dataset. Il modello ha invece una maggiore difficoltà a riconoscere i tweet che contengono hate speech, che sono i meno numerosi e i più ambigui. Questo potrebbe essere dovuto alla scarsità di dati per questa classe, alla soggettività dei giudizi degli annotatori e alla presenza di termini o espressioni che possono avere diversi significati a seconda del contesto.

## Conclusioni

Questo progetto ha dimostrato che è possibile usare un modello di deep learning preallenato per classificare dei tweet in base al linguaggio d'odio e offensivo. Il modello ha ottenuto una buona prestazione complessiva, ma ha mostrato dei limiti nella classificazione dei tweet più rari e complessi. Per migliorare il modello, si potrebbero provare le seguenti strategie:

- aumentare il numero di dati per la classe hate speech, usando tecniche di data augmentation o integrando il dataset con altre fonti
- usare un modello più specifico per il dominio dei social media, come `bertweet` o `roberta-base-openai-detector`
- usare un approccio multilingua, per gestire i tweet che contengono parole o frasi in altre lingue oltre all'inglese
- usare un approccio multitask, per incorporare altri compiti correlati alla classificazione, come la rilevazione di sarcasmo, ironia o emotività

## Fonti

- Davidson, T., Warmsley, D., Macy, M., & Weber, I. (2017, May). Automated hate speech detection and the problem of offensive language. In Eleventh international aaai conference on web and social media.
- Devlin, J., Chang, M. W., Lee, K., & Toutanova, K. (2018). Bert: Pre-training of deep bidirectional transformers for language understanding. arXiv preprint arXiv:1810.04805.
- Hugging Face. Transformers: State-of-the-art Natural Language Processing.
- Scikit-learn. sklearn.metrics.classification_report.
