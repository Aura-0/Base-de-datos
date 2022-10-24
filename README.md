# Base-de-datos
Analisis de bases de datos, tarea 2 de pandas y  2 parcial
Las mejores ciudades para vivir
La información con cuál se realizará el siguiente analisis descriptivo es parte de un estudio que hizo Arcadis en colaboración de el Centre for economics and Business Research, en donde buscaban hacer un ranking para encontrar cuales son las mejores ciudades para vivir y que cuiden los siguientes 3 aspectos:

Calidad de vidad de las personas
Relación con el medio ambiente
Salud economica de la ciudad
Tomando en cuenta lo anterior Arcadis realizo un ranking del promedio sobre 100 ciudades de diferentes partes del mundo, con ayuda de 32 indicadores que no son representados en nuestra base de datos.

Información adicional proporcionada:

Pais en donde se encuentra
Continente Integrantes: Aura Orozpe Ariadna Lozano
[2]
3 s
#importamos las diferentes librerias que utilizaremos.
import pandas as pd
import matplotlib as plt
import seaborn as sns
[3]
0 s
#Ulizamos pd.read para poder leer la base de datos con la que  que trabajamos
data= pd.read_csv('GreenCities-Data.csv').head(20)
data.head(100)

[4]
0 s
#Las dimensiones con las que cuenta nuestra database, como si fuera una matriz
data.shape
(20, 7)
[5]
0 s
#Nos muestra la información basica de nuestra base de datos
data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 20 entries, 0 to 19
Data columns (total 7 columns):
 #   Column     Non-Null Count  Dtype 
---  ------     --------------  ----- 
 0   city       20 non-null     object
 1   People     20 non-null     int64 
 2   Planet     20 non-null     int64 
 3   Profit     20 non-null     int64 
 4   Overall    20 non-null     int64 
 5   Country    20 non-null     object
 6   Continent  20 non-null     object
dtypes: int64(4), object(3)
memory usage: 1.2+ KB
[6]
0 s
#Resumen estadistico-descriptivo de los atributos numericos con los que contamos
data.describe()

¿Cuales son los 10 paises con mayor calidad de vida?
[7]
0 s
#Seleccionamos la columnas a trabajar
data[["People","Country"]]

[8]
0 s
#queremos saber el ranking por pais, escogemos minimo debido que estan ordenados de 1° lugar al 100°
#De esta manera sabemos cuál es el lugar mas alto que tiene cada pais dentro del ranking
#Esto debido a que hay paises que cuentan con mas de una ciudad dentro de la data
df_people=data.groupby(by = "Country").People.min().sort_values(ascending=True)
df_people
Country
South Korea     1
Netherlands     2
Germany         3
Austria         4
Czechia         6
Sweden         14
Australia      17
Spain          18
France         20
Denmark        24
Switzerland    27
U.K.           37
Singapore      48
China          81
Name: People, dtype: int64
[9]
0 s
#Mostramos el grupo de datos ahora  dentro de un data frame
df_people= pd.DataFrame(df_people)
df_people

[10]
0 s
#Nuestro top 10 de Paises con mayor calidad de vida
df_people.head(10)

2.¿Qué parametros fueron considerados para determinar la calificación?

Los parametros utilizados para darle esa puntación a cada una de las ciudades es:

La tasa de salud
Educación que recibe la población
Desigualdad de los ingresos
Conciliación de la vida laboral y personal
Relación entre asalariados y dependientes.
Interpreten detalladamente el summary-estadistico descriptivo de los atributos numericos
[11]
0 s
#Describe con ayuda a conocer mejor el summary estadístico-descriptivo de los atributos numéricos
data.describe()

Interpretación:
Count= Expresa que hay 100 registros totales
Mean = Promedio, expresa que el promedio de rankings, no es un valor que nos - Sirva ya que valúa las posiciones
STD = Calcula la desviación standar de los datos, como todos son rankings es la misma texto del vínculo
Min = indica que el valor mínimo (primer lugar) del ranking es 1
25% = primer cuartil abarca del número 1 al 25 en posición
50% = Segundo cuartil del 25 al 50 en posición
75% = Tercer cuartil del 50 al 75 en posición
Max = indica que el valor máximo (último) del ranking es el 100
Boxplot del análisis descriptivo de los datos
[12]
0 s
#Ralización de boxplot para saber la distribución de los datos
data.plot(kind="box")

Calcula la media y varianza de los paises con mayor población.¿Cuál es la media poblacional de paises con mayor calidad de vida?
[13]
0 s
#Creamos nueva tabla ordenada ascendentemente en base a People 
data1 = data.groupby("People")
data1 = data.sort_values(["People"],ascending=True)
data1

[14]
0 s
#Eliminamos los valores (columnas) que no nos sirven y sólo dejamos People y Country(qué sera nuestra columna de unión)
data1 = data1.drop(["Planet", "city", "Profit", "Overall", "Continent"], axis=1)
data1

[15]
0 s
#Creamos dos listas una de países y otra de sus habitantes y lo convertimos en un data frame 
#Este dataframe contiene los mismos valores de country que el data frame original
df_población = pd.DataFrame()
paises = ["China", "South Korea", "Netherlands", "Germany", "Austria", "Czechia", "Sweden", "Australia", "Spain", "France", "Denmark", "Switzerland", "U.K.", "Singapore"]
Población_millones = [1389000000,51269183 , 17440000, 83783945, 8978929, 10708982, 1054236, 25499881, 47435597,65273512 , 5873420, 5792203, 67081000, 5454000] 
df_población["Country"] = paises 
df_población["Habitantes"] = Población_millones
df_población

[16]
0 s
#Ordenamos la tabla de mayor a menor 
#Aquí tenemos nuestro TOP 10 países más poblados 
data_order = df_población.sort_values(["Habitantes"],ascending=False)
data_order

[17]
0 s
#Unimos l df anterior con el data frame que habíamos reducido previamente
df_master = pd.merge(data1,data_order,left_on="Country",right_on="Country")
df_master

[18]
0 s
#Ahora sacamos el promedio de los paises en la categoría de people 
Best_Country= df_master.groupby("Country")[["People"]].mean().sort_values(["People"], ascending=True)
Best_Country


[19]
0 s
#Sacamos el promedio de habitantes 
Best_Country2= df_master.groupby("Country")[["Habitantes"]].mean().sort_values(["Habitantes"], ascending=True)
Best_Country2


[20]
0 s
#Como los valores eran muy altos, los dividimos ente un millón 
Best_Country2.Habitantes=Best_Country2.Habitantes/1000000
Best_Country2

[21]
0 s
#Creamos otro df que tenga nuestra columna común (Country) y Overall 
dataoverall = data[["Country" , "Overall"]]
[22]
0 s
#Unimos nuestros dos data frames para obtener country, habitantes y overall
df_mastercomplete = pd.merge(Best_Country2,dataoverall,left_on="Country",right_on="Country")
df_mastercomplete.sort_values(by= "Overall", ascending= True).head(10)

[23]
0 s
#Agrupamos para ahora saber cuales son los mejores paises por un promedio de los factores, en su calidad de vida.
Media_Mejores= data.groupby("Country")[["People"]].mean().sort_values(["People"],ascending=True).head(10)
Media_Mejores

[24]
0 s
#Agrupamos para ahora saber la variación

p_var=df_master.groupby("Country")[["People"]].var().sort_values(["People"],ascending=True)
p_var2= p_var.dropna()
p_var2

5a.Calcula la media y la varianza del parametro Salud Social de los mejoresciudades para vivir ¿Cuál es la medida de las peores ciudades?

[25]
0 s
#Hacemos una agrupación para conocer cual es la medida de las peores ciudades, ordenamos en descendente porque los últimos números son los peores 
df_master.groupby("Country")[["People"]].mean().sort_values(["People"], ascending =False).head()

[26]
0 s
#Variación de las peores ciudades
peores_var=df_master.groupby("Country")[["People"]].var().sort_values(["People"],ascending=False)
peores = peores_var.dropna()
peores

Conclusiones: Podemos observar que China es el peor país para vivir de acuerdo con nuestros valores calculando la media y la varianza. Por otro lado, el Sur de Corea es el mejor lugar para vivir de acuerdo con su capacidad de mantener en las mejores condiciones la calidad de vida de sus habitantes, a pesar de que se encuentra en el quinto lugar de número de habitantes, lo que nos muestra que una población numerosa puede mantener un buen estilo de vida. Para tener un mejor manejo y análisis de la información al momento de sacar los valores para conocer el promedio de habitantes, notamos que obtuvimos un número muy alto, por lo que dividimos esto entre un millón.

[27]
0 s
Best_Country2.plot(kind="bar", title="Población mundial")

[28]
0 s
#Grafica de barras para visualizar la información
df_people.head(10).plot(kind="bar",title = "Top 10 paises con mayor calidad de vida")
