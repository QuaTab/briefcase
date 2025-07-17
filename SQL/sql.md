
### 1. Вывести студентов, которые сдавали дисциплину «Основы баз данных», указать дату попытки и результат. Информацию вывести по убыванию результатов тестирования.
- SELECT name_student, date_attempt, result FROM attempt
- JOIN student 
-	ON attempt.student_id = student.student_id
- JOIN subject 
-	ON attempt.subject_id = subject.subject_id
- WHERE name_subject = "Основы баз данных"
- ORDER BY result DESC;

### 2. Вывести название, жанр и цену тех книг, количество которых больше 8, в отсортированном по убыванию цены виде.
- SELECT title, name_genre, price
- FROM book
- JOIN genre 
- ON genre.genre_id = book.genre_id
- WHERE amount > 9
- ORDER BY price DESC;

### 3. Посчитать сколько и каких экземпляров книг нужно заказать поставщикам, чтобы на складе стало одинаковое количество экземпляров каждой книги, равное значению самого большего количества экземпляров одной книги на складе. Вывести название книги, ее автора, текущее количество экземпляров на складе и количество заказываемых экземпляров книг. Последнему столбцу присвоить имя Заказ. В результат не включать книги, которые заказывать не нужно.
- SELECT title, author, amount, 
- (SELECT MAX(amount) FROM book) - amount AS Заказ
- FROM book
- WHERE amount != (
- SELECT MAX(amount)
- FROM book );

### 4.Найдите автора, у которого наибольшее количество постов с тегами. Выведите имя и фамилию автора, количество его таких постов и список этих постов.
with maximum_tags as (
select first_name, last_name, count(1) as counter_tags, author_id
from posts
join post_tag on post_tag.post_id = posts.id
join tags on tags.id = post_tag.tag_id
join authors on authors.id = posts.author_id
group by author_id, first_name, last_name
order by counter_tags desc
limit 1
),

count_posts as (
select count(author_id) as total, author_id from posts
group by author_id
order by total desc
limit 1
)

select authors.first_name as Имя, authors.last_name as Фамилия, title as Сообщение, count_posts.total as Количество_сообщений
from posts 
join maximum_tags on posts.author_id = maximum_tags.author_id
join authors on authors.id = posts.author_id
join count_posts on count_posts.author_id = posts.author_id
