- ## Resumo das versões e configurações do CISIS

  **I. Versão ISIS ou versão LIND: 2 mundos, com o mesmo M/F mas com I/F diferentes**

  - Mundo 1: ISIS – I/F (.ifp) com postagens de 8 bytes contendo 4 componentes:

  ```
     mfn – 
     tag de 3 bytes – 2 bytes 
     occ – 1 byte – ocorrência a cada % 
     cnt – 2 bytes – contador de chave dentro de cada ocorrência
  ```

  - Mundo 2: LIND – I/F (.iyp) com lançamentos de apenas 1 componente

  ```
     mfn – 3 bytes (ou 4 bytes na versão LIND4 ou LINDG4)
  ```

  A lista de postagens de cada chave é implementada como uma lista de mfn's ou bitstring (1 bit para cada mfn de M/F), ou para ocupar menos espaço

  
  **II. Versão FFI: para bancos de dados com registros maiores que 64Kbyte**

  - todos os campos referentes ao tamanho dos registros M/F implementados em 4 bytes
  - portanto os M/F gerados com e sem FFI não são compatíveis

  
  **III. Versão LIND4: para bancos de dados com mais de 16 mil registros**

  - todos os campos referentes a mfn e controles e ponteiros I/F implementados em 4 bytes
  - portanto, o I/F gerado com LIND não é compatível com LIND4 ou LINDG4
  - enquanto isso, os M/F gerados com ISIS, LIND, LIND4 ou LINDG4 são totalmente compatíveis

  
  **4. versão G: biblioteca e utilitários compilados com a opção LARGE FILE SUPPORT do GCC**

  - Suportamos arquivos maiores que 2 Gbyte
  - Arquivos gerados com a versão G são compatíveis desde que não ultrapassem 2Gbyte
  - btw, é possível compilar a versão ISIS com esta opção (seria chamada ISISG)

  
  **V. Sobre as configurações (opções de compilação)** :

  - Tamanho das chaves de I/F

  1. 1. LE1 – tamanhos de chave curtos (normalmente 10 ou 16)
     2. LE2 – tamanho das teclas longas (normalmente 30 ou 60)

  

  - Tamanho dois registradores de M/F

  1. 1. MAXMFRL – tamanho máximo de dois registros M/F
     2. Por padrão, é 32K para multiusuário e 63K para modo de usuário único.

  ```
  Nota 1: para manter a compatibilidade do CDS/ISIS v3 com v2, o GDB usou o sinal do componente mfrl 
           como sinalizador de registro bloqueado por outro usuário...
  Nota 2: consulte a fonte cisis, vi que alguém na BIREME abaixou o limite de 64K para 63K
  ```

  por padrão, a versão FFI é 1Mbyte

  
  Para arquivos maiores que 2Gigabytes, você *deve usar* uma versão xxxGyyy. Sugiro ISISG4 ou LINDG4, pois acho que as bases de dados que ele vai manipular não precisam de registros longos (ou seja, versões FFI).

  G significa suporte a arquivos grandes Gcc (ou seja, mais de 2 Gb.)

  As versões xxxx4 suportam I/F gerado a partir de M/F com mais de 16 milhões de registros.

  
  **VI. Outras opções de compilação**

  1. Há mais de uma dúzia de opções de configuração da Biblioteca CISIS
  2. Sobre o desenvolvimento do CISIS em 2010
  3. Para sua informação, meu último desenvolvimento foi feito pelo utilitário MX, uma função da Biblioteca CISIS, ainda mal testada uma vez – não é sua própria fonte mxaot.c. Ele deve precisar de alguns ajustes para um melhor encapsulamento, como, por exemplo, chamar a função mx em seu próprio mxaot.c ou, melhor ainda, como um proc() na linguagem do formato!

  
  **VII. Versões que usei não pelo meu médico (sobre Mortalidade; nada a ver com bancos de dados)**

  ```
  $LIND/mx que 
   Interface CISIS v5.7e/G/PC/W/L/M/32767/16/60/I/64bits - Utility MX 
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ISI/ UTL/INVX/B7/FAT/CIP/CGI/MX/W 
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  $LINDG4/mx que 
   interface CISIS v5.7e/G/PC/512G/W/L4/M/32767/16/60/I/64bits - Utility MX 
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W 
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  $FFI/mx que 
   Interface CISIS v5.7e/G/PC/W/F/L/M/1048576/16/60/I/64bits - Utilitário MX 
   CISIS Interface v5.7e/.iy0/Z/GIZ/DEC/ ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W 
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  $FFIG4/mx que 
   Interface CISIS v5.7e/G/PC/512G/W/F/L4/M/1048576/16/60/I/64bits - Utility MX 
   CISIS Interface v5.7e/.iy0/Z/GIZ/ DEC/ISI/UTL/INVX/B7/FAT/CIP/CGI/MX/W 
   Copyright (c)BIREME/PAHO 2010. http://reddes.bvsalud.org/projects/cisis
  ```

  ## indexação

  ### Memória na técnica 4

  Parâmetros para aumentar a capacidade de memória para poder indexar por palavra sem ter estouro no DIR do registrador virtual e aumentar a memória do PFT

  ```
  mx mfrl=64000 fmtl=64000 dbtexts maxlk1=<coloqueomáximodepalavrasqueirágerarotextomaislongo> maxlk2=<vocêpensaqueésuficiente,masvocêtambémpodeespecificá-lo> fst=@ fullinv=dbtextos
  ```

  ### significado de invx

  M/F com campo repetitivo 101. ^p é o prefixo da busca; ^y é o I/F correspondente.

  Ex.: se tiver cdsti com as palavras do título, cdsau com os autores, cdskw com as palavras-chave [root@dis-tardelli ~]# mx null "proc='a101|^p*^ycdsti|a101|^pTI ^ ycdsti| a101|^pAB ^ycdsau|a101|^pAU ^ycdsAU|a101|^pKW ^ycdskw|'" contagem=1 agora cria=cds.invx

  ```
  mfn= 1 
   101 "^p*^ycdsti" 
   101 "^pTI ^ycdsti" 
   101 "^pAB ^ycdsau" 
   101 "^pAU ^ycdsAU" 
   101 "^pKW ^ycdskw"
  ```

  então:

  ```
  mx cds invx=cds.invx "bool=plantas e água" procure em cdsti 
   mx cds invx=cds.invx "bool=TI plantas e TI água" idem 
   mx cds invx=cds.invx "bool=[TI]plantas e água " idem 
   mx cds invx=cds.invx "bool=TI plantas e TI água e AU Magalhães e KW o que for" pesquisa nos índices indicados por esses prefixos
  ```

  btw, o iah.xis converte as entradas INDEX e FILE em um registro com os 101 campos que é passado para a função de pesquisa do CISIS!

  ### Significado da extensão .iy0

  A extensão .iy0 é específica para LIND. Ele é gerado pelo utilitário MKIY0, que adiciona os arquivos .cnt, .n01, .n02, .ly1, .ly2 .iyp em um único arquivo .iy0. Por design, deve haver pelo menos uma chave curta e uma chave longa no referido I/F. A operação I/F em ambiente LIND, tenta abrir um .iy0, se existir. Caso contrário, abra os 6 arquivos que compõem o I/F.

  ### técnica de indexação 351

  IT 351 é uma extensão do FST para preparação MEDLINE. Processa o campo MeSH Headings: extrai descritor, descritor + qualificador e qualificador. Tive que investigar a fonte (kkk).

  ```
  Por exemplo: [root@dis-tardelli ~]# mx null "proc='a10|Abstraindo e indexando como tópico/MT*|a10|Armazenamento e recuperação de informações/SN*/MT|'" "fst=1 
  351 (v10/ )" 
  mfn= 1 
   10 "Resumo e indexação como tópico/MT*" 
   10 "Armazenamento e recuperação de informações/SN*/MT" 
    1 "Resumo e indexação como tópico^m1^o1^c1^l2" 
    1 " Resumo e indexação como tópico TÓPICO/MT*^m1^o1^c1^l2" 
    1 "/MT*^m1^o1^c1^l1" 
    1 "ARMAZENAMENTO E RECUPERAÇÃO DE INFORMAÇÕES^m1^o1^c2^l2" 
    1 "ARMAZENAMENTO E RECUPERAÇÃO DE INFORMAÇÕES/SN *^m1^o1^c2^l2" 
    1 "/SN*^m1^o1^c2^l1" 
    1 "ARMAZENAMENTO E RECUPERAÇÃO DE INFORMAÇÕES^m1^o1^c3^l2" 
    1 "ARMAZENAMENTO E RECUPERAÇÃO DE INFORMAÇÕES/MT^m1^o1^c3^l2" 
    1 "/MT^m1^o1^c3^l1" 
   ..x
  ```

  ### Como indexar HTML palavra por palavra

  AOT, 29/04/2019

  **configurar a tabela gizmo para inserir/garantir o espaço adequado em um arquivo html adequado para indexação de palavras - adicione novas entradas conforme necessário**

  ```
  $LIND/mx null count=0 create=gizhtml2fst 
  $LIND/mx null count=1 "proc='a1|<|a2| <|'" -all append=gizhtml2fst 
  $LIND/mx null count=1 "proc=' a1|>|a2|> |'" -all append=gizhtml2fst 
  $LIND/mx null count=1 "proc='a1|<p>| a2| |'" -all append=gizhtml2fst 
  $LIND/mx null count= 1 "proc='a1|<p/>| a2| |'" -all append=gizhtml2fst 
  $LIND/mx null count=1 "proc='a1|<br>| a2| |'" -all append=gizhtml2fst 
  $LIND/mx null count=1 "proc='a1|<br/>| a2| |'" -all append=gizhtml2fst 
  $LIND/mx null count=1 "proc='a1| |a2| |'" - all append=gizhtml2fst
  ```

  **adicionar caracteres ISO-8859-1 compatíveis com HTML (retirados de https://www.freeformatter.com/html-entities.html )**

  Ver [ISO-8859-1.tab](http://abcdwiki.net/Iso-8859-1.tab)

  ```
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v2'|a2|'v2*1.1'|'" -all append=gizhtml2fst 
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v3'|a2|'v2*1.1'|'" -all append=gizhtml2fst
  ```

  **html e amostra M/F**

  ```
  echo "<body><p>Este é um texto <b>BOLDFACE</b>.<p><BR>E este é um novo parágrafo.</body>" >htmltext.html 
  ls -l index.html 
  
  $ LIND/mx null count=0 create=marx 
  $LIND/mx null "proc='a1|htmltext.html|'" count=1 append=marx 
  $LIND/mx null "proc='a1|index.html|'" contagem = 1 anexar = marx
  ```

  **mostrar índice de palavras de texto**

  ```
  $LIND/mx mfrl=60000 fmtl=60000 uctab=ansi actab=ansi marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1' </1>'" "fst=2 8 '/TW_/',mpu,v1" agora | mais
  ```

  **índice de palavra de texto gene**

  ```
  $LIND/mx mfrl=60000 fmtl=60000 marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1'</1>'" "fst=2 8 '/TW_/',mpu,v1" fullinv/ansi=marx
  ```

  **índice de palavras de texto de teste**

  ```
  $LIND/mx marx "bool=tw_boldface e tw_paragraph" lw=0 agora pft=mfn,x1,v1/ 
  #$LIND/mx marx "bool=tw_mortalidade e tw_obito" lw=0 agora pft=mfn,x1,v1/
  ```

  ### filtrar caracteres

  **Request** : uma função 'keepac()' para manter apenas os caracteres definidos no actab, permitindo, por exemplo, uma construção mais fácil de chaves exclusivas (como precisamos no RDA), eliminando todos os caracteres não alfanuméricos de uma string, por exemplo, interpunção, espaços...

  **Resposta** : prepare um gizmo com dois tipos de entradas

  ```
     caractere para manter|caractere para manter 
     caractere para deletar|
  ```

  e então usá-lo em um "proc='Gesegizmo,tagthatyouwanttotransform'" no MX ou, eventualmente, em uma especificação de formato

  ```
     proc('Gesegizmo,tagyouwanttotransform') 
  ```

  Atenção: nunca use um gizmo diretamente no formato de um FST; aplique gizmo nos dados e extraia as chaves

  ## parâmetros

  ### tmpx

  Se especificado, é o M/F onde a função de pesquisa do CISIS criará os resultados da pesquisa. Isso permite que você use expressões misturando termos com os resultados anteriores ou apenas esses, como (#1 ou #2 ou #3) e spinak

  

  ### gizp[/h]

  interrompido. Indicou um M/F para gerar as mudanças de dados feitas por um gizmo=. Acho que serviu para aprender sobre a compactação de dados que fizemos com o MEDLINE para publicar em CDROM.

  ### jdi

  Implementação do algoritmo Journal Descriptor Indexing do NLM. Foi utilizado para fazer um ranking dos registros LILACS de acordo com os Journal Descriptors dos periódicos onde foram publicados.

  

  ### proc= Gmark ou Gmarx

  proc=Gmark faz a marcação ("sensibilização" ou realce) de um texto.

  O navegador DeCS (decs.bireme.br?) exibe os registros já sensibilizados (ver [http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver/ decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas%20Disease](http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver/decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas Disease) )

  ### proc=Gmarx

  é para analisar elementos XML.

  Foi o "meio de produção" que usei para sobreviver à introdução do XML na BIREME. Aliás, recebemos MEDLINE em XML e a conversão para ISIS precisava ser processada em um PC porque tínhamos um utilitário de conversão lá. Com proc=Gmarx (que realmente não é tão genérico assim) consegui carregar MEDLINE, Cochrane e outros bancos de dados de XML.

  ### proc=X[acrescentar]

  proc=se condição então 'Xxxx' fi" grava o registro atual no master xxx

  

  ## Linguagem de Formatação

  ### Striptags

  função de linguagem de formatação 'striptags()' para descartar todas as tags HTML ou XML em uma string

  ```
  "proc='d10<10 99 1>blah blah <xxx xxx xx x> blah blah <xxxx xxx> blah blah</mark></10>'" cria tag 10 com "blah blah blah blah blah" 
     99 = máximo comprimento das marcas <> 
      1 = comprimento mínimo resultante para criar o campo 10
  ```

  Exemplo

  ```
  s1:=s(cat\... uma página da web inteira) 
   "proc='d10<10 500 1>',s1,'</mark></10>'" (extrai até 500 caracteres entre <. . .> ) 
   fst=10 4 v10
  ```

  ### Especificação de atualização de campo para adicionar texto entre <tag> e </tag> e retirar html ou outras marcas de marcação

  sintaxe:

  ```
     proc=<tag>[ <marklen>[ <fldlen>]]><text></<tag>
  ```

  Onde:

  ```
     <tag> tag de campo 
     <marklen> comprimento máximo da marca [padrão: comprimento <text>] 
     <fldlen> comprimento mínimo do campo resultante para adicionar uma nova ocorrência do campo <tag> [padrão: 0]
  ```

  função:

  ```
     adicione <text> como uma nova ocorrência do campo <tag> 
     <marks> até <marklen> caracteres são removidos de <text> 
     não adicione uma nova ocorrência do campo <tag> se o texto resultante for menor que <fldlen> caracteres
  ```

  exemplos:

  ```
    mx null count=1 "proc='<10>x<mark>y</mark>z</10>'" 
    mfn= 1 
    10 "xyz" 
  
    mx null count=1 "proc='<10 0>x< mark>y</mark>z</10>'"      
    o 0 indica que não descarta nenhuma marcação 
    mfn= 1 
    10 "x<mark>y</mark>z" 
  
    mx null count=1 "proc='< 10 5 >x<mark>y</mark>z</10>'"     
    5 é o comprimento máximo que ele descarta, então ele descarta <mark> mas NÃO descarta </mark> 
    mfn= 1 
    10 "x<mark> y< /mark>z" 
  
    mx null count=1 "proc='<10 6>x<mark>y</mark>z</10>'"
    mfn= 1 
    10 "xy</mark>z" 
  
    mx null count=1 "proc='<10 7>x<mark>y</mark>z</10>'" 
    mfn= 1 
    10 "xyz" 
    10 "texto"
   
    mx null \"proc='<10>texto</10>'\"
    mfn= 1 
  
    mx null \"proc='<10>x<mark>y</mark>z</10>'\" 
    mfn= 1 
    10 "xyz" 
  
    mx null \"proc='<10 99 3>x <mark>y</mark>z</10>'\"     
    O 3 diz que se o campo produzir pelo menos 3 caracteres gera o campo 
    mfn= 1 
    10 "xyz" 
  
    mx null \"proc='<10 99 4 > x<mark>y</mark>z</10>'\"    
    Como a instrução NÃO gera 4 caracteres, o campo retorna vazio 
    mfn= 1
  ```

  

  ## Como usar o utilitário MX no ambiente CGI

  **Sintaxe do URL** :

  ```
    http://server/cgi-bin/mx/cgi= <file.pft>?parm1=value1&parm2=value2&parm3=value3&...
  ```

  **Operação** :

  MX define um registro M/F nulo, analisa a variável de ambiente CGI QUERY_STRING (assumindo REQUEST_METHOD=GET) ou a entrada padrão CGI (se REQUEST_METHOD=POST) e armazena os pares de parâmetros de chamada (nome e valor) em um campo repetível com subcampos usando tag 2000 anos:

  ```
    2000 “^nparm1^vvalor1” 
    2000 “^nparm2^vvalor2” 
    2000 “^nparm3^vvalor3”
  ```

  MX executa a especificação de formato <file.pft>, com a especificação de largura de linha definida para o valor máximo de comprimento de registro, e cada linha de saída gerada por esta especificação de formato é tomada como parâmetros de execução MX reais

  ```
    o parâmetro do nome da base de dados deve ser especificado como “db=nome completo do caminho” 
    um parâmetro “agora” sempre deve ser gerado 
    O parâmetro “cipar=” não é suportado
  ```

  
  <arquivo.pft> exemplo:

  ```
    /* MX CGI show - Listas ../html/<arquivo>
  use: http://172.22.164.36/cgi-bin/mx/cgi=@show?file=<arquivo> */
    /* analisa parâmetros de chamada CGI */ 
       proc('d2001' 
           (if v2000^n='file' then 'a2001|'v2000^v '|',break fi) 
       ) 
       proc(if v2001= then 'd2001a2001|sample.htm |' fi )
     
    /* gera parâmetros de execução MX */ 
      `db=null`/ 
      `count=1`/ 
       if right(v2001,4)='.txt' then 
          `prolog='Content-type: text/plain'/ #`/ 
       else 
          `prolog='Tipo de conteúdo: text/html'/#`/ 
       fi 
       `pft=cat('../html/`v2001`')/`/ 
           if right(v2001,4)='. TXT'então 
              `epilog=#`/ 
           fi 
       `lw=0`/ 
       `now`/ 
       `uctab=ansi`/
      /* fim do programa MX CGI */
  ```

  #### Exemplo de chamada MX em ambiente CGI

  Fazer a próxima

  1. copie um executável mx no diretório cgi-bin do seu servidor web (o mesmo utilizado na geração do master e do inverter de cds, por favor)
  2. copie o [arquivo mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) (anexo) no mesmo diretório
  3. abra um navegador e digite: http://seuservidor/cgi-bin/mx/cgi=@mx.pft?db=dirondeestacds/cds&bool=plants and water&pft=@&now

  O [mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) simplesmente passa pelo campo repetitivo 2000 (que é onde como primeiro passo o mx constrói os parâmetros passados na URL) e “exibe” os parâmetros para a efetiva execução do MX. tão simples =

  ## Compilação

  **como compilar CISIS para Windows 64-bits?** (Borland 5.2 é apenas 32 bits)

  tente um GCC para Windows, veja o GCC para Windows: http://mingw-w64.org/doku.php Você tem que investigar esse ponto, porque eu também gostaria de usá-lo no Windows 64-bits.

  **ajustando CISIS para ser usado em um IDE moderno** (Visual Studio, Netbeans, Code:Blocks...)

  AOT: Aliás, lembro que o antigo CISIS já estava compilado em Microsoft C alguma coisa.

  **Truncamento especial para comprimento LE2**

  Egbert: precisamos manipular (truncamento especial para comprimento LE2), em cifst.c, a string a ser passada para inserção no IF B-Tree (fst_link()); temos uma função para realizar essa manipulação e ela funciona bem quando testada na tela (o modo de teste mostrando apenas as chaves extraídas, por exemplo 'mx mydb fst=@') mas ao fazer a indexação real (mx mydb fst=@ fullinv=mydb now -all) a string aparece novamente como antes de a função ser chamada. Então, onde inserir essa função logo antes da inserção para evitar que o memcp subsequente e outras funções desfaçam a manipulação?

  AOT: Por favor, forneça um exemplo real (dados, fst, tudo para reproduzi-lo)
