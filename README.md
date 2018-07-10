# utl_select_the_15_top_baseball_hitters_top_n_values_from_a_table
Select the 15 top baseballhitters top n values from a table.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.


    Select the 15 top baseballhitters top n values from a table

    https://tinyurl.com/yc5mmawc
    https://github.com/rogerjdeangelis/utl_select_the_15_top_baseball_hitters_top_n_values_from_a_table

    Original Topic: Which statement should I use to display the top 15 players with the highest efficiency rating?

       Same result WPS for SQL but WPS does not support 'open=defer' yet.


      TWO SOLUTIONS

         1. Proc SQL
         2. HASH ( my lame code - demonstrate open=defer?)

         10        set wrk.have(obs=1) wrk.havSrt(in=True obs=15) open=defer;
                                                                  ^
         ERROR: Found "open" when expecting one of END, KEY, NOBS or POINT

    SAS Forum
    https://tinyurl.com/ycoobuht
    https://communities.sas.com/t5/SAS-Procedures/which-statement-should-I-use-to-display-the-top-15-players-with/m-p/476657


    INPUT
    =====

    HAVE total obs=100

       PLAYER    HITS

          1       24
          2        8
          3       38
          4        9
          5       25
          6        8
          7        3
          8       10
          9       44
         ....

    EXAMPLE OUTPUT  TOP 15
    ----------------------

    WORK.WANT total obs=15

      Obs    PLAYER    HITS

        1      84       98
        2      97       97
        3      38       97
        4      15       96
        5      49       96
        6      95       96
        7      42       94
        8      94       94
        9      90       92
       10      60       91
       11      76       91
       12      29       90
       13      14       90
       14      86       88
       15      46       87


    PROCESS
    =======

    1. Proc SQL

       proc sql;
         reset outobs=15;
         create
            table want as
         select
            *
         from
            have
         order
            by hits descending
       ;quit;

    2. HASH ( my lame code - demonstrate open=defer?)

       options obs=max;
       data want;

          *if 0 then set have;

         if _n_=1 then do;
          declare hash sortha(dataset: 'have', ordered:'d', multidata: 'y');
          sortha.definekey ("hits" );
          sortha.defineData(all:'yes');
          sortha.definedone();
          sortha.output(dataset:'havSrt');
         end;

          set have(obs=1) havSrt(in=True obs=15) open=defer;
          if true then output;

       run;quit;


    OUTPUT
    ======

    WORK.WANT total obs=15

      Obs    PLAYER    HITS

        1      84       98
        2      97       97
        3      38       97
        4      15       96
        5      49       96
        6      95       96
        7      42       94
        8      94       94
        9      90       92
       10      60       91
       11      76       91
       12      29       90
       13      14       90
       14      86       88
       15      46       87


    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
      do player=1 to 100;
        hits=int(100*uniform(1234));
        output;
      end;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;
    SAS see process

    *WPS;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
       proc sql;
         reset outobs=15;
         select
            *
         from
            wrk.have
         order
            by hits descending
       ;quit;
    run;quit;
    ');


    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
       data wrk.want;

         if _n_=1 then do;
          declare hash sortha(dataset: "wrk.have", ordered:"d", multidata: "y");
          sortha.definekey ("hits" );
          sortha.defineData(all:"yes");
          sortha.definedone();
          sortha.output(dataset:"havSrt");
         end;

          set wrk.have(obs=1) wrk.havSrt(in=True obs=15) open=defer;
          if true then output;

       run;quit;
    ');

