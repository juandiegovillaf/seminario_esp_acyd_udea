# Habitantes de calle
------------
**Autores:**
- Laura Estefania Noriega
- Juan Diego Villa

**Tabla de contenido**

[TOC]
## Propuesta de Proyecto
Precisar los alcances del fenómeno de la población habitante de calle desde una perspectiva social y de salud pública en el territorio de Medellín.

## Objetivo del Proyecto
Clasificar y segmentar la población de los habitantes de calle del municipio de Medellín para predecir patrones de comportamiento y la probabilidad de que una persona termine en situación de calle con base en características y factores que determinan esta condición, y así contribuir a la toma de decisiones con respecto a las necesidades que se visibilizan dentro de este grupo poblacional.


## Ejecución del notebook
A continuación se enlista los pasos que son necesario para la ejecución del Notebook

### NoteBook
El notebook se encuentra en la carpeta [momento evaluativo 3](), en la que se encuentra el archivo de extensión  `py`, el cual contine toda la estructura para el análisis. En la carpeta [data](https://github.com/juandiegovillaf/seminario_esp_acyd_udea/tree/main/data) se encuentran los datos asociados al análisis, tanto la base de datos codificada, como la metadata que contiene la descripción y diccionario de cada una de las variables que se presentarán a continuación en este numeral.

#### Estrucura del notebook
- Cargue de librerías
- Definición de Multiple_plot 
- Lectura de datos y metadata
- Imputación de valores para algunas variables.
- Asignar etiquetas a los códigos por variable  para análisis.
- Reemplazar variables de tipo `NaN` por `None`
- Convertir variables cuantitativas
- Análisis descriptivo de las variables númericas
- Concatenar mismas variables en una.
- Análisis descriptivo por diagrama de barras para variables cualitativas
- Verificar registros duplicados
- Verificar valores faltantes (nulos) 
- Eliminación de encuestas incompletas

### importar librerías
Se comienza por la importación de librerías, en caso de que alguna no esté instalada se debe instalar así, por ejemplo:
```python
#en caso de una librería
pip install pandas 
#en caso de dos o más librerías separar por espacio
pip install wsgiref boto numpy
```

###Dataset
El data set cuenta en su forma natural con 129 variables y 13252 registros, esta se encuentra en la carpeta `data`, pero al ser un repositorio público, esta información puede ser consultada a través de un link como el siguiente ejemplo:

```python
link_dataset_csv   = 'https://raw.githubusercontent.com/juandiegovillaf/seminario_esp_acyd_udea/main/data/CHC_2019.csv'
```
###Metadatos
El archivo de metadatos es de extensión `json`, el cual contiene la información de cada una de las variables, ya que cada una por su extensión, tiene un código asociado. También en el se encuentra el diccionario asociado a cada uno de las codificaciones que tiene las variables cualitativas.  Un ejemplo ilustrativo con la variable _departamento_:

```json
//Ejemplo ilustrativo:
"P1":{
      "descripcion":"1. Departamento",
      "diccionario":{
         "05":"Antioquia",
         "08":"Atlantico",
         "17":"Caldas",
         "68":"Santander",
         "76":"Valle del cauca"
      }
   },
```
###  Imputación de valores 
La imputación de variables se realizó porque la naturaleza de algunas variables es  solo indicar una categoría, de lo que se tiene indicio de que era una pregunta de selección multiple, por lo que cada opción es una variable, así que las que no fueron seleccionadas, se convirtieron en "no".


### Reemplazar variables de tipo `NaN` por `None`
Dado la mayoría de variables cualitativas, los valores de tipo `NaN` son para de tipo númerico, sin embargo estas se pasan a  `None` para su posterior tratamiento.


### Convertir variables cuantitativas
Algunas variables tienen por naturaleza ser cuantitativa, por lo que se hace la conversión de estas.

### Eliminación de variables
| Código  |Variable   |Razón de eliminación   |
| ------------ | ------------ | ------------ |
|DIRECTORIO|DIRECTORIO| Identificación de la encuesta|
|TIP_FOR|Formulario aplicado en|No aporta información|
|P2|2. Clase|Solo hay una categoría|
|CTL_1|Tipo de diligenciamiento del cuestionario|No aporta información|
|P8R|8. ¿Cuántos años cumplidos tiene usted?|Recodificación en otra variable|
|P9|9. ¿Usted es hombre o mujer?|Recodificación en otra variable|
|P15R|15. ¿De acuerdo con su CULTURA, PUEBLO, O RASGOS FISICOS, usted es o se reconoce como:|Mala codificación |
|P20|20. ¿Usted ha sido diagnosticado con alguna de las siguientes enfermedades:|Encabezado de sección de respuestas|
|P23|23. ¿Cuánto tiempo lleva usted viviendo en la calle? Años|Encabezado de sección de respuestas|
|P26|26. ¿Usted recibe algún tipo de ayuda:|Encabezado de sección de respuestas|
|P30|30. Actualmente, ¿usted consume:|Encabezado de sección de respuestas|
|P33|33. Su seguridad en la calle se ha visto afectada por|Encabezado de sección de respuestas|
|P33_2|33.2 ¿En los últimos 30 días, usted ha sido victima de|Encabezado de sección de respuestas|
|P35|35. Sexo:|Recodificación en otra variable|
|P36R|36. Edad estimada|Recodificación en otra variable|
|P37S1|37. La entrevista no se realizó porque la persona estaba: Muy alterada por el efecto de sustancias psicoactivas|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S2|37. La entrevista no se realizó porque la persona estaba: Dormida|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S3|37. La entrevista no se realizó porque la persona estaba: Con actitud agresiva|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S4|37. La entrevista no se realizó porque la persona estaba: Aparentemente con problemas de salud mental|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S5|37. La entrevista no se realizó porque la persona estaba: Totalmente desinteresada|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S6|37. La entrevista no se realizó porque la persona estaba: Hay condiciones de riesgo para los encuestadores|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|P37S7|37. La entrevista no se realizó porque la persona estaba: Otra|Se eliminarán las encuestas incompletas, por lo que esta variable pierde relevancia|
|COMPLETA|Identificador de finalización de la encuesta|Luego de filtrar las encuestas completas, se elimina por falta de información|

### Concatenar mismas variables en una.
En la encuesta, algunos participantes optaron por no responder a variables como la edad y el sexo. Para estos casos, el encuestador, basándose en su criterio experto, asignó el sexo y estimó la edad aproximada de los encuestados. De este modo, se creó una nueva variable que combina ambas columnas.



### Verificar valores faltantes (nulos)
Se verifican la cantidad de valores nulos en cada una de las variables para tener presente que tratamiento se les dará

### Análisis descriptivo de las variables númericas
Se realiza un análisis descriptivo por medio de una matriz de correlación, en la que se valida la relación de las variables numéricas.

 ### Análisis descriptivo por diagrama de barras para variables cualitativas
 Por la metodología de diagramas descriptivos se valida si hay presencia de otras variables que sean dominantes con respecto a otras categorías. Si se presenta esta situación, se considera ser eliminada.



### Segunda depuración de variables
Se hace un segundo barrido de eliminación  de variables y a continuación se muestran las variables que será eliminados y su razón.

| Código  |Variable   |Razón de eliminación   |
| ------------ | ------------ | ------------ |
|P12|12. ¿En qué municipio duerme usted habitualmente?| La concentración de la respuesta está sobre una categoría|
|P17S4|17.1 ¿Cuál(es): Intento de suicidio?| la respuesta se centra en una categoría específica|
|P17S7|17.1 ¿Cuál(es): Enfermedades de transmisión sexual (venéreas)?| La respuesta se centra en una categoría específica|
|P19|19. ¿Lo atendieron?|Es variable dependiente de la P18 y la frecuencia de ella es baja|
|P20S3|20. ¿Usted ha sido diagnosticado con alguna de las siguientes enfermedades: 3. Cáncer?| La respuesta se centra en una categoría específica|
|P20S5|20. ¿Usted ha sido diagnosticado con alguna de las siguientes enfermedades: 5. VIH- SIDA| La respuesta se centra en una categoría específica|
|^P32S|Catergorías de por qué no utiliza los programas donde se atiende a los habitantes de la calle | Por frecuencias bajas en las personas que sí conoce estos programas.

### Verificar registros duplicados
Se verifican si hay presencia de valores duplicados en la base de datos. Al validar luego de la depuración de variables, se entiende que son participantes que tienen las mismas respuestas, no se eliminarán ya que estos datos "duplicados" le da peso a cierto grupo que pueden cumplir con ciertas carecteristicas comunes entre ellos.


### Validar valores atipicos
En la edad, que es de las variables que tenemos cuantitativas, se valida si hay presencia de valores atípicos mediante la metología gráfica de diagrama de cajas y bigotes.
