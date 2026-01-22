# 🚀 Guia de Criação de API com Django Rest Framework (Passo a Passo)

Este documento descreve **cada etapa** da criação de uma API utilizando **Django** e **Django Rest Framework**, com explicações claras do que está sendo feito em cada comando ou arquivo.

---

## 📦 Criando o Ambiente Virtual

```bash
python3 -m venv env
```

🔹 **O que isso faz?**  
Cria um ambiente virtual chamado `env`.  
O ambiente virtual isola as dependências do projeto, evitando conflitos com outros projetos Python instalados na máquina.

---

## ▶️ Ativando o Ambiente Virtual

```bash
source env/bin/activate
```

🔹 **O que isso faz?**  
Ativa o ambiente virtual, fazendo com que todos os pacotes instalados sejam usados apenas dentro desse projeto.

---

## 📥 Instalando Dependências

```bash
pip install djangorestframework
```

🔹 **O que isso faz?**  
Instala o **Django Rest Framework**, biblioteca que facilita a criação de APIs REST com Django (serialização, views, autenticação, etc.).

---

## 🏗️ Criando o Projeto Django

```bash
django-admin startproject nomedoprojeto
```

🔹 **O que isso faz?**  
Cria a estrutura base de um projeto Django, incluindo:
- configurações
- URLs principais
- gerenciamento do projeto

📌 Abra o projeto no **VS Code**.  
Todos os próximos comandos devem ser executados no terminal do VS Code.

---

## ⚙️ Configuração Inicial (`settings.py`)

```python
INSTALLED_APPS = [
    ...
    'nomedaaplicacao',
    'rest_framework',
]
```

🔹 **O que isso faz?**  
Registra a aplicação criada e o Django Rest Framework para que o Django reconheça:
- os models
- serializers
- views
- migrations

---

## ▶️ Ativando o Ambiente Virtual no VS Code

```bash
source ../env/bin/activate
```

🔹 **O que isso faz?**  
Garante que o VS Code esteja usando o ambiente virtual correto para executar o projeto.

---

## 📁 Criando a Aplicação

```bash
python manage.py startapp nomedaaplicacao
```

🔹 **O que isso faz?**  
Cria uma aplicação Django responsável por uma parte específica do sistema (ex: usuários, produtos, pedidos).

---

## 🧱 Models (Representação do Objeto no Banco)

```python
from django.db import models

class NomeDoModel(models.Model):
    atributos
```

🔹 **O que isso faz?**  
Define a estrutura da tabela no banco de dados.  
Cada model representa uma tabela, e cada atributo representa uma coluna.

---

## 🗄️ Migrações do Banco de Dados

```bash
python manage.py makemigrations
```

🔹 **O que isso faz?**  
Cria os arquivos de migração com base nos models.

```bash
python manage.py migrate
```

🔹 **O que isso faz?**  
Aplica as migrações no banco de dados (SQLite3 por padrão), criando as tabelas.

---

## 🔄 Serialização de Dados (JSON)

```python
from nomedaaplicacao.models import nomedoprojeto
from rest_framework import serializers

class NomeDaAplicacaoSerializer(serializers.ModelSerializer):
    class Meta:
        model = nomedoprojeto
        fields = ['atributo1', 'atributo2']
```

🔹 **O que isso faz?**  
Transforma objetos Python (models) em JSON e vice-versa.  
Isso permite que a API envie e receba dados corretamente.

---

## 🌐 Views (Requisições e Respostas)

```python
from nomedaaplicacao.models import nomedoprojeto
from nomedaaplicacao.serializers import nomedoprojetoSerializer
from rest_framework import viewsets

class NomeDoProjetoViewSet(viewsets.ModelViewSet):
    queryset = nomedoprojeto.objects.all()
    serializer_class = nomedoprojetoSerializer
```

🔹 **O que isso faz?**  
Define as operações da API:
- GET
- POST
- PUT
- DELETE  

O `ModelViewSet` já fornece todas essas operações automaticamente.

---

## 🔗 URLs da Aplicação

```python
from rest_framework.routers import DefaultRouter
from nomedaaplicacao.views import NomeDoProjetoViewSet

router = DefaultRouter()
router.register(r'', NomeDoProjetoViewSet)

urlpatterns = router.urls
```

🔹 **O que isso faz?**  
Cria automaticamente as rotas da API com base no ViewSet.

---

## 🌍 URLs do Projeto

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('nomedoprojeto/', include('nomedaaplicacao.urls')),
]
```

🔹 **O que isso faz?**  
Conecta as URLs da aplicação às URLs principais do projeto.

---

## ✅ Conclusão

Ao final dessas etapas, você terá:
- Ambiente virtual configurado
- API REST funcional
- Banco de dados integrado
- Rotas automáticas
- Código organizado e escalável

📌 Ideal para projetos de **cadastro e gerenciamento de usuários**.