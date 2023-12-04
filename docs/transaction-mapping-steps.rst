# How to consume the updated transaction codes

## Introduction

This document sets out the steps to consume the updated transaction mapping file. The file contains three worksheets:

1. KEN_SSD_Trans Codes:- Contains the various transaction codes
2. KEN_SSD_SAM_Trans_CD_Type Rels:- Contains the relationship between SAM transaction codes and the internal transaction codes
3. KEN_SSD_SAM_Trans_Type_Grp_Rels:- Contains the relationship between SAM transaction groups and SAM transaction types
 
## Steps

### Generating csv files from the mapping file

**NB**: The Excel files are provided in the script folder to act as reference point as well as allow us to define formula to generate extra columns as CSV usually lose formula once they are persisted

* Value of <SCRIPT_FOLDER> is \DataImport\Initial. The path is relative to the installation directory

The following files will be modified:

1. <SCRIPT_FOLDER>\transaction_codes\transaction_codes.csv

* The contents of this file will be modified using **KEN_SSD_Trans Codes** worksheet. See <SCRIPT_FOLDER>\transaction_codes\transaction_codes.xlsx
> * Copy contents of *KEN_SAM_CODE* field in the source file into column *A* of the transaction_codes.xlsx file
> * Copy contents of *DESCRIPTION* field in the source file into column *B* of the transaction_codes.xlsx file 
> * Copy contents of *SHORT.DESC* field in the source file into columns *E* and *H* of the transaction_codes.xlsx file 
* Additional fields will be generated. See the excel files for more details
* Export the contents of the worksheet as CSV
* Copy the csv file onto <SCRIPT_FOLDER>\transaction_codes\

2. <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.csv

* The contents of this file will be modified using **KEN_SSD_SAM_Trans_CD_Type Rels** worksheet. See <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.xlsx
> * Copy contents of *KEN_SAM_CODE* field in the source file into column *A* of the SAM_TRANS_CODE_TYPE_RELATION.xlsx file
> * Copy contents of *SAM_TRANSACTION_TYPE_CD* field in the source file into column *B* of the SAM_TRANS_CODE_TYPE_RELATION.xlsx file 
* 3 additional fields will need to be generated. See the excel file for more details
* Export the contents of the worksheet as CSV
* Copy the csv file onto <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\

3. <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.csv

* The contents of this file will be modified using **KEN_SSD_SAM_Trans_Type_Grp_Rels** worksheet. See <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.xlsx
> * Copy contents of *GROUP_TRANSACTION_TYPE_CD* field in the source file into column *A* of the SAM_TRANS_CODE_TYPE_RELATION.xlsx file
> * Copy contents of *TRANSACTION_TYPE_CD* field in the source file into column *B* of the SAM_TRANS_CODE_TYPE_RELATION.xlsx file 
* * Additional fields will be generated. See the excel files for more details
* 3 additional fields will need to be generated. See the excel file for more details
* Export the contents of the worksheet as CSV
* Copy the csv file onto <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\

**NOTE**: Each CSV entry must contain *;;* at the end of each line. Otherwise the file will not import. As a hint, you can press the spacebar when inside cells *I1* and *J1*. Remember to save the file again before exporting to CSV

**NB**: It is expected that the file will always have the same format

### Importing the file

* You will need to run the following scripts to have the files imported. however, daily ETL runs will import the files automatically

1. <SCRIPT_FOLDER>\transaction_codes\transaction_codes.bat
1. <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.bat
1. <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.bat
