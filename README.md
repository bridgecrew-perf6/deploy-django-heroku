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

    from aeroclube.settings.base import *

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

> 6. No arquivo `wsgi.py` e `manage.py`:


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

