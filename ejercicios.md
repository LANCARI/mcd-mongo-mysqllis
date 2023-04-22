# Practica Modelo Relacional.

La siguiente practica evalua su conocimiento de consultas con Modelo Relacional.

## Como enviar sus respuesta.

1. Debe crear este archivo en su `fork` con el mismo nombre.
2. Debe copiar el contenido.
3. Debe comenzar a resolver las consultas.
4. Debe guardar el progreso en cada paso, puede cambiar la respuesta las veces que quiera, pero debe guardar el progreso cada vez. (Muy importante).

## Guardar progreso.

Para guardar su progreso cada vez que termine un ejercicio, o cambie una consulta. Ejecuta lo siguiente en un nuevo terminal:

```bash
git add .
git commit -m "Enviando progreso"
git push
```

## Ejercicios:

1. Listar la cantidad de tiendas por ciudad.

Salida:
```
+---------+------------+-------------+
| city_id | city       | store_count |
+---------+------------+-------------+
|     300 | Lethbridge |           1 |
|     576 | Woodridge  |           1 |
+---------+------------+-------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

select count(c.city_id) as city_id,city,count(s.address_id) as address_id
from city c 
join address a on c.city_id=a.city_id
join store s on a.address_id=s.address_id
join staff st on st.store_id=s.store_id
group by city;
```

2. Listar la cantidad de películas que se hicieron por lenguaje.

Salida:
```
+-------------+----------+------------+
| language_id | language | film_count |
+-------------+----------+------------+
|           1 | English  |       1000 |
|           2 | Italian  |          0 |
|           3 | Japanese |          0 |
|           4 | Mandarin |          0 |
|           5 | French   |          0 |
|           6 | German   |          0 |
+-------------+----------+------------+
```

Respuesta:
```sql

select count(f.language_id), f.language_id, l.name
from film f, language l
where f.language_id = l.language_id
group by f.language_id, l.name;

select * from language l



3.  Seleccionar todos los actores que participaron mas de 35 peliculas.

Salida:

```
+----------+------------+-----------+------------+
| actor_id | first_name | last_name | film_count |
+----------+------------+-----------+------------+
|       23 | SANDRA     | KILMER    |         37 |
|       81 | SCARLETT   | DAMON     |         36 |
|      102 | WALTER     | TORN      |         41 |
|      107 | GINA       | DEGENERES |         42 |
|      181 | MATTHEW    | CARREY    |         39 |
|      198 | MARY       | KEITEL    |         40 |
+----------+------------+-----------+------------+
6 rows in set (0.01 sec)
```

Respuesta:



-- Su respuesta aqui:

select count(actor_id), actor_id from film_actor 
where contar > 35 
group by actor_id;

select * from film limit 5;

select actor.actor_id, actor.first_name, actor.last_name, count(film_actor.film_id) as film_count
from (actor
INNER JOIN film_actor ON actor.actor_id= film_actor.actor_id)
group by actor_id
having count(film_actor.film_id)> 35;


4. Mostrar el listado de los 10 de actores que mas peliculas realizó en la categoria `Comedy`.

Salida:
```
+----------+------------+-----------+-------------------+
| actor_id | first_name | last_name | comedy_film_count |
+----------+------------+-----------+-------------------+
|      196 | BELA       | WALKEN    |                 6 |
|      143 | RIVER      | DEAN      |                 5 |
|      149 | RUSSELL    | TEMPLE    |                 5 |
|       82 | WOODY      | JOLIE     |                 4 |
|      127 | KEVIN      | GARLAND   |                 4 |
|       58 | CHRISTIAN  | AKROYD    |                 4 |
|       24 | CAMERON    | STREEP    |                 4 |
|      129 | DARYL      | CRAWFORD  |                 4 |
|       83 | BEN        | WILLIS    |                 4 |
|       37 | VAL        | BOLGER    |                 4 |
+----------+------------+-----------+-------------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

select actor.actor_id, actor.first_name, actor.last_name, count(film_category.category_id) as comedy_film_count
from ((((actor
JOIN film_actor ON actor.actor_id= film_actor.actor_id)
JOIN film ON film_actor.film_id = film.film_id) 
JOIN film_category ON film.film_id = film_category.film_id)
JOIN category ON film_category.category_id = category.category_id)
where category.name = 'Comedy'
group by actor_id
order by count(film_category.category_id) DESC
LIMIT 10;

5. Obtener la lista de actores que NO han participado en ninguna película de categoría "Comedy":

Salida:
```
+----------+------------+-------------+
| actor_id | first_name | last_name   |
+----------+------------+-------------+
|        3 | ED         | CHASE       |
|        6 | BETTE      | NICHOLSON   |
|        7 | GRACE      | MOSTEL      |
|        8 | MATTHEW    | JOHANSSON   |
|        9 | JOE        | SWANK       |
... Se corta por longitud
|      189 | CUBA       | BIRCH       |
|      190 | AUDREY     | BAILEY      |
|      195 | JAYNE      | SILVERSTONE |
|      200 | THORA      | TEMPLE      |
+----------+------------+-------------+

53 filas total
```
Respuesta:
```sql
select actor.actor_id, actor.first_name, actor.last_name
from ((((actor
JOIN film_actor ON actor.actor_id= film_actor.actor_id)
JOIN film ON film_actor.film_id = film.film_id) 
JOIN film_category ON film.film_id = film_category.film_id)
JOIN category ON film_category.category_id = category.category_id)
where category.name <> 'Comedy'
group by actor_id


```
