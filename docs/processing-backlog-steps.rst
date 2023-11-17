=======================
How to process backlogs
=======================

.. note::

For correct and full coverage, it is recommended that for each skipped day, a separate batch is run.

There are 3 important dates that you need to be aware

#. **Backlog_start_date**:- This is the start of the backlog period. Format is YYYY-MM-DD
#. **Backlog_end_date**:- This is the end of the backlog period. Format is YYYY-MM-DD
#. **Backlog_current_running_date**:- Any date between the backlog start and end dates for whose batch we are currently processing. Format is YYYY-MM-DD

Steps
=====
#. Ensure worldcheck daily delta and delete files are loaded (copied onto the respective folders) for each of the days included in the backlog. In most times, these files will be already loaded since this is done by a separate SQL Agent job. The ETL also attempts to load the respective files in case the SQL Agent job did not succeed. It is always good to confirm that these files exist

* These values are set and specified within Control-M environment and are passed as parameters to the ETL scripts


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
