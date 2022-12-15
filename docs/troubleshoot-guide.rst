===========================
Basic Troubleshooting Guide
===========================

This guide summarizes various issues that may arise during the day to day running of the ETL and NICE Actimize batch jobs. For each issue, there is a list of possible remedy or cause:


.. note::
   
   **What are the services that depend on service account NTKEN\\SA003237**

      * Oracle Linked Server to the EDW that has been created under ETL SQL Server database to retrieve data from EDW
      * Control-M client
      * Control-M ordered jobs
      * The credential and proxy linked to the service account which has been created under ETL SQL Server instance inorder to run the SSIS packages      

Common issues
-------------

#. **How to check if ETL process loaded all the data required.**

   * Exchange rates and Worldcheck files are critical data that needs to be loaded for the SAM and WLF batch jobs to generate the necessary alerts. To view the results of an ETL process, open the **StagingKE.AML.Batch_Sanity_Check** table and run the below query:
   
      .. code-block:: sql

         SELECT * FROM StagingKE.AML.Batch_Sanity_Check WHERE Batch_ID=145; //Replace 145 with the BatchID you are interested in

      .. image:: _static/images/batch_sanity.png
         :alt: Batch sanity check
      
      * The **Checksum** column specifies if the data in the source database table is the same as the data in the destination UDM_STG database table. Any record that has a Checksum value of 0 means that the data load for that specific table did not complete successfully

#. **Check if SAM and WLF batch jobs completed**

   * Login to the **Actimize Server Monitor** in either WLF and SAM servers and monitor the batch job status. See :ref:`Deployment Setup <deployment-setup>`:

      .. image:: _static/images/actimize_server_monitor.png         
         :alt: Nice Actimize Server Monitor

#. **When NTKEN\\SA003237 user password expires and is reset, what are the services and accounts that need remapping**

    * Oracle Linked Server to the EDW that has been created under ETL SQL Server database to retrieve data from EDW

      .. image:: _static/images/sql_developer_login.png
         :alt: SQL Developer login

    * Update the **RunAs** created in Control-M client.
    * Update the credential and proxy linked to the service account which has been created under ETL SQL Server instance 

      * SQL server credential

         .. image:: _static/images/sql_server_credential.png
            :alt: SQL Server credential

      * SSIS package execution proxy

         .. image:: _static/images/sql_server_proxy.png
            :alt: SQL Server proxy

#. **Control-M jobs cannot start at all**
   
   * Control-M jobs are usually run as user NTKEN\\SA003237. Most likely the password for that user has expired. If the password is reset, check above issue on the services that will need remapping

      .. image:: _static/images/control_m_runas.png
            :alt: Control-M runas

   * Check if the executing user has **Log on as a batch job** privilege

      .. image:: _static/images/logon_as_batch.png
            :alt: Log on as batch
  
#. **ETL process cannot load data from EDW**

   * Check that the password for NTKEN\\SA003237 has not expired

      * Try login into Oracle EDW database

         .. image:: _static/images/ssis_execution_log.png
            :alt: All SSIS executions

   * Confirm from EDW that there is valid data
   * Check the SSIS Execution logs. See :ref:`Execute Package <execute-package>`
      
      * Load SSIS All Executions Report
    
         .. image:: _static/images/ssis_execution_log.png
            :alt: All SSIS executions


      * SSIS executions summary
           
         .. image:: _static/images/ssis_execution_log_all.png
            :alt: All SSIS summary
