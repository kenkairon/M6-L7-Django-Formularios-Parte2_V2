# M6-L7-Django-Formularios-Parte2_V2
Educativo y de Aprendizaje Personal

---

## Tabla de Contenidos
- [Requisitos](#requisitos)
- [Configuración del Entorno](#configuración-del-entorno)
- [Activación del Entorno](#Activación-del-Entorno)
- [Configuración Inicial](#configuración-inicial)
- [Pasos del Proyecto](#pasos-del-proyecto)
  - [Configuración del Proyecto](#configuración-del-proyecto)
  - [Creación de Vistas y Modelos](#creación-de-vistas-y-modelos)
 

---

## Requisitos

- Python 3.9 o superior
- Django 4.0 o superior
- Bootstrap 5

---

## Configuración del Entorno

1. Crear el entorno virtual:
   ```bash
   python -m venv venv


## Activación del Entorno

2. Activar el entorno virtual:
    ### Windows
    ```bash
    venv\Scripts\activate

## Configuración Inicial
## Instalar Django y Guardar dependencias

3. Intalación Django
    ```bash
    pip install django

4. Instalamos la actualizacion de pip
    ```bash
    python.exe -m pip install --upgrade pip

5. Instalamos Boostrap 5
    ```bash
    pip install django django-bootstrap-v5

## Guardar las dependencias
6. Instalación dependencias
    ```bash
   pip freeze > requirements.txt

## Pasos del Proyecto
7. Crear el Proyecto
    ```bash
    django-admin startproject formulario

8. Ingresar al directorio del Proyecto
    ```bash
    cd formulario

9. Creamos la Aplicación form
    ```bash
    python manage.py startapp form

## Configuración del Proyecto

10. Conectar el proyecto con la aplicación: Agregar 'fom' y ' 'bootstrap5'' en la lista INSTALLED_APPS dentro del archivo formulario/settings.py

    ```bash
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'form',
    'bootstrap5',
    ]

11. Configuración del proyecto: formulario/settings.py en la sección TEMPLATES

    ```bash
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR,'templates'],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    
12. creo una carpeta templates/base.html y agregas {% load static %} al principio lo descomentas y funciona
    ```bash
   <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        {% load bootstrap5 %}
        {% bootstrap_css %}
        <title>{% block title %}Django App{% endblock %}</title>
    </head>

    <body class="bg-light">
        {% include 'include/navbar.html' %}
        {% block content %}

        <div class="container mt-4">

        </div>
        {% endblock %}

    </body>

    </html>

13. Agrege en templates/include/navbar.html
    ```bash
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container-fluid">
            <a class="navbar-brand" href="/">Django App</a>
            <div class="collapse navbar-collapse">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'author_list' %}">Authors</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'book_list' %}">Books</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
## Creación de vistas y modelos

14. en la aplicacion form creamos el fomulario forms.py
    ```bash
    from django import forms
    from .models import Author, Book

    class AuthorForm(forms.ModelForm):
        class Meta:
            model = Author
            fields = ['name']
            widgets = {
                'name': forms.TextInput(attrs={
                    'class': 'form-control',
                    'placeholder': 'Enter author name',
                }),
            }

    class BookForm(forms.ModelForm):
        class Meta:
            model = Book
            fields = ['title', 'author']
            widgets = {
                'title': forms.TextInput(attrs={
                    'class': 'form-control',
                    'placeholder': 'Enter book title',
                }),
                'author': forms.Select(attrs={
                    'class': 'form-select',
                }),
            }
15. form/views.py
    ```bash
    from django.shortcuts import render, redirect
    from .forms import AuthorForm, BookForm
    from .models import Author, Book

    def author_create(request):
        if request.method == 'POST':
            form = AuthorForm(request.POST)
            if form.is_valid():
                form.save()
                return redirect('author_list')
        else:
            form = AuthorForm()
        return render(request, 'form/author_form.html', {'form': form})

    def author_list(request):
        authors = Author.objects.all()
        return render(request, 'form/author_list.html', {'authors': authors})

    def book_create(request):
        if request.method == 'POST':
            form = BookForm(request.POST)
            if form.is_valid():
                form.save()
                return redirect('book_list')
        else:
            form = BookForm()
        return render(request, 'form/book_form.html', {'form': form})

    def book_list(request):
        books = Book.objects.all()
        return render(request, 'form/book_list.html', {'books': books})

16. la creo urls.py forms/urls.py
    ```bash
    from django.urls import path
    from . import views

    urlpatterns = [
        path('authors/', views.author_list, name='author_list'),
        path('authors/new/', views.author_create, name='author_create'),

        path('books/', views.book_list, name='book_list'),
        path('books/new/', views.book_create, name='book_create'),
    ]
17. formulario/urls.py
    ```bash
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('form.urls')),
    ]
18. python manage.py migrate

19. Creamos en el form  templates/form/author_form.html
    ```bash
    {% extends "base.html" %}

    {% block title %}Create Author{% endblock %}

    {% block content %}
        <h1 class="text-center mb-4">Create Author</h1>
        <div class="card p-4 shadow-sm">
            <form method="post">
                {% csrf_token %}
                {{ form.as_p }}
                <button type="submit" class="btn btn-primary">Save</button>
            </form>
        </div>
    {% endblock %}

19. Creamos en el form  form/templates/author_list.html
    ```bash
    {% extends "base.html" %}

    {% block title %}Author List{% endblock %}

    {% block content %}
        <h1 class="text-center mb-4">Authors</h1>
        <ul class="list-group">
            {% for author in authors %}
            <li class="list-group-item">{{ author.name }}</li>
            {% endfor %}
        </ul>
        <a href="{% url 'author_create' %}" class="btn btn-success mt-3">Add new author</a>
    {% endblock %}

20. Creamos en el form  form/templates/book_form.html
    ```bash
    {% extends "base.html" %}

    {% block title %}Create Book{% endblock %}

    {% block content %}
        <h1 class="text-center mb-4">Create Book</h1>
        <div class="card p-4 shadow-sm">
            <form method="post">
                {% csrf_token %}
                {{ form.as_p }}
                <button type="submit" class="btn btn-primary">Save</button>
            </form>
        </div>
    {% endblock %}
21. Creamos en el form  form/templates/book_list.html
     ```bash
    {% extends "base.html" %}

    {% block title %}Book List{% endblock %}

    {% block content %}
        <h1 class="text-center mb-4">Books</h1>
        <ul class="list-group">
            {% for book in books %}
            <li class="list-group-item">{{ book.title }} by {{ book.author.name }}</li>
            {% endfor %}
        </ul>
        <a href="{% url 'book_create' %}" class="btn btn-success mt-3">Add new book</a>
    {% endblock %}

22. form/models.py
    ```bash
    from django.db import models

    class Author(models.Model):
        name = models.CharField(max_length=100)

        def __str__(self):
            return self.name

    class Book(models.Model):
        title = models.CharField(max_length=100)
        author = models.ForeignKey(Author, on_delete=models.CASCADE)
