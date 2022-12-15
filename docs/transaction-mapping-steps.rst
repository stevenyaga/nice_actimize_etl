========================================
How to consume updated transaction codes
========================================

This document sets out the steps to consume the updated transaction mapping file. The file contains three worksheets:

#. **KEN_SSD_Trans Codes**:- Contains the various transaction codes
#. **KEN_SSD_SAM_Trans_CD_Type Rels**:- Contains the relationship between SAM transaction codes and the internal transaction codes
#. **KEN_SSD_SAM_Trans_Type_Grp_Rels**:- Contains the relationship between SAM transaction groups and SAM transaction types
 

Steps
=====

1. Generating csv files from the mapping file
---------------------------------------------

.. note::

    * **NB**: The Excel files are provided in the script folder to act as reference point as well as allow us to define formula to generate extra columns as CSV usually lose formula once they are persisted
    * The tag **<SCRIPT_FOLDER>** used hereunder implies the location where the ETL project files are located in the production server. Currently the location is E:\NiceActimizeETL on server 10.235.54.59


The following files will be modified:

   #. *<SCRIPT_FOLDER>\transaction_codes\transaction_codes.csv*

      * The contents of this file will be modified using **KEN_SSD_Trans Codes** worksheet. See <SCRIPT_FOLDER>\transaction_codes\transaction_codes.xlsx
      * * Additional fields will be generated. See the excel files for more details

   #. *<SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.csv*
       
       * The contents of this file will be modified using **KEN_SSD_SAM_Trans_CD_Type Rels** worksheet. See <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.xlsx
       * 3 additional fields will need to be generated. See the excel file for more details

   #. *<SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.csv*

      * The contents of this file will be modified using **KEN_SSD_SAM_Trans_Type_Grp_Rels** worksheet. See <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.xlsx
      * Additional fields will be generated. See the excel files for more details

.. note::
    
    **NB**: It is expected that the file will always have the same format


2. Importing the file
---------------------

* You will need to run the following scripts to have the files imported

.. code-block:: bash
    
    <SCRIPT_FOLDER>\transaction_codes\transaction_codes.bat
    <SCRIPT_FOLDER>\SAM_TRANS_CODE_TYPE_RELATION\SAM_TRANS_CODE_TYPE_RELATION.bat
    <SCRIPT_FOLDER>\Sam_Trans_Type_Group_Relation\Sam_Trans_Type_Group_Relation.bat
