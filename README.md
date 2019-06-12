# curso-django
Curso de iniciación al desarrollo de aplicaciones web con Python y Django paso a paso

### Paso 1: Crear proyecto y aplicación Django

- Crear el entorno virtual

        mkvirtualenv (ENV_NAME)

* Activate entorno

		workon (ENV_NAME)

* Instalar Django

		pip install django

* Create el proyecto Django

		django-admin startproject (PROJECT_NAME)
		

	La estructura de ficheros resultante es la siguiente:
	```
	project/
	    manage.py
	    project/
		__init__.py
		settings.py
		urls.py
		wsgi.py
	```
	- Inicia el servidor con el comando `python manage.py runserver` dentro del directorio del proyecto

* Crear la aplicación
		
		
	- Posicionate en el directorio del proyecto  `$ cd <outer_project_folder>`
	- Crear la aplicación:
	
	`python manage startapp (MY_APP)`
	
	- Dentro del directorio de la aplicación, crear un fichero llamado `urls.py`

	La estructura de ficheros resultante es la siguiente:
	```
	project/
	    manage.py
	    db.sqlite3
	    project/
		__init__.py
		settings.py
		urls.py
		wsgi.py
	    app/
		migrations/
		    __init__.py
		__init__.py
		admin.py
		apps.py
		models.py
		tests.py
		urls.py
		views.py
	```
	- Incluir a aplicación dentro del fichero `settings.py` del proyecto, añadiendo el nombre a la lista `INSTALLED_APPS`:
  
	```python
	INSTALLED_APPS = [
		'app',
		# ...
	]
	```
		
* Crear la migración

		python manage.py makemigration
		
* Lanzar la migración	

		python manage.py migrate myapp
		
### Paso 2: Crear la primera vista

- Dentro del directorio de la app, abrir `views.py` y añadir lo siguiente:
	```python
	from django.http import HttpResponse

	def index(request):
	    return HttpResponse("Hello, World!")
	```
- Abrir (o crear) el fichero  `urls.py` y añadir el patrón para la siguiente ruta:
	```python
	from django.urls import path
	from . import views

	urlpatterns = [
	    path('', views.index, name='index'),
	]
	```
- Dentro del directorio del proyecto, editar `urls.py` para incluir la redirección al fichero  `urls.py` de la alpicación:

	```python
	urlpatterns = [
	    path("", include('app.urls')),
	]
	```

- El resultado debe ser el siguiente:

	```python
	from django.contrib import admin
	from django.urls import include, path

	urlpatterns = [
	    path('app/', include('app.urls')),
	    path('admin/', admin.site.urls),
	]
	```

- De este modo tenemos un `urls.py` en cada directorio de nuestras aplicaciones gestionados desde el `urls.py` del directorio del proyecto.

### Paso 3: Crear el modelo 

- Editar el fichero `models.py` de la aplicación creando las clases Empresa y Trabajador:
	```python
	from django.db import models

	class Empresa(models.Model):
	   # No es necesario crear un campo para la Primary KEy, Django creará automáticamente un IntegerField.
	   nombre = models.CharField(max_length=50)
	   cif = models.CharField(max_length=12)

	class Trabajador(models.Model):
	   # No es necesario crear un campo para la Primary KEy, Django creará automáticamente un IntegerField.
	   # Campo para la relación one-to-many
	   empresa = models.ForeignKey(Empresa, on_delete=models.CASCADE)
	   nombre = models.CharField(max_length=40)
	   fecha_nacimiento = models.DateField()
	   # Es posible indicar un valor por defecto mediante 'default'
	   antiguedad = models.IntegerField(default=0)
	```

- Aplica los cambios realizados en el modelo mediante los siguientes comandos:
	```
	$ python manage.py makemigrations <app_name>
	$ python manage.py migrate
	```

### Paso 4: Añadir y consultar registros desde la API

	```python
	>>> from GestorRedesWebApp.models import Empresa, Trabajador
	>>> empresa = Empresa(nombre="Egibide", cif="26136015B")
	>>> empresa.save()
	# Django le asigna un id.
	>>> empresa.id
	1
	# Acceder a sus atributos
	>>> empresa.nombre
	Egibide
	>>> Empresa.objects.all()
	<QuerySet [<Empresa: Empresa object (1)>]>
	>>> Empresa.objects.filter(nombre__contains='Egibide').count()
	1
	```

### Paso 5: Uso de la aplicación de Administración

- Crear un usuario para la aplicación de Administración 

	```
	python manage.py createsuperuser
	```

- Iniciar el servidor y entrar

	```
	python manage.py runserver
	```

- Entrar en la aplicación http://127.0.0.1:8000/admin 

### Paso 6: Mostrar información mediante vistas

TODO


