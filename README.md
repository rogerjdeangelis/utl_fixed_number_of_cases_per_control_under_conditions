# utl_fixed_number_of_cases_per_control_under_conditions
How to assign multiple controls to each case under specified conditions. Clinical Case/Control.

    ```  Match patients to a fixed number of controls on condition                                                                                                    ```
    ```                                                                                                                                                               ```
    ```   Assumption                                                                                                                                                  ```
    ```      We are guaranteed that at least 2 controls with weight > 40 lbs                                                                                          ```
    ```      are available for each case.                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```  I am a little confused so lets set up an example;                                                                                                            ```
    ```                                                                                                                                                               ```
    ```   WORKING CODE                                                                                                                                                ```
    ```    SAS/WPS                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```    merge case control;                                                                                                                                        ```
    ```    by name;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```    if weight>40 then cnt=cnt+1;                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    if cnt<=2 then output;                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```    if cnt=2 then cnt=9;                                                                                                                                       ```
    ```    if last.name then cnt=0;                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  HAVE                                                                                                                                                         ```
    ```  ====                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  WORK.CASE total obs=19                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```  Case dataset is unique on name                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  Obs    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                                             ```
    ```                                                                                                                                                               ```
    ```    1    Alfred      M      14     69.0      112.5                                                                                                             ```
    ```    2    Alice       F      13     56.5       84.0                                                                                                             ```
    ```    3    Barbara     F      13     65.3       98.0                                                                                                             ```
    ```    4    Carol       F      14     62.8      102.5                                                                                                             ```
    ```    5    Henry       M      14     63.5      102.5                                                                                                             ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WORK.CONTROL total obs=48                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  At least 2 dups for every case witht the condition                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  Obs    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                                             ```
    ```                                                                                                                                                               ```
    ```    1    Alfred      M      14     69.0      112.5                                                                                                             ```
    ```    2    Alfred      M      14     69.0      112.5                                                                                                             ```
    ```                                                                                                                                                               ```
    ```    3    Alice       F      13     56.5       84.0                                                                                                             ```
    ```    4    Alice       F      13     40.0       40.0   * Need more bOdy weight drop this control                                                                 ```
    ```    5    Alice       F      13     56.5       84.0   Three but we want the 1st and third                                                                       ```
    ```                                                                                                                                                               ```
    ```    6    Barbara     F      13     65.3       98.0                                                                                                             ```
    ```    7    Barbara     F      13     65.3       98.0                                                                                                             ```
    ```    8    Carol       F      14     62.8      102.5                                                                                                             ```
    ```    9    Carol       F      14     62.8      102.5                                                                                                             ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WANT                                                                                                                                                         ```
    ```  ===                                                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```  Up to 40 obs WORK.MATCH total obs=38                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  Obs    CNT    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                                      ```
    ```                                                                                                                                                               ```
    ```    1     1     Alfred      M      14     69.0      112.5                                                                                                      ```
    ```    2     2     Alfred      M      14     69.0      112.5                                                                                                      ```
    ```                                                                                                                                                               ```
    ```    3     1     Alice       F      13     56.5       84.0                                                                                                      ```
    ```    4     2     Alice       F      13     56.5       84.0  Dropped one control for body weight                                                                 ```
    ```                                                                                                                                                               ```
    ```    5     1     Barbara     F      13     65.3       98.0                                                                                                      ```
    ```    6     2     Barbara     F      13     65.3       98.0                                                                                                      ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```  data case;                                                                                                                                                   ```
    ```    set sashelp.class;                                                                                                                                         ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  data control;                                                                                                                                                ```
    ```    set sashelp.class;                                                                                                                                         ```
    ```    output;output;                                                                                                                                             ```
    ```    if uniform(5731)>.5 then do; weight=40;output;end;                                                                                                         ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  * sas;                                                                                                                                                       ```
    ```  data match;                                                                                                                                                  ```
    ```    retain cnt 0;                                                                                                                                              ```
    ```    merge case control;                                                                                                                                        ```
    ```    by name;                                                                                                                                                   ```
    ```    if weight>40 then cnt=cnt+1;                                                                                                                               ```
    ```    if cnt<=2 then output;                                                                                                                                     ```
    ```    if cnt=2 then cnt=9;                                                                                                                                       ```
    ```    if last.name then cnt=0;                                                                                                                                   ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *wps;                                                                                                                                                        ```
    ```  %utl_submit_wps64('                                                                                                                                          ```
    ```  libname wrk sas7bdat "%sysfunc(pathname(work))";                                                                                                             ```
    ```  data wrk.match;                                                                                                                                              ```
    ```    retain cnt 0;                                                                                                                                              ```
    ```    merge wrk.case wrk.control;                                                                                                                                ```
    ```    by name;                                                                                                                                                   ```
    ```    if weight>40 then cnt=cnt+1;                                                                                                                               ```
    ```    if cnt<=2 then output;                                                                                                                                     ```
    ```    if cnt=2 then cnt=9;                                                                                                                                       ```
    ```    if last.name then cnt=0;                                                                                                                                   ```
    ```  run;quit;                                                                                                                                                    ```
    ```  ');                                                                                                                                                          ```
    ```                                                                                                                                                               ```

