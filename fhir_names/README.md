To populate the f_person table:  
  1)  First, f_person table needs to be created using omoponfhir_f_person_ddl.txt.
  2)  create the names schema and load the names data using the names.dmp file  ( NOTE: this assumes that user has appropriate permissions to create a schema ). The following command should create and populate names.  
    
      psql -d postgres < names.dmp  
        
  2) run the insert commands in insert_names_ti_f_person.sql script.  If your postgres setup is using its own schema name under the existing database, you may need to include schema name in front of table names in the ddl script.  In this case, you will need to modify and insert the schema name in this script to the correct schema for the omop_v5 schema prior to running.  
  
