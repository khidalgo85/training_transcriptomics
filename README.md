
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- badges: start -->

![forthebadge](https://img.shields.io/badge/GEMM-Building-orange)
![forthebadge](https://forthebadge.com/images/badges/built-with-science.svg)

<!-- badges: end -->

# Pipeline: RNA-Seq <img src="imgs/1.png" align="right" width = "120px"/>

**Autor: MsC. Kelly Hidalgo**

Pipeline para el tratamiento de datos de RNA-seq y ensayos de expresión
diferencial. Para este tutorial serán usados los datos obtenidos en el
artículo [RNA-seq analysis of antibacterial mechanism of *Cinnamomum
camphora* essential oil against *Escherichia
coli*](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7980702/pdf/peerj-09-11081.pdf).
Brevemente, en este estudio fue evaluado el poder bactericida del aceite
esencial de la planta *Cinnamomum camphora* contra *Escherichia coli*
ATCC8739. Fue determinada la concentración mínima inhibitoria (MIC). Con
el fin de evaluar el mecanismo antibacterial del aceite esencial, fue
llevado a cabo un ensayo de expresión diferencial bajo tres
tratamientos, en los cuales fue secuenciado el transcriptoma (RNA-seq)
usando la plataforma Illumina NovaSeq. Las tres condiciones fueron, *E.
coli* tratada con agua, acetona y el aceite esencial en diferentes
concentraciones (1/8, 1/4 y 1/2 MIC).

Los datos pueden ser descargados desde el
[NCBI](https://www.ncbi.nlm.nih.gov/sra?term=SRP289443).

## [**SLIDES**]()

# WORKFLOW

img src=“imgs/workflow.png” align=“center”/&gt;

------------------------------------------------------------------------

# 0. Introducción a UNIX

El shell de Unix es un interpretador de línea de comando. Es una
herramenta poderosa que permite a los usuarios a ejecutar tareas
complejas y poderosas, generalmente con algunas líneas de código. En
este tutorial, usted va entrar en la “pantalla negra” 🖥 , va a aprender
y ejercitar algunos comandos básicos e indispensables, para navegar y se
desenvolver en el *terminal*.

Normalmente la interacción del humano con el computador sucede por medio
de un teclado y un mouse, interfaces gráficas y/o sistemas de
reconocimiento de voz. La manera más comúm de interacturar, es llamada
como interface gráfica de usuário *(**G**raphical **U**ser
**I**nterface)*. Cuando se trabaja en una GUI, las ordenes son dadas
haciendo click con el mouse y usando interaciones orientadas por
diferentes menús. Eso funciona muy bien para escalas pequeñas, pues es
muy intuitivo. Pero ahora imagine que usted necesita ejecutar una tarea
en mil archivos en diferentes carpetas, por ejemplo, copiar la última
línea de todos los archivos y pegarlos en un único archivo. Serían
necesarias muchas horas (tal vez días) ejecutando el proceso y además
podría cometer errores. Es ahí cuando shell es muy útil, ya que por ser
una interface de línea de comando *(**C**ommand **L**ine **I**nterface)*
y un lenguaje de srcipt, permite procesar tareas repetitivas como las
del ejemplo, siendo realizadas de una forma rápida y automática. El uso
de shell es fundamental para el uso de uma amplia variedad de
herramientas ⚒ de bioinformática 🖥. Este tutorial le va a servir para
hacer un uso eficaz de estos recursos.

## 0.1. Login no servidor

Para los usuarios de Windows es necesario la instalación del software
[MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html#:~:text=MobaXterm%20Home%20Edition%20v22.1%0A(Installer%20edition)).

<img src="imgs/SSH_MobaXterm.png" align="center"/>

Después de instalado, clique en la opción *session*, escoja el icono
*ssh* y complete los datos del login: *remote host* (dirección IP).
Después seleccione la opción *specify name* y complete con el nombre del
usuario. Por último será solicitada la contrasenha. Puede salvar los
datos del login.

**Nota**

-   Siempre que ud vea una caja como ésta 👇🏼, es para que usted dígite
    el contenido en la línea de comando y presione \[enter\] ⌨️ para
    **“correr”** el comando.

``` bash
ls
```

## 0.2 Comandos básicos

### `ssh`

`ssh` (Secure Shell) es un protocolo que garantiza que el usuario y el
servidor remoto intercambien informaciones de manera segura e dinámica.
Sirve especificamente para conectarse a un servidor remoto.

    $ ssh -X <user_name>@<ip.address>

### `pwd` (*Print Working Directory*)

Los directorios 📁 son como cajas. Siempre que usted está usando el
shell, usted está dentro de alguna caja **(Directorio)** 📁 de su
computador 💻 o servidor, llamado **directorio actual de trabajo**. Los
comandos solamente leen y graban archivos en el directorio actual de
trabajo (sí usted no indicar otro camino), por lo tanto, es
**importante** :exclamation: saber en cual directorio está antes de
ejecutar un comando. El comando `pwd` muestra donde está.

    pwd

**Output**

     /home/user

### Sintaxis de los comandos

`comando [opciones] [archivo]`

El comando es separado de las opciones (o argumentos, flags) e do
archivo 📄 por un espacio. Los argumentos pueden mudar el comportamiento
del comando. Y el archivo 📄 indica para el comando sobre lo que va a
operar (p.e. archivos 📄 y directorios 📁). A veces los argumentos y el
archivo 📄 son llamados de **parámetros**. Un comando puede tener más de
un argumento y/o archivos 📄 y también podria no tener ninguno de los
dos. Las opciones usualmente tienen un guión y una letra (p.e. `-h`) o
dos guiones y una palabra (p.e. `--help`), **sin** espacio entre lo(s)
guion(es) y la letra/palabra. Vamos a ver con ejemplos prácticos.

**Importante:**:exclamation: el lenguaje usado en UNIX es sensible a
letras mayúsculas e minúsculas (*case sensitive*), es un error común.
**Siempre atento** :exclamation:

### `ls` Listar

Con el comando `ls` ud puede ver (listar) lo que hay dentro del
**directorio actual de trabajo** 📁.

    ls 

**Output**

    Documentos Downloads Imagens

Usted puede usar el **argumento** `-F` para indicar para el comando `ls`
que muestre lo que es cada elemento (directorio o archivo). `/`
significa que es un directorio 📁, el `*` significa que es un archivo
ejecutable y sí no tiene ningún símbolo significa que es un archivo 📄.

El ***flag*** 🚩 `--help` 🆘 es bien importante:exclamation:, y puede ser
usado en **cualquier** comando. El muestra más información sobre el
comando, y como usarlo 🤙🏼. Explore o menú help

Outros **flags** 🚩 interessantes são: `ls -l`, que lista o conteúdo da
pasta 📁 com informações extras, como as permisões, o tamanho, a data 📅 e
hora 🕙 de criação, e o nome de cada um dos elementos. `ls -a` que lista
todos os arquivos incluídos os ocultos. `ls -t` lista os arquivos em
ordem cronolôgica.

Otros **flags** 🚩 interesantes son: `ls -l`, que lista el contenido de
la carpeta 📁 con informaciones extras, como los permisos, el tamaño, la
fecha 📅 y hora 🕙 de creación, y el nombre de cada uno de los
elementos.`ls -a` que lista todos los archivos incluídos los ocultos.
`ls -t` lista los archivos en orden cronológica.

    ls -l

Los permisos se deben entender así: d = Directorio rwx = *read, write* y
*execute*

Se deben leer de tres en tres caracteres. Así, en nuestro ejemplo de
arriba, ninguno de los elementos son directorios porque no inician con
la letra d. La primera tripleta tiene las letras r e w, o sea el usuario
puede leer y escribir esos elementos. La segunda tiene solamente la
letra r, o sea el grupo (pueden ser creados grupos de usuarios) solo
pueden leer esos archivos. Por último, la última tripleta, también solo
tiene a letra r, o sea **todos** los usuarios solo pueden leer los
elementos listados.

### `mkdir` Make Dir

Usted ya aprendió a explorar carpetas 📁 y archivos 📄, ahora va a
aprender como se crean. El comando `mkdir` sirve para crear carpetas 📁.
Vamos a crear varias 📁📁

    # Crea una carpeta llamada datos
    mkdir datos
    # Crea una carpeta llamada analisis y otra llamada tutorial
    mkdir analisis tutorial
    # Lista el contenido 
    ls -F

Note que: 1) puede crear más de una carpeta por línea de comando y 2)
puede diferenciar los archivos de los directorios, pues éstos están de
color azul y con una `/` al final.

Usando el comando `tree` puede ver “graficamente” la organización de los
directorios y archivos dentro del directorio actual.

#### *Tips*

-   No 🚫 use espacios en los nombres de sus carpetas 📁 o archivos 📄
    (p.e. ~~coleta 2020~~). Siempre separe las palabras con `-`, `_` o
    con mayúsculas (p.e. `coleta_2020`, `coleta-2020` o `coletaMaio` 👍🏼)

-   No 🚫 comience nombres con `-`

-   Asigne para sus elementos nombres fáciles de recordar y escribir y
    que describan lo que contienen.

-   Não use caracteres espaciais

### `cd` Change Directory

El comando `cd` sirve para cambiar de **directorio actual de trabajo**
📁. Vaya al directorio `datos/`

    cd datos/

Condirme donde está con el comando `pwd`

Para volver al directorio anterior

    cd ..

El `..` significa: directorio que contiene el directorio actual (o sea
un directorio arriba). Confirme con:

    pwd

**Output**

    /home/user

Ahora entre en la carpeta 📁 `analisis/` y cree otra 📁 llamada
`colecta_2020`, confirme con `ls`

    ## Cambie de directorio
    cd analisis/
    ## Cree un nuevo directorio
    mkdir colecta_2020
    ## Confirme
    ls

**Output**

    colecta_2020/

Ahora entre a la carpeta que acabó de crear usando el comando `cd` y
verifique donde está con `pwd`, después regrese para el directorio base
`/home/user` 📁 y verifique nuevamente.

Al finalizar esos comandos verifique la organización de los directorios
usando el comando `tree`

Por último vuelva a la carpeta `home/user/analisis/coleta_2020` usando
`cd` en una línea de comando solamente. Confirme con `pwd`. Vuelva
nuevamente para la carpeta raíz también con solo una línea de comando.
**Pista**, recuerde que `..` significa directorio de arriba.

#### *Tip* ️

Usted puede usar la tecla Tab :keyboard: para autocompletar las
palabras. Así, economiza tiempo ⏳, y evita errores de digitación ,
porque el sistema solo va a completar los nombres que existan en el
**directorio actual de trabajo**.

Solo tiene que esrcibir las primeras letras de la palabra. p.e. col:

    ## Cambie de carpeta
    cd analisis/col

Oprima :keyboard: \[Tab\]. Automáticamente se no existe ningún otro
elemento que comience por “col”, la palabra coleta\_2020 va a ser
autocompletada.

**Output**

``` coffeesrcipt=
cd analises/colecta_2020
```

Sí por alguna razón existe otro elemento que también comience con “col”,
oprima dos vezes Tab :keyboard: y el sistema va a mostrar las opciones
de palabras que inicien con “col”.

    ## Estando en analisis/ cree uma pasta llamada colecta_2019
    mkdir colecta_2019

Ahora, usted quiere entrar en esa carpeta nueva. Use el comando `cd`
para cambiar de carpeta y use \[Tab\] \[Tab\] para que el sistema
muestre las opciones con comienzo “col”.

    cd col

\[Tab\] \[Tab\]

### `nano` (editor de texto)

**Sintaxis** `nano <nombre_del_archivo>`

Ahora ud va a crear un archivo 📄 `test.txt` dentro de la carpeta 📁
`tutorial/`

    ## Cambie de directorio
    cd tutorial/
    ## Abra el editor de texto nano 
    nano test.txt

Cuando abrir el editor de texto, escriba: “Este es un test” y cierre el
archivo con \[Ctrl + o\] para salvar. En la línea blanca abajo, el
editor de texto preguntará sí quiere mantener el nombre que le dio al
comienzo `test.txt`. \[Enter\] para confirmar. \[Ctrl + x\] para salir.
Confirme que el archivo fue creado con el comando `ls`.

Sí ud quiere entrar de nuevo al arquivo y modificarlo, deve usar el de
nuevo el comando `nano test.txt`.

### `mv` move

El comando `mv` sirve para mover Archivos 📄 de una carpeta 📁 a otra 📁.
Además este comando también puede ser usado para cambiar los nombres de
los elementos. Para mover un archivo de una carpeta 📁 a otra 📁 la
**sintaxis** del comando es: `mv archivo.txt nuevodirectorio/`. En
nuestro ejemplo:

    mv test.txt ../datos/
    ## Confirme
    ls ../datos/

Ud uso `../`, porque ud estaba dentro de la carpeta 📁
`/home/user/tutorial/` y necesitaba volver para 📁 `/home/user/` (📁
carpeta arriba de `tutorial/`) para continuar el camino para 📁 `datos/`.

Ahora use el comando `mv` para cambiar el nombre del archivo 📄
`test.txt` por `prueba.txt`. **Sintaxis**
`mv nombredelarchivo.txt nuevonombredelarchivo.txt`

    ## Cambie de directorio
    cd ../datos/
    ## Confirme
    ls

**Output**

    test.txt

    ## Cambie el nombre del archivo
    mv test.txt prueba.txt
    ## Confirme
    ls

**Output**

    prueba.txt

### `cp` copy

El comando `cp` es similar a `mv`, pero él copia el archivo 📄 en vez de
moverlo. Ahora copie el archivo `/home/user/datos/prueba.txt` en la
carpeta `/home/user/analisis/`. **ATENCIÓN:exclamation:! Haga eso desde
el directorio inicial** `/home/user/`.

**Sintaxis**
`cp directorio/nombredelarchivo.txt nuevodirectorio/nombredelarchivo.txt`

    ## Dónde estoy?
    pwd

**Output**

    /home/user/datos/

No olvide del **tip** 💁🏻‍♀ de usar \[Tab\]

    ## Copiar el archivo
    cp datos/prueba.txt analisis/
    ## Confirme
    ls datos/

**Output**

    prueba.txt

    ## Listar
    ls analisis/

**Output**

    colecta_2019   colecta_2020   prueba.txt

Ud puede usar el comando `cp` para copiar varios archivos 📄📄 en una
línea de comando solamente.

**Sintaxe**
`cp archivo1.txt archivo2.txt archivo3.txt carpetadedestino/`

### `rm` remove

Con o comando `rm` ud puede remover archivos 📄 y/o carpetas 📁.
**CUIDADO**:exclamation: **PRECAUCIÓN**:exclamation: este comando no
tiene reversa, una vez ud oprima \[enter\] no hay como recuperar el
archivo 📄 o carpeta 📁, entonces revise y piense bien antes de rodar este
comando.

**Sintaxis**

`rm directorio/nombredelarchivo.txt`

Ud va a remover el archivo `prueba.txt` de la carpeta 📁
`/home/user/datos`. Sí necesita, use el comando `pwd` para confirmar en
que directorio está.

    ## Remover desde /home/user/
    rm dados/prova.txt
    ## Confirme
    ls

Para eliminar una carpeta 📁 ud necesita del **flag** 🚩`-r`. Elimine la
carpeta 📁 `datos/`

    ## Remover el directorio
    rm -r datos/

### Otros comandos

Para los siguientes comando vamos a crear dos nuevos archivos de texto
llamados `bssA_1.txt` y `bssA_2.txt`, en cada uno vamos a pegar una
secuencia del gen *bssA* que codifica para la enzima *Benzylsuccinate
synthase*.

    ## Cambiar de directorio
    cd tutorial/
    ##Abrir el editor de texto nano
    nano bssA_1.txt

Copie a sequência
[aqui](https://www.ncbi.nlm.nih.gov/nuccore/MW762608.1?report=fasta).
Copie la secuencia desde
[aqui](https://www.ncbi.nlm.nih.gov/nuccore/MW762608.1?report=fasta).

**Atención:** El comando de teclas :keyboard: \[Ctrl + V\] no funciona
en el terminal de Linux. Use \[Ctrl + Shift + V\].

Salve y cierre el editor. Sí quiere, puede confirmar que el archivo fue
creado con el comando `ls`, y entrando en el archivo con el comando
`nano` e el nombre del archivo.

    ## Abrir el editor de texto nano
    nano bssA_1.txt

Repita el proceso para crear el archivo `bssA_2.txt` copiando ésta
[secuencia](https://www.ncbi.nlm.nih.gov/nuccore/FJ810633.1?report=fasta).

### `less`

Este comando serve para imprimir en la pantalla el contenido de un
archivo. Para salir digite `q`

    less bssA_1.txt

Para salir \[Ctrl + c\]

### `head`

Muestra las primeras 10 lineas del archivo

    head bssA_2.txt

### `tail`

Muestra las últimas 10 lineas del archivo

    tail bssA_2.txt

Sí ud quiere, puede aumentar el número de líneas que esos dos últimos
comandos muestran, adicionado un argumento con el número de líneas que
desee imprimir en la pantalla.

    ## Últimas 12 linhas
    tail -12 bssA_1.txt
    ## Primeras 13 linhas
    head -13 bssA_2.txt

### `cat` concatenate

Este comando sirve para juntar dos archivos en uno. Es muy útil para
juntar archivos tipo `.fasta` con secuencias.

    ## Concatenar
    cat bssA_1.txt bssA_2.txt > bssA_all.txt
    ## Confirme
    ls

En el ejemplo anterior, fueron concatenados los archivos `bssA_1.txt` y
`bssA_2.txt` dentro del archivo `bss_all.txt`.

### `wc` Word count

Este comando sirve para contar las líneas, palabras o caracteres de los
archivos.

    ## Contar lineas, palabras y caracteres
    wc bssA_1.txt

**Output**

    28        36      1907 bssA_1.txt

El archivo 📄 `bssA_1.txt` tiene 28 lineas, 36 palabras y 1907
caracteres.

### `grep`

Com o `grep` ud puede buscar un patrón dentro de un archivo. Por ejemplo
en un archivo de secuencias `.fasta` cada secuencia comienza con el
simbolo `>` o podria buscar una secuencia de nucleótidos específica
(p.e. ATCTTGCA)

    grep -c '>' bssA_2.txt

    grep -c 'CGA' bssA_1.txt

El *flag* `-c` es para que el comando solo muestre el número de líneas
que hacen *match* con lo que está siendo procurado.

El comando `grep` tiene varios *flags* diferentes, recuerde que puede
conocer todos ellos digitando `grep --help` para entrar en el menú de
ayuda del comando.

### `find`

Con el comando `find` ud puede buscar archivos con una palabra clave.

    ## Buscar
    find . -name '*.txt'

Lea el comando así: buscar en el \``directorio actual de trabajo`
cualquier archivo que termine con `*.txt`. El simbolo `*` significa
cualquier caracter. Sí ud escribe \`bss\*, el sistema va a entender que
ud está interesado en cualquier elemente que comience con “bss”.

**Output**

    ./bssA_1.txt
    ./bssA_2.txt
    ./bssA_all.txt

Ud podria buscar la palabra en cualquier directorio 📁 del pc modificando
el comando. Procure todos os archivos terminados en ‘.txt’ en la carpeta
raíz `/home/user/`.

    ## Procurar
    find ../ -name "*.txt"

Outros exemplos:

    find ../datos/ -name 'prueba*' # busca archivos que comienzan con prueba dentro de la carpeta /home/user/datos/
    find ../datos/ -iname 'prueba*' #igual pero ignora sí son mayúsculas o minuscúlas.

### `wget`

El comando `wget` sirve para hacer *download* de archivos en la web y
almacenarlos en el **directorio actual de trabajo**, es muy útil para
descargar bases de datos.

**Sintaxis**

    wget https://direccionoweb.com

### `gzip`

Este comando es para compactar y descompactar archivos 📄.

    ## Comprimir
    gzip tutorial/*

Así, el comando `gzip` compactó todos los archivos que están dentro de
la carpeta `tutorial/`. Use el comando `ls` para observar la nueva
extensión de los archivos.

Para descompactar el comando es:

    ## Descompactar
    gzip -d tutorial/bssA_all.txt.gz

### Comandos útiles de Linux

    df    # Muestra el espacio disponible en el disco
    free -g     # info de la memoria
    uname -a    # Muestra la información de la máquina
    du -sh    # muestra el espacio en uso en el disco
    du sh *     # mueestra el espacio usado en disco por archivos y/o directorios 
    du -s * | sort -nr    # Muestra el espacio usado en el disco por archivos y/o directorios ordenados por tamaño
    top     # Muestra el top de consumidores de memoria y CPU 
    who     # Muestra quien está logado en el sistema
    ps    # Muestra los procesos ejecutados por el usuario
    ps -e     # Muestra todos los procesos en ejeción en el sistema
    ps -o %t -p <pid>     # Muestra cuanto tiempo lleva ejecutando un determinado proceso (pid)
    kill <pid>    # Mata el proceso
    paste <archivo1> <archivo2> > <archivo.saida>     # Junta lineas de archivos y separa por tabs (muy útil para tablas)
    cmp <archivo1> <archivo2>     # Muestra las coincidencias de dos archivos
    diff <archivo1> <archivo2>    # Muestra las diferencias entre dos archivos
    csplit -f out fasta_batch "%^>%" "/^>/" "{*}"     # Divide un archivo fasta en varios archivos a cada '>' (cada nueva secuencia)

    sort -k 2,2 -k 3,3n archivo.in > archivo.out    # ordena la tabla, la columna 2 alfabeticamente y la columna 3 numericamente, -k para columna, -n para numerico
    join -1 1 -2 1 <tabla1> <tabla2>    # Junta dos tablas con base a los números especificados de las columnas. De la tabla 1 la columna 1 y de la tabla 2 la colunma 2. Se asume que las tablas están ordenadas.

### `screen`

Screen es una aplicación desarrollada para Linux, que tiene como
objetivo la multiplexación de terminales. O sea, él divide el terminal
físico en varias sesiones virtuales. Funciona así, sí ud está trabajando
en una sesión del terminal, usando screen, cuando apague su computador,
la sesión continuará corriendo los procesos y ud podrá volver a acceder
a ella.

Para iniciar una nueva sesión de screen, es solo digitar en la línea de
comando `screen`. Aparecerá un texto en la pantalla, puede apretar
\[enter\]. De esa manera el sistema criará una neuva sesión virtual e el
nombre de esa sesión será asignado por el sistema. Sin embargo, sí ud
quiere puede darle un nombre a la sesión, para eso es necesario comenzar
con el siguiente comando:

    screen -S mysesson

Para salir de la sesión mantenga oprimido \[Ctrl\], en seguida oprima
\[a\] seguido de \[d\]. Para volver a la sesión use los seguientes
comandos:

    screen -ls # lista las sesiones activas
    screen -r nombredelasesion # entra en la sesión deseada

Para eliminar la sesión oprima \[Ctrl + d\]

### Scripts simples de uma linha de comando

Para renombras muchos archivos *.old* a \*.new. Para probar primero,
subtituya `do mv` por `do echo mv`.

    for i in *.input; do mv $i ${i/\.old/\.new}; done
    for i in *\ *; do mv "$i" "${i// /_}"; done # Substituye espacios en nombres de archivos por underscores

### `scp` Secure Copy Between Machines

`scp` es un comando que sirve para copiar elementos entre el servidor y
su computador y viceversa.

**Sintaxis** `scp source target`

Del servidor para su computador:

    scp user@ip.adress:camin/al/archivo.txt camino/en/su/pc

Del computador para el servidor:

    scp camino/en/su/pc/archivo.txt user@ip.adress:camino/donde/quiere/copiar/enel/servidor/

Se ud quier copiar un directorio completo basta colocar el *flag* `-r`
después de `scp`

### Compresión e Descompresión de archivos

Además de `gzip`, existen otros tipos de compresión de archivos, tales
como `.tar` y `.zip`.

**Compresión**

    tar -cvf archivo.tar midirectorio/ #comprime midirectorio, y el nombre del archivo compreso será archivo.tar
    zip -r midrectorio.zip midirectorio/ #comprime el directorio midirectorio/ en un archivo llamado midirectorio.zip

**Visualizar**

    tar -tvf archivo.tar

**Extrair**

    tar -xvf archivo.tar
    unzip midirectorio.zip

# I. Gestión de Herramientas Bioinformáticas

## Anaconda

Es recomendable instalar Anaconda, pues es la forma más fácil para
instalar las herramientas bioinformáticas. Anaconda es una distribución
libre y abierta de los lenguajes *Python* y *R*, utilizada en ciencia de
datos y bioinformática. Las diferentes versiones de los programas se
administran mediante un sistema de gestión llamado *conda*, el cual hace
bastante sencillo instalar, correr y actualizar programas.
[Aqui](https://conda.io/projects/conda/en/latest/user-guide/install/index.html)
se encuentran las instrucciones para la instalación de Anaconda. Después
de instalado *Anaconda* y su gestor *Conda*, podran ser creados
*ambientes virtuales* para la instalación de las diferentes herramientas
bioinformáticas que serán usadas.

## Algunos comandos de `conda`

    conda create -n miambiente # Crea un ambiente llamado miambiente
    conda activate miambiente # Activa el ambiente mi ambiente
    conda env list # Lista todos los ambientes creados
    cond list # Estando dentro de un ambiente, lista las herramentas instaladas en ese ambiente

Para instalación de cada herramienta, visite la página
<https://anaconda.org/> y en la caja de busqueda procure por la
herramienta que desea, y econtratá el comonado para instalación.

**Nota**: Las herramientas usadas en este tutorial serán instaladas a
medida que van apareciendo dentro del pipeline.

# II. Adquisición y organización de los datos

## 2.1. Organización de directorios

Para mantener el orden durante la ejecución del *pipeline* es
recomendado crear directorios secuenciales a medida que son realizados
los diferentes pasos durante el proceso.

Usando el comando `mkdir`, cree un directorio base para todo el proceso.
Asigne el nombre de transcriptomica

    mkdir transcriptomica

Entre al nuevo directorio usando el comando `cd` (*change directory*)

    cd transcriptomica/

Cree un nuevo directorio para almacenar las secuencias brutas.

> **Tip:** Debido a que la mayoria de las etapas del workflow son
> secuenciales, es recomendable nombrar los directorios comenzando con
> un número y así mantener la organización.

    mkdir 00.RawData

## 2.2 Adquisición de datos

### 2.2.1. SRA Toolkit

Para la adquisición de datos, será instalado el programa [SRA
Toolkit](https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit),
el cual nos permitirá descargar las secuencias directamente del NCBI en
el servidor.

Descargue el programa en el siguiente
[link](https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit)

Seleccione el archivo según su sistema operativo. En este tutorial será
instalada la version para [Ubuntu Linux
64bits](https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz).

Usando el comando `wget`, descargue el archivo de la wed directamente en
el servidor

    # Salga de la carpeta transcriptomica
    cd ..

    # Descargue el programa en la carpeta /home/user/
    wget --output-document sratoolkit.tar.gz https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz

Será descargada un archivo compactado (`sratoolkit.tar.gz`). Use el
comando tar para descompactar

    tar -vxzf sratoolkit.tar.gz

El archivo será descompactado en la carpeta `sratoolkit.3.0.0-ubuntu64/`

Configure el camino al programa

    export PATH=$PATH:$PWD/sratoolkit.3.0.0-ubuntu64/bin

Verifique que la configuración está correcta

    which fastq-dump

Deberá aparecer el camino al directorio que contiene el programa:

    /home/user/sratoolkit.3.0.0-ubuntu64/bin/fastq-dump

Antes de entrar en el menú de configuración, seleccione un directorio
vacio para usarlo como el repositorio de las secuencias a descargar.
Para esto siga la siguiente secuencia de comandos:

    # Cree un directorio llamado secuencias-ncbi
    mkdir secuencias-ncbi

Ingrese al menú de configuración con `vdb-config -i`

-   Verifique que la opción *Remote Access* está habilitada
-   Ingrese a la pestaña *Cache* y verifique que la opción *local
    file-caching* está habilitada.
-   En la opción \*Location of user-repository, busque el directorio
    creado anteriormente `secuencias-ncbi/`
-   En la pestaña *AWS*, habilite la opción *report cloud instance
    identity*
-   Salve las configuraciones y salga del menú con x.

### 2.2.2. Descargando las secuencias

Para la ejecución de este *pipeline* serán usadas las secuencias de
RNA-seq del artículo en estudio y secuencias del genoma de *E.coli*
ATCC8739, el cual será usado como referencia para el ensamble de los
transcriptomas.

**Secuencias RNA-seq**

Para las secuencias de RNA-seq, usando el comando `nano`, cree un
archivo llamado `SRR-seqs-RNA.txt` en donde serán listados los códigos
de acceso (SRR number) de los *datasets*. Siga la secuencia de códigos
presentados a continuación:

    # entre en la carpeta secuencias-ncbi/
    cd secuencias-ncbi/

    # Cree el archivo con nano
    nano SRR-seqs-RNA.txt

Será abierto el programa nano para editar el archivo `SRR-seqs-RNA.txt`

Los códigos de accesso son los siguientes:

SRR12922100 SRR12922099 SRR12922098 SRR12922089 SRR12922097 SRR12922096
SRR12922095 SRR12922094 SRR12922093 SRR12922092 SRR12922091 SRR12922090

Copie y pegue dentro del archivo en el terminal. Para cerrar \[Ctrl +
X\], confirme que desea guardar los cambios con S.

Debido a que es un proceso que va a demorar, es recomendable hacerlo
usando el programa screen.

    screen -S sra

El siguiente comando le permitirá descargar los archivos `.sra`

    prefetch --option-file SRR-seqs-RNA.txt

Comenzará a ser descargadas cada una de las muestras según los códigos
de acceso. Cierre la sesión de screen con \[Ctrl + A + D\]. Cuando
quiera volver a la sesión use el comando `screen -r sra`.

Al terminar el proceso, en la carpeta `secuencias-ncbi`, se encontrarán
una serie de directorios, la carpeta `sra` contiene los archivos `.sra`.
Use los comandos `cd` para cambiar de directorio y `ll` para listar el
contenido.

El siguiente paso es transformar los archivos `.sra` para `.fastq`.
Usando la función `for`, puede automatizar el proceso para que todas los
archivos sean transformados secuencialmente.

    for i in ./*.sra
    do
    fasterq-dump --split-files $i
    done

Use el comando `ll` para revisar el contenido de la carpeta. Mueva los
archivos `.fastq` para la carpeta creada para las secuencias brutas
`transcriptomica/00.RawData`

    mv *.fastq ../../transcriptomica/00.RawData/

Confirme con `ll` que efectivamente los archivos se encuentran en la
nueva ubicación.

**Ensamble Genoma *E.coli* ATCC8739**

Este ensamble se encuentra en la pagina de la coleccion
[ATCC](https://genomes.atcc.org/). Cree una cuenta de usuario, use el
buscador de la página para encontrar el genoma y descargue el ensamble.
Use la interface gráfica de MobaXterm para subir el archivo dentro de la
carpeta `transcriptomica/`. Use el comando `mv` para colocar el ensamble
del genoma dentro de la carpeta `00.RawData/`.

**Secuencias Genoma *Escherichia coli* ATCC 25922**

Con el fin de aprender y practicar el montaje de genomas, serán
descargadas las secuencias brutas de la secuenciación del genoma
completo de la *Escherichia coli* ATCC 25922.

**Nota:** Este genoma no será usado para el proceso de ensamble del
transcriptoma.

El código de acceso a estas secuencias es SRR2889880. Use el programa
SRA Tool kit para descargar este conjunto de datos.

    prefetch SRR2889880

Convierta el archivo `.sra` a archivos `.fastq`.

    fasterq-dump --split-files SRR2889880

Use el comando `mv` para mover los archivos `.fastq` del genoma de *E.
coli* desde la carpeta `secuencias-ncbi/sra/` para la carpeta
`/home/user/transcriptomica/01.RefGenome/`

## 2.3. Organización de los *datasets*

Debido a que los nombres de los archivos son los códigos de acceso SRR,
es recomendable cambiarlos por nombres más fáciles de entender y
reconocer cada archivo. Use la información en la siguente tabla y el
comando `mv` para cambiar los nombres. Para facilitar primero use el
comando `cd` para entrar en el directorio `00.RawData/`.

| Código SRR  | Muestra                           | Nombre Archivo |
|-------------|-----------------------------------|----------------|
| SRR12922100 | 1/2MIC camphor essential oil (R3) | oil1-2\_R3     |
| SRR12922098 | 1/2MIC camphor essential oil (R2) | oil1-2\_R2     |
| SRR12922099 | 1/2MIC camphor essential oil (R1) | oil1-2\_R1     |
| SRR12922089 | acetone                           | acetone        |
| SRR12922097 | 1/4MIC camphor essential oil (R3) | oil1-4\_R3     |
| SRR12922096 | 1/4MIC camphor essential oil (R2) | oil1-4\_R2     |
| SRR12922095 | 1/4MIC camphor essential oil (R1) | oil1-4\_R1     |
| SRR12922094 | 1/8MIC camphor essential oil (R3) | oil1-8\_R3     |
| SRR12922093 | 1/8MIC camphor essential oil (R2) | oil1-8\_R2     |
| SRR12922092 | 1/8MIC camphor essential oil (R1) | oil1-8\_R1     |
| SRR12922091 | water (R2)                        | water\_R2      |
| SRR12922090 | water (R1)                        | water\_R1      |
| SRR2889880  | genoma *E. coli* ATCC 25922       | ecoli          |

*Nota:* Recuerde que debe tener dos archivos con el mismo nombre que
componen una muestra por ser una secuenciación *paired-end*, con
excepción del genoma de *E. coli* ATCC 25922 que es *single-end*. Por
ejemplo: `oil1-2_R3_1.fq` y `oil1-2_R3_2.fq`.

**Tip:** Use la tecla TAB para facilitar y evitar errores a la hora de
digitar el nombre de los archivos.

# III. Control de calidad

## 3.1. Control de Calidad

### 3.1.1. Evaluación de la calidad

Para la evaluación de la calidad será usado el programa
[FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) que
es una herramienta que permite observar graficamente la calidad de las
secuencias de Illumina.

**Instalación**

Las instrucciones para instalación usando conda se encuentran
[aqui](https://anaconda.org/bioconda/fastqc). Sin embargo aqui en este
tutorial también serán presentadas

Como ya fue explicado anteriorimente, con conda es posible crear
ambientes virutuales para instalar las herramientas bioinformáticas. El
primer ambiente que será creado se llamará **quality**, donde se
instalaran los programas relacionados con este proceso.

    conda create -n quality

Durante el proceso, el sistema preguntará sí desea proceder con la
creación del ambiente, con las opciones y/n (si o no). Escriba `y` y
después de eso el ambiente virtual estará creado.

Para instalar las herramientas dentro del ambiente anteriormente creado,
es necesario activarlo

    conda activate quality

El ambiente estará activo cuando el nombre de éste se encuentra en el
comienzo de la linea de comando, así: `(quality) user@server:~/$`.

Posteriormente se procede a la instalación del programa:

    conda install -c bioconda fastqc

De nuevo será cuestionado si desea continuar con el proceso o no.
Escriba `y`.

**Uso**

La primera etapa del proceso es la evaluación de la calidad de las
secuencias cortas (Illumina) usando *FastQC*, con el objetivo de
determinar sí es necesario trimar o filtrar las secuencias de baja
calidad en los próximos pasos.

Ésta etapa es para identificar principalmente las secuencias *outlier*
con baja calidad ($Q< 20$).

Active el ambiente `quality`:

    conda activate quality

    ## Confirme que está en el directorio raíz
    pwd

Debe estar em `~/transcriptomica/`. Si ese no es el resultado del
comando `pwd`, use el comando `cd` para llegar en el directorio base.

> **Tip:** **TODAS** las herramientas bioinformáticas tienen un manual
> de ayuda que puede ser accesado en la linea de comando, usando el flag
> `-h` o `--help`. **ANTES de ejecutar una hierramenta siempre lea el
> manual**. Este comando lista todos los argumentos disponibles para la
> herramienta y explica como deben/pueden ser usados.

> **Atención:** Para el uso correcto y seguro del servidor verifique ele
> número de núcleos disponibles para el usuario en el momento del
> análisis. **NUNCA** trabaje con el total de los núcleos de la máquina.
> Ejecute **FastQC**:

    ## Cree un directorio para salvar el output de FastQC
    mkdir 01.FastqcReports

    ## Ejecute usando 10 threads
    fastqc -t 10 00.RawData/* -o 01.FastqcReports/

**Sintaxis**

    fastqc -t <num núcleos> <input_directory> -o <output_directory>

El comando `fastqc` tiene varias opciones o parametros, entre ellas,
escoger el número de núcleos de la máquina para correr el análisis, para
este caso `-t 10`. El input es el directorio que contiene las secuencias
de illumina `00.RawData/*`, el `*` indica al sistema que puede analizar
todos los archivos que están dentro de ese directorio. El output,
indicado por el parametro `-o`, es el directorio donde se desea que sean
guardados los resultados del análisis. A continuación se encuentra una
explicación detallada de cada output generado.

**Outputs**

-   Reportes html `oil1-2_R3.html`: Aqui es posible ver toda información
    de calidad graficamente.
-   Zip files `oil1-2_R3.zip`: Aqui se encuentran cada uno de los
    gráficos de manera separada. \*IGNORE\*\*

Descargue los archivos `html` y explore en su *web browser*.

Revise cada uno de los gráficos y datos que son generados en estos
reportes, los cuales permiten determinar los parametros para el siguente
paso, en el cual se filtraran y eliminaran las secuencias y bases con
baja calidad. En este
[link](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/)
se encuentra la explicación de cada una de las partes del reporte y como
interpretar los resultados.

Los reportes mostraron que las secuencias de RNA-seq en términos
generales son de alta calidad. Los gráficos que mostraron errores,
pueden ser ignorados, puesto que es normal que esto pase con librerias
de RNA-seq. Por esta razón estas secuencias no serán
depuradas/filtradas. Sin embargo las secuencias *single-end* del genoma
de *E. coli*, presentan varios problemas de calidad. El gráfico *Per
base sequence quality* muestra que algunas bases finales de las
secuencias tienen una calidad inferior a 20 ($Q<20$). El gráfico *Per
base sequence content*, muestra que las primeras 18 bases presentan
diferentes proporción de GC y AT. El gráfico *Per sequence GC content*
muestra que hay una leve desviación entre la distribución teórica del
%GC y la distribución real de %GC por *reads*. Analizando cada uno de
los gráficos son decididos los parametros para la etapa de depuración.
Para este caso es necesario filtrar por calidad, cortar las primeras
bases y filtrar por tamaño.

### 3.1.2. Depuración/*Trimming*

Según fue evaluado en el control de calidad, será necesario filtrar
algunas lecturas del genoma de *E. coli*. Las secuencias de las
librerias de RNA-seq no pasarán por el proceso de depuración. Para esta
etapa será usado o programa [Trimmomatic
v0.39](http://www.usadellab.org/cms/?page=trimmomatic) que permite
filtrar (remover) lecturas o *reads* cortas de baja calidad.

Trimmomatic tiene vários parametros que pueden ser considerados para
filtrar lecturas con baja calidad. Aqui usaremos algunos. Si quiere
saber que otros parametros y como funciona cada uno de ellos, consulte
el
[manual](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf).

**Instalación**

Como se trata de una herramienta que participa dentro del proceso de
control de calidad, será instalada dentro del ambiente virtual
**quality**

    # Si no está activado el ambiente
    conda activate quality

    # Instale Trimmomatic
    conda install -c bioconda trimmomatic

**Uso**

Para los datos aqui analizados se usará la siguiente linea de comando:

    # Activa el ambiente quality
    conda activate quality

    # Crie un directorio para salvar las lecturas limpias
    mkdir 02.CleanData

    # Ejecute Trimmomatic
    trimmomatic SE -threads 10 00.RawData/ecoli.fq 02.CleanData/ecoli.fq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 HEADCROP:18 MINLEN:80 ILLUMINACLIP:/home/drm/anaconda3/pkgs/trimmomatic-0.36-6/share/trimmomatic-0.36-6/adapters/NexteraPE-PE.fa:2:30:10

**Sintaxis** `trimmomatic SE -threads input output [opciones]`

El comando anterior tiene varias partes. Primero, el nombre del comando
es `trimmomatic`, a continuación la opción `SE` indica para el programa
que las secuencias que irán a ser analizadas son de tipo *single end*,
para *paired-end*, use `PE`. Después se encuentra el input, si se
tratara de secuencias *paired-end* tendria dos inputs, forward (pair1) y
reverse (pair2). Después el output. En el caso de *paired-end* serán
varios outputs, siendo primero las secuencias forward pareadas (limpias)
y no pareadas y después las secuencias reverse. Por último se encuentran
los parametros de depuración. Para este caso usamos los parametros
`SLIDINGWINDOW`, `LEADING`, `TRAILING`, `HEADCROP`, `MINLEN` y
`ILLUMINACLIP`.

-   SLIDINGWINDOW: genera una ventana deslizante, que en este caso va de
    4 en 4 bases, cálcula el promedio del *Phred Score* y si está por
    debajo de 15 esas bases son cortadas.
-   LEADING: corta bases del comienzo de la lectura si están por debajo
    de *threshold* de calidad
-   TRAILING: corta bases del final de la lectura si están por debajo de
    *threshold* de calidad.
-   HEADCROP: corta bases del inicio de la lectura
-   MINLEN: Secuencias con menor tamaño del establecido, son descartadas
-   ILLUMINACLIP: Elimina bases que corresponden con la secuencia de
    adaptadores usados en la plataforma Illumina.

Después de correr Trimmomatic es necesario evaluar la calidad de las
secuencias generadas (“limpias”) usando nuevamente FastQC.

    fastqc -t 10 02.CleanData/* -o 01.FastqcReports/

Descargue el reporte `01.FastqcReports/ecoli.html`.

Observe que ahora todas las bases en ambos archivos tienen $Q>30$.
Después del proceso de filtrado sobrevivieron $60'258.702$ *reads*, es
decir $85.4%$ de las secuencias iniciales ($70'560.424$).

### 3.1.3. Cobertura

Cuando se trabaja con secuenciación de genomas, el siguiente paso
después de conocer la cantidad de secuencias obtenidas y remover las
secuencias con baja calidad, es calcular la cobertura o profundidad de
la secuenciación para ese genoma. El cálculo de la cobertura está dado
por la siguiente ecuación:

$$C = (L*N)/G$$ Donde: L es igual al tamaño de las lecturas, N es igual
al número de lecturas, y G es igual al tamaño aproximado del genoma.

Así, entonces para el genoma del presente tutorial, con el
secuenciamiento Illumina tenemos:

$$C = ((83bp) * 60258702)/5209985bp$$

    #> [1] "La cobertura es de 960X"

# IV. Genómica

## 4.1 Ensamblaje *de novo*

Un ensamblaje es el proceso de alineamiento de secuencias cortas con el
objetivo de recuperar una secuencia mayor. En el caso del ensamblaje *de
novo* son usados algorítmos con base en *k-mers*, que son subsecuencias
com tamaño definido por el usuario (i.e. 21-mers).

[Spades v3.15.3](https://github.com/ablab/spades) es uno de los
ensambladores de genomas, más conocido y con mejores resultados, y puede
ser usado tanto para lecturas cortas como largas. Lea atentamente el
[manual](http://cab.spbu.ru/files/release3.15.2/manual.html), ya que
este programa tiene muchas opciones diferentes. Spades usa el algoritmo
del *Grafo de Bruijn* para el montaje de las secuencias.

**Instalación**

Siga las siguientes instrucciones para la instalación de **Spades**
dentro de ambiente virtual llamado *assembly*.

    # Cree el ambiente assembly
    conda create -n assembly

    # Active el ambiente virtual
    conda activate assembly

    # Instale Spades
    conda install -c bioconda spades

**Uso**

El ensamble será realizado ejecutando el comando:

> **Tip:** El programa nohup permite ejecutar tareas en segundo plano,
> con el objetivo de mantener la ejecución del comando aún con perdida
> de conexión

    nohup spades.py --careful -s 02.CleanData/ecoli.fq -k 21,33,55,77,99,111,127 -o 03.Assembly/01.Careful -t 40

**Sintaxe**

-   *Paired-end*

<!-- -->

    spades.py -1 <pair1> -2 <pair2> -k <kmers list> -o <output_directory> -t <num_nucleos>

-   *Single-end*

<!-- -->

    spades.py -s <single-end-reads> -k <kmers list> -o <output_directory> -t <num_nucleos>

Con el objetivo de comparar ensambles, ejecute nuevamente el comando
anterior sin el flag `--careful`. Este flag intenta reducir el número de
mismatches e indels cortos y es recomendado para el ensamble de genomas
de procariotas. Debe cambiar la carpeta de salida para no sobre escribir
los resultados del primer ensamble, use `03.Assembly/02.NoCareful`.

En el [manual](http://cab.spbu.ru/files/release3.15.2/manual.html)
encuentra más detalles.

**Output**

-   `corrected/`: contiene las reads corregidas por **BayesHammer** en
    `.fastq.gz`

-   `scaffolds.fasta`: contiene los scaffolds obtenidos

-   `contigs.fasta`: contiene los contigis obtenidos

-   `assembly_graph_with_scaffolds.gfa`: contiene el grafo del montaje
    en formato GFA 1.0.

-   `assembly_graph.fastg`: contiene el grafo del montaje en formato
    FASTG

## 4.2 Evaluación del ensamble

El ensamble debe ser evaluado a través de métricas que representan la
calidad del genoma. En este paso será calculado el N50, el número de
contigs, el tamaño del genoma y que tan completo y contaminado está.

### 4.2.1. Quast

[Quast v5.0.2](http://quast.sourceforge.net/docs/manual.html) (*QUality
ASsesment Tool*) es posible evaluar las principales estadísticas del
montaje (i.e. N50, número de contigs, tamaño total del montaje, tamaño
de los contigs, etc). **Quast** genera una serie de archivos y reportes
donde es posible observar esas estadísticas básicas del montaje. Serán
comparados los montajes obtenidos anteriormente, con el objetivo de
escoger el mejor, para las siguientes etapas.

**Instalación**

    # Cree el ambiente quast
    conda create -n quast

    # Active el ambiente quast
    conda activate quast

    # Instale Quast
    conda install -c bioconda quast

**Uso**

    # Vuelva al diretorio base

    # Crie um diretório para el output
    mkdir 04.AssemblyQuality
    mkdir 04.AssemblyQuality/01.Quast

    # Ejecute Quast
    quast.py 03.Assembly/01.Careful/scaffolds.fasta 03.Assembly/02.NoCareful/scaffolds.fasta -o 04.AssemblyQuality/01.Quast

**Sintaxis**
`quast.py path/to/assembly/contigs.fasta -o path/to/output/`

**Interpetación de los resultados**

La idea de usar **Quast**, aparte de evaluar las estidísticas básicas
del montaje, es comparar varios montajes para escoger el mejor. Por
ejemplo: entre menor sea el número de contigs es mejor, porque significa
que el genoma quedó menos fragementado. Y eso se reflejará en el tamaño
de los contigs que serán grandes. El valor de N50, es mejor entre mayor
sea. Así mismo, es ideal menor número de gaps y Ns.

**Outputs**

Explore el directorio de output usando el comando `ls`.

-   `04.AssemblyQuality/01.Quast/report.html`: Este reporte puede ser
    abierto en un *web browser* y contiene las informaciones más
    relevantes. Como número de contigs, tamaño del mayor contig, tamaño
    total del montaje, N50, etc.

Los contigs de tamaño menor de 500 bp no tienen un valor representativo
en el ensamble, por lo tanto es recomendable filtrar esas secuencias.

El programa bbmap tiene un script que permite realizar la depuración por
tamaño. Instale bbmap en un ambiente llamado bioinfo:

    # Cree el ambiente bioinfo
    conda install -n bioinfo

    # Active el ambiente bioinfo
    conda activate bioinfo

    # Instale bbmap
    conda install -c bioconda bbmap

    # Ejecute bbmap (careful)
    reformat.sh in=03.Assembly/01.Careful/scaffolds.fasta out=03.Assembly/01.Careful/scaffolds_filtered.fasta minlength=500

    # Ejecute bbmap (No careful)
    reformat.sh in=03.Assembly/02.NoCareful/scaffolds.fasta out=03.Assembly/02.NoCareful/scaffolds_filtered.fasta minlength=500

Ejecute nuevamente Quast, evaluando los dos ensambles, los contigs
filtrados y sin filtar.

    # Active el ambiente quast
    conda activate quast

    # Ejecute Quast
    quast.py 03.Assembly/01.Careful/scaffolds.fasta 03.Assembly/01.Careful/scaffolds_filtered.fasta 03.Assembly/02.NoCareful/scaffolds.fasta 03.Assembly/02.NoCareful/scaffolds_filtered.fasta -o 04.AssemblyQuality/01.Quast

### 4.2.2. CheckM

Para evaluar que tan completo y contaminado están los ensambles es usada
la herramienta [CheckM](https://github.com/Ecogenomics/CheckM/wiki). La
cual usa una base de datos propia de genes ortólogos de copia única.

Instale el programa en un ambiente llamado `checkm`

    # Cree el ambiente checkm
    conda create -n checkm

    # Active el ambiente
    conda activate checkm

    # Instale checkm
    conda install -c bioconda checkm-genome

Ejecute el ánalisis para todos los ensambles, filtrados y no filtrados.
Los ensambles deben estar todos en un mismo directorio, entonces cree un
nuevo directorio dentro de la carpeta `03.Assembly/` llamado
`03.Scaffolds/`. Después entre en cada carpeta de los ensambles y usando
el comando `mv`, cambie el nombre de los archivos, con el fin de
diferenciarlos (actualmente tienen el mismo nombre) entre careful y no
careful. Por último, nuevamente use `mv` para mover los archivos para el
directorio creado anteriormente (`03.Assembly/03.Scaffolds/`)

    cd 03.Assembly/01.Careful

    mv scaffolds.fasta careful.fasta

    mv scaffolds_filtered.fasta careful_filtered.fasta

    mv careful* ../03.Scaffolds

    cd ../02.NoCareful

    mv scaffolds.fasta Nocareful.fasta

    mv scaffolds_filtered.fasta Nocareful_filtered.fasta

    mv Nocareful* ../03.Scaffolds

Después de organizado los archivos, ya es posible ejecutar CheckM:

    # Cree el directorio para el output
    cd ../../

    mkdir 04.AssemblyQuality/02.Checkm

    checkm lineage_wf 03.Assembly/03.Scaffolds/ 04.AssemblyQuality/02.Checkm -t 40 -x fasta --tab > 04.AssemblyQuality/02.Checkm/output.txt

**Sintaxis**

    checkm lineage_wf <input_directory/> <output_directory/> -t <num_nucleos> -x <format> --tab > output.txt

Explore el archivo de salida `04.AssemblyQuality/02.Checkm/output.txt`
usando el comando `less`. Descargue el reporte en su computador.

Para más detalles sobre la interpretación del reporte visite este
[link.](https://www.biostars.org/p/447744/)

## 4.3. Anotación Taxonómica

La clasificación con base en datos de genoma tiene mayor poder de
resolución en comparación al usar apenas un gene marcador, pues esta
abordaje analiza múltiples genes que presentan un resultado mucho más
robusto de las relaciones de parentesco del organismo de interés.
[GTDB-tk](https://ecogenomics.github.io/GTDBTk/index.html) es una
herramienta que identifica 120 genes marcadores y los compara con una
base de datos curada y constamente actualizada.

Instale el prohrama en un ambiente llamado `gtdbtk`

    # Cree el ambiente gtdbtk
    conda create -n gtdbtk

    # Active el ambiente
    conda activate gtdbtk

    # Instale GTDBtk
    conda install -c conda-forge -c bioconda gtdbtk

Para descargar y configurar la base de datos, siga las
[instrucciones](https://ecogenomics.github.io/GTDBTk/installing/bioconda.html#step-3-download-and-alias-the-gtdb-tk-reference-data).

Ejecute GTDBtk usando el seguinte comando:

    gtdbtk classify_wf --genome_dir 03.Assembly/03.Scaffolds/ --out_dir 05.TaxonomyAnnotation/ -x fasta --cpus 40

**Sintaxis**

    gtdbtk classify_wf --genome_dir <input_directory/> --out_dir <output_directory/> -x <format> --cpus <num_nucleos>

El programa genera varios archivos de salida que están resumidos en
`05.TaxonomyAnnotation/gtdbtk.bac120.summary.tsv`.

## 4.4. Predicción de genes

el objetivo de esta etapa es procurar los ORFs (*Open Reading Frames*)
dentro de los contig/scaffolds. Es decir, predecir donde inician e
terminan los genes. Basicamente el programa busca codones de inicio y de
*stop*.Para este objetivo será usada la herramienta
[Prodigal](https://github.com/hyattpd/prodigal/wiki).

**Instalación**

Instale el programa en el ambiente bioinfo

    # Active el ambiente
    conda activate bioinfo

    # Instale Prodigal
    conda install -c bioconda prodigal

Para ejecutar el programa utilice el seguiente comando:

    # Cree una carpeta para guardar el resultado
    mkdir 06.GenePrediction

    #
    prodigal -f gff -i 03.Assembly/03.Scaffolds/careful_filtered.fasta -o 06.GenePrediction/careful_filtered_orf.gff -a 06.GenePrediction/careful_filtered_proteins.faa -d 06.GenePrediction/careful_filtered_ORFnucleotides.fa -s 06.GenePrediction/careful_filtered_genes

**Sintaxis**

    prodigal -f <output_format> -i <input_assembly.fasta> -o <output_gbk_file> -a <output_protein_seq_file> -d <output_nucleotides_seq_file> -s <output_coordinates_file>

El archivo `06.GenePrediction/careful_filtered_orf.gff` incluyendo las
secuencias condificantes y no codificantes y las posiciones en cada
contig. Adicionalmente, el programa genera archivos con las secuencias
de nucleótidos (`06.GenePrediction/careful_filtered_ORFnucleotides.fa`)
y proteínas (`06.GenePrediction/careful_filtered_proteins.faa`) de cada
gene predicho.

*Nota:* Ejecute el mismo comando en el genoma de *E. coli* ATCC8739. Use
`ecoli_genome_` como prefijo para los nombres de los archivos de salida.

# V. Transcriptómica

En esta sección serán abordadas las etapas y programas para la
evaluación librerias de RNA-seq.

### 5.1. Remoción de secuencias de rRNA

Aunque durante el proceso de extracción de RNA se use un kit para la
depleción de RNA ribosomal, puede suceder que en las librerias de
RNA-seq aún existan algunas secuencias de rRNA, las cuales podrán ser
eliminadas usando el programa
[SortmeRNA](https://github.com/biocore/sortmerna), que a su vez usa
bases de dados de rRNA, tales como Rfam 5S, Rfam 5.8S,
SILVA\_138.1\_NR99\_SSURef\_tax\_silva and
SILVA\_138.1\_LSURef\_tax\_silva, para buscar y eliminar esas secuencias
de las bibliotecas.

**Instalación**

Instale SortmeRNA en un ambiente virtual llamado mapping

    # cree el ambiente
    conda create -n mapping

    # Active el ambiente
    conda activate mappin

    # Instale SortmeRNA
    conda install -c bioconda sortmerna

Las bases de datos son descargadas de las páginas de [SILVA
database](https://www.arb-silva.de/) y [Rfam](https://rfam.xfam.org/).

**Uso**

    for i in 00.RawData/*_1.fq
    do
    SAMPLE=$(basename $i _1.fq)
    nohup sortmerna --ref /home/drm/Documentos/database/Rfam/Rfam_5S.fa --ref /home/drm/Documentos/database/Rfam/Rfam_5.8S.fa --ref /home/drm/Documentos/database/silva/SILVA_138.1_NR99_16S_28S.fasta --ref /home/drm/Documentos/database/silva/SILVA_138.1_23S_28S.fasta --reads $i --reads 00.RawData/${SAMPLE}_2.fq --paired_in --threads 50 --workdir 07.SortmeRNA/${SAMPLE}/ --idx-dir 07.SortmeRNA/${SAMPLE}/ --kvdb 07.SortmeRNA/${SAMPLE}/kvdb --fastx --aligned 07.SortmeRNA/${SAMPLE}/${SAMPLE}_aligned.fasta --other 07.SortmeRNA/${SAMPLE}/${SAMPLE}_other.fasta --out2
    done

El comando anterior, ejecuta SortmeRNA en todas las bibliotecas de
RNA-seq. Al final de proceso, cada muestra tendrá una carpeta dentro del
directorio `07.SortmeRNA/`. Los archivos terminados en `_other_fwd.fq` y
`_other_rev.fq`, contiene las secuencias que *no* alinearon contra las
bases de dados de rRNA, es decir libres de rRNA. Estos serán los
archivos que serán usados para los siguientes pasos.

### 5.2. Alineamiento a um genoma de referencia

Después de la evaluación de la calidad y remoción de rRNA, se debe hacer
un alineamiento o mapeo de las lecturas para determinar en qué parte del
genoma o de dónde se originaron las lecturas. Hay una serie de
herramientas que realizan esta función. En este tutorial usaremos
[HISAT2](https://github.com/DaehwanKimLab/hisat2), pero también una
herramienta como [STAR](https://github.com/alexdobin/STAR) o
[TopHat2](https://github.com/infphilo/tophat), hacen el trabajo.

En este caso será usado el genoma obtenido desde la página de la
colección ATCC, que debe estar almacenado en la carpeta `03.Assembly/`.

La primera etapa del proceso con
[HISAT2](https://daehwankimlab.github.io/hisat2/), es llamada de
busqueda de semillas (*seeds*), y consiste en que para cada read el
programa buscará la porción de la lectura más larga que coincida
exactamente con una o más ubicaciones en el genoma de referencia
(*seed1*). Posteriormente, volverá a buscar la parte no mapeada de la
lectura para encontrar la siguiente secuencia más larga que coincida
exactamente con el genoma de referencia (*seed2*). Y así hasta que
encuentra las *seeds* de todas las lecturas. Esta forma de mapeo hace
que el algorítmo sea eficiente y rápido. La segunda etapa, comprende la
clusterización de *seeds* próximas, para generar una read completa.
Después, el programa unirá las *seeds* baseado en el mejor alineamiento
de las reads (*score* de *mismatches*, *indels*, *gaps*, etc)

**Instalação**

HISAT2 v2.2.1 será instalado en el ambiente virutal mapping. También es
necesario instalar la herramienta [SamTools](http://www.htslib.org/),
que sirve en la manipulación de archivos `.sam` y `.bam`.

    # Activa el ambiente
    conda activate mapping

    # Instala
    conda install -c bioconda hisat2

    # Instala SamTools
    conda install -c bioconda samtools

#### 5.2.1. Indexando el genoma de referencia

El primer paso para el alineamiento es indexar el genoma. Cree un
directorio para todo el proceso de mapeo (p.e. `08.Mapping.RNA`)

    # Indexando
    hisat2-build -p 10 03.Assembly/Escherichia_coli_ATCC_8739.fasta 08.Mapping.RNA/Escherichia_coli_ATCC_8739

**Sintaxis** `hisat2-build -p # [genome] [index]`

-   `-p`: número de threads
-   `[genome]`: Genoma de referencia
-   `[index]`: índice

#### 5.2.2. Mapeo

En el primer comando **HISAT2** va a hacer el alinemiento, el cual
generará una serie de archivos `.sam` (*Sequence aligment Map*). En el
segundo comando, **SamTools** transformará a archivos `.bam` (*Binary
Alignment Map*), los cuales son una versión comprimida de `.sam`.

    hisat2  --dta -p 15 -x 08.Mapping.RNA/Escherichia_coli_ATCC_8739 -1 00.RawData/acetone_1.fq -2 00.RawData/acetone_2.fq -S 08.Mapping.RNA/acetone.sam

    # loop
    for i in 00.RawData/*_1.fq
    do
    sample=$(basename $i _1.fq)
    hisat2 --dta -p 15 -x 08.Mapping.RNA/Escherichia_coli_ATCC_8739 -1 $i -2 00.RawData/${sample}_2.fq -S 08.Mapping.RNA/${sample}.sam
    done

**Sintaxis**

`hisat2 --dta -p <num_nucleos> -x <index> -1 RNA-seqs_pair1 -2 RNA-seqs_pair2`

-   `--dta`: Genera un reporte que será usado en el próximo passo.
-   `-p`: Número de núcleos
-   `-x`: índice
-   `-1`: Secuencias RNA-seq pair 1
-   `-2`: Secuencias RNA-seq pair 2

#### 5.2.3. Transformando los archivos del mapeo

El primer paso es transformar el archivo de salida del mapeo (`.sam`)
para `.bam`

    samtools sort -o 08.Mapping.RNA/acetone.bam 08.Mapping.RNA/acetone.sam

    # loop
    for i in 08.Mapping.RNA/*.sam
    do
    sample=$(basename $i .sam)
    samtools sort -o 08.Mapping.RNA/${sample}.bam $i
    done

**Sintaxis**

    samtools sort -o <output.bam> <input.sam>

### 5.3. Ensamble del transcriptoma

El objetivo de esta etapa es el montaje del transcriptoma, usando los
alineamientos del paso anterior. En este tutorial, usaremos
[StringTie](https://ccb.jhu.edu/software/stringtie/), el cual emplea
algoritmos eficientes para la recuperación de la estructura del
trasncriptoma y la estimación de la abundancia a partir de lecturas de
RNA-Seq alineadas con un genoma de referencia. Toma como entrada los
alineamientos SAM/BAM, ordenados por coordenadas y produce una salida
GTF que consiste en transcritos ensamblados y sus niveles estimados
(FPKM / TPM y valores de cobertura).

**Instalación**

    # Activa el ambiente assembly
    conda activate assembly

    # Instale StringTie
    conda install -c bioconda stringtie

Cree un directorio para la salida del proceso:

    # Cree un directorio para el montaje
    mkdir 09.AssemblyRNA

    # StringTie
    stringtie acetone.bam -p 25 -G 06.GenePrediction/ecoli_genome_orf.gff -o 09.AssemblyRNA/acetone_stringtie.gtf

    # Loop
    for i in 08.Mapping.RNA/*.bam
    do
    sample=$(basename $i .bam)
    stringtie $i -p 25 -G 06.GenePrediction/ecoli_genome_orf.gff -o 09.AssemblyRNA/${sample}_stringtie.gtf
    done

**Sintaxis**

    stringtie <sorted.bam> -p <num_nucleos> -G <reference.genome.gff> -o <output.gtf>  -l <transcriptos_prefix>

El archivo de salida `.gtf` contiene las definiciones estructurales de
los transcriptos ensamblados por **StringTie** a partir de la
información de mapeo.

Use `less` para ver el contenido del archivo de salida
`09.AssemblyRNA/acetone.gtf`.

### 5.3.1. Transcriptoma final

A continuación serán unidos los transcriptomas de cada muestra para
obtner el transcriptoma final. Para esto, será creado un archivo
(`mergelist.txt`) con el nombre de los *output* obtenidos en el comando
de StringTie.

    # Creando el archivo mergelist.txt
    echo -e 'acetone_stringtie.gtf\noil1-2_R1_stringtie.gtf\noil1-2_R2_stringtie.gtf\noil1-2_R3_stringtie.gtf\noil1-4_R1_stringtie.gtf\noil1-4_R2_stringtie.gtf\noil1-4_R3_stringtie.gtf\noil1-8_R1_stringtie.gtf\noil1-8_R2_stringtie.gtf\noil1-8_R3_stringtie.gtf\nwater_R1_stringtie.gtf\nwater_R2_stringtie.gtf ' > 09.AssemblyRNA/mergelist.txt 

Ejecute el comando `merge` de StringTie:

    cd 09.AssemblyRNA

    stringtie --merge -G ../06.GenePrediction/ecoli_genome_orf.gff -o stringtie_merged.gtf mergelist.txt

### 5.3.2. Extrayendo las secuencias

Hasta aqui tenemos el archivo final de coordenadas genómicas
(`09.AssemblyRNA/stringtie_merged.gtf`). En esta etapa usaremos ese
archivo para extraer las secuencias en un archivo `.fasta`, usando el
programa [GFFREAD](https://github.com/gpertea/gffread).

**Instalación**

Instale el programa en el ambiente bioinfo.

    # Active el ambiente
    conda activate bioinfo

    # Instale el ambiente
    conda install -c bioconda gffread

**Uso**

Para extraer las secuencias, ejecute el siguiente comando:

    cd ..

    gffread 09.AssemblyRNA/stringtie_merged.gtf -g 03.Assembly/Escherichia_coli_ATCC_8739.fasta -w 09.AssemblyRNA/all_transcripts.fasta

## 5.4. Cuantificación de la expresión

Después de obtenidos los transcriptos, estos deben ser cuantificados. El
programa que será usado en este tutorial, se llama
[Kallisto](https://pachterlab.github.io/kallisto/). Este software
cuantifica las abundancias de transcriptos. Se basa en la novedosa idea
de pseudoalineación para determinar rápidamente la compatibilidad de
lecturas con los *targets*, sin necesidad de alineación.

**Instalación**

Instale Kallisto el ambiente **bioinfo**

    # Activa el ambiente bioinfo
    conda activate bioinfo

    # Instale Kallisto
    conda install -c bioconda kallisto

Cree un directorio para el proceso de cuantificación

    mkdir 10.TranscriptsQuantification

El primer paso es indexar el archivo
`09.AssemblyRNA/all_transcripts.fasta`:

    kallisto index -i 09.AssemblyRNA/index_transcritos 09.AssemblyRNA/all_transcripts.fasta

A continuación serán cuantificados los transcriptomas de cada libreria.

    kallisto quant -i 09.AssemblyRNA/index_transcritos -o 10.TranscriptsQuantification/acetone -b 1000 -t 20 00.RawData/acetone_1.fq 00.RawData/acetone_2.fq

    # Loop
    for i in 00.RawData/*_1.fq
    do
    sample=$(basename $i _1.fq)
    kallisto quant -i 09.AssemblyRNA/index_transcritos -o 10.TranscriptsQuantification/${sample} -b 1000 -t 20 $i 00.RawData/${sample}_2.fq
    done

Serán creadas carpetas para cada muestra y dentro se encuentra el
archivo de salida de **Kallisto** que es una tabla llamada
`abundance.tsv` con el ID de cada transcrito y el valor de **TPM** para
cada uno de ellos, además del tamaño de cada uno.

Use `less` para ver el contenido de la tabla `abundance.tsv` que se
encuentra dentro de la carpeta `12.TranscriptQuantification/`.

El siguiente paso es unir las tablas de abundancia de cada muestra y
construir la matriz de abundancia para iniciar el análisis de expresión
diferencial. El script `abundance_estimates_to_matrix.pl`, de la
herramienta [Trinity](https://github.com/trinityrnaseq/trinityrnaseq)
permite la construcción de la matriz de abundancia. En este tutorial no
se usará como tal el programa Trinity, que es un ensamblador de RNA-seq
*de novo*, recomendado para tratar datos de metatranscriptómica. Sin
embargo, es necesario descargar el repositorio de la herramienta:

    git clone https://github.com/trinityrnaseq/trinityrnaseq

El comando anterior descargará una carpeta llamada `trinityrnaseq/`. El
script estará en el directorio `trinityrnaseq/util/`

Antes de ejecutar el script, es necesario modificar los nombres de las
tablas de abundancia de cada muestra, de manera que puedan ser
eficientemente diferenciados entre ellos.

    cp 10.TranscriptsQuantification/acetone/abundance.tsv 10.TranscriptsQuantification/acetone.tsv

    cp 10.TranscriptsQuantification/oil1-2_R1/abundance.tsv 10.TranscriptsQuantification/oil1-2_R1.tsv

    cp 10.TranscriptsQuantification/oil1-2_R2/abundance.tsv 10.TranscriptsQuantification/oil1-2_R2.tsv

    cp 10.TranscriptsQuantification/oil1-2_R3/abundance.tsv 10.TranscriptsQuantification/oil1-2_R3.tsv

    cp 10.TranscriptsQuantification/oil1-4_R1/abundance.tsv 10.TranscriptsQuantification/oil1-4_R1.tsv

    cp 10.TranscriptsQuantification/oil1-4_R2/abundance.tsv 10.TranscriptsQuantification/oil1-4_R2.tsv

    cp 10.TranscriptsQuantification/oil1-4_R3/abundance.tsv 10.TranscriptsQuantification/oil1-4_R3.tsv

    cp 10.TranscriptsQuantification/oil1-8_R1/abundance.tsv 10.TranscriptsQuantification/oil1-8_R1.tsv

    cp 10.TranscriptsQuantification/oil1-8_R2/abundance.tsv 10.TranscriptsQuantification/oil1-8_R2.tsv

    cp 10.TranscriptsQuantification/oil1-8_R3/abundance.tsv 10.TranscriptsQuantification/oil1-8_R3.tsv

    cp 10.TranscriptsQuantification/water_R1/abundance.tsv 10.TranscriptsQuantification/water_R1.tsv

    cp 10.TranscriptsQuantification/water_R2/abundance.tsv 10.TranscriptsQuantification/water_R2.tsv

Ejecute el script

    # Active el ambiente bioinfo
    conda activate bioinfo

    # Entre al directorio 10
    cd 10.TranscriptsQuantification/

    # Ejecute el script
    perl trinityrnaseq/util/abundance_estimates_to_matrix.pl --est_method kallisto --gene_trans_map none acetone.tsv oil1-2_R1.tsv oil1-2_R2.tsv oil1-2_R3.tsv oil1-4_R1.tsv oil1-4_R2.tsv oil1-4_R3.tsv oil1-8_R1.tsv oil1-8_R2.tsv oil1-8_R3.tsv water_R1.tsv water_R2.tsv

Los archivos de salida son:

-   `kallisto.isoform.TMM.EXPR.matrix`: matriz normalizada por TMM
-   `kallisto.isoform.TPM.not_cross_norm`: matriz de TPM
-   `kallisto.isoform.counts.matrix`: matriz de counts
-   `kallisto.isoform.TPM.not_cross_norm.TMM_info.txt`: estadística de
    los factores de normalización por profundidad de la secuenciación
-   `kallisto.isoform.TPM.not_cross_norm.runTMM.r`: script en R para
    realizar la normalización en TMM.

## 5.5. Expresión diferencial

El análisis de expresión diferencial consiste en comparar la expresión
de los genes en diferentes condiciones e identificar genes que son
bastante más o menos expresados.

Este análisis puede ser realizado con diferentes bibliotecas del
software R, tales como
[edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html),
[DESeq2](https://github.com/mikelove/DESeq2) y
[Sleuth](https://github.com/pachterlab/sleuth). Aqui serán abordado el
análisis con DESeq2. Este análisis será ejecutado en el ambiente
RStudio.

> # Pausa para una brevisiiima introducción al lenguaje R
>
> ## 1. Instalación
>
> Para instalar R, de click [aqui](https://vps.fmvz.usp.br/CRAN/). En
> seguida instele
> &gt;[RStudio](https://www.rstudio.com/products/rstudio/download/#download).
>
> <img src="imgs/R.png" align="center"/>
>
> RStudio debe lucir parecido a la imagen anterior, con cuatro paneles:
> i) Editor de texto/códigos, ii) consola, iii) Espacio de trabajo e
> historial y iv) Plots, archivos y menú de ayuda.
>
> ## 2. Instalación, actualización y remoción de librerias
>
> -   **install.packages()**: Instala librerias desde el repositorio
>     CRAN. **Nota:** Existen otras maneras de instalar librerias cuando
>     éstas no se encuentran en CRAM, p.e. GitHub, BiocLite, devtools.

``` r
install.packages('ggplot2')
install.packages('tidyverse')
```

> -   Información de ayuda

    help(package = 'ggplot2')

> ## 3. Tipos de datos, constantes y variables
>
> **Tipos de datos**
>
> -   Númericos
>
> -   Enteros
>
> -   Reales
>
> -   Imaginários
>
> -   Caracteres/string o texto
>
> -   Texto
>
> -   Lógicos
>
> -   TRUE
>
> -   FALSE
>
> **Asignación de valores a variables**

``` r
a <- 3
b <- 2
a + b 
#> [1] 5
```

> **Tipos básicos de datos:** númericos, caracteres y lógicos

``` r
a <- 3 
class(a)
#> [1] "numeric"
```

``` r
nombre <- "Maria"
class(nombre)
#> [1] "character"
```

``` r
class(TRUE)
#> [1] "logical"
```

> **Dando nombres a las variables**
>
> Se pueden usar letras, números y underline.
>
> **Reglas**
>
> -   No puede comenzar por número
> -   Puede comenzar por punto seguido de una letra o secuencia de
>     letras.
> -   No puede comenzar por punto seguido de números.
>
> **NOTA** R es un lenguaje *case sensitive*, o sea es sensible a
> mayúscula y minúscula.
>
> ## 4. Operadores y funciones matemáticas
>
> | **Operação**    | **Descrição**                |
> |-----------------|------------------------------|
> | x + y           | Adición                      |
> | x - y           | Substracción                 |
> | x \* y          | Multiplicación               |
> | x / y           | División                     |
> | x ^ 2 (x \*\*y) | Ptenciación o exponenciación |
> | x %% y          | Resto de la división         |
> | x %/% y         | Cosciente (Parte Entera)     |

``` r
17/2 #División exacta
#> [1] 8.5
17%/%2 #Parte exacta
#> [1] 8
17%%2 # Resto de la división
#> [1] 1
```

> **Funciones aritméticas y trigonométricas básicas**

``` r
log2(9)
#> [1] 3.169925
sqrt(9)
#> [1] 3
cos(60)
#> [1] -0.952413
```

> *Otras funciones* `sin(), tan()`
>
> ## 5. Estructura de datos
>
> ### 5.1. Vectores
>
> Es la combinación de múltiples valores (númericos, caracteres o
> lógicos)

``` r
mivector <- c(12,32,7,89) 
class(mivector) #tipo de información contenida en el vector
#> [1] "numeric"
length(mivector) # tamaño del vector
#> [1] 4
letras <- c('a','b','c','d')
class(letras)
#> [1] "character"
length(letras)
#> [1] 4
vector.logico <- c(FALSE,FALSE,TRUE,F,T)
class(vector.logico)
#> [1] "logical"
length(vector.logico)
#> [1] 5
```

> **Formas de crear vectores numéricos**
>
> -   1.  función `c()` concatenar
>
> -   2.  Operador `:`
>
> -   3.  función `seq()`
>
> -   4.  función `rep()`
>
> ### 5.2. Matrices
>
> Es la combinación de múltiples vectores, con el mismo tipo de datos
> (númericos, caracteres o lógicos).

``` r
m1 <- matrix(data = c(2,5,7,8,21,32,22,11), nrow=4,
             ncol = 2, byrow = TRUE)
m1
#>      [,1] [,2]
#> [1,]    2    5
#> [2,]    7    8
#> [3,]   21   32
#> [4,]   22   11
```

> ### 5.3. Data Frames
>
> Matrices y vectores poseen una limitación, solo guardan un tipo de
> información (lógicas, numéricas, texto). Mientras que los data frame
> pueden contener todo tipo de información en un solo objeto. El data
> frame es bidimensional, en líneas y columnas, y en cada columna puede
> tener un tipo de información. Normalmente, cada colunma es una
> variable y las líneas contienen los valores.
>
> Algunas características son:
>
> -   Las columnas tendrán siempre nombres
> -   Los nombres de las líneas deben ser únicos
> -   Cada columna debe presentar un tipo único de datos
> -   Cada columna debe tener el mismo número de líneas

``` r
df <- data.frame(nombres=c('Ana','Juan','Vera'), notas=c(10,2,7.5), situacion=c(T,F,T), row.names = c('l1','l2','l3'))
View(df)
str(df) # presenta la estructura de los datos
#> 'data.frame':    3 obs. of  3 variables:
#>  $ nombres  : chr  "Ana" "Juan" "Vera"
#>  $ notas    : num  10 2 7.5
#>  $ situacion: logi  TRUE FALSE TRUE
```

> Esta fue la brevisiima introducción al lenguaje R. Para mayores
> detalles y profundidad de los conocimientos en este lenguaje, consulte
> otros materiales.

## 5.5. Expresión diferencial (continuación)

Compacte las carpetas `10.TranscriptsQuantification/`

    zip -r 10.TranscriptsQuantification.zip 10.TranscriptsQuantification/

Descargue la carpeta a su computador usando el comando `scp`

    scp user@direccionip:/home/user/transcriptomica/10.TranscriptsQuantification.zip camino/en/su/computador

## 5.6. Anotación Funcional

Antes de hacer el análisis de expresión diferencial, los transcriptos
deben ser anotados, para conocer su función, para lo cual los
transcriptos serán funcionalmente anotados por homología, usando bases
de datos e um alineador.

Para el alineamiento será usado el programa
[Diamond](https://github.com/bbuchfink/diamond/wiki/3.-Command-line-options),
e una base de dayos curada como [KEGG](https://www.kegg.jp/).

**Instalación**

Instale Diamond en un ambiente virtual llamado diamond

    # Cree el ambiente
    conda create -n diamond

    # Active el ambiente
    conda activate diamond

    # Instale Diamond
    conda install -c bioconda diamond

Descargue las bases de datos
[aqui](https://drive.google.com/file/d/1ZxjJdwh1izP32X5CH-B8SN0DK2WAAAvr/view?usp=sharing).
Una vez descargadas las bases de datos, debe subir los archivos al
servidor y almacenarlos en una carpeta llamada `dbs`

Use o siguiente comando para ejecutar Diamond:

    # Cree un directorio para la salida
    mkdir 11.FunctionalAnnotation

    # Diamond
    diamond blastx --more-sensitive --threads 50 -k 1 -f 6 qseqid qlen sseqid sallseqid slen qstart qend sstart send evalue bitscore score length pident qcovhsp --id 60 --query-cover 60 -d dbs/keggdb.dmnd --query 09.AssemblyRNA/all_transcripts.fasta -o 11.FunctionalAnnotation/kegg.txt --tmpdir /dev/shm

El archivo de *output* es una tabla que contiene los códigos de los
genes y las anotaciones (número KEGG), además de otras informaciones
relacionadas al alineamiento de cada secuencia (i.e. cobertura, % de
identidad). Debido a que los genes fueron anotados apenas con los
números KEGG, es necessario cruzar esta información con las anotaciones
detalladas para cada número KEGG, que indican la función y categoria de
los genes. A continuación encontrará una série de pasos para obtener la
tabla final de anotación funcional:

1.  Separar la colunma de los códigos KEGG

<!-- -->

    perl -pe 's/\|?(?:\s+gi|ref)?\|\s*/\t/g' 11.FunctionalAnnotation/kegg.txt > 11.FunctionalAnnotation/kegg_formated.txt

2.  Cortar las columnas 1 (Contig ID) y 4 (KEGG ID) e grabar en un nuevo
    archivo

<!-- -->

    cut -f1,4 11.FunctionalAnnotation/kegg_formated.txt > 11.FunctionalAnnotation/kegg_ID_contigs_keggs.tsv

3.  Ordenando

<!-- -->

    sort -k2,2 11.FunctionalAnnotation/kegg_ID_contigs_keggs.tsv > 11.FunctionalAnnotation/kegg_ID_contigs_keggs_sorted.tsv

4.  Descargue el archivo con las informaciones de los códigos KEGG

<!-- -->

    curl -L https://figshare.com/ndownloader/files/33953774 -o kegg.tsv

    # ordene
    sort -k1,1 kegg.tsv > kegg_sorted.tsv

    # mueva el archivo para 11.FunctionalAnnotation/
    mv kegg_sorted.tsv 11.FunctionalAnnotation

6.  Asigne nombres a las columnas

<!-- -->

    echo -e "target_id\tkegg_id\tlevel1\tlevel2\tlevel3\tGenName" > 11.FunctionalAnnotation/kegg_randomic_analysis.tsv

5.  Asignación randômica

<!-- -->

    cat 11.FunctionalAnnotation/kegg_ID_contigs_keggs_sorted.tsv | while read line ; do echo "$line" | join -1 2 -2 1 -e"NA" -o1.1,0,2.2,2.3,2.4,2.5 -t $'\t' - 11.FunctionalAnnotation/kegg_sorted.tsv | shuf -n1 >> 11.FunctionalAnnotation/kegg_randomic_analysis.tsv ; done

6.  Asigne nombres a las columnas

<!-- -->

    echo -e "target_id\tkegg_id\tlevel1\tlevel2\tlevel3\tGenName" > transcripts_annotations.tsv

7.  Descargue y explore a tabla final de anotaciones
    `11.FunctionalAnnotation/kegg_randomic_analysis.tsv`.

Guarde el archivo en la carpeta `10.TranscriptsQuantification/`

En este punto puede cerrar su conexión con el servidor y trabajar en el
ambiente de RStudio localmente.

En su computador, cree un directorio de trabajo y en èl, descompacte las
carpetas descargadas del servidor.

# RStudio

Abra RStudio, cree un nuevo proyecto: File &lt; New Project &lt;
Existing Directory

Escoja el directorio donde descompactó las carpetas descargadas del
servidor.

Una vez el nuevo proyecto esté creado, abra un script: File &lt; New
File &lt; R Script. En este script debera copiar la serie de comandos
que encontrará a continuación:

1.  Instalando las bibliotecas necesarias:

``` r
# DESeq2
if (! require(DESeq2)) {
  source("https://bioconductor.org/biocLite.R")
  biocLite("DESeq2")
}

# Enhanced Volcano

BiocManager::install("EnhancedVolcano")

# tidyverse
install.packages("tidyverse")

# tibble
install.packages("tibble")
```

2.  Activando las bibliotecas

``` r
#  Librerias
library(DESeq2)
library(EnhancedVolcano)
library(tidyverse)
library(tibble)
```

3.  Leyendo las tablas

``` r
# Leyendo tabla de anotaciones
anot <- read.delim("10.TranscriptsQuantification/kegg_randomic_analysis.tsv",
                   sep = "\t") %>% 
  separate(GenName, c("GenName", "Function"), sep = ';') %>% 
  select(target_id, kegg_id, GenName)


# Leyendo la matriz de counts
data = read.table("10.TranscriptsQuantification/kallisto.isoform.counts.matrix", 
                  header=T, row.names=1, com='')
```

4.  Redondeando los valores de la matriz de expresión y filtrando (al
    menos dos counts por transcripto)

``` r
# redondeando los valores
rnaseqMatrix = round(data)

# Corte a por lo menos 2 counts por transcripto
rnaseqMatrix = rnaseqMatrix[rowSums(rnaseqMatrix) >= 2,]
```

5.  Generando la tabla del diseño experimental

``` r
conditions = data.frame(conditions=factor(c("acetone", rep("oil_tretated_0.5", 3),
                                            rep("oil_treated_0.25",3), 
                                            rep("oil_treated_0.125",3),
                                            rep("control",2))))
rownames(conditions) = colnames(rnaseqMatrix)
```

6.  Generando el objeto DESeq

``` r
ddsFullCountTable <- DESeqDataSetFromMatrix(
  countData = rnaseqMatrix,
  colData = conditions,
  design = ~ conditions)
```

7.  Cálculo de la expresión diferencial

``` r
dds = DESeq(ddsFullCountTable, test="Wald", fitType="mean")
```

8.  Comparando **Acetona vs control**

``` r
# Comparando acetona vc control
acetonevscontrol <- results(dds, alpha=.05, lfcThreshold=1, 
                               contrast = c("conditions","acetone","control"))


acetonevscontrol.df <- as.data.frame(acetonevscontrol) %>% 
  rownames_to_column(var = "target_id")



# Juntando tabla de expresión diferencial y anotación funcional
final.table <- full_join(anot,acetonevscontrol.df) %>% 
  column_to_rownames(var='target_id')
```

9.  Volcano Plot

``` r
EnhancedVolcano(final.table,
                lab = final.table$GenName,
                x = 'log2FoldChange',
                y = 'pvalue',
                pCutoff = 0.05,
                FCcutoff = 0.5,
                xlim = c(-12, 12),
                pointSize = 1.5,
                labSize = 2.5,
                colAlpha = 0.7,
                legendLabSize = 12,
                legendIconSize = 4.0,
                drawConnectors = TRUE,
                widthConnectors = 0.2,
                title = "Differential Expression Analysis",
                subtitle = "Control vs. Acetone",
                max.overlaps = 20,
                colConnectors = 'grey30')
```

<img src="imgs/unnamed-chunk-22-1.png" width="100%" />

A continuación se repetirá el proceso para comparar el tratamiento con
aceite a 1/2 del MIC y el control

8.  Comparando **Aceite 1/2 MIC vs control**

``` r
# Control vs oil_tretated_0.5
controlvsoil0.5 <- results(dds, alpha=.05, lfcThreshold=1, 
                            contrast = c("conditions","oil_tretated_0.5","control"))


controlvsoil0.5.df <- as.data.frame(controlvsoil0.5) %>% 
  rownames_to_column(var = "target_id")


# Juntando tabla de expresión diferencial y anotación funcional
final.table <- full_join(anot,controlvsoil0.5.df) %>% 
  column_to_rownames(var='target_id')
```

9.  Volcano Plot

``` r
EnhancedVolcano(final.table,
                lab = final.table$GenName,
                x = 'log2FoldChange',
                y = 'pvalue',
                pCutoff = 0.05,
                FCcutoff = 0.5,
                xlim = c(-12, 12),
                pointSize = 1.5,
                labSize = 2.5,
                colAlpha = 0.7,
                legendLabSize = 12,
                legendIconSize = 4.0,
                drawConnectors = TRUE,
                widthConnectors = 0.2,
                title = "Differential Expression Analysis",
                subtitle = "Control vs. Cinnamomum camphora Oil (1/2 MIC)",
                max.overlaps = 20,
                colConnectors = 'grey30')
```

<img src="imgs/unnamed-chunk-24-1.png" width="100%" />

Use los comandos anteriores, modificando la condición(es) que quiere
comparar.

10. Plot PCA

``` r
#Criando PCA para visualização da clusterização das amostras
rld <- rlog(dds)

plotPCA(rld, intgroup = "conditions")
```

<img src="imgs/unnamed-chunk-25-1.png" width="100%" />

# VI Referencias

-   Andrews, S. (2017). FastQC: a quality control tool for high
    throughput sequence data. 2010

-   [Trimmomatic: a flexible trimmer for Illumina sequence
    data](https://academic.oup.com/bioinformatics/article-abstract/30/15/2114/2390096)(Trimmomatic:
    a flexible trimmer for Illumina sequence data)

-   [SPAdes: a new genome assembly algorithm and its applications to
    single-cell
    sequencing](https://www.liebertpub.com/doi/full/10.1089/cmb.2012.0021)(Bankevich,
    A., Nurk, S., Antipov, D., Gurevich, A. A., Dvorkin, M., Kulikov, A.
    S., … & Pevzner, P. A. (2012). SPAdes: a new genome assembly
    algorithm and its applications to single-cell sequencing. Journal of
    computational biology, 19(5), 455-477.)

-   [QUAST: quality assessment tool for genome
    assemblies](https://academic.oup.com/bioinformatics/article-abstract/29/8/1072/228832)(Gurevich,
    A., Saveliev, V., Vyahhi, N., & Tesler, G. (2013). QUAST: quality
    assessment tool for genome assemblies. Bioinformatics, 29(8),
    1072-1075.)

-   [GFF utilities: GffRead and
    GffCompare](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7222033.2/)(Pertea,
    G., & Pertea, M. (2020). GFF utilities: GffRead and GffCompare.
    F1000Research, 9.)

-   [eggNOG: automated construction and annotation of orthologous groups
    of
    genes](https://academic.oup.com/nar/article-abstract/36/suppl_1/D250/2506468)(Jensen, L.
    J., Julien, P., Kuhn, M., von Mering, C., Muller, J., Doerks, T., &
    Bork, P. (2007). eggNOG: automated construction and annotation of
    orthologous groups of genes. Nucleic acids research, 36(suppl\_1),
    D250-D254.)

-   [Fast and sensitive protein alignment using
    DIAMOND](https://www.nature.com/articles/nmeth.3176)(Buchfink, B.,
    Xie, C., & Huson, D. H. (2015). Fast and sensitive protein alignment
    using DIAMOND. Nature methods, 12(1), 59-60.)

-   [Fast genome-wide functional annotation through orthology assignment
    by
    eggNOG-mapper](https://academic.oup.com/mbe/article-abstract/34/8/2115/3782716)(Huerta-Cepas,
    J., Forslund, K., Coelho, L. P., Szklarczyk, D., Jensen, L. J., Von
    Mering, C., & Bork, P. (2017). Fast genome-wide functional
    annotation through orthology assignment by eggNOG-mapper. Molecular
    biology and evolution, 34(8), 2115-2122.)

-   [Graph-based genome alignment and genotyping with HISAT2 and
    HISAT-genotype](https://www.nature.com/articles/s41587-019-0201-4)(Kim,
    D., Paggi, J. M., Park, C., Bennett, C., & Salzberg, S. L. (2019).
    Graph-based genome alignment and genotyping with HISAT2 and
    HISAT-genotype. Nature biotechnology, 37(8), 907-915.)

-   [STAR: ultrafast universal RNA-seq
    aligner](https://academic.oup.com/bioinformatics/article-abstract/29/1/15/272537)(Dobin,
    A., Davis, C. A., Schlesinger, F., Drenkow, J., Zaleski, C., Jha,
    S., … & Gingeras, T. R. (2013). STAR: ultrafast universal RNA-seq
    aligner. Bioinformatics, 29(1), 15-21.)

-   [The sequence alignment/map format and
    SAMtools](https://academic.oup.com/bioinformatics/article-abstract/25/16/2078/204688)(Li,
    H., Handsaker, B., Wysoker, A., Fennell, T., Ruan, J., Homer, N., …
    & Durbin, R. (2009). The sequence alignment/map format and SAMtools.
    Bioinformatics, 25(16), 2078-2079.)

-   [StringTie enables improved reconstruction of a transcriptome from
    RNA-seq reads](https://www.nature.com/articles/nbt.3122)(Pertea, M.,
    Pertea, G. M., Antonescu, C. M., Chang, T. C., Mendell, J. T., &
    Salzberg, S. L. (2015). StringTie enables improved reconstruction of
    a transcriptome from RNA-seq reads. Nature biotechnology, 33(3),
    290-295.)

-   [kallisto: A command-line interface to simplify computational
    modelling and the generation of atomic
    features](https://joss.theoj.org/papers/10.21105/joss.03050.pdf)(Caldeweyher, E.
    (2021). kallisto: A command-line interface to simplify computational
    modelling and the generation of atomic features. Journal of Open
    Source Software, 6(60), 3050.)

-   [United States Department of Energy Systems Biology
    Knowledgebase](https://www.nature.com/articles/nbt.4163)(Arkin AP,
    Cottingham RW, Henry CS, Harris NL, Stevens RL, Maslov S, et
    al. KBase: The United States Department of Energy Systems Biology
    Knowledgebase. Nature Biotechnology. 2018;36: 566. doi:
    10.1038/nbt.4163)

-   [KEGG Mapper for inferring cellular functions from protein
    sequences](https://onlinelibrary.wiley.com/doi/abs/10.1002/pro.3711)(Kanehisa,
    M., & Sato, Y. (2020). KEGG Mapper for inferring cellular functions
    from protein sequences. Protein Science, 29(1), 28-35.)

-   [iPath: interactive exploration of biochemical pathways and
    networks](https://www.sciencedirect.com/science/article/pii/S0968000408000236)(Letunic,
    I., Yamada, T., Kanehisa, M., & Bork, P. (2008). iPath: interactive
    exploration of biochemical pathways and networks. Trends in
    biochemical sciences, 33(3), 101-103.)

------------------------------------------------------------------------

# BONUS Plataformas online para bioinformática

Existen diversas plataformas online que permiten realizar todos los
procesos aprendidos en este tutorial, usando servidores públicos y
interfaces gráficas.

### [Galaxy](https://usegalaxy.org/)

Es una plataforma con diversas herramientas para diferentes procesos. La
mayoria que fueron usadas en este tutorial se encuentran en Galaxy.

<img src="imgs/galaxy.png" align="center"/>

**HELP** Click en el símbolo del birrete, en la parte superior.
Encontrará diversos tutoriales con videos.

### [KBase](https://www.kbase.us/)

Es una plataforma con diversas herramientas para diferentes procesos. La
mayoria que fueron usadas en este tutorial se encuentran en KBase.

<img src="imgs/kbase.png" align="center"/>

[**HELP**](https://www.kbase.us/learn/)

### [KofamKOALA](https://www.genome.jp/tools/kofamkoala/)

Plataforma para anotar baseado en el algorítmo HMMER. También existen
otras plataformas baseadas en otros algorítmos, como
[BlastKOALA](https://www.kegg.jp/blastkoala),
[GhostKOALA](https://www.kegg.jp/ghostkoala).

<img src="imgs/koala.png" align="center"/>

### [IPATH3](https://pathways.embl.de/)

Herramienta online para visualización y análisis de mapas metabólicos.

<img src="imgs/ipath.png" align="center"/>

[**Videos de ayuda**](https://pathways.embl.de/video_tutorial.cgi)
[**Páginas de ayuda**](https://pathways.embl.de/help.cgi)

------------------------------------------------------------------------

## Trobleshooting

Dudas, ayuda, consejos…

Kelly Hidalgo Martinez 📧 <khidalgo@javeriana.edu.co>
