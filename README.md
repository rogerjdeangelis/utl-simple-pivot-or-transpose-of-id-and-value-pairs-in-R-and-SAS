# utl-simple-pivot-or-transpose-of-id-and-value-pairs-in-R-and-SAS
Simple pivot or transpose of id and value pairs in R and SAS
    Simple pivot or transpose of id and value pairs in R and SAS

    https://tinyurl.com/y8gdhfha
    https://github.com/rogerjdeangelis/utl-simple-pivot-or-transpose-of-id-and-value-pairs-in-R-and-SAS

      Two Solutuins
             a. SAS
             b.R

    StackOverflow
    https://stackoverflow.com/questions/61511610/partial-pivot-wide-in-r


    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
     input Time Y;
    cards4;
    1 2
    1 3
    1 2
    2 5
    2 7
    2 5
    3 10
    3 9
    3 8
    ;;;;
    run;quit;

     WORK.HAVE total obs=9

      Time     Y

        1      2
        1      3
        1      2
        2      5
        2      7
        2      5
        3     10
        3      9
        3      8


    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

     WORK.WANT total obs=3

      Time     COL1    COL2    COL3

        1        2       3       2
        2        5       7       5
        3       10       9       8

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

      __ _     ___  __ _ ___
     / _` |   / __|/ _` / __|
    | (_| |_  \__ \ (_| \__ \
     \__,_(_) |___/\__,_|___/

    ;

    proc transpose data=have out=want;
      by time;
    run;quit;

    *_        ____
    | |__    |  _ \
    | '_ \   | |_) |
    | |_) |  |  _ <
    |_.__(_) |_| \_\

    ;


    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     input Time Y;
    cards4;
    1 2
    1 3
    1 2
    2 5
    2 7
    2 5
    3 10
    3 9
    3 8
    ;;;;
    run;quit;

    %utl_submit_r64('
    library(dplyr);
    library(tidyr);
    library(SASxport);
    library(data.table);
    library(stringr);
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    want <- have  %>%
        mutate(rn = str_c("COL", rowid(TIME))) %>%
        pivot_wider(names_from = rn, values_from = Y);
    write.xport(want,file="d:/xpt/want.xpt");
    ');

    libname xpt xport "d:/xpt/want.xpt";
    data want;
      set xpt.want;
    run;quit;
    libname xpt clear;


