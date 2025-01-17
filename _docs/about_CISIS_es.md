
- ## Resumen de versiones y configuraciones de CISIS

  **I. versão ISIS ou versão LIND: 2 mundos, com mesmos M/F mas com distintos I/F**

  - Mundo 1: ISIS – I/F (.ifp) com postings de 8 bytes contendo 4 componentes:

  ```
     mfn – 3 bytes
     tag – 2 bytes
     occ – 1 byte – ocorrência a cada %
     cnt – 2 bytes – contador da ”chave” dentro de cada ocorrência
  ```

  - Mundo 2: LIND – I/F (.iyp) com postings de apenas 1 componente

  ```
     mfn – 3 bytes (ou 4 bytes na versão LIND4 ou LINDG4)
  ```

  a lista de postings de cada chave é implementada como lista de mfn´s ou bitstring (1 bit para cada mfn do M/F), o que ocupar menos espaço

  
  **II. versão FFI : para bases de dados com registros com maiores do que 64Kbyte**

  - todos os campos referentes a tamanho dos registro do M/F implementados em 4 bytes
  - portanto os M/F gerados com e sem FFI não são compatíveis

  
  **III. versão LIND4 : para bases de dados com mais de 16 milhões de registros**

  - todos os campos referentes a mfn e controles e pointers do I/F implementados em 4 bytes
  - portanto os I/F gerados com LIND não são compatíveis com LIND4 ou LINDG4
  - entretanto, os M/F gerados com ISIS, LIND, LIND4 ou LINDG4 são totalmente compatíveis

  
  **IV. versão G: biblioteca e utilitários compilados com a opção LARGE FILE SUPPORT do GCC**

  - admitem arquivos maiores do que 2Gbyte
  - arquivos gerados com versão G são compatíveis se não excederem 2Gbyte
  - btw, pode-se compilar versão ISIS com essa opção (seria chamada de ISISG)

  
  **V. Sobre as configurações (opções de compilação)**:

  - Tamanho das chaves do I/F

  1. 1. LE1 – tamanho das chaves curtas (tipicamente 10 ou 16)
     2. LE2 – tamanho das chaves longas (tipicamente 30 ou 60)

  

  - Tamanho dos registros do M/F

  1. 1. MAXMFRL – tamanho máximo dos registros do M/F
     2. Por default, é 32K para multiusuário e 63K se operado em modo monousuário

  ```
   Nota 1: para manter a compatibilidade do CDS/ISIS v3 com a v2, GDB usou o sinal do componente mfrl 
           como flag de registro locked por outro usuário... 
   Nota 2: consultei o fonte cisis.h e vi que alguém em BIREME baixou o limite de 64K para os 63K
  ```

  por default, na versão FFI é 1Mbyte

  
  Para archivos más grandes de 2Gigabyte, *hay que usar* una version xxxGyyy. Sugiero ISISG4 o LINDG4, pues creo que las bases que el va a manipular no necesitan de registros largos (o sea, versiones FFI).

  G stands for Gcc large file support (o sea, mas de 2Gb.)

  Las versiones xxxx4 soportan I/F generados desde M/F con mas de 16 millones de registros.

  
  **VI. Outras opções de compilação**

  1. Há mais de uma dezena de opções de configuração da Biblioteca CISIS
  2. Parei o desenvolvimento do CISIS em 2010
  3. FYI, meu último desenvolvimento foi fazer do utilitário MX uma função da Biblioteca CISIS, todavia apenas testada uma vez – no próprio fonte mxaot.c. Deve necessitar alguns ajustes para um melhor encapsulamento, como, por exemplo, chamar a função mx no próprio mxaot.c ou, melhor ainda, como uma proc() na linguagem de formato!

  
  **VII. Versões que tenho utilizado no meu doutorado (sobre Mortalidade; nada que ver bases de dados)**

  ```
   $LIND/mx what
   CISIS Interface v5.7e/G/PC/W/L/M/32767/16/60/I/64bits - Utility MX
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  ```

  ```
   $LINDG4/mx what
   CISIS Interface v5.7e/G/PC/512G/W/L4/M/32767/16/60/I/64bits - Utility MX
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  ```

  ```
   $FFI/mx what
   CISIS Interface v5.7e/G/PC/W/F/L/M/1048576/16/60/I/64bits - Utility MX
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  ```

  ```
   $FFIG4/mx what
   CISIS Interface v5.7e/G/PC/512G/W/F/L4/M/1048576/16/60/I/64bits - Utility MX
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  ```

  ## Indización

  ### Memoria en técnica 4

  Parámetros para aumentar la capacidad de memoria para poder indizar por palabra sin tener overflow en el DIR del registro virtual y aumentar la memoria de PFT

  ```
   mx mfrl=64000 fmtl=64000 textosdb maxlk1=<pongaelmaximodepalabrasquevaagenerareltextomaslargo> maxlk2=<creaoqueyaessuficienteperotambienpuedesespecificarlo> fst=@ fullinv=textosdb
  ```

  ### meaning of invx

  M/F con campo repetitivo 101. ^p es prefijo para la busqueda; ^y es el I/F que corresponde.

  Ej: si tienes cdsti con las palabras del titulo, cdsau con los autores, cdskw con las keywords [root@dis-tardelli ~]# mx null "proc='a101|^p*^ycdsti|a101|^pTI ^ycdsti|a101|^pAB ^ycdsau|a101|^pAU ^ycdsAU|a101|^pKW ^ycdskw|'" count=1 now create=cds.invx

  ```
   mfn=     1
   101  "^p*^ycdsti"
   101  "^pTI ^ycdsti"
   101  "^pAB ^ycdsau"
   101  "^pAU ^ycdsAU"
   101  "^pKW ^ycdskw"
  ```

  entonces:

  ```
   mx cds invx=cds.invx "bool=plants and water" busca em cdsti
   mx cds invx=cds.invx "bool=TI plants and TI water" idem
   mx cds invx=cds.invx "bool=[TI]plants and water" idem
   mx cds invx=cds.invx "bool=TI plants and TI water and AU Magalhaes and KW whatever" busca em los indices indicados por esos prefijos
  ```

  btw, el iah.xis convierte las entradas INDEX y FILE para un registro con los campos 101 que es pasado a la función de busqueda del CISIS!

  ### Significado del la extension .iy0

  La extension .iy0 es especifica para LIND. Es generada por el utilitario MKIY0, lo cual agrega el .cnt, .n01, .n02, .ly1, .ly2 .iyp en un unico archivo .iy0. Por diseño, hay que haver por lo menos una llave curta y una lunga en dicho I/F. La operacion del I/F en ambiente LIND, intenta abrir un .iy0, si existira. Caso contrario, abre los 6 archivos que forman el I/F.

  ### indexing technique 351

  IT 351 es una extension de la FST para preparación de MEDLINE. Procesa el campo MeSH Headings: extrae descriptor, descriptor + calificador y calificador. Tuve que investigar el fuente (kkk).

  ```
   Ej: [root@dis-tardelli ~]# mx null "proc='a10|Abstracting and Indexing as Topic/MT*|a10|Information Storage and Retrieval/SN*/MT|'" "fst=1 
  351 (v10/)"
  mfn=     1
   10  "Abstracting and Indexing as Topic/MT*"
   10  "Information Storage and Retrieval/SN*/MT"
    1  "ABSTRACTING AND INDEXING AS TOPIC^m1^o1^c1^l2"
    1  "ABSTRACTING AND INDEXING AS TOPIC/MT*^m1^o1^c1^l2"
    1  "/MT*^m1^o1^c1^l1"
    1  "INFORMATION STORAGE AND RETRIEVAL^m1^o1^c2^l2"
    1  "INFORMATION STORAGE AND RETRIEVAL/SN*^m1^o1^c2^l2"
    1  "/SN*^m1^o1^c2^l1"
    1  "INFORMATION STORAGE AND RETRIEVAL^m1^o1^c3^l2"
    1  "INFORMATION STORAGE AND RETRIEVAL/MT^m1^o1^c3^l2"
    1  "/MT^m1^o1^c3^l1"
   ..x
  ```

  ### Como indizar HTML palabra por palabra

  AOT, 4/29/2019

  **setup gizmo table to insert/assure proper space in a html file suitable for word indexing - add new entries as required**

  ```
  $LIND/mx null count=0                               create=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<|a2| <|'"     -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|>|a2|> |'"     -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<p>|   a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<p/>|  a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<br>|  a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<br/>| a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1| |a2| |'" -all append=gizhtml2fst
  ```

  **add HTML supported ISO-8859-1 characters (taken from https://www.freeformatter.com/html-entities.html)**

  Ver [Iso-8859-1.tab](http://abcdwiki.net/Iso-8859-1.tab)

  ```
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v2'|a2|'v2*1.1'|'" -all append=gizhtml2fst
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v3'|a2|'v2*1.1'|'" -all append=gizhtml2fst
  ```

  **html files and sample M/F**

  ```
  echo "<body><p>This is a <b>BOLDFACE</b> text.<p><BR>And this is a new paragraph.</body>" >htmltext.html
  ls -l index.html
  
  $LIND/mx null                            count=0 create=marx
  $LIND/mx null "proc='a1|htmltext.html|'" count=1 append=marx 
  $LIND/mx null "proc='a1|index.html|'"    count=1 append=marx 
  ```

  **show text word index**

  ```
  $LIND/mx mfrl=60000 fmtl=60000 uctab=ansi actab=ansi marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1'</1>'" "fst=2 8 '/TW_/',mpu,v1" now | more
  ```

  **gen text word index**

  ```
  $LIND/mx mfrl=60000 fmtl=60000                       marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1'</1>'" "fst=2 8 '/TW_/',mpu,v1" fullinv/ansi=marx
  ```

  **test text word index**

  ```
  $LIND/mx marx "bool=tw_boldface and tw_paragraph" lw=0 now pft=mfn,x1,v1/
  #$LIND/mx marx "bool=tw_mortalidade and tw_obito"  lw=0 now pft=mfn,x1,v1/
  ```

  ### Filtrar caracteres

  **Solicitud**: a function 'keepac()' to only keep the characters defined in actab, allowing e.g. easier construction of unique keys (as we need in RDA) by dropping all non-alphanumeric characters of a string, e.g. interpunction, spaces...

  **Respuesta**: preparar un gizmo con dos tipos de entradas

  ```
     caracter a mantener|caracter a mantener 
     caracter a borrar|
  ```

  y entonces usarlo en una "proc='Gesegizmo,tagquequieretransformar'" el MX o, eventualmente, en una especificacion de formato

  ```
     proc('Gesegizmo,tagquequieretransformar') 
  ```

  Warning: nunca usar un gizmo diretamente en el formato de una FST; aplicar gizmo en el dato y despues extraer las llaves

  ## Parámetros

  ### tmpx

  Si especificado, es el M/F donde la función de busqueda del CISIS va a crear los resultados de las busquedas. Eso permite usar expresiones meszlando términos con los resultados anteriores o solo esos, como (#1 or #2 or #3) and spinak

  

  ### gizp[/h]

  Descontinuado. Indicaba un M/F para generar los cambios de datos realizados por un gizmo=. Creo que fue usado para conocer la compresion de datos que haciamos con MEDLINE para publicarlo en CDROM

  ### jdi

  Implementacion del algoritmo Journal Descriptor Indexing de la NLM. Fue usado para hacer un ranking de registros LILACS según los Journal Descriptors de las revistas donde fueron publicados

  

  ### proc= Gmark or Gmarx

  proc=Gmark hace el markup ("sensibilación" o highlight) de un texto.

  El browser del DeCS (decs.bireme.br?) despliega los registros ya sensibilizados (vea [http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver/decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas%20Disease](http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver/decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas Disease))

  ### proc=Gmarx

  es para parsing de elementos XML.

  Fue el "medio de produccion" que he utilizado para sobrevivir a la introduccion de XML en BIREME. BTW, recibiamos MEDLINE en XML y la conversion a ISIS necesitaba ser procesada en un PC porque ahi teniamos un utilitario de conversion. Con proc=Gmarx (que en verdad no es tan generica asi) pude cargar MEDLINE, Cochrane y otras bases desde XML.

  ### proc=X[append]

  proc=if condition then 'Xxxx' fi" graba el registro corriente al master xxx

  

  ## Lenguaje de Formateo

  ### Striptags

  formatting language function 'striptags()' to drop all HTML- or XML-tags in a string

  ```
   "proc='d10<10 99 1>bla bla <xxx xxx xx x> bla bla <xxxx xxx> bla bla</mark></10>'" crea el tag 10 con "bla bla  bla bla  bla bla"
     99 = largo maximo de las marcas <>
      1 = largo minimo resultante para crear el campo 10
  ```

  Ejemplo

  ```
   s1:=s(cat\... una página web completa)
   "proc='d10<10 500 1>',s1,'</mark></10>'"    (extrae hasta 500 chars entre los delimintadores <...> )
   fst=10 4 v10
  ```

  ### Field update specification to add text between <tag> and </tag> and strip html or other markup marks

  sintax:

  ```
     proc=<tag>[ <marklen>[ <fldlen>]]><text></<tag>
  ```

  where:

  ```
     <tag>               field tag
     <marklen>           max mark length [default: <text> length]
     <fldlen>            min resulting field length to add a new occ of field <tag> [default: 0]
  ```

  function:

  ```
     add <text> as a new occurrence of field <tag>
     <marks> up to <marklen> chars are stripped from <text>
     do not add a new occurrence of field <tag> if the resulting text is shorter than <fldlen> chars
  ```

  examples:

  ```
    mx null count=1 "proc='<10>x<mark>y</mark>z</10>'"
    mfn=     1
    10  "xyz"
  
    mx null count=1 "proc='<10 0>x<mark>y</mark>z</10>'"     
    el 0 indica que no descarta ningun markup
    mfn=     1
    10  "x<mark>y</mark>z"
  
    mx null count=1 "proc='<10 5>x<mark>y</mark>z</10>'"    
    5 es el largo máximo que descarta, por esa razón descarta <mark> pero NO descarta </mark>
    mfn=     1
    10  "x<mark>y</mark>z"
  
    mx null count=1 "proc='<10 6>x<mark>y</mark>z</10>'"
    mfn=     1
    10  "xy</mark>z"
  
    mx null count=1 "proc='<10 7>x<mark>y</mark>z</10>'"
    mfn=     1
    10  "xyz"
  
    mx null \"proc='<10>text</10>'\"
    mfn=     1
    10  "text"
  
    mx null \"proc='<10>x<mark>y</mark>z</10>'\"
    mfn=     1
    10  "xyz"
  
    mx null \"proc='<10 99 3>x<mark>y</mark>z</10>'\"    
    El 3 dice que si el campo produce 3 chars como mínimo genera el campo
    mfn=     1
    10  "xyz"
  
    mx null \"proc='<10 99 4>x<mark>y</mark>z</10>'\"   
    Como la instrucción NO genera 4 chars, entonces el campo vuelve vacío
    mfn=     1
  ```

  

  ## Como usar o utilitario MX em ambiente CGI

  **URL syntax**:

  ```
    http://server/cgi-bin/mx/cgi=<file.pft>?parm1=value1&parm2=value2&parm3=value3 &...
  ```

  **Operation**:

  MX sets a null M/F record, parses the CGI QUERY_STRING environment variable (assuming REQUEST_METHOD=GET) or the CGI standard input (if REQUEST_METHOD=POST) and stores the calling parameter pairs (name and value) in a subfielded repeatable field using tag 2000 as:

  ```
    2000 “^nparm1^vvalue1”
    2000 “^nparm2^vvalue2”
    2000 “^nparm3^vvalue3”
  ```

  MX executes the format specification <file.pft>, with line width specification set to the maximum record length value, and each output line generated by this format specification is taken as an actual MX running parameters

  ```
    data base name parameter must be specified as “db=full path name”
    a “now” parameter must always be generated
    “cipar=” parameter is not supported
  ```

  
**< file.pft >**
  
example:

```
            /* MX CGI show - Lists ../html/<file>
        use: http://172.22.164.36/cgi-bin/mx/cgi=@show?file=<file> */
            /* parses CGI calling parms  */
            proc('d2001'
                (if v2000^n='file'   then 'a2001|'v2000^v  '|',break fi)
            )
            proc(if v2001= then 'd2001a2001|sample.htm|' fi  )
            
            /* generates MX running parms  */
            `db=null`/
            `count=1`/
            if right(v2001,4)='.txt' then
                `prolog='Content-type: text/plain'/#`/
            else
                `prolog='Content-type: text/html'/#`/
            fi
            `pft=cat('../html/`v2001`')/`/
                if right(v2001,4)='.txt' then
                    `epilog=#`/
                fi
            `lw=0`/
            `now`/
            `uctab=ansi`/
            /* end of  MX CGI show  */
```

#### Ejemplo de llamada del MX en ambiente CGI

Hacer lo siguiente

1. copie um executável mx en el diretório cgi-bin de su servidor web (el mismo usada en la geração do master y del invertido de cds, por favor)
2. copia el arquivo [mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) (anexo) nesse mesmo diretório
3. abre um browser e digita: http://seuservidor/cgi-bin/mx/cgi=@mx.pft?db=dirondeestacds/cds&bool=plants and water&pft=@&now

El [mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) simplemente recorre el campo repetitivo 2000 (que es donde como primer paso el mx arma los parámetros pasados en la URL) y "despliega" los parámetros para la ejecución efectiva del MX. Así de simple=

## Compilación

**how to compile CISIS for Windows 64-bits ?** (Borland 5.2 is only 32-bits)

probar un GCC para Windows, Mirá al GCC para Windows: http://mingw-w64.org/doku.php Hay que investigar ese punto, pues yo tambien quisera usarlo en Windows 64-bits.

**adjusting CISIS to be used in a modern IDE** (Visual Studio, Netbeans, Code:Blocks...)

AOT: Btw, me acuerdo que el viejo CISIS ya fue compilado en Microsoft C something.

**Special truncation to LE2 length**

Egbert: we need to manipulate (special truncation to LE2 length), in cifst.c, the string to be passed on for insertion in the to IF B-Tree (fst_link()); we have a function to perform that manipulation and it works well when tested on the screen (the test-mode by only showing the extracted keys, e.g. 'mx mydb fst=@') but when doing the real indexing (mx mydb fst=@ fullinv=mydb now -all) the string again appears as before the function was called. So where to insert that function right before the insertion to avoid that subsequent memcp and other functions undo the manipulation ?

AOT: Pls, provide a real example (data, fst, everything to reproduce it)
