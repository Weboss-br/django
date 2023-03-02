# django

Trata-se de um projeto pessoal de estudo do Django.

Utiliza-lo no .venv

* Para criar o AmbienteVirtual(Virtualenv)

python -m venv ./venv

Ativar a venv:
.\.venv\Scripts\activate

Instalar o Django: 
pip instal django

Vamos criar um arquivo requirements.txt para salvar as dependencias do projeto/venv.
pip freeze > requirements.txt

Dessa forma, se precisar usar o venv, utilize o "pip install -r requirements.txt"

Adicionar o .gitignore: .venv

Ok, tudo pronto para criar o projeto:

django-admin startproject controle_gastos .
(chamado)     (comando)    (nome do projeto) *ponto para ele não criar uma pasta raiz do projeto com o mesmo nome

O Django é baseado em projetos e várias APP`s. Então, vamos Criar a primeira APP:
python manage.py startapp contas
(chamando python)
        (arquivo a ser 'executado')
                (cria app) (nome do app)
*Ele deve criar uma pasta com o nome do app. Nesse caso contas.

Agora vamos registrar o app no aplicativo. Vá em controle_gastos/Settings.py e em "INSTALLED_APPS" coloca o nome do app. Nesse caso, contas.

Banco de dados: 
Agora vamos criar nosso BD:
python manage.py migrate

Ele criou o BD com várias tabelas. Permissions, Usernames, Sessions, etc

Executar nossa aplicação pela primeira vez:
python manage.py runserver

Depois vou criar um super usuário. O Django já cria isso com a app instalada django.contrib.admin
python manage.py createsuperuser

admin
123456

Agora vamos para as Rotas. A primeira rota, já configurada é a Admin. Elas são configuradas no arquivo url.py em urlpatterns

Para criar a segunda view, primeiro faremos a rota. 

No arquivo: controle_gastos/url.py
from contas.views import home

Depois colocar o path(rota): 
urlpatterns
        path('contas/', home),

Agora vamos para: contas/views.py

Exemplo dado na documentação do Django: 
        from django.http import HttpResponse
        import datetime

        def current_datetime(request):
        now = datetime.datetime.now()
        html = "<html><body>It is now %s.</body></html>" % now
        return HttpResponse(html)

No caso iremos trocar o "current_datetime" por "home", pois foi assim que nomeamos ela anteriormente.
Agora pode acessar o http://127.0.0.1:8000/contas/

Ele deverá mostrar a data atual do sistema.

TEMPLATES:
Nas Views.py
Para iniciar, já comenta o #html = "<html><body>It is now %s.</body></html>" % now
                           #return HttpResponse(html)


    return render(request, 'contas/home.html')

Dentro da APP(contas), criar uma pasta chamada "templates" e outra com o mesmo nome do app(contas).
Isso evita que se for colocado em outro template de outro app um arquivo de mesmo nome, que haja conflito. 

url.py
Já que chamamos de home, vamos subtituir "contas" por home
     path('home/', home)

MODELS: 

Na pasta de APP arquivo models.py

Separar os gastos por categoria, então vamos criar uma classe:

class Categoria()

Todas as classes precisam herdar de models.Model:

class Categoria(models.Model):
    nome = models.CharField(max_length=100)
    dt_criacao = models.DateTimeField(auto_now_add=True)

Ele inseriu fields de tamanho max e data de criação automática. 

Agora vamos inserir no BD. Desativando o runserver, faça o seguinte comando: 
python manage.py makemigrations

Ele vê tudo que foi criano de novo no projeto e cria um arquivo migrations, dentro da pasta migrations, (0001_initial) e ele descreve como django deve criar as tabelas no BD. 