# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** CARLOS EDUARDO BEZERRA FERREIRA  
**Matrícula:** ____________________  
**Email:** CEDUARDOBF@GMAIL.COM  
**Data:** 27 / 02 / 2026

---

## QUESTÃO 1 - Consulta Básica com Filtros

### Comando Utilizado
```
db.movies.find(
  {
    genres: "Drama",
    year: {
      $gte: 2010,
      $lte: 2015
    },
    "imdb.rating": {
      $gt: 7.5,
      $type: "number"
    }
  },
  {
    _id: 0,
    title: 1,
    year: 1,
    genres: 1,
    "imdb.rating": 1
  }
)
.sort({ "imdb.rating": -1 })
.limit(20)
```

### Resultado Obtido
- **Quantidade de documentos encontrados:** 352
- **5 primeiros filmes (título e rating):**
  1. Drishyam
  2. Most Likely to Succeed
  3. Kaakkaa Muttai
  4. Killswitch
  5. The Great Alone

### Screenshot
Ver arquivos anexos.
### Observações (opcional)


---

## QUESTÃO 2 - Agregação com Agrupamento

### Pipeline Utilizada
```
db.movies.aggregate([
  {
    $match: {
      countries: { $exists: true, $type: "array", $ne: [] }
    }
  },
  { $unwind: "$countries" },
  {
    $group: {
      _id: "$countries",
      totalFilmes: { $sum: 1 }
    }
  },
  { $sort: { totalFilmes: -1 } },
  { $limit: 10 },
  {
    $project: {
      _id: 0,
      country: "$_id",
      totalFilmes: 1
    }
  }
])
```

### Resultado Obtido

| Posição | País | Quantidade de Filmes |
|---------|------|----------------------|
| 1º | USA | 10921 | 
| 2º | UK | 2652 |
| 3º | France | 2647 |
| 4º | Germany | 1494 |
| 5º | Canada | 1260 |
| 6º | Italy | 1217 |
| 7º | Japan | 786 |
| 8º | Spain | 675 |
| 9º | India | 564 |
| 10º | Australia | 470 |

### Screenshot
Ver arquivos anexos.

### Observações (opcional)


---

## QUESTÃO 3 - Pipeline com $unwind e $group

### Pipeline Utilizada
```
db.movies.aggregate([
  {
    $match: {
      cast: { $exists: true, $type: "array", $ne: [] },
      "imdb.rating": { $exists: true, $type: "number" }
    }
  },
  { $unwind: "$cast" },
  {
    $group: {
      _id: "$cast",
      totalFilmes: { $sum: 1 },
      mediaRating: { $avg: "$imdb.rating" }
    }
  },
  { $sort: { totalFilmes: -1 } },
  { $limit: 5 },
  {
    $project: {
      _id: 0,
      ator: "$_id",
      totalFilmes: 1,
      mediaRating: { $round: ["$mediaRating", 2] }
    }
  }
])
```

### Resultado Obtido

| Posição | Ator | Qtd Filmes | Rating Médio |
|---------|------|------------|--------------|
| 1º | Gèrard Depardieu | 67 | 6.69 |
| 2º | Robert De Niro | 58 | 6.96 |
| 3º | Michael Caine | 51 | 6.71 |
| 4º | Bruce Willis | 49 | 6.41 |
| 5º | Samuel L. Jackson | 48 | 6.4 |

### Screenshot
Ver arquivo anexo.

### Observações (opcional)


---

## QUESTÃO 4 - Agregação com $lookup

### Pipeline Utilizada
```javascript
// Cole aqui sua pipeline com $lookup
db.movies.aggregate([


])
```

### Resultado Obtido

**Filme 1:**
- Título: 
- Ano: 
- Quantidade de comentários: 
- Primeiros 3 usuários:
  1. Nome: __________ | Email: __________
  2. Nome: __________ | Email: __________
  3. Nome: __________ | Email: __________

**Filme 2:**
- Título: 
- Ano: 
- Quantidade de comentários: 
- Primeiros 3 usuários:
  1. Nome: __________ | Email: __________
  2. Nome: __________ | Email: __________
  3. Nome: __________ | Email: __________

### Screenshot
_[Anexar screenshot ou indicar arquivo: questao04.png]_

### Observações (opcional)

---

## QUESTÃO 5 - Agregação com Múltiplos Estágios

### Pipeline Utilizada
```javascript
// Cole aqui sua pipeline completa
db.movies.aggregate([


])
```

### Resultado Obtido

| Gênero | Qtd Filmes | Rating Médio |
|--------|------------|--------------|
| | | |
| | | |
| | | |
| | | |

### Explicação
**Por que esses gêneros são considerados subestimados?**


### Screenshot
_[Anexar screenshot ou indicar arquivo: questao07.png]_

---
