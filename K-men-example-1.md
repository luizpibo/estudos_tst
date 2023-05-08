# Exemplo de estimativa de similaridade entre usando python

## Instalação das bibliotecas necessárias


```python
$ pip install -U scikit-learn pandas nltk matplotlib-venn numpy
```

## Exemplo


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib_venn import venn2
from sklearn.feature_extraction.text import CountVectorizer
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem.snowball import SnowballStemmer
from nltk.stem.wordnet import WordNetLemmatizer
```

### Tokenização

Simplificando, é dividir algum texto em tokens, esses tokens normalmente são as palavras contidas no texto, se um texto tem 10 palavras, possivelmente (depende da metodologia) terá 10 tokens. Também podem ser divididos por letras ou conjunto de palavras. A metodologia fica a critério de quem fará os tokens.

### Unigrams

São tokens com apenas uma palavra em cada token. Exemplo de lista de tokens com unigram para a seguinte frase:

"Deus é bom o tempo todo" - {"deus","bom", "tempo", "todo"}.

Neste caso, não consideramos palavras com apenas um caractere, também não contem os acentos e todas as palavras estão em minúsculo.

### Bigrams

Como o nome já sugere, são tokens tendo duas palavras em cada token, sendo a segunda palavra sempre sequência da terceira. E a partir do Bigram podemos ter trigrams, e quantos n-grams quisermos, mas geralmente só é utilizado até "3 grams". Exemplo de lista de tokens com bigram.

"Deus é bom o tempo todo" - {"deus bom", "bom tempo", "tempo todo"}'

### Tokenizando com Unigrams e Bigrams

"Deus é bom o tempo todo"

{"deus","bom","tempo","todo","deus bom","bom tempo","tempo todo"}


```python
t1 = "era uma vez um lugarzinho no meio do nada com sabor de chocolate e cheiro de terra molhada"
t2 = "era uma vez a riqueza contra a simplicidade, uma mostrando para a outra quem dava mais felicidade"
t3 = "pra gente ser feliz, temos que cultivar as nossas amizades, os amigos de verdade, temos que mergulhar na própria fantasia na nossa liberdade"

vect = CountVectorizer(analyzer = "word", ngram_range=(1,2))

vocab = vect.fit([t1,t2,t3])
```

Para mostrar todas a palavras e bigramas contidos no vocabulário


```python
vocab.vocabulary_
```




    {'era': 24,
     'uma': 85,
     'vez': 90,
     'um': 83,
     'lugarzinho': 34,
     'no': 50,
     'meio': 38,
     'do': 22,
     'nada': 48,
     'com': 10,
     'sabor': 73,
     'de': 18,
     'chocolate': 8,
     'cheiro': 6,
     'terra': 81,
     'molhada': 42,
     'era uma': 25,
     'uma vez': 87,
     'vez um': 92,
     'um lugarzinho': 84,
     'lugarzinho no': 35,
     'no meio': 51,
     'meio do': 39,
     'do nada': 23,
     'nada com': 49,
     'com sabor': 11,
     'sabor de': 74,
     'de chocolate': 19,
     'chocolate cheiro': 9,
     'cheiro de': 7,
     'de terra': 20,
     'terra molhada': 82,
     'riqueza': 71,
     'contra': 12,
     'simplicidade': 77,
     'mostrando': 43,
     'para': 60,
     'outra': 58,
     'quem': 69,
     'dava': 16,
     'mais': 36,
     'felicidade': 28,
     'vez riqueza': 91,
     'riqueza contra': 72,
     'contra simplicidade': 13,
     'simplicidade uma': 78,
     'uma mostrando': 86,
     'mostrando para': 44,
     'para outra': 61,
     'outra quem': 59,
     'quem dava': 70,
     'dava mais': 17,
     'mais felicidade': 37,
     'pra': 62,
     'gente': 31,
     'ser': 75,
     'feliz': 29,
     'temos': 79,
     'que': 66,
     'cultivar': 14,
     'as': 4,
     'nossas': 54,
     'amizades': 2,
     'os': 56,
     'amigos': 0,
     'verdade': 88,
     'mergulhar': 40,
     'na': 45,
     'própria': 64,
     'fantasia': 26,
     'nossa': 52,
     'liberdade': 33,
     'pra gente': 63,
     'gente ser': 32,
     'ser feliz': 76,
     'feliz temos': 30,
     'temos que': 80,
     'que cultivar': 67,
     'cultivar as': 15,
     'as nossas': 5,
     'nossas amizades': 55,
     'amizades os': 3,
     'os amigos': 57,
     'amigos de': 1,
     'de verdade': 21,
     'verdade temos': 89,
     'que mergulhar': 68,
     'mergulhar na': 41,
     'na própria': 47,
     'própria fantasia': 65,
     'fantasia na': 27,
     'na nossa': 46,
     'nossa liberdade': 53}



O número ao lado de cada token, é o número de referência.

## Matriz de Tokens

O método _fit_transform_ cria matrizes, onde cada coluna referencia um token, e cada linha seria um tipo de evento, no nosso caso são frases.


```python
# t1 e t2 são nossos dois textos, e aplicamos a real transform!
texts = vect.fit_transform([t1,t2,t3])
texts_array = texts.toarray() #Transformando em array para olhar a matriz
print(texts_array)
vocab.vocabulary_
```

    [[0 0 0 0 0 0 1 1 1 1 1 1 0 0 0 0 0 0 2 1 1 0 1 1 1 1 0 0 0 0 0 0 0 0 1 1
      0 0 1 1 0 0 1 0 0 0 0 0 1 1 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
      0 1 1 0 0 0 0 0 0 1 1 1 1 1 0 1 0 0 1 0 1]
     [0 0 0 0 0 0 0 0 0 0 0 0 1 1 0 0 1 1 0 0 0 0 0 0 1 1 0 0 1 0 0 0 0 0 0 0
      1 1 0 0 0 0 0 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 0 0 0 0 0 0 0 1 1 1
      1 0 0 0 0 1 1 0 0 0 0 0 0 2 1 1 0 0 1 1 0]
     [1 1 1 1 1 1 0 0 0 0 0 0 0 0 1 1 0 0 1 0 0 1 0 0 0 0 1 1 0 1 1 1 1 1 0 0
      0 0 0 0 1 1 0 0 0 2 1 1 0 0 0 0 1 1 1 1 1 1 0 0 0 0 1 1 1 1 2 1 1 0 0 0
      0 0 0 1 1 0 0 2 2 0 0 0 0 0 0 0 1 1 0 0 0]]
    




    {'era': 24,
     'uma': 85,
     'vez': 90,
     'um': 83,
     'lugarzinho': 34,
     'no': 50,
     'meio': 38,
     'do': 22,
     'nada': 48,
     'com': 10,
     'sabor': 73,
     'de': 18,
     'chocolate': 8,
     'cheiro': 6,
     'terra': 81,
     'molhada': 42,
     'era uma': 25,
     'uma vez': 87,
     'vez um': 92,
     'um lugarzinho': 84,
     'lugarzinho no': 35,
     'no meio': 51,
     'meio do': 39,
     'do nada': 23,
     'nada com': 49,
     'com sabor': 11,
     'sabor de': 74,
     'de chocolate': 19,
     'chocolate cheiro': 9,
     'cheiro de': 7,
     'de terra': 20,
     'terra molhada': 82,
     'riqueza': 71,
     'contra': 12,
     'simplicidade': 77,
     'mostrando': 43,
     'para': 60,
     'outra': 58,
     'quem': 69,
     'dava': 16,
     'mais': 36,
     'felicidade': 28,
     'vez riqueza': 91,
     'riqueza contra': 72,
     'contra simplicidade': 13,
     'simplicidade uma': 78,
     'uma mostrando': 86,
     'mostrando para': 44,
     'para outra': 61,
     'outra quem': 59,
     'quem dava': 70,
     'dava mais': 17,
     'mais felicidade': 37,
     'pra': 62,
     'gente': 31,
     'ser': 75,
     'feliz': 29,
     'temos': 79,
     'que': 66,
     'cultivar': 14,
     'as': 4,
     'nossas': 54,
     'amizades': 2,
     'os': 56,
     'amigos': 0,
     'verdade': 88,
     'mergulhar': 40,
     'na': 45,
     'própria': 64,
     'fantasia': 26,
     'nossa': 52,
     'liberdade': 33,
     'pra gente': 63,
     'gente ser': 32,
     'ser feliz': 76,
     'feliz temos': 30,
     'temos que': 80,
     'que cultivar': 67,
     'cultivar as': 15,
     'as nossas': 5,
     'nossas amizades': 55,
     'amizades os': 3,
     'os amigos': 57,
     'amigos de': 1,
     'de verdade': 21,
     'verdade temos': 89,
     'que mergulhar': 68,
     'mergulhar na': 41,
     'na própria': 47,
     'própria fantasia': 65,
     'fantasia na': 27,
     'na nossa': 46,
     'nossa liberdade': 53}



Os números presentes na matriz mostram quantas vezes determinado token apareceu em nossas frases. Em textos maiores, ou projetos maiores, é comum vermos grandes matrizes cheias de zero.

## Bora calcular!

A primeira coisa que precisamos fazer é calcular a intersecção entre as matrizes, e para isso pegamos o valor mínimo de cada coluna, por exemplo, se o valor da primeira linha da quarta coluna é 1, e da segunda linha for 0, então o valor mínimo será 0, não havendo, portando, ali, uma intersecção.


```python
intersection_list = np.amin(texts_array, axis = 0) # calculando a intersecção
intersection_list
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0], dtype=int64)



Depois devemos fazer a soma de todas as intersecções


```python
sum = np.sum(intersection_list)
sum
```




    0




```python
x1 = t1
x2 = t2
x3 = t3

vect1 = CountVectorizer(analyzer = 'word', ngram_range = (1,2))
vect2 = CountVectorizer(analyzer = 'word', ngram_range = (1,2))

vecx1 = vect1.fit([x1])
vecx2 = vect2.fit([x2])
```


```python

```
