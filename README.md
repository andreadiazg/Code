# Code
SQL 


QUESTION 1 

with t1 as 
      (SELECT film.title, category.name as Category_name, rental.inventory_id, 
	      inventory.film_id, rental.rental_id
       from inventory
       join rental 
       on inventory.inventory_id=rental.inventory_id
       join film 
       on film.film_id=inventory.film_id
       join film_category
       on film_category.film_id=film.film_id
       join category
       on category.category_id=film_category.category_id
where category.name= 'Animation' or category.name='Children' or category.name='Classics'
      or category.name='Comedy' or category.name='Family' or category.name='Music')

select distinct title, Category_name , 
       count(rental_id) over (partition by title) as rental_count
from t1
order by category_name

QUESTION 2 

SELECT film.title, category.name as Category_name,film.rental_duration,
       ntile(4) over (order by rental_duration) as standard_quartile
       from category
       join film_category  
       on category.category_id=film_category.category_id
       join film 
       on film_category.film_id=film.film_id
where category.name= 'Animation' or category.name='Children' or category.name='Classics'
      or category.name='Comedy' or category.name='Family' or category.name='Music' 

QUESTION 3

with t1 as 
(SELECT film.title, category.name as Category_name,film.rental_duration,
       ntile(4) over (order by rental_duration) as standard_quartile
       from category
       join film_category  
       on category.category_id=film_category.category_id
       join film 
       on film_category.film_id=film.film_id
where category.name= 'Animation' or category.name='Children' or category.name='Classics'
      or category.name='Comedy' or category.name='Family' or category.name='Music')
	  
select category_name, standard_quartile, count (title)
from t1
group by 1,2
order by 1,2
