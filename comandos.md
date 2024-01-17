curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"

curl -XGET "localhost:9200/bank/_search?pretty"



PAGINACION:

Recuperar los primeros 20 documentos del indice "bank":

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "size": 20
}
'

Recuperar los siguientes 20 documentos del indice "bank":

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "size": 20,
  "from": 20
}
'

Buscar los datos de las personas que residen en Texas (TX) y devolver los primeros 15 resultados:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "state": "TX" } },
  "size": 15
}
'




ORDENACION:

Recuperar los datos de los residentes en el estado de Los Angeles (LA) y mostrar los resultados ordenados por edad de forma ascendente:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "state": "LA" } },
  "sort": { "age": { "order": "asc" } }
}
'

Recuperar los datos de los residentes en el estado de New Jersey (NJ) y mostrar los resultados ordenados por su balance de forma ascendente:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "state": "NJ" } },
  "sort": { "balance": { "order": "asc" } }
}
'



FILTROS:

Recuperar los datos de usuarios cuyo estado de residencia sea Los Angeles (LA) pero que su ciudad de residencia no sea Loretto, y que ademas su edad sea superior a los 33 años:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "state": "LA" } },
        { "range": { "age": { "gt": 33 } } }
      ],
      "must_not": [
        { "match": { "city": "Loretto" } }
      ]
    }
  }
}

Recuperar los datos de usuarios cuyo estado de residencia sea Ohio (OH) y su edad sea superiror a 390 años:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "state": "OH" } },
        { "range": { "age": { "gt": 390 } } }
      ]
    }
  }
}




BUSQUEDAS DIFUSAS:

Utilizar una usqueda difusa para recuperar los datos de la persona residente en Wyoming, pero utilizando "Woyming" como termino de busqueda:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "fuzzy": { "state": "Woyming" } }
}
'

Modificar la busqueda anterior para mostrar los datos de la misma persona, pero utilizando "Wyomin" como termino de busqueda:

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "fuzzy": { "state": "Wyomin" } }
}
'

PREFIJOS Y COMODINES:

Recuperar los datos de las personas cuyos apellidos comiencen por "Mc":

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "prefix": { "lastname": "Mc" } }
}
'

Recuperar los datos de las personas cuya ciudad de residencia comience por la letra "G" y acabe con "field":

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "wildcard": { "city": "G*field" } }
}
'

EXP REGULARES:

Utilizando expresiones regulares, recuperar los datos de las personas cuyo nombre de empleador comience por la letra "A" y este compuesto por 4 o 5 letras, p.e. los empleadores "Avit" o "Amtap":

curl -XGET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "regexp": { "employer": "A.{3,4}" } }
}
'
