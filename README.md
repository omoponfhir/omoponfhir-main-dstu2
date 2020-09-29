OMOPonFHIR Implementation version 1
=
- Supports OMOP v5.2
- Supports DSTU2
- STU3: STU3 is available from https://github.com/omoponfhir/omoponfhir-main/ 

Database Dependencies
-
This application requires an OMOP V5.2 database to work. Please do the follows to set up your database.
1. Goto https://athena.ohdsi.org to download Vocabularies.
2. Get v5.2 DDLs from https://github.com/OHDSI/CommonDataModel/tree/v5.2.2
3. Get docker image of PostgreSQL: docker pull postgres
4. Run docker run --name omopv52 -p 5432:5432 -e POSTGRES_PASSWORD=<password> -d postgres
5. Create database and schema
6. Run DDL from ODHSI
7. Run DDLs for omoponfhir_v5.2_f_observation_view_ddl.txt and omoponfhir_v5.3_f_person_ddl.txt in fhir_names folder.
8. Load vocabularies downloaded from Athena.
9. Load your dataset if you have any.
10. Use fhir_names/ folder to populate f_person with synthetic names
11. Index all IDs in OMOP tables

If you aleady have your own OMOP v5.2 database that you want to use with OMOPonFHIR software stack, please do the follows,
1. Do step #7 above
2. Make sure you can read f_observation_view and f_person table is created.
3. Do step #10 above. 
4. Make sure you have your f_person table is populated with names. f_person table MUST have the same number of entries as person table
5. Index person_id in f_person table

How to install and run.
-
To download,
git clone --recurse https://github.com/omoponfhir/omoponfhir-main-dstu2.git

Before running the application update the values of the JDBC_URL, JDBC_USERNAME, and JDBC_PASSWORD environment variables in the Dockerfile. They must contain the data necessary for your application to connect to an OMOP V5 database. After updating the ENV variables in the Dockerfile, you can do either

1. use docker compose to build and run
```
sudo docker-compose up --build -d
```

2. or use docker build and run
```
sudo docker build -t omoponfhir-dstu2 .
sudo docker run --name omoponfhir-dstu2 -p 8080:8080 -d omoponfhir-dstu2:latest
```

NOTE: The OMOPonFHIR server is set to READ-ONLY. If you want to write to OMOPonFHIR, web.xml in WEB-INFO must have readOnly set to False.

## Environment Variables

Environment variables are required to be set in order for OMOPonFHIR server to run correctly. The follows are environment variables. If Docker is used, these can be specified in the Dockerfile. If rancher is used for the docker image, the rancher can set the environment variable. If running from Java application server, please refer to the application server setting to add the environment variables.

- JDBC_PASSWORD="dbpassword"
- JDBC_USERNAME="dbusername"
- JDBC_URL="jdbc:..."
- SMART_INTROSPECTURL="http://[your_omoponfhir_host]/smart/introspect"
- SMART_AUTHSERVERURL="http://[your_omoponfhir_host]/smart/authorize"
- SMART_TOKENSERVERURL="http://[your_omoponfhir_host]/smart/token"
- AUTH_BEARER="static bearer token that you want to use. this has full access scope *.* of smart on fhir"
- AUTH_BASIC="your_username:your_secret"
- FHIR_READONLY="True or False"
- SERVERBASE_URL="base URL if you are behind proxy. If this is not set then local hostname will be used"
- (optional) LOCAL_CODEMAPPING_FILE_PATH="if you want to load local code mapping to OMOP concept table. Put path to file here"
- MEDICATION_TYPE="Applied to MedicationRequest only. Set to 'local' if you want medication to be in contained. set to 'link' if you want to use Reference link to Medicaiton resource. Set this to empty string or do not include in env variable to use CodeableConcept"

Application URLs
-
- TestPage UI - http://<my_host>:8080/omoponfhir2/
- API - http://<my_host>:8080/omoponfhir2/fhir
- SMART on FHIR - http://<my_host>:8080/omoponfhir2/smart/

Additional Information
-
Status: Details available in http://omoponfhir.org/

Contributors:
- Lead: Myung Choi
- DSTU2 to OMOPv5 Mapping: Saul Crumpton
- DevOp/Deployment: Eric Soto
- Docker/VM Management: Michael Riley
 
Please use Issues to report any problems. If you are willing to contribute, please fork this repository and do the pull requests.
