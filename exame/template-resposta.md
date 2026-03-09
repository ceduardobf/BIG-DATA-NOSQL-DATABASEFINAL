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
    year: { $gte: 2010, $lte: 2015 },
    "imdb.rating": { $gt: 7.5 }
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
  3. Killswitch
  4. Kaakkaa Muttai
  5. The Great Alone

### Screenshot
Ver arquivo PDF anexo.
### Observações (opcional)


---

## QUESTÃO 2 - Agregação com Agrupamento

### Pipeline Utilizada
```
db.movies.aggregate([
  {
    $match: {
      countries: { $exists: true, $ne: [] }
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
Ver arquivo PDF anexo.

### Observações (opcional)


---

## QUESTÃO 3 - Pipeline com $unwind e $group

### Pipeline Utilizada
```
db.movies.aggregate([
  {
    $match: {
      cast: { $exists: true, $ne: [] },
      "imdb.rating": { $exists: true }
    }
  },
  { $unwind: "$cast" },
  {
    $group: {
      _id: "$cast",
      quantidadeFilmes: { $sum: 1 },
      ratingMedio: { $avg: "$imdb.rating" }
    }
  },
  { $sort: { quantidadeFilmes: -1 } },
  { $limit: 5 },
  {
    $project: {
      _id: 0,
      ator: "$_id",
      quantidadeFilmes: 1,
      ratingMedio: { $round: ["$ratingMedio", 2] }
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
Ver arquivo PDF anexo.

### Observações (opcional)


---

## QUESTÃO 4 - Agregação com $lookup

### Pipeline Utilizada
```
db.comments.aggregate([
  {
    $match: {
      movie_id: { $exists: true }
    }
  },
  {
    $group: {
      _id: "$movie_id",
      totalComentarios: { $sum: 1 }
    }
  },
  { $sort: { totalComentarios: -1 } },
  { $limit: 5 },
  {
    $lookup: {
      from: "movies",
      localField: "_id",
      foreignField: "_id",
      as: "movie"
    }
  },
  { $unwind: "$movie" },
  {
    $lookup: {
      from: "comments",
      let: { movieId: "$_id" },
      pipeline: [
        {
          $match: {
            $expr: { $eq: ["$movie_id", "$$movieId"] }
          }
        },
        { $sort: { date: 1 } },
        { $limit: 3 },
        {
          $project: {
            _id: 0,
            name: 1,
            email: 1
          }
        }
      ],
      as: "primeirosUsuarios"
    }
  },
  {
    $project: {
      _id: 0,
      title: "$movie.title",
      year: "$movie.year",
      totalComentarios: 1,
      primeirosUsuarios: 1
    }
  }
])
```

### Resultado Obtido

**Filme 1:**
- Título: The Taking of Pelham 1 2 3
- Ano: 2009
- Quantidade de comentários: 161
- Primeiros 3 usuários:
  1. Nome: Ashlee Hart | Email: ashlee_hart@fakegmail.com
  2. Nome: Sandor Clegane | Email: rory_mccann@gameofthron.es
  3. Nome: Justin Williams | Email: justin_williams@fakegmail.com

**Filme 2:**
- Título: 50 First Dates
- Ano: 2004
- Quantidade de comentários: 158
- Primeiros 3 usuários:
  1. Nome: Ashlee Hart | Email: ashlee_hart@fakegmail.com
  2. Nome: Nichole Miller | Email: nichole_miller@fakegmail.com
  3. Nome: Paul Bailey | Email: paul_bailey@fakegmail.com

### Screenshot
Ver arquivo PDF anexo.

### Observações (opcional)

---

## QUESTÃO 5 - Agregação com Múltiplos Estágios

### Pipeline Utilizada
```
db.movies.aggregate([
  {
    $match: {
      genres: { $exists: true, $ne: [] },
      "imdb.rating": { $exists: true }
    }
  },
  { $unwind: "$genres" },
  {
    $group: {
      _id: "$genres",
      quantidadeFilmes: { $sum: 1 },
      ratingMedio: { $avg: "$imdb.rating" }
    }
  },
  {
    $match: {
      quantidadeFilmes: { $gte: 10, $lte: 50 },
      ratingMedio: { $gt: 7.0 }
    }
  },
  {
    $project: {
      _id: 0,
      genero: "$_id",
      quantidadeFilmes: 1,
      ratingMedio: { $round: ["$ratingMedio", 2] }
    }
  },
  { $sort: { ratingMedio: -1 } }
])
```

### Resultado Obtido

| Gênero | Qtd Filmes | Rating Médio |

|  News  |     44     |     7.25     |

### Explicação
Esses gêneros são considerados subestimados porque possuem uma quantidade relativamente pequena de filmes na base, mas apresentam rating médio elevado. Isso indica que, embora apareçam menos vezes, seus filmes tendem a ter boa qualidade, segundo a avaliação do IMDB.

### Screenshot
Ver arquivo PDF anexo.
---
