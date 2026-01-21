# 🚀 Criando uma API REST com Django do Zero

Este guia é um **passo a passo completo** para criar uma **API REST de Todo List usando Django e Django REST Framework**, pensado para **iniciantes** e ideal para salvar no GitHub como material de estudo ou portfólio.

---

## 🎯 Objetivo

Criar uma **API REST funcional** que permita:

* Criar tarefas (Todo)
* Listar tarefas
* Atualizar tarefas
* Deletar tarefas

Tudo isso **sem frontend**, usando apenas Django REST Framework.

---

## 🧱 ETAPA 1 — Preparação do Ambiente

Nesta etapa, preparamos o ambiente de desenvolvimento. Isso garante que o projeto Django rode de forma isolada, sem interferir em outros projetos ou no Python do sistema.

### 1️⃣ Criar a pasta do projeto

```bash
mkdir todo_api
cd todo_api
```

---

### 2️⃣ Criar o ambiente virtual

O **ambiente virtual (venv)** cria um espaço isolado para instalar bibliotecas apenas deste projeto, evitando conflitos com outros projetos Python.

```bash
python3 -m venv venv
```

---

### 3️⃣ Ativar o ambiente virtual

Ativar a venv faz com que todos os comandos `python` e `pip` usem o Python isolado do projeto.

```bash
source venv/bin/activate
```

Se estiver ativo, aparecerá algo como:

```
(venv)
```

---

### 4️⃣ Instalar dependências

Aqui instalamos:

* **Django** → framework web
* **Django REST Framework** → criação de APIs REST

```bash
pip install django djangorestframework
```

---

### 5️⃣ Criar o projeto Django

Este comando cria a **estrutura base do Django**, com configurações globais do projeto (settings, urls, wsgi).

```bash
django-admin startproject core .
```

Estrutura inicial:

```
todo_api/
├── core/
│   ├── settings.py
│   ├── urls.py
├── manage.py
```

---

## 🧩 ETAPA 2 — Criar o App

No Django, o projeto é dividido em **apps**. Cada app representa um módulo do sistema (ex: usuários, tarefas, produtos).

### 6️⃣ Criar o app da API

```bash
python3 manage.py startapp todos
```

---

### 7️⃣ Registrar o app e o DRF

Aqui informamos ao Django quais apps fazem parte do projeto. Se um app não estiver em `INSTALLED_APPS`, o Django simplesmente ignora ele.

📄 `core/settings.py`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'rest_framework',
    'todos',
]
```

---

## 🗄️ ETAPA 3 — Model (Banco de Dados)

Os **models** representam as tabelas do banco de dados. Cada classe vira uma tabela, e cada atributo vira uma coluna.

📄 `todos/models.py`

```python
from django.db import models

class Todo(models.Model):
    titulo = models.CharField(max_length=100)
    concluida = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titulo
```

---

### 8️⃣ Criar e aplicar migrations

* `makemigrations` → cria os arquivos de versionamento do banco
* `migrate` → aplica essas mudanças no banco de dados

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

---

## 🔁 ETAPA 4 — Serializer

O **serializer** converte dados do banco (Python/Django) para JSON e vice-versa. Ele é essencial para APIs REST.

📄 `todos/serializers.py`

```python
from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = '__all__'
```

---

## 🌐 ETAPA 5 — Views (API)

As **views** definem o comportamento da API: o que acontece quando alguém faz um GET, POST, PUT ou DELETE.

📄 `todos/views.py`

```python
from rest_framework.viewsets import ModelViewSet
from .models import Todo
from .serializers import TodoSerializer

class TodoViewSet(ModelViewSet):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
```

---

## 🔀 ETAPA 6 — Rotas (URLs)

As rotas ligam a URL da requisição à view correta. Aqui usamos o **router do DRF**, que cria o CRUD automaticamente.

📄 `todos/urls.py`

```python
from rest_framework.routers import DefaultRouter
from .views import TodoViewSet

router = DefaultRouter()
router.register('todos', TodoViewSet)

urlpatterns = router.urls
```

📄 `core/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('todos.urls')),
]
```

---

## ▶️ ETAPA 7 — Rodar o Servidor

Este comando inicia o servidor local de desenvolvimento do Django.

```bash
python3 manage.py runserver
```

Acesse no navegador:

```
http://127.0.0.1:8000/api/todos/
```

🎉 API funcionando!

---

## 🧪 ETAPA 8 — Testes

O Django REST Framework fornece uma interface web para testar a API sem precisar de ferramentas externas.

A interface do Django REST permite testar:

* GET
* POST
* PUT
* DELETE

Sem precisar de frontend.

---

## 🚀 Desafios para Evolução

Tente fazer **sem olhar este guia**:

1. Adicionar campo `prioridade`
2. Criar validação no serializer (mínimo 3 caracteres)
3. Ordenar por `created_at`
4. Adicionar autenticação (JWT)
5. Documentar com Swagger

---
