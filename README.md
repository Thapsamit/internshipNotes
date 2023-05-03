

# Django Notes:-


- ### What is signals?
- it can have pre save post save methods,
- for example after user created in database we can give like alert message to users or anything else(action)

- ### What is django's messaging framework


## How to write next in django
- {% url 'delete-skill' skill.skills_uuid %}?next=/account
- what is does is that after going to delete skill and deleting the skill we can navigate to account


## what is _set in djnago models query?

- In Django models, the _set attribute is a way to access related objects from another model using a foreign key or a many-to-many relationship.

- For example, let's say we have a Author model and a Book model, where each book has an author foreign key field that references an author:

```python
class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)

```


- With this setup, we can use the _set attribute on an Author instance to access all related Book instances. For example, if we have an Author instance called author, we can access all related Book instances like this:


```python

author_books = author.book_set.all()

```


- Note that we can customize the name of the related manager by specifying a related_name attribute on the foreign key field. For example, we could define the Book model like this:

```python 

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, on_delete=models.CASCADE, related_name='books')

```

- With this setup, we can access all related Book instances for an Author instance using the books attribute instead of book_set:
```python
author_books = author.books.all()


```

















