# utl-identifying-the-xml-node-and-importing-the-xml-table
Identifying the xlm node and importing the xml table 
    Identifying the xml node and importing the xml table                                                  
                                                                                                          
    github                                                                                                
    https://tinyurl.com/y8kbjsqg                                                                          
    https://github.com/rogerjdeangelis/utl-identifying-the-xlm-node-and-importing-the-xml-table           
                                                                                                          
    StackOverflow                                                                                         
    https://stackoverflow.com/questions/62087841/read-xml-data-with-rvest                                 
                                                                                                          
    Allan Cameron profile                                                                                 
    https://stackoverflow.com/users/12500315/allan-cameron                                                
                                                                                                          
    *_                   _                                                                                
    (_)_ __  _ __  _   _| |_                                                                              
    | | '_ \| '_ \| | | | __|                                                                             
    | | | | | |_) | |_| | |_                                                                              
    |_|_| |_| .__/ \__,_|\__|                                                                             
            |_|                                                                                           
    ;                                                                                                     
                                                                                                          
                                                                                                          
    https://www.sec.gov/Archives/edgar/data/1026081/000092189520001626/infotable.xml                      
                                                                                                          
    The node is '<nameOfIssuer>'                                                                          
                                                                                                          
    Samkple xml text (this text repeats)                                                                  
                                                                                                          
    <infoTable>                                                                                           
    <nameOfIssuer>BANCORP 34 INC</nameOfIssuer>                                                           
    <titleOfClass>COM</titleOfClass>                                                                      
    <cusip>05970V106</cusip>                                                                              
    <value>1081</value>                                                                                   
    <shrsOrPrnAmt>                                                                                        
    <sshPrnamt>99192</sshPrnamt>                                                                          
    <sshPrnamtType>SH</sshPrnamtType>                                                                     
    </shrsOrPrnAmt>                                                                                       
    <investmentDiscretion>OTR</investmentDiscretion>                                                      
    <votingAuthority>                                                                                     
    <Sole>0</Sole>                                                                                        
    <Shared>99192</Shared>                                                                                
    <None>0</None>                                                                                        
    </votingAuthority>                                                                                    
    </infoTable>                                                                                          
    ...                                                                                                   
                                                                                                          
    *            _               _                                                                        
      ___  _   _| |_ _ __  _   _| |_                                                                      
     / _ \| | | | __| '_ \| | | | __|                                                                     
    | (_) | |_| | |_| |_) | |_| | |_                                                                      
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                     
                    |_|                                                                                   
    ;                                                                                                     
                                                                                                          
    WORK.WANT total obs=28                                                                                
                                                                                                          
    Obs    COMPANY                                                                                        
                                                                                                          
      1    BANCORP 34 INC                                                                                 
      2    BANC OF CALIFORNIA INC                                                                         
      3    BANKWELL FINL GROUP INC                                                                        
      4    CBM BANCORP INC                                                                                
      5    CARTER BK & TR MARTINSVILLE                                                                    
      6    CITIZENS FINL GROUP                                                                            
      7    CIVISTA BANCSHARES INC                                                                         
      8    COLUMBIA FINL INC                                                                              
      9    CONNECTONE BANCORP INC NEW                                                                     
     10    FSB BANCORP INC                                                                                
     11    FIRST UTD CORP                                                                                 
     12    HV BANCORP INC                                                                                 
     13    HARBORONE BANCORP INC NEW                                                                      
     14    INVESTORS BANCORP INC NEW                                                                      
    ...                                                                                                   
                                                                                                          
    *                                                                                                     
     _ __  _ __ ___   ___ ___  ___ ___                                                                    
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                   
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                   
    | .__/|_|  \___/ \___\___||___/___/                                                                   
    |_|                                                                                                   
    ;                                                                                                     
                                                                                                          
    %utlfkil(d:/xpt/want.xpt);                                                                            
                                                                                                          
    proc datasets lib=work nolist;                                                                        
      delete want;                                                                                        
    run;quit;                                                                                             
                                                                                                          
    %utl_submit_r64('                                                                                     
    library(rvest);                                                                                       
    library(SASxport);                                                                                    
    url <- "https://www.sec.gov/Archives/edgar/data/1026081/000092189520001626/infotable.xml";            
    want<-as.data.frame(                                                                                  
            url %>% read_xml() %>%                                                                        
            xml_ns_strip() %>%                                                                            
            xml_nodes("nameOfIssuer") %>%                                                                 
            xml_text());                                                                                  
    want[] <- lapply(want, function(x) if(is.factor(x)) as.character(x) else x);                          
    colnames(want)<-c("Company");                                                                         
    write.xport(want,file="d:/xpt/want.xpt");                                                             
    ');                                                                                                   
                                                                                                          
    libname xpt xport "d:/xpt/want.xpt";                                                                  
    data want;                                                                                            
      set xpt.want;                                                                                       
    run;quit;                                                                                             
    libname xpt clear;                                                                                    
                                                                                                          
