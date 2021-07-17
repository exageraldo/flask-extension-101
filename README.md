# Flask Extension 101

# Apresentação

<img height="auto" width="200" style="border-radius:50%" src="foto-em-familia.jpg" alt="Fotografia batida em formato de selfie.
Descrição da foto da esquerda para direita: eu, Geraldo Castro, homem branco de barba escura curta e cabelos escuros amarrado pra trás. Na foto estou usando um casaco preto e óculos e estou sorrindo; no meio está nosso cachorro Xicó. Ele é tamanho médio e o pelo dele é longo, de cor caramelo. Ele está usando uma roupinha de croche vermelha e está sentado em uma cadeira de escritório preta, olhando para a camera na hora da foto; e na ponta esquerda está minha namorada, Luanda Dantas, mulher branca de cabelos cacheados presos em formato de coque e usando fones de ouvido preto. Ela está usando óculos e casaco escuro e está sorrindo para a foto.">

- Geraldo (que não é Geraldo) Castro
- Pessoa Desenvolvedora de Software @ ThoughtWorks
- Mossoró/RN
- Grupy-RN
- Vegetariano & Diabético

# Considerações iniciais

## Ponto de partida

Para melhor compreender esse conteudo, eu recomendo que você possua:

- Entendimento de código Python (recomendo o curso [Python para Zumbis](https://www.youtube.com/playlist?list=PLUukMN0DTKCtbzhbYe2jdF4cr8MOWClXc), um curso básico de programação em python)
- Conhecimento/experiencia mínima no framework (recomendo o tutorial do Miguel Grinberg, [Flask Mega Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). UPDATE: [Flask 2.0 e mais](https://blog.miguelgrinberg.com/post/flask-mega-tutorial-update-flask-2-0-and-more); em inglês).

## Sobre o que vamos conversar

- [Entendendo Extensões](#Entendendo-extensões)
    - [O que é uma extensão](#O-que-é-uma-extensão)
    - [O que não é uma extensão](#O-que-não-é-uma-extensão)
    - [Como encontrar](#Como-encontrar)
    - [Exemplos](#Exemplos-de-extensões)
    - [Usando uma extensão](#Usando-uma-extensão)
    - [Sobre criar uma extensão](#Sobre-criar-uma-extens%C3%A3o)
- [Construindo nossa própria extensão](#Construindo-nossa-própria-extensão)
    - [Anatomia de uma extensão](#Anatomia-de-uma-extensão)
    - [Estrutura do projet](#Estrutura-do-projeto)
    - [Copiango e colando as configurações](#Copiando-e-colando-as-configurações)
    - [Hello, FlaskExt](#Hello,-FlaskExt)
    - [FlaskExt](#FlaskExt)

## Configurações

O único pacote necessário para esse tutorial é o próprio Flask.

Mas atenção: sugiro instalar uma versão anterior a 2.0.

```python
pip install flask==1.1.2
```

## TL;DR

- Entender o que é e como funciona uma Extensão do Flask
- Conseguir dar seus primeiros passos no desenvolvimento de suas próprias extensões

```python
from flask import Flask

app = Flask(__name__)
api = FlaskExt(app)

@api.get('/')
@api.get('/<user>')
def hello_world(user=None):
    if not user:
        return {'hello': 'world'}
    return {'hello': user}
```

## Nota sobre o Flask 2

- [Notas de lançamento](https://flask.palletsprojects.com/en/2.0.x/changes/#version-2-0-0) (em ingles)
- [Pull Request #3907 - Add syntatic sugar for route registration](https://github.com/pallets/flask/pull/3907/files)
- Iniciei esse material em 2019!

# Entendendo extensões

Flask é um micro web-framework construido em cima de tecnologias como:
- [Werkzeug](https://werkzeug.palletsprojects.com/en/2.0.x/): é uma biblioteca abrangente/compreensiva de aplicações WSGI.
- [Jinja](https://jinja.palletsprojects.com/en/3.0.x/): é uma engine/motor rápida, expressiva e extensível de templates/modelos.
- [itsdangerous](https://github.com/pallets/itsdangerous): conjunto de ferramentas que ajudam a passar os dados para ambientes não confiáveis e para recuperá-los sãos e salvos.
- [click](https://click.palletsprojects.com/en/8.0.x/): biblioteca para criar interfaces de linha de comando (CLI) de uma forma compreensível e com o mínimo de linhas de código possivel.

O fato de ser "micro" não limita nos de crescer.

Sendo um microframework, o Flask freqüentemente requer alguns passos repetitivos para que bibliotecas de terceiros funcionem.

## O que é uma extensão

As <u>extensões</u> são pacotes extras que acrescentam funcionalidade a uma aplicação Flask.

Por exemplo, uma extensão pode adicionar suporte para envio de e-mail ou conexão a um banco de dados. Algumas extensões adicionam novas estruturas inteiras para ajudar a construir certos tipos de aplicações, como uma API REST.

[Flask Doc](https://flask.palletsprojects.com/en/2.0.x/extensions/#extensions) - Tradução livre

O grande ecossistema de extensões do Flask facilita às desenvolvedoras a construção de recursos comuns de aplicações web, tais como autenticação, operações de banco de dados e APIs, mesmo que o suporte não seja nativo do proprio framework.

O projeto é por escolha um contraste da abordagem de "baterias inclusas" do Django.

A decisão de projeto de cada estrutura é uma abordagem viável, dependendo das necessidades e exigências da aplicação que você está construindo.

[Flask Extensions, Plug-ins and Related Libraries](https://www.fullstackpython.com/flask-extensions-plug-ins-related-libraries.html) - Tradução Livre

## O que não é uma extensão

|   | É uma extensão? |
|---|---|
| [boto3](https://github.com/boto/boto3)  | não |
| [alembic](https://github.com/sqlalchemy/alembic)  | não |
| [sqlalchemy](https://github.com/sqlalchemy/sqlalchemy)  | não |
| [flask-sqlalchemy](https://github.com/pallets/flask-sqlalchemy)  | sim |
| [flask-migrate](https://github.com/miguelgrinberg/Flask-Migrate)  | sim |

## Como encontrar

As extensões são normalmente nomeadas iniciando <strike>ou finalizando</strike> com a palavra "`Flask`":
- Flask-Extensao
- <strike> Extensao-Flask</strike> (não costumo encontrar nesse formato)

Você pode procurar no PyPI por pacotes com a tag/etiqueta [`Framework :: Flask`](https://pypi.org/search/?c=Framework+%3A%3A+Flask).

[Flask Doc](https://flask.palletsprojects.com/en/2.0.x/extensions/#finding-extensions) - Tradução livre

## Exemplos de extensões

- [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/): É uma extensão para o Flask que adiciona suporte para SQLAlchemy à sua aplicação.
- [Flask-Login](https://flask-login.readthedocs.io/en/latest/): Fornece gerenciamento de sessão de usuário para Flask.
- [Flask-Ask](https://github.com/johnwheeler/flask-ask): é uma extensão para a construção das habilidades/skills da Alexa (Amazon) usando um estilo familiar de organização das funções do Flask.
- [Flask-Debug-toolbar](https://github.com/flask-debugtoolbar/flask-debugtoolbar): uma barra de ferramentas sobreposta para depuração de aplicações Flask.

## Usando uma extensão

<b><u>Consulte a documentação de cada extensão para instalação, configuração e instruções de uso.</u></b> Geralmente, as extensões puxam sua própria configuração do `app.config` e são passadas em uma instância de aplicação durante a inicialização. Por exemplo, uma extensão chamada "Flask-Foo" pode ser usada desta forma:

```python
from flask_foo import Foo


app = Flask(__name__)
app.config.update(
    FOO_BAR='baz',
    FOO_SPAM='eggs',
)
Foo(app)
```

[Flask Doc](https://flask.palletsprojects.com/en/2.0.x/extensions/#using-extensions) - Tradução livre

Usando o `application factory`:
```python
from flask_foo import Foo


foo = Foo()

def create_app():
    app = Flask(__name__)
    app.config.update(
        FOO_BAR='baz',
        FOO_SPAM='eggs',
    )
    foo.init_app(app)

```

## Sobre criar uma extensão

Apesar do [PyPI](https://pypi.org/search/?c=Framework+%3A%3A+Flask) ter muitas extensões do Flask, você pode não encontrar uma que atenda às suas necessidades.

Se este for o caso, você pode criar a sua própria extensão.

Leia [Desenvolvimento de Extensões para Flask](https://flask.palletsprojects.com/en/2.0.x/extensiondev/) para desenvolver sua própria extensão.

[Flask Doc](https://flask.palletsprojects.com/en/2.0.x/extensions/#building-extensions) - Tradução livre

# Construindo nossa própria extensão

## Anatomia de uma extensão

- Nomenclatura: `flask_algomais` / `Flask-AlgoMais`.
- Garantir que funcione com várias instâncias de aplicação ao mesmo tempo.
- **A extensão deve possuir um arquivo `setup.py` e ser registrada no PyPI.**
- Licença: BSD, MIT ou algua mais livre/liberal.

[Flask Doc](https://flask.palletsprojects.com/en/2.0.x/extensiondev/#anatomy-of-an-extension) - Tradução livre

## Estrutura do projeto

```
flask-ext
├── README.md
├── LICENSE
├── flask_ext.py
└── setup.py
```

- `README.md`: considere colocar aqui todas as informações possiveis sobre o projeto.
- `LICENSE`: a licença selecionada para o projeto.
- `flask_ext.py`: código da nossa extensão.
- `setup.py`: contem as informações de instalação e distribuição da extensão.

## Copiando e colando as configurações

Esse arquivo é muito importante para o projeto.
É nele onde vamos apontar onde está a extensão que será instalada.

```python
from setuptools import setup


setup(
    name='Flask-Ext',
    version='1.0',
    url='http://example.com/flask-ext/',
    license='BSD',
    author='Geraldo Castro',
    author_email='exageraldo@example.com',
    description='Very short description',
    long_description=__doc__,
    py_modules=['flask_ext'],
    zip_safe=False,
    include_package_data=True,
    platforms='any',
    install_requires=[
        'Flask==1.1.2'
    ],
    classifiers=[
        'Framework :: Flask',
        'Environment :: Web Environment',
        'Intended Audience :: Developers',
        'License :: OSI Approved :: BSD License',
        'Operating System :: OS Independent',
        'Programming Language :: Python',
        'Topic :: Internet :: WWW/HTTP :: Dynamic Content',
        'Topic :: Software Development :: Libraries :: Python Modules'
    ]
)
```

## Hello, FlaskExt

Nosso "`Hello, World`" no mundo das extensões:

```python
class FlaskTestExt(object):
    def __init__(self, app=None):
        if app is not None:
            print('Hello')
            self.init_app(app)
    
    def init_app(self, app):
        print('Python Nordeste!')
```

Utilizando:


```python
from flask import Flask

app = Flask(__name__)
FlaskTestExt(app)

# Saida:
# Hello
# Python Nordeste!
```
```python
from flask import Flask

app = Flask(__name__)
test_ext = FlaskTestExt()
test_ext.init_app(app)

# Saida:
# Python Nordeste!
```

## FlaskExt

A ideia dessa extensão é de criar atalhos para adicionar rotas de forma mais explicita/"legivel":

```python
class FlaskExt(object):
    def __init__(self, app=None):
        if app is not None:
            self.init_app(app)
    
    def init_app(self, app):
        self.app = app

    def _method_route(self, method, rule, options):
        return self.app.route(rule, methods=[method], **options)

    def get(self, rule, **options):
        '''A decorator to GET resources.'''
        return self._method_route("GET", rule, options)

    def post(self, rule, **options):
        '''A decorator to POST resources.'''
        return self._method_route("POST", rule, options)

    def put(self, rule, **options):
        '''A decorator to PUT resources.'''
        return self._method_route("PUT", rule, options)

    def delete(self, rule, **options):
        '''A decorator to DELETE resources.'''
        return self._method_route("DELETE", rule, options)

    def patch(self, rule, **options):
        '''A decorator to PATCH resources.'''
        return self._method_route("PATCH", rule, options)
```

### Atenção!

Situações como essa são possiveis de acontecer e o erro acaba não sendo de facil entendimento:

```python
@api.post('/', methods=['GET'])
@api.post('/<user>', methods=['GET'])
def hello_world(user=None):
    if not user:
        return {'hello': 'world'}
    return {'hello': user}
```

Erro:
```
return self.app.route(rule, methods=[method], **options)
TypeError: flask.app.Flask.route() got multiple values for keyword argument 'methods'
```

Podemos ajeitar isso adicionando uma condição no nosso `_method_route`:

```python
    def _method_route(self, method, rule, options):
        if "methods" in options:
            raise TypeError("Use the 'route' decorator to use the 'methods' argument.")

        return self.app.route(rule, methods=[method], **options)
```

## Perguntas?

### Obrigado!