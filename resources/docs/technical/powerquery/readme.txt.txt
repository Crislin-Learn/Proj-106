POWER QUERY
===========

Esta carpeta contiene los componentes de Power Query utilizados para
la extracción, transformación y preparación de los datos del proyecto.

Estructura:

Functions/
Funciones M reutilizables que encapsulan procesos de transformación
que pueden ser utilizados por diferentes consultas.

Parameters/
Parámetros utilizados para controlar valores configurables del proyecto,
como rutas de archivos, fuentes de datos u otros valores que puedan
cambiar entre entornos.

Queries/
Consultas M propias del proyecto. Aquí se incluyen las consultas de
staging, transformación y salida utilizadas para construir el modelo.

Templates/
Consultas o patrones de código M preparados para ser reutilizados como
punto de partida en futuros proyectos. No necesariamente forman parte
del modelo del proyecto actual.