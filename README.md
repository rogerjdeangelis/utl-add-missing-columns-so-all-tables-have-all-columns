# utl-add-missing-columns-so-all-tables-have-all-columns
Add missing columns so all tables have all columns,
    Add missing columns so all tables have all columns                                                                    
                                                                                                                          
       1. If you know the missing variables, types and lengths (datatep and sql solutions)                                
          Tom Robinson barefootguru@gmail.com                                                                             
       2. If you do NOT know the missing variables (datastep solution)                                                    
          Based on work by Mark Keintz mkeintz@wharton.upenn.edu                                                          
       3. If you do NOT know the missing variables (pure SQL)                                                             
                                                                                                                          
      Good question                                                                                                       
                                                                                                                          
      If you have dozens of tabls take a look at macros bdo_over and barray                                               
      by Bart Jablonski yabwon@gmail.com (links plus macro in)                                                            
                                                                                                                          
      github                                                                                                              
      https://tinyurl.com/y9lrtjm2                                                                                        
      https://github.com/rogerjdeangelis/utl-add-missing-columns-so-all-tables-have-all-columns                           
                                                                                                                          
      SAS-L                                                                                                               
      https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;4845ae8c.1901d                                                         
                                                                                                                          
      macros                                                                                                              
      https://tinyurl.com/y9nfugth                                                                                        
      https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                          
                                                                                                                          
    INPUT                                                                                                                 
    =====                                                                                                                 
                                                                                                                          
    * MAKE DATA;                                                                                                          
    data hav1 hav2(drop=name) hav3;                                                                                       
      set sashelp.class(obs=5 keep=name sex age);                                                                         
    run;quit;                                                                                                             
                                                                                                                          
    WORK.HAV1 total obs=5                                                                                                 
                                                                                                                          
       NAME      SEX    AGE                                                                                               
                                                                                                                          
      Alfred      M      14                                                                                               
      Alice       F      13                                                                                               
      Barbara     F      13                                                                                               
      Carol       F      14                                                                                               
      Henry       M      14                                                                                               
                                                                                                                          
    WORK.HAV2 total obs=5   * name is missing;                                                                            
                                                                                                                          
      SEX    AGE                                                                                                          
                                                                                                                          
       M      14                                                                                                          
       F      13                                                                                                          
       F      13                                                                                                          
       F      14                                                                                                          
       M      14                                                                                                          
                                                                                                                          
    WORK.HAV3 total obs=5                                                                                                 
                                                                                                                          
       NAME      SEX    AGE                                                                                               
                                                                                                                          
      Alfred      M      14                                                                                               
      Alice       F      13                                                                                               
      Barbara     F      13                                                                                               
      Carol       F      14                                                                                               
      Henry       M      14                                                                                               
                                                                                                                          
                                                                                                                          
    EXAMPLE OUTPUT  ( add name to hav2 with missing vales and correct length)                                             
    -------------------------------------------------------------------------                                             
                                                                                                                          
    WORK.HAV2 total obs=5                                                                                                 
                                                                                                                          
       NAME      SEX    AGE                                                                                               
                                                                                                                          
      Alfred      M      14                                                                                               
      Alice       F      13                                                                                               
      Barbara     F      13                                                                                               
      Carol       F      14                                                                                               
      Henry       M      14                                                                                               
                                                                                                                          
                                                                                                                          
    PROCESS                                                                                                               
    =======                                                                                                               
                                                                                                                          
    1. If you do know the missing variables and types and lengths)                                                        
    --------------------------------------------------------------                                                        
                                                                                                                          
    * MAKE DATA;                                                                                                          
    data hav1 hav2(drop=name) hav3;                                                                                       
      set sashelp.class(obs=5 keep=name sex age);                                                                         
    run;quit;                                                                                                             
                                                                                                                          
    proc sql;                                                                                                             
       alter table hav3                                                                                                   
          add name char(8)                                                                                                
    ;quit;                                                                                                                
                                                                                                                          
                                                                                                                          
    proc sql;                                                                                                             
       alter table hav2                                                                                                   
          add name char(8)                                                                                                
    ;quit;                                                                                                                
                                                                                                                          
                                                                                                                          
    proc sql;                                                                                                             
       alter table hav3                                                                                                   
          add name char(8)                                                                                                
    ;quit;                                                                                                                
                                                                                                                          
                                                                                                                          
    data hav1;                                                                                                            
      retain name '12345678';                                                                                             
      set hav2;                                                                                                           
    run;quit;                                                                                                             
                                                                                                                          
    data hav2;                                                                                                            
      retain name '12345678';                                                                                             
      set hav2;                                                                                                           
    run;quit;                                                                                                             
                                                                                                                          
    data hav3;                                                                                                            
      retain name '12345678';                                                                                             
      set hav3;                                                                                                           
    run;quit;                                                                                                             
                                                                                                                          
                                                                                                                          
    2. If you do NOT know the missing variables (datastep solution)                                                       
    ---------------------------------------------------------------                                                       
                                                                                                                          
    * MAKE DATA;                                                                                                          
    data hav1 hav2(drop=name) hav3;                                                                                       
      set sashelp.class(obs=5 keep=name sex age);                                                                         
    run;quit;                                                                                                             
                                                                                                                          
    data hav1st hav2nd hav3rd;                                                                                            
                                                                                                                          
      if _n_=0 then                                                                                                       
           set hav1(obs=0) hav2 (obs=0) hav3 (obs=0);                                                                     
                                                                                                                          
      set hav1(in=h1) hav2(in=h2) hav3(in=h3);                                                                            
      select;                                                                                                             
        when (h1) output hav1st;                                                                                          
        when (h2) output hav2nd;                                                                                          
        when (h3) output hav3rd;                                                                                          
      end; /* otherwise not needed */                                                                                     
                                                                                                                          
    run;                                                                                                                  
                                                                                                                          
                                                                                                                          
    3. If you do NOT know the missing variables (pure SQL)                                                                
    ------------------------------------------------------                                                                
                                                                                                                          
    * MAKE DATA;                                                                                                          
    data hav1 hav2(drop=name) hav3;                                                                                       
      set sashelp.class(obs=5 keep=name sex age);                                                                         
    run;quit;                                                                                                             
                                                                                                                          
    proc sql;                                                                                                             
    create                                                                                                                
      table mta as                                                                                                        
    select                                                                                                                
      hav1.*                                                                                                              
     ,hav2.*                                                                                                              
     ,hav3.*                                                                                                              
    from                                                                                                                  
      hav1(obs=0),  hav2(obs=0),  hav3(obs=0)                                                                             
    ;                                                                                                                     
      create                                                                                                              
         table hav1Fix as                                                                                                 
      select                                                                                                              
         *                                                                                                                
      from                                                                                                                
         hav1 full outer join mta                                                                                         
      on 1=1                                                                                                              
    ;                                                                                                                     
      create                                                                                                              
         table hav2Fix as                                                                                                 
      select                                                                                                              
         *                                                                                                                
      from                                                                                                                
         hav2 full outer join mta                                                                                         
      on 1=1                                                                                                              
    ;                                                                                                                     
      create                                                                                                              
         table hav3Fix as                                                                                                 
      select                                                                                                              
         *                                                                                                                
      from                                                                                                                
         hav3 full outer join mta                                                                                         
      on 1=1                                                                                                              
                                                                                                                          
    ;quit;                                                                                                                
                                                                                                                          
                                                                                                                          
