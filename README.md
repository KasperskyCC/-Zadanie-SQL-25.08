# Zadanie 25.08

## 1. Wyświetl listę książek
```
SELECT *
FROM books;
```

## 2. Wyświetl książki alfabetycznie
```
SELECT *
FROM books
ORDER BY title;
```

## 3. Wyświetl tylko wypożyczone książki
```
SELECT *
FROM books
WHERE rented=true;
```

## 4. Ile jest w tym momencie wypożyczonych książek
```
SELECT COUNT(id)
AS Ile_jest_w_tym_momencie_wypożyczonych_książek
FROM books
WHERE rented=true;
```

## 5. Ile jest książek danego autora
```
SELECT COUNT(id)
AS Ile_jest_książek_danego_autora
FROM authors
WHERE name='dany autor';
```

## 6. Która książka była wypożyczana najwięcej razy
Zakładając, że każda dodana książka to unikalna pozycja:
```
SELECT books.title AS title, 
COUNT(rentals.book_id) AS top_rented_b
FROM books 
INNER JOIN rentals ON books.id = rentals.book_id 
GROUP BY books.title, rentals.book_id 
HAVING COUNT(rentals.book_id) = (
SELECT MAX(top_rented_b)
FROM (
SELECT book_id,COUNT(rentals.book_id)
AS top_rented_b
FROM rentals GROUP BY book_id)rentals
);
```
Zakładając, że dodane książki mogą się powielać:
```
SELECT books.title AS title,
COUNT(rentals.book_id) AS top_rented_b
FROM books
INNER JOIN rentals ON books.id = rentals.book_id
GROUP BY books.title
HAVING COUNT(rentals.book_id) = (
SELECT MAX(top_rented_b)
FROM (
SELECT books.title AS title,
COUNT(rentals.book_id) AS top_rented_b
FROM books
INNER JOIN rentals ON books.id = rentals.book_id
GROUP BY books.title)books);
```

## 7. Która książka miała co najmniej 2 wypożyczenia
Zakładając, że każda dodana książka to unikalna pozycja:
```
SELECT books.title AS title,
COUNT(rentals.book_id) AS top_rented_b
FROM books
INNER JOIN rentals ON books.id = rentals.book_id
GROUP BY books.id
HAVING COUNT(rentals.book_id) >=2
```
Zakładając, że dodane książki mogą się powielać:
```
SELECT books.title AS title,
COUNT(rentals.book_id) AS top_rented_b
FROM books
INNER JOIN rentals ON books.id = rentals.book_id
GROUP BY books.title
HAVING COUNT(rentals.book_id) >=2
```
