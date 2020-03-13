# utl-remove-duplicate-words-from-a-sentence-hash-solution
Remove duplicate words from a sentence hash solution

    Remove duplicate words from a sentence hash solution

    github
    https://tinyurl.com/ssgl3o6
    https://github.com/rogerjdeangelis/utl-remove-duplicate-words-from-a-sentence-hash-solution

    SAS Forum
    https://tinyurl.com/ronlftj
    https://communities.sas.com/t5/SAS-Programming/sas-concatenate-strings-without-duplicate-value/m-p/631718


    other hash repos
    https://github.com/rogerjdeangelis?tab=repositories&q=hash+in%3Aname&type=&language=

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;


    filename ft15f001 "d:/txt/have.txt";
    parmcards4;
    spanner, span, spaniel, span
    span, span, spaniel, span
    san, span, spaniel, san
    ;;;;
    run;quit;


    d:/txt/have.txt

    spanner, span, spaniel, span
    span, span, spaniel, span
    san, span, spaniel, san

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WORK.WANT total obs=3

      ID  SENTENCE

       1  spaniel,span,spanner
       2  spaniel,span
       3  spaniel,span,san

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    proc datasets lib=work;
      delete havNrm want;
    run;quit;

    data want;

     if _n_=0 then do; %let rc=%sysfunc(dosubl('
         data havNrm;
             retain id 1;
             infile "d:/txt/have.txt"  delimiter="," line=lyn;
             input word$ @@;
             id = id + (lyn=2);  * the missing comma inrements lyn pointer;
         run;quit;
         '));
     end;

     length sentence $200;
     if _n_=1 then do;

        dcl hash H () ;
        h.definekey  ("id","word") ;
        h.definedata ("id","word") ;
        h.definedone () ;

     end;

     do until(last.id);

       set havNrm;
       by id;

       if h.check()=0 then continue;

       h.add();
       sentence=catx(',',word,sentence);

     end;

     drop word;
     output;

     h.clear();

    run;quit;

