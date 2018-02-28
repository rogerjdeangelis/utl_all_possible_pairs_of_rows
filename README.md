# utl_all_possible_pairs_of_rows
All possible pairs of rows. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    All possible pairs of rows

    github
    https://github.com/rogerjdeangelis/utl_all_possible_pairs_of_rows

    https://goo.gl/QUpEXD
    https://stackoverflow.com/questions/49019251/how-to-get-the-combinations-of-multiple-vectors-in-r

    Karsten W profile
    https://stackoverflow.com/users/216064/karsten-w

    INPUT
    =====

     WORK.HAVE total obs=3

      Obs    A    B

       1     A    1
       2     B    2
       3     C    3

      WANT

       WORK.WANTANS total obs=6

        Pair   A    B

         1     A    1
         1     B    2

         2     A    1
         2     C    3

         3     B    2
         3     C    3

    PROCESS (working code - neat indexing WPS/Proc R or IML/R)
    ==========================================================

         want <- combn(nrow(have), 2, FUN=function(sub) x[sub,], simplify=FALSE);

         * add pair number;
         data want;
           retain pair 0;
           set wantwps;
           if mod(_n_-1,2)=0 then pair=pair+1;
         run;quit;

    OUTPUT
    ======

     WORK.WANT total obs=6

       PAIR    A    B

         1     A    1
         1     B    2

         2     A    1
         2     C    3

         3     B    2
         3     C    3

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    libname sd1 "d:/sd1";
    options validvarname=upcase;
    data sd1.have;
     input a$ b;
    cards4;
     A 1
     B 2
     C 3
    ;;;;
    run;quit;

    *
    __      ___ __  ___
    \ \ /\ / / '_ \/ __|
     \ V  V /| |_) \__ \
      \_/\_/ | .__/|___/
             |_|
    ;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk "%sysfunc(pathname(work))";
    libname hlp "C:\Program_Files\SASHome\SASFoundation\9.4\core\sashelp";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    library(tidyr);
    library(data.table);
    have<-as.data.frame(read_sas("d:/sd1/have.sas7bdat"));
    want <- combn(nrow(have), 2, FUN=function(sub) have[sub,], simplify=FALSE);
    want<-rbindlist(want);
    endsubmit;
    import r=want data=wrk.wantwps;
    run;quit;
    ');

    15        import r=want data=wrk.wantwps;
    NOTE: Creating data set 'WRK.wantwps' from R data frame 'want'
    NOTE: Data set "WRK.wantwps" has 6 observation(s) and 2 variable(s)


