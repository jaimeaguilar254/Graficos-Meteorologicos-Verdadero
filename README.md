# Graficos-Meteorologicos-Verdadero
practica pandas
#Importacion de librerías necesarias
import pandas as pd  #Se importó esta librería ya que nos permite una libre manipulación de nuestros archivos(txt)
import matplotlib.pyplot as plt #Esta importación de la librería matplotlib nos sirve para expresar nuestras variables en forma gráfica.


inicio=input('Código del observatorio: ') #Se pide al usuario ingresar el código de los archivos importados, que consta solo de tres letras en mayúsculas.
codigo_observatorio='homog_mo_'+inicio    #Se define una variable la cual contiene el nombre que ingreso el usuario precedido por el faltante para completar el nombre del archivo.
df_t=pd.read_table('titulo1.txt',encoding='latin1')  #Lectura del archivo seleccionado por el usuario
df_t.set_index(["Code"],inplace=True) #Se busca el índice 'código' y se lo reemplaza con el código que el usuario ingreso anteriormente
df_T=df_t.loc[inicio] #Esta varaible se utiliza para usar la columna inicial.
name=df_T["Name"] #Se define una varaible la cual reemplaza 'Name' con el mismo método usado anteriormente.
code=str(inicio) #Se transforma a string 
altitud=df_T["Altitude"] #Esta variable contiene la altitud buscada en el archivo seleccionado por el usuario
region=df_T["Climate region"] #En este caso la variable es la región.

# Se creó dos listas para los obersarvatorios que no tienen Temperatura ó Precipitación.
sinT=['SAR'] #Lista con observatorio sin Temperatura.
sinP=['JUN', 'RAG'] #Lista con observatorios sin Precipitación.

### Se crearon tres condiciones para que cada observatorio tenga su grafica de acuerdo a los datos que contenga el archivo seleccionado###
#En esta condicion ingresaran los observatorios que no tienen Precipitacion en sus datos.
if str(inicio) in sinP:
    
    graf = pd.read_csv(codigo_observatorio +'.txt',header=15, delimiter='\s+') #Leemos el archivo y a su vez se especificó que el encabezado comienza en la linea 15, ya que a partir de ahí nos interesa para poder graficar.
    df = pd.DataFrame(graf) #Crea un Data Frame
    mean_t = df['Temperature'].mean() #Se saca la media de la Temperatura que se necesita para la gráfica. 
    
    graf = pd.read_csv(codigo_observatorio +'.txt',header=15, delimiter='\s+')
    df["Year"] = df["Year"].apply(str) #Transforma a string las variable ¨Year¨,¨Month¨ utilizando el .apply(str)
    df["Month"] = df["Month"].apply(str)
    df["Fecha"] = df["Year"] + "-" + df["Month"]
    df["Fecha"]=pd.to_datetime(df["Fecha"]) #Se cambia al tipo, para que el intérprete lo tome y trate como fecha.
    df.drop("Year",axis=1,inplace=True) #Sirve para eliminar lo que se encuentra entre comillas.
    df.drop("Month",axis=1,inplace=True)

    ##Gráfica
    
    fig, ax1 = plt.subplots() #Crea variables para crear la figura, teniendo que dar atributos posteriormente. 
    color = 'red' #Color a usar para la grafica de Temperatura

    ax1.set_ylabel('Temperatura', color=color) #Nombre del eje y.
    ax1.plot(df['Fecha'],df['Temperature'], ls='-', color=color) #Gráfica de los datos 
    ax1.axhline(y= mean_t, linestyle="--", color="darkred",lw=4) #Gráfica de la media de los datos de la Temperatura
    ax1.tick_params(axis='y', labelcolor=color) #Color de los parametros del eje y.
    ax1.set_xlabel('Fecha') #Nombre del eje x.
      
    fig.tight_layout()  
     
    plt.title(df_T["Name"]+ " | "+ code + " | "+df_T["Altitude"]+ " | "+df_T["Climate region"],     #Para las posiciones del título
          fontdict={'family': 'serif',     #Para el ajuste del tipo de letra, color, 
                    'color' : 'black',     #altura, tamaño, localización del titulo 
                    'weight': 'bold',
                    'size': 16},
          loc='center') 
    
    ax1.legend(['Temperatura', 'media_t'],loc= 'upper left') #Este apartado es utilizado para la leyenda, especificando que utilice la variable temperatura al igual que su media, y que su ubicación sea en la parte superior izquierda.
    plt.show()
    
#En esta condicion ingresaran los observatorios que no tienen Temperatura en sus datos.
elif str(inicio) in sinT:
    graf = pd.read_csv(codigo_observatorio +'.txt',header=15, delimiter='\s+') #Leemos el archivo y a su vez se comenzara a trabajar desde la linea 15.
    df = pd.DataFrame(graf) #Crea un Data Frame
    mean_p = df['Precipitation'].mean() #Se saca la media de la Precipitacion. 
    
    graf = pd.read_csv(codigo_observatorio +'.txt',header=15, delimiter='\s+')
    df["Year"] = df["Year"].apply(str)
    df["Month"] = df["Month"].apply(str)
    df["Fecha"] = df["Year"] + "-" + df["Month"]
    df["Fecha"]=pd.to_datetime(df["Fecha"])
    df.drop("Year",axis=1,inplace=True)
    df.drop("Month",axis=1,inplace=True)
    
    #Gráfica
    fig, ax1 = plt.subplots()
    color = 'blue' #Color a usar para la grafica de Precipitacion
    ax1.set_ylabel('Precipitacion', color=color) #Nombre del eje y
    ax1.plot(df['Fecha'],df['Precipitation'], ls='-', color=color) #Grafica de los datos.
    ax1.axhline(y= mean_p, linestyle="--", color="darkblue",lw=4) #Grafica de la media de los datos de la Precipitacion 
    ax1.tick_params(axis='y', labelcolor=color) #Color de los parametros del eje y.
    ax1.set_xlabel('Fecha') #Nombre del eje x.
      
    fig.tight_layout()  
     
    plt.title(df_T["Name"]+ " | "+ code + " | "+df_T["Altitude"]+ " | "+df_T["Climate region"],
          fontdict={'family': 'serif', 
                    'color' : 'black',
                    'weight': 'bold',
                    'size': 16},
          loc='center')
    ax1.legend(['Precipitacion', 'media_t'],loc= 'upper right') #En este apartado se utiliza la variable Precipitación, y la media de la misma, al igual que su ubicación que especifica ser en la parte superior derecha.
    plt.show()
    
#En esta condicion ingresaran los observatorios que tienen Precipitacion y Temperatura en sus datos.
else:
    graf = pd.read_csv(codigo_observatorio +'.txt',header=18, delimiter='\s+')#Leemos el archivo y a su vez se especificó que el encabezado comienza en la linea 18, ya que a partir de ahí nos interesa para poder graficar.
    df = pd.DataFrame(graf) #Crea en forma de un Data Frame
    mean_t = df['Temperature'].mean() #Se saca la media de la Temperatura. 
    mean_p = df['Precipitation'].mean() #Se saca la media de la Precipitacion.
      
    graf = pd.read_csv(codigo_observatorio +'.txt',header=18, delimiter='\s+')
    df["Year"] = df["Year"].apply(str)
    df["Month"] = df["Month"].apply(str)
    df["Fecha"] = df["Year"] + "-" + df["Month"]
    df["Fecha"]=pd.to_datetime(df["Fecha"])
    df.drop("Year",axis=1,inplace=True)
    df.drop("Month",axis=1,inplace=True)
    
    #Grafica
    
    fig, ax1 = plt.subplots()
    
    color = 'red' #Color a usar para la grafica de Temperatura.      
    ax1.set_ylabel('Temperatura', color=color) #Nombre del eje y de la izquierda
    ax1.plot(df['Fecha'],df['Temperature'], ls='-', color=color) #Grafica de los datos de Temperatura.
    ax1.axhline(y= mean_t, linestyle="--", color="darkred",lw=4) #Grafica de la media de los datos de la Temperatura 
    ax1.tick_params(axis='y', labelcolor=color) #Color de los parametros del eje y de la izquierda
    ax1.set_xlabel('Fecha')
    
    ax2 = ax1.twinx()  #Comparten el mismo eje x
    
    color = 'blue' #Color a usar para la grafica de Precipitacion.
    ax2.set_ylabel('Precipitacion', color=color) #Nombre del eje y de la derecha  
    ax2.plot(df['Fecha'],df['Precipitation'], ls='-', color=color) #Grafica de los datos de Precipitacion.
    ax2.axhline(y= mean_p, linestyle="--", color="darkblue",lw=4) #Grafica de la media de los datos de la Precipitacion 
    ax2.tick_params(axis='y', labelcolor=color) #Color de los parametros del eje y de la derecha.
    
    fig.tight_layout()  
        
    plt.title(df_T["Name"]+ " | "+ code + " | "+df_T["Altitude"]+ " | "+df_T["Climate region"],   #Para las posiciones del título
          fontdict={'family': 'serif',           #Para el ajuste del tipo de letra, color,
                    'color' : 'black',           #altura, tamaño, localización del titulo  
                    'weight': 'bold',
                    'size': 16},
          loc='center')    
    
    ax1.legend(['Temperatura', 'media_T'],loc= 'upper left') #En este caso, se especifica graficar varias variables, tanto temperatura como precipitación con sus respectivas medias, y su ubicación.
    ax2.legend(['Precipitacion', 'media_P'],loc= 'upper right') #Cabe recalcar que no se especifica el color, ya que el color está guardado en la variable.
    
    plt.show() #Se utiliza para mostrar la gráfica.
