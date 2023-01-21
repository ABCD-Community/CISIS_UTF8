- ## Summary of CISIS versions and configurations

  **I. ISIS version or LIND version: 2 worlds, with the same M/F but with different I/F**

  - World 1: ISIS – I/F (.ifp) with 8-byte postings containing 4 components:

  ```
     mfn – 3 bytes
     tag – 2 bytes
     occ – 1 byte – occurrence at each %
     cnt – 2 bytes – counter gives ”key” within each occurrence
  ```

  - World 2: LIND – I/F (.iyp) with postings of only 1 component

  ```
     mfn – 3 bytes (or 4 bytes in the LIND4 or LINDG4 version)
  ```

  The list of postings of each key is implemented as a list of mfn's or bitstring (1 bit for each mfn of M/F), or to occupy less space

  
  **II. FFI version: for databases with records larger than 64Kbyte**

  - all the fields referring to the size of the M/F records implemented in 4 bytes
  - therefore the M/F generated with and without FFI are not compatible

  
  **III. LIND4 version: for databases with more than 16 thousand records**

  - all fields referring to mfn and controls and I/F pointers implemented in 4 bytes
  - therefore the I/F generated with LIND are not compatible with LIND4 or LINDG4
  - meanwhile, the M/F generated with ISIS, LIND, LIND4 or LINDG4 are fully compatible

  
  **IV. version G: library and utilities compiled with the LARGE FILE SUPPORT option of GCC**

  - We support files larger than 2Gbyte
  - Files generated with version G are compatible if they will not exceed 2Gbyte
  - btw, it is possible to compile the ISIS version with this option (it would be called ISISG)

  
  **V. About the configurations (compilation options)** :

  - Size of the keys of I/F

  1. 1. LE1 – short key sizes (typically 10 or 16)
     2. LE2 – size of long keys (typically 30 or 60)

  

  - Size two registers of M/F

  1. 1. MAXMFRL – maximum size of two M/F records
     2. By default, it is 32K for multi-user and 63K for single-user mode.

  ```
  Note 1: To maintain compatibility of CDS/ISIS v3 with v2, GDB uses the signal of the mfrl component
           as registry flag locked by another user...
  Note 2: consult the source cisis. I saw that someone in BIREME lowered the 64K limit for 63K
  ```

  by default, the FFI version is 1Mbyte

  
  For files larger than 2Gigabyte, you *must use* a xxxGyyy version. I suggest ISISG4 or LINDG4, since I think that the databases that he is going to manipulate do not need long records (ie, FFI versions).

  G stands for Gcc large file support (ie, more than 2Gb.)

  The xxxx4 versions support I/F generated from M/F with more than 16 million records.

  
  **SAW. Other compilation options**

  1. There are more than a dozen CISIS Library configuration options
  2. About the development of CISIS in 2010
  3. FYI, my latest development was made by the MX utility, a function of the CISIS Library, still barely tested once – not its own source mxaot.c. It should need some tweaking for a better encapsulation, like, for example, calling the mx function in its own mxaot.c or, better still, like a proc() in the format language!

  
  **VII. Versions that I have used not by my doctor (about Mortality; nothing to do with databases)**

 
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


  ## Indexing

  ### Memory in technique 4

  Parameters to increase the memory capacity to be able to index by word without having overflow in the DIR of the virtual register and to increase the memory of PFT

  ```
  mx mfrl=64000 fmtl=64000 dbtexts maxlk1=<putthemaximumofwordsthatwillgeneratethelongesttext> maxlk2=<youthinkthatisenoughbutyoucanalsospecifyit> fst=@ fullinv=dbtexts
  ```

  ### meaning of invx

  M/F with repetitive field 101. ^p is prefix for the search; ^y is the corresponding I/F.

  Eg: if you have cdsti with the words in the title, cdsau with the authors, cdskw with the keywords [root@dis-tardelli ~]# mx null "proc='a101|^p*^ycdsti|a101|^pTI ^ycdsti| a101|^pAB ^ycdsau|a101|^pAU ^ycdsAU|a101|^pKW ^ycdskw|'" count=1 now create=cds.invx

  ```
  mfn= 1
   101 "^p*^ycdsti"
   101 "^pTI ^ycdsti"
   101 "^pAB ^ycdsau"
   101 "^pAU ^ycdsAU"
   101 "^pKW ^ycdskw"
  ```

  then:

  ```
  mx cds invx=cds.invx "bool=plants and water" search em cdsti
   mx cds invx=cds.invx "bool=TI plants and TI water" idem
   mx cds invx=cds.invx "bool=[TI]plants and water" idem
   mx cds invx=cds.invx "bool=TI plants and TI water and AU Magalhaes and KW whatever" searches the indices indicated by those prefixes
  ```

  btw, the iah.xis converts the INDEX and FILE entries to a record with the 101 fields which is passed to the CISIS search function!

  ### Meaning of the .iy0 extension

  The .iy0 extension is specific to LIND. It is generated by the MKIY0 utility, which adds the .cnt, .n01, .n02, .ly1, .ly2 .iyp into a single .iy0 file. By design, there must be at least one short key and one long key on said I/F. The I/F operation in LIND environment, tries to open an .iy0, if it exists. Otherwise, open the 6 files that make up the I/F.

  ### indexing technique 351

  IT 351 is an extension of the FST for MEDLINE preparation. Processes the MeSH Headings field: extracts descriptor, descriptor + qualifier, and qualifier. I had to investigate the source (kkk).

  ```
  Eg: [root@dis-tardelli ~]# mx null "proc='a10|Abstracting and Indexing as Topic/MT*|a10|Information Storage and Retrieval/SN*/MT|'" "fst=1
  351 (v10/)"
  mfn= 1
   10 "Abstracting and Indexing as Topic/MT*"
   10 "Information Storage and Retrieval/SN*/MT"
    1 "ABSTRACTING AND INDEXING AS TOPIC^m1^o1^c1^l2"
    1 "ABSTRACTING AND INDEXING AS TOPIC/MT*^m1^o1^c1^l2"
    1 "/MT*^m1^o1^c1^l1"
    1 "INFORMATION STORAGE AND RETRIEVAL^m1^o1^c2^l2"
    1 "INFORMATION STORAGE AND RETRIEVAL/SN*^m1^o1^c2^l2"
    1 "/SN*^m1^o1^c2^l1"
    1 "INFORMATION STORAGE AND RETRIEVAL^m1^o1^c3^l2"
    1 "INFORMATION STORAGE AND RETRIEVAL/MT^m1^o1^c3^l2"
    1 "/MT^m1^o1^c3^l1"
   ..x
  ```

  ### How to index HTML word by word

  AOT, 4/29/2019

  **setup gizmo table to insert/assure proper space in a html file suitable for word indexing - add new entries as required**

  ```
  $LIND/mx null count=0 create=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<|a2| <|'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|>|a2|> |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<p>| a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<p/>| a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<br>| a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1|<br/>| a2| |'" -all append=gizhtml2fst
  $LIND/mx null count=1 "proc='a1| |a2| |'" -all append=gizhtml2fst
  ```

  **add HTML supported ISO-8859-1 characters (taken from https://www.freeformatter.com/html-entities.html )**

  See [Iso-8859-1.tab](http://abcdwiki.net/Iso-8859-1.tab)

  ```
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v2'|a2|'v2*1.1'|'" -all append=gizhtml2fst
  $LIND/mx seq=ISO-8859-1.tab "proc='a1|v3'|a2|'v2*1.1'|'" -all append=gizhtml2fst
  ```

  **html files and sample M/F**

  ```
  echo "<body><p>This is a <b>BOLDFACE</b> text.<p><BR>And this is a new paragraph.</body>" >htmltext.html
  ls -l index.html
  
  $LIND/mx null count=0 create=marx
  $LIND/mx null "proc='a1|htmltext.html|'" count=1 append=marx
  $LIND/mx null "proc='a1|index.html|'" count=1 append=marx
  ```

  **show text word index**

  ```
  $LIND/mx mfrl=60000 fmtl=60000 uctab=ansi actab=ansi marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1' </1>'" "fst=2 8 '/TW_/',mpu,v1" now | more
  ```

  **gene text word index**

  ```
  $LIND/mx mfrl=60000 fmtl=60000 marx "proc='Gload/1='v1" "proc='Ggizhtml2fst,1'" "proc='d1<1 99 1>'v1'</1>'" "fst=2 8 '/TW_/',mpu,v1" fullinv/ansi=marx
  ```

  **test text word index**

  ```
  $LIND/mx marx "bool=tw_boldface and tw_paragraph" lw=0 now pft=mfn,x1,v1/
  #$LIND/mx marx "bool=tw_mortality and tw_obito" lw=0 now pft=mfn,x1,v1/
  ```

  ### filter characters

  **Request** : a function 'keepac()' to only keep the characters defined in actab, allowing eg easier construction of unique keys (as we need in RDA) by dropping all non-alphanumeric characters of a string, eg interpunction, spaces...

  **Answer** : prepare a gizmo with two types of inputs

  ```
     character to keep|character to keep
     character to delete |
  ```

  and then use it in a "proc='Gesegizmo,tagthatyouwanttotransform'" in the MX or, eventually, in a format specification

  ```
     proc('Gesegizmo,tagyouwanttotransform') 
  ```

  Warning: never use a gizmo directly in the format of an FST; apply gizmo on the data and then extract the keys

  ## parameters

  ### tmpx

  If specified, it is the M/F where the CISIS search function will create the search results. This allows you to use expressions mixing terms with the previous results or just those, like (#1 or #2 or #3) and spinak

  

  ### gizp[/h]

  discontinued. It indicated an M/F to generate the data changes made by a gizmo=. I think it was used to learn about the data compression we did with MEDLINE to publish it on CDROM.

  ### jdi

  Implementation of the Journal Descriptor Indexing algorithm of the NLM. It was used to make a ranking of LILACS records according to the Journal Descriptors of the journals where they were published.

  

  ### proc= Gmark or Gmarx

  proc=Gmark makes the markup ("sensitization" or highlight) of a text.

  The DeCS browser (decs.bireme.br?) displays the already sensitized records (see [http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver /decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas%20Disease](http://decs.bvsalud.org/cgi-bin/wxis1660.exe/decsserver/?IsisScript=../cgi-bin/decsserver/decsserver.xis&search_language=i&interface_language=p&previous_page=homepage&task=exact_term&search_exp=Chagas Disease) )

  ### proc=Gmarx

  is for parsing XML elements.

  It was the "production medium" that I have used to survive the introduction of XML in BIREME. BTW, we received MEDLINE in XML and the conversion to ISIS needed to be processed on a PC because we had a conversion utility there. With proc=Gmarx (which really isn't that generic like that) I was able to load MEDLINE, Cochrane and other databases from XML.

  ### proc=X[append]

  proc=if condition then 'Xxxx' fi" write current record to master xxx

  

  ## Formatting Language

  ### Striptags

  formatting language function 'striptags()' to drop all HTML- or XML-tags in a string

  ```
  "proc='d10<10 99 1>blah blah <xxx xxx xx x> blah blah <xxxx xxx> blah blah</mark></10>'" creates tag 10 with "blah blah blah blah blah blah"
     99 = maximum length of marks <>
      1 = minimum resulting length to create the field 10
  ```

  Example

  ```
  s1:=s(cat\... an entire web page)
   "proc='d10<10 500 1>',s1,'</mark></10>'" (extracts up to 500 chars between <...> delimiters)
   fst=10 4 v10
  ```

  ### Field update specification to add text between <tag> and </tag> and strip html or other markup marks

  syntax:

  ```
     proc=<tag>[ <marklen>[ <fldlen>]]><text></<tag>
  ```

  where:

  ```
     <tag> field tag
     <marklen> max mark length [default: <text> length]
     <fldlen> min resulting field length to add a new occ of field <tag> [default: 0]
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
    mfn= 1
    10 "xyz"
  
    mx null count=1 "proc='<10 0>x<mark>y</mark>z</10>'"     
    0 indicates that it does not rule out any markup
    mfn= 1
    10 "x<mark>y</mark>z"
  
    mx null count=1 "proc='<10 5>x<mark>y</mark>z</10>'"    
    5 is the maximum length it discards, for that reason it discards <mark> but does NOT discard </mark>
    mfn= 1
    10 "x<mark>y</mark>z"
  
    mx null count=1 "proc='<10 6>x<mark>y</mark>z</10>'"
    mfn= 1
    10 "xy</mark>z"
  
    mx null count=1 "proc='<10 7>x<mark>y</mark>z</10>'"
    mfn= 1
    10 "xyz"
  
    mx null \"proc='<10>text</10>'\"
    mfn= 1
    10 "text"
  
    mx null \"proc='<10>x<mark>y</mark>z</10>'\"
    mfn= 1
    10 "xyz"
  
    mx null \"proc='<10 99 3>x<mark>y</mark>z</10>'\"    
    The 3 says that if the field produces at least 3 chars it generates the field
    mfn= 1
    10 "xyz"
  
    mx null \"proc='<10 99 4>x<mark>y</mark>z</10>'\"   
    Since the instruction does NOT generate 4 chars, then the field returns empty
    mfn= 1
  ```

  

  ## How to use the MX utility in CGI environment

  **URL syntax** :

  ```
    http://server/cgi-bin/mx/cgi= <file.pft>?parm1=value1&parm2=value2&parm3=value3&...
  ```

  **Operation** :

  MX sets a null M/F record, parses the CGI QUERY_STRING environment variable (assuming REQUEST_METHOD=GET) or the CGI standard input (if REQUEST_METHOD=POST) and stores the calling parameter pairs (name and value) in a subfielded repeatable field using tag 2000 years:

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

  
  <file.pft> example:

  ```
    /* MX CGI show - Lists ../html/<file>
  use: http://172.22.164.36/cgi-bin/mx/cgi=@show?file=<file> */
    /* parses CGI calling parms */
       proc('d2001'
           (if v2000^n='file' then 'a2001|'v2000^v '|',break fi)
       )
       proc(if v2001= then 'd2001a2001|sample.htm|' fi )
    
    /* generate MX running parms */
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
      /* end of MX CGI show */
  ```

  #### Example of MX call in CGI environment

  Do the next

  1. copy an executável mx in the cgi-bin directory of your web server (the same one used in the generation of the master and the inverter of cds, please)
  2. copy the [file mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) (annex) in the same directory
  3. open a browser and type: http://seuservidor/cgi-bin/mx/cgi=@mx.pft?db=dirondeestacds/cds&bool=plants and water&pft=@&now

  The [mx.pft](http://abcdwiki.net/wiki/es/index.php?title=Mx.pft) simply goes through the repetitive field 2000 (which is where as the first step the mx builds the parameters passed in the URL) and "displays" the parameters for the effective execution of the MX. that simple =

  ## Compilation

  **how to compile CISIS for Windows 64-bits ?** (Borland 5.2 is only 32-bit)

  try a GCC for Windows, Look at the GCC for Windows: http://mingw-w64.org/doku.php You have to investigate that point, because I would also like to use it on Windows 64-bits.

  **adjusting CISIS to be used in a modern IDE** (Visual Studio, Netbeans, Code:Blocks...)

  AOT: Btw, I remember that the old CISIS was already compiled in Microsoft C something.

  **Special truncation to LE2 length**

  Egbert: we need to manipulate (special truncation to LE2 length), in cifst.c, the string to be passed on for insertion in the to IF B-Tree (fst_link()); we have a function to perform that manipulation and it works well when tested on the screen (the test-mode by only showing the extracted keys, eg 'mx mydb fst=@') but when doing the real indexing (mx mydb fst=@ fullinv=mydb now -all) the string again appears as before the function was called. So where to insert that function right before the insertion to avoid that subsequent memcp and other functions undo the manipulation?

  AOT: Pls, provide a real example (data, fst, everything to reproduce it)
