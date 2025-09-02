# Snowflake-Lineage
A utility to dynamically extract field level backward traceability lineage in tabular manner for tables and views in snowflake 

# Programming Language Used
Python (>=3.9)

# Dependencies
It's a stand-alone Python notebook that can be imported as is in Snowflake. If one wishes to NOT use snowflake, the program file an be used as .py and be run from a git pipeline, but we need to make sure to have the following libraries installed i.e. kept in a requirement.txt file in the repository -

snowflake-connector-python
pandas

# Prerequisites
1. The user in use should already be authorised and authenticated in snowflake, the program will use BROWSERAUTHENTICATION method. (part of the parameterization, discussed in detail below)
2. The user should AT LEAST have read access to the target schemas mentioned. (part of the parameterization, discussed in detail below)
3. The user should have use access to the snowflake warehouse mentioned. (part of the parameterization, discussed in detail below)
4. The warehouse should have query privilege in the target database and ALL OTHER source databases that may have lineage associations. (part of the parameterization, discussed in detail below)
5. Save the notebook under a schema where the user has read access (**Let's assume the location as notebook_database.notebook_schema.sourcetotargetmapping**)
6. A schema named **ARTIFACTS** should be created under the target database where the lineage data would be populated in a table named **LINEAGE**. (part of the parameterization, discussed in detail below)

# How to operate?
The notebook/program can be utilized in one of several ways -

1. Use the parametermization technique. From a snowsight SQL query worksheet, run the notebook using the following command-
   EXECUTE NOTEBOOK notebook_database.notebook_schema.sourcetotargetmapping('<user_name>,<snowflake_warehouse>,<target_database>,<list_of_target_schemas>')
   
   # Semantics:
   
     a. user -> snowflake user (MANDATORY) : STRING
   
     b. warehouse -> snowflake warehouse (MANDATORY) : STRING
   
     c. target_database -> target database (MANDATORY) : STRING
   
     d. list_of_target_schemas -> the intended schemas under the target database (OPTIONAL) : comma-separated string representation of a list of schemas

   # Syntactically correct commands -
   
   #the code will check for SC1 and SC2 schemas under DB1 databse for lineage data
   EXECUTE NOTEBOOK notebook_database.notebook_schema.sourcetotargetmapping('USER1,WH1,DB1,SC1,SC2')
   
   #the code will check for SC1 schema under DB1 databse for lineage data
   EXECUTE NOTEBOOK notebook_database.notebook_schema.sourcetotargetmapping('USER1,WH1,DB1,SC1')
   
   #the code will check for ALL schemas under DB1 databse for lineage data
   EXECUTE NOTEBOOK notebook_database.notebook_schema.sourcetotargetmapping('USER1,WH1,DB1')

2. Update the code at the Cell2 of the Python notebook-
   
   <img width="462" height="107" alt="image" src="https://github.com/user-attachments/assets/cef259cf-bf5c-4612-8b36-52d7420aaa40" />

   Feel free to customize the user, warehouse, database and schema variables as per your need.
  
3. Schedule the notebook from snowflake notebook section with parameters





