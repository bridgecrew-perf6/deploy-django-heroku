# 🏫 Deploy projeto Django usando Heroku

Tutorial como fazer deploy de um projeto Django usando Heroku [Python + Django + Heroku]

## 🗂️ Sumário

-  [📚 Pré-requisitos](#-conceitos)
Settings

# 📚 Pré-requisitos

- [git](https://git-scm.com/downloads)
- [Cadastrar conta no Heroku](https://signup.heroku.com/)
- [Interface de linha de comando Heroku](https://devcenter.heroku.com/articles/heroku-cli)

# Settings

[The Twelve-Factor App](https://12factor.net/pt_br/)

> 1. Criar pasta `settings` em `blog/`

> 2. Mover `settings.py` para a pasta criada

> 3. Renomear `settings.py` para `base.py`

> 4. Criar novo arquivo chamado `heroku.py`

Conteúdo arquivo `heroku.py`:

```python
    import environ

    from blog.settings.base import *

    env = environ.Env()

    DEBUG = env.bool("DEBUG", False)

    SECRET_KEY = env("SECRET_KEY")

    ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")

    DATABASES = {
        "default": env.db(),
    }
```
Sobrescrevendo as variáveis locais.

> No `desenvolvimento local` as variáveis estão definidas diretamente no arquivo local `base.py`.
> Os valores das variáveis no `ambiente de produção` veem das variáveis de ambiente criadas no mesmo.


> 5. Instalar `Environ`

```shell
pip install django-environ
```

> 6. No arquivo `manage.py`:


alterar:

```python
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'blog.settings')
```

para:
```python
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'blog.settings.base')
```

> 7. No arquivo `base.py`:

alterar:

```python
BASE_DIR = Path(__file__).resolve().parent.parent
```

para:
```python
BASE_DIR = Path(__file__).resolve().parent.parent.parent
```

# WhiteNoise

> É usado para servir os arquivos estáticos, para configurar pasta seguir a documentação.

> [Documentação](http://whitenoise.evans.io/en/stable/index.html)

> Adicionar em `settings.py`:

```python
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'staticfiles'
STATICFILES_STORAGE = 'django.contrib.staticfiles.storage.StaticFilesStorage'
```


```shell
pip install whitenoise --upgrade
```

## gunicorn

> Servidor recomendado para rodar aplicações no servidor Heroku

```python
pip install gunicorn
```

## psycopg2

> Adaptador de PostgreSQL para Python Django

## Django on Heroku

> Instalar biblioteca

```python
pip install django-on-heroku
```

Adicione no início do arquivo `base.py`

```python
import django_on_heroku
```

Adicione ao final do arquivo `base.py`

```python
django_on_heroku.settings(locals())
```

# Requirements

```shell
pip freeze > .\requirements.txt
```

# Criar arquivos

1. Criar arquivo `runtime.txt` contendo:

```txt
python-3.10.1
```

> Específica a versão do python que será rodada

2. Criar arquivo `Procfile` contendo:

> Específica os comandos que serão rodados ao iniciar nossa aplicação

```txt
release: python3 manage.py migrate

web: gunicorn base.wsgi --preload --log-file –
```

# Heroku CLI

1. Abrir powerShell na pasta do projeto

2. Fazer login no heroku

```shell
heroku login
```

3. Criar app no heroku

```shell
heroku create
```

## Variáveis de ambiente

> Configurar variáveis de ambiente

```shell
heroku config:set
```

## 🔖 ALLOWED_HOSTS

> No retorno do comendo `heroku create` copiar a url sem `https://` e utilizar:

```shell
heroku config:set ALLOWED_HOSTS=blograul.herokuapp.com
```

## 🔖 DJANGO_SETTINGS_MODULE

```shell
heroku config:set DJANGO_SETTINGS_MODULE=blog.settings.heroku
```

## 🔖 SECRET_KEY

```shell
heroku config:set SECRET_KEY=Jak@LDDf^1uCJoLLmhjh^Wizp4hz
```

## 🔖 DEBUG

```shell
heroku config:set DEBUG=False
```

## 🔖 DISABLE_COLLECTSTATIC

```shell
heroku config:set DISABLE_COLLECTSTATIC=1
```

# Criar banco de dados Postgres

```shell
heroku addons:create heroku-postgresql:hobby-dev
```

# Colocar aplicação no ar

```shell
git push heroku main
```

# 👨‍💻 Acessando aplicação

```shell
heroku open
```

# Criando super usuário

```shell
heroku run python manage.py createsuperuser
```

> ou

```shell
heroku run bash
```

# Heroku Dashboard

- [Apps](https://dashboard.heroku.com/apps/)

# Alterar nome aplicação

[Alterar em configurações](https://dashboard.heroku.com/apps/blograul/settings)

```shell
heroku git:remote -a newname
```