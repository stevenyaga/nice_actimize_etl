=======================
How to process backlogs
=======================

.. note::

The assumption here is that for each skipped day, a separate batch is run.

There are 4 important variables that you need to be aware

#. **IS_PROCESSING_BACKLOG**:- A value of 1 specifies that we are in a backlog state. A value of 0 specifies we are in normal running mode
#. **BACKLOG_START_DATE**:- This is the start of the backlog period. Format is YYYY-MM-DD
#. **BACKLOG_END_DATE**:- This is the end of the backlog period. Format is YYYY-MM-DD
#. **BACKLOG_CURRENTLY_RUNNING_DATE**:- Date for whose batch we are currently processing. Format is YYYY-MM-DD

- **NB**. The ETL will derive balances using the transactions to date going backward into the backlog dates

Step 1: Verify loaded Worldcheck files
#####################################

Ensure worldcheck daily delta and delete files are loaded (copied onto the respective folders) for each of the days included in the backlog. In most times, these files will be already loaded since this is done by a separate SQL Agent job. The ETL also attempts to load the respective files in case the SQL Agent job did not succeed. It is always good to confirm that these files exist

* Log into Control-M and navigate to Planning tab

.. image:: _static/images/loading_controlm_workspace.png
   :width: 800
   :alt: Loading Control-M workspace

* Open workspaces by going to **Load Folders and Jobs** tab. Select **KEN_PRD** folder then click on **Open** button at the bottom

.. image:: _static/images/loading_controlm_workspace_2.png
   :width: 800
   :alt: Loading Control-M folders

* Specify backlog dates from within the Control-M environment. 

Step 2: Modify backlog variable values within Control-M
#######################################################

Double click on **01_Specify_Backlog_Processing_Values** node

.. image:: _static/images/backlog_variables_node.png
   :width: 800
   :alt: Backlog variables node

Specify the values for **IS_PROCESSING_BACKLOG**, **BACKLOG_START_DATE**, **BACKLOG_END_DATE** and **BACKLOG_CURRENTLY_RUNNING_DATE**. The date format is **YYYY-MM-DD**

.. image:: _static/images/edit_backlog_variables.png
   :width: 800
   :alt: Loading Control-M workspace

Step 3: Order folders
#####################

Order the folder and start the execution making sure to modify the **BACKLOG_CURRENTLY_RUNNING_DATE** variable for the day you are processing backlog

.. image:: _static/images/controlm_order_folder.png
   :width: 800
   :alt: Ordering Control-M folder

Step 4: Rinse and Repeat
########################

If you this is not the last day of backlog processing, repeat steps 1, 2 and 3 until you are done processing the backlog. However, if this is the last day, perform step 2 but this time set the value of **IS_PROCESSING_BACKLOG** to 0

Occassional errors in backlog processing
########################################

During processing of backlog, you might occassional errors in Control-m for jobs that are performing validation. These failures do not necessarily mean that we should stop processing, but rather each one should be handled on its own. These jobs are:

.. image:: _static/images/validation_jobs.png
   :width: 800
   :alt: Validation jobs

1. **Check_Previous_Actimize_Jobs_Status**. This job under the **00_HOUSE_KEEPING** folder checks if the previous day's Actmize jobs completed or not. In most times this will always pass. If it does not pass, please seek further assistance
2. **10_Check_if_Required_Data_Loaded**. This job in **02_GET_BATCH_INFO** folder checks that we have minimum datasets available (party, account and balances) before we can generate alerts. A failure here should lead to further investigation. Run the following query in the ETL server. Replace {YOUR_BATCH_ID} with the batch id of interest. You can extract the batch id by checking the output of the immediate job **09_ShowActimizeBatchDetails**

.. code-block:: sql

  SELECT --COUNT(*), 
  SOURCE
  FROM[StagingKE].[AML].[Batch_Sanity] WHERE EnvironmentType='PROD' AND BatchID={YOUR_BATCH_ID}
  AND Destination in ('[UDM].[UDM_STG].[ACCOUNT]', '[UDM].[UDM_STG].[BALANCE]', 
  '[UDM].[UDM_STG].[PARTY]') AND DestinationCount=0
 GROUP BY SOURCE

Check the output of the query. Note the following:

* If the queryset includes **[StagingKE].[AML].[Account]** or **[StagingKE].[AML].[Party]**, rerun preceeding jobs within the ETL folder. It is likely incomplete data was loaded earlier
* If the queryset includes **[StagingKE].[AML].[Balance]**, check if the batch date falls on a Sunday or a public holiday. If yes, you can set the Job to OK. Else, consult the ETL developer. However, even if the batch date falls on a holiday and we are not in backlog processing mode, please consult the ETL developer. There is likely to be a data problem

3. **02_Verify_Actimize_Jobs_Status**. This job in **03_INITIALIZE_JOBS** folder checks that we have properly set the flag signifying that are already running alerts against a specific batch. This ensures that we do not have multiple batch runs at the same time.
4. **05_DATA_QUALITY_CHECKS**. Jobs in this folder checks for duplicate entity data in the context of CDD. Any failure here points to a serious issue. A failure here can only be resolved by the ETL developer

.. note::   
   #. While in backlog processing period, disable daily scheduling of jobs by specifying order method to **None (Manual Order)**
   #. When you are done processing backlog, enable daily scheduling of jobs by specifying order method to **Automatic (Daily)**
   #. Ensure the ETL process begins within such a time when synchronization into EDW has completed. This is to avoid loading incomplete data
   #. Ensure to process backlogs days sequentially a day after the other
   #. Modifying backlog variables will require you to checkout the Control-M workspace and later checking it in or ordering the workspaces. Therefore make sure the account you are using to login into Control-m has the priviliges to edit and modify workspaces

.. warning::
   When you are done processing the backlog set the value of **IS_PROCESSING_BACKLOG** variable to **0**, otherwise it will still behaving like it is in a backlog state
