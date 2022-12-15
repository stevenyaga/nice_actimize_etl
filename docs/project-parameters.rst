.. _package-parameters:

=======================
SSIS Project Parameters
=======================

* Parameters are used to assign values to SSIS package properties. A parameter specifies the data that will be used by a package. You can scope parameters to the package level or project level with package parameters and project parameters, respectively. Parameters can be used in expressions or tasks. When the project is deployed to the catalog, you can assign a literal value for each parameter or use the default value that was assigned at design time. In place of a literal value, you can also reference an environment variable. Environment variable values are resolved at the time of package execution.

* There are parameters that apply to a specific SSIS package while some apply to the entire project (All SSIS scripts)

* Please review all the parameters and modify those that need modification. Not all parameters will require updating

* There are specific project parameters that need to be set after deployment for the scripts to run properly:

	.. image:: _static/images/parameters.png
		:width: 600
		:alt: Project parameters
 
* Parameters can be modified by right clicking on the package and select **Execute**

	.. image:: _static/images/edit_parameters.png
		:width: 600
		:alt: Edit project parameters
