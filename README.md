# Base de Dados Sakila


Este roteiro prático detalha a base de dados Sakila disponibilizada pelo MySQL, que inclui  _schema_ e dados de uma loja de aluguel de DVDs.

Para uso, basta seguir os passos descritos no [guia de instalação](https://dev.mysql.com/doc/sakila/en/sakila-installation.html). Basicamente, você precisa criar o banco usando o script `sakila-schema.sql`. Em seguida, você deve inserir os dados usando o script `sakila-data.sql`.

O link para acessar os [scripts](https://dev.mysql.com/doc/index-other.html) está disponível na página, conforme mostrado na imagem a seguir. 

<img src="img/guia.png" width="800">


Para dowload dos scripts, selecione a opção `sakila database` no menu `Example Databases`. 


<img src="img/dowload_scripts.png" width="800">


O material inclui também detalhes sobre o modelo ER e exemplos de consultas.





## Exemplos de consultas SQL


Alguns livros também baseiam-se nesta base de dados, provendo exemplos de consultas. Os exemplos a seguir são extraídos do  livro [Learning SQL](https://www.oreilly.com/library/view/learning-sql-3rd/9781492057604/).


Recupere o ID, nome e o sobrenome de todos os atores, ordenando por sobrenome e depois nome.

```
SELECT actor_id, first_name, last_name
FROM actor
ORDER BY 3,2
```

Recupere o ID, nome e o sobrenome de todos os atores cujo sobrenome seja igual a 'WILLIAMS' ou 'DAVIS'.

```
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name = 'WILLIAMS' OR last_name = 'DAVIS';
```

Recuper os IDs dos clientes que alugaram um filme em 5 de julho de 2005, sendo uma única linha para cada ID de cliente distinto.

```
SELECT DISTINCT customer_id
FROM rental
WHERE date(rental_date) = '2005-07-05';
```


Liste  todos os clientes cujo sobrenome contém um A na segunda posição e um W em qualquer lugar após o A.

```
SELECT first_name, last_name
FROM customer
WHERE last_name LIKE '_A%W%';
```

Escreva uma consulta que retorne o título de cada filme que possuia um ator cujo o primeiro nome é JOHN.

```
SELECT f.title
FROM film f
INNER JOIN film_actor fa ON f.film_id = fa.film_id
INNER JOIN actor a ON fa.actor_id = a.actor_id
WHERE a.first_name = 'JOHN';
```

Escreva uma consulta que encontre o nome e o sobrenome de todos os atores e clientes cujo sobrenome começa com L.

```
SELECT first_name, last_name
FROM actor
WHERE last_name LIKE 'L%'
UNION
SELECT first_name, last_name
FROM customer
WHERE last_name LIKE 'L%';
```

Escreva uma consulta que encontre o nome e o sobrenome de todos os atores e clientes cujo sobrenome começa com L. Os dados devem ser ordenados pelo sobrenome.

```
SELECT first_name, last_name
FROM actor
WHERE last_name LIKE 'L%'
UNION
SELECT first_name, last_name
FROM customer
WHERE last_name LIKE 'L%'
ORDER BY last_name;
```

Elabore uma consulta que conte o número de linhas na tabela de pagamentos.

```
SELECT count(*) FROM payment;
```

Lista o número de pagamentos realizados por cada cliente, mostrando o ID do cliente e o valor total pago.

```
SELECT customer_id, count(*), sum(amount)
FROM payment
GROUP BY customer_id;
```

Lista o número de pagamentos realizados por cada cliente, mostrando o ID do cliente e o valor total pago, apenas para os clientes que realizaram 40 ou mais pagamentos.

```
SELECT customer_id, count(*), sum(amount)
FROM payment
GROUP BY customer_id
HAVING count(*) >= 40;
```

Localize todos os filmes de ação (category.name = 'Action'). Utilize uma consulta aninhada sem correção (_subselect_).

```
SELECT title
FROM film
WHERE film_id IN
    (SELECT fc.film_id
    FROM film_category fc 
    INNER JOIN category c ON fc.category_id = c.category_id
    WHERE c.name = 'Action');
```


## Referências

Alan Beaulieu. [Learning SQL](https://www.oreilly.com/library/view/learning-sql-3rd/9781492057604/). 3ed, 2020.

[Sakila Sample Database](https://dev.mysql.com/doc/sakila/en/sakila-introduction.html). MySQL. Acesso em Maio, 2023.
