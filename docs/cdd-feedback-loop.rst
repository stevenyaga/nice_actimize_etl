=================
CDD Feedback Loop
=================

* This functionality was developed to implement near-real-time CDD scoring. The setup is as follows:

- After a successful CDD run, a batch script extracts current CDD scoring as assigned by NICE Actimize CDD routines
- The extracted scores are stored into a table in the database
- After the next successful CDD run, the same batch script will extract the current customer scores and compare with the previously assigned score. If there is a change in the scoring, the delta entries are persisted onto a file stored on a network folder. T24 will then pick this delta file and update the customer scores


Implementation
==============

* The shared folder where delta files are dumped is hosted on a linux machine. There are permission issues associated with this folder that were stopping the batch script from dumping the delta file onto that folder. To resolve this and to make the CDD feedback loop consistent with the rest of the ETL design, the following was done:

1. Created an SSIS package (T24DeltaFeedbackLoop.dtsx) that calls the batch script instead of how the batch was being called directly. The justification for this is explained in step 2
2. Created an SQL Server Job called T24Delta_Sql_Job that executes the T24DeltaFeedbackLoop package. The argument for this is that SQL server jobs can be run using a service account without issues. This helps us bypass the access denied error that we were facing since SQL Server is able to authenticate with the service account as expected
3. The CDD_Feedback_Loop folder has been ported into the NiceActimizeETL solution, meaning it is now a sub-folder within the ETL solution. This means that the files that had been copied onto the WLF server (10.168.134.52) are no longer valid and are thus stale. The justification for this is that ETL solution and its peripherals need to be maintained under a single repository for ease of maintenance and deployment. Another reason is that the SSIS package created in step 1 needs access to the local folder within the NiceActimizeETL solution that hosts the other extraction batch files
4. Control-M production has been updated accordingly to reflect this change where the 05_Delta_to_CSV job executes the  T24Delta_Sql_Job SQL job instead of calling the batch file directly.  


Deployment
==========

* The SSIS package (T24DeltaFeedbackLoop.dtsx) was added to the core ETL loop for execution together with the other SSIS scripts

* A new job was added onto the CDD sub-folder within Control-M to ensure it is executed right after the CDD job is completed
