# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** CARLOS EDUARDO BEZERRA FERREIRA  
**Matrícula:** ____________________  
**Email:** CEDUARDOBF@GMAIL.COM  
**Data:** 27 / 02 / 2026

---

## QUESTÃO 1 - Consulta Básica com Filtros

### Comando Utilizado
´´´
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
```javascript
// Cole aqui sua pipeline de agregação
db.movies.aggregate([


])
```

### Resultado Obtido

| Posição | País | Quantidade de Filmes |
|---------|------|----------------------|
| 1º | | |
| 2º | | |
| 3º | | |
| 4º | | |
| 5º | | |
| 6º | | |
| 7º | | |
| 8º | | |
| 9º | | |
| 10º | | |

### Screenshot
_[Anexar screenshot ou indicar arquivo: questao02.png]_

### Observações (opcional)


---

## QUESTÃO 3 - Pipeline com $unwind e $group

### Pipeline Utilizada
```javascript
// Cole aqui sua pipeline de agregação
db.movies.aggregate([


])
```

### Resultado Obtido

| Posição | Ator | Qtd Filmes | Rating Médio |
|---------|------|------------|--------------|
| 1º | | | |
| 2º | | | |
| 3º | | | |
| 4º | | | |
| 5º | | | |

### Screenshot
_[Anexar screenshot ou indicar arquivo: questao03.png]_

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
