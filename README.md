# utl-createa-a-dataset-for-each-variable-and-adding-a-common-primary-key
Createa a dataset for each variable and adding a common primary key
    Createa a dataset for each variable and adding a common primary key

      Example

         Using sashelp.class create four tables with two columns each
         AGE, SEX, HEIGHT and WEIGHT.

         Each table will have the variable  NAME(primary key) and one of the variables from SASHELP.CLASS

         For big data partitioning this can aid in parrallel processing.


    GitHub
    https://tinyurl.com/ctu34xs4
    https://github.com/rogerjdeangelis/utl-createa-a-dataset-for-each-variable-and-adding-a-common-primary-key

    SAS Forum
    https://tinyurl.com/44dej66y
    https://communities.sas.com/t5/SAS-Programming/SAS-Macro-To-create-multiple-dataset-from-one-dataset/m-p/724130

    reklated github
    https://tinyurl.com/y6uzqbm2
    https://github.com/rogerjdeangelis/utl_thirteen_algorithms_to_split_a_table_based_on_groups_of_data

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    * The variable name that contains the primary key;

    SASHELP.CLASS

    Up to 40 obs from SASHELP.CLASS total obs=19

    Obs    NAME       SEX    AGE    HEIGHT    WEIGHT

      1    Joyce       F      11     51.3       50.5
      2    Louise      F      12     56.3       77.0
      3    Alice       F      13     56.5       84.0
    ...

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    * this creates a macro array of all variable names in any table except the variable 'name';

    * incase you rerun;
    %arraydelete(vrs);           * delete array;
    proc datasets lib=work kill; * empty work'
    run;quit;

    * note variable 'name' is not in the macro array;
    %array(vrs,values=%varlist(sashelp.class,prx=/^(?!NAME)/i));

    /*
    %put &=vrs1 ;
    %put &=vrs2 ;
    %put &=vrs3 ;
    %put &=vrs4 ;

    %put &=vrsn ;

    * what is in your array
    VRS1=SEX
    VRS2=AGE
    VRS3=HEIGHT
    VRS4=WEIGHT
    VRSN=4
    */

    /* you can use macro  macro variables inside do_over.
       ie &dsn &primarykey.
       Or you can wrap in a macro.
       Or you can have do_over call a macro
    */

    %do_over(vrs,phrase=%str(
        data work.?;
           set sashelp.class(keep = name ?);
        run;quit;
    ));
    run;quit;

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                   _ |_|
      ___ ___   __| | ___
     / __/ _ \ / _` |/ _ \
    | (_| (_) | (_| |  __/
     \___\___/ \__,_|\___|

    ;

    * if you want the generated code;
    data;
    %do_over(vrs,phrase=%str(put
        "data work.?;
           set sashelp.class(keep = name ?);
         run;quit;";
    ));
    run;quit;

    data work.SEX;      set sashelp.class(keep = name SEX);     run;quit;
    data work.AGE;      set sashelp.class(keep = name AGE);     run;quit;
    data work.HEIGHT;   set sashelp.class(keep = name HEIGHT);  run;quit;
    data work.WEIGHT;   set sashelp.class(keep = name WEIGHT);  run;quit;

    *            _               _     _        _     _
      ___  _   _| |_ _ __  _   _| |_  | |_ __ _| |__ | | ___  ___
     / _ \| | | | __| '_ \| | | | __| | __/ _` | '_ \| |/ _ \/ __|
    | (_) | |_| | |_| |_) | |_| | |_  | || (_| | |_) | |  __/\__ \
     \___/ \__,_|\__| .__/ \__,_|\__|  \__\__,_|_.__/|_|\___||___/
                    |_|
    ;

    proc contents data=work._all_;
    run;quit;

    * if you want the code ;

    * Four daatsets placed in work library;

                Member   Obs, Entries
    #  Name     Type      or Indexes   Vars

    1  AGE      DATA          19        2
    2  HEIGHT   DATA          19        2
    3  SEX      DATA          19        2
    4  WEIGHT   DATA          19        2


    AGE total obs=19

    Obs    NAME       AGE

      1    Joyce       11
      2    Louise      12
      3    Alice       13
    ..

    HEIGHT total obs=19

    Obs    NAME       HEIGHT

      1    Joyce       51.3
      2    Louise      56.3
      3    Alice       56.5
    ...

    SEX total obs=19

    Obs    NAME       SEX

      1    Joyce       F
      2    Louise      F
      3    Alice       F
    ...

    WEIGHT total obs=19

    Obs    NAME       WEIGHT

      1    Joyce        50.5
      2    Louise       77.0
      3    Alice        84.0
    ...


