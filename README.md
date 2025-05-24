# WattWise

Before you begin, ensure the following are installed on your system:

Docker Desktop

Git

Visual Studio Code 



Step 1: Clone the Project Repository into vscode


Step 2: Prepare the Input Data
Ensure the raw CSV files are placed into a folder on your local machine, then map this paath into the NiFi container as /input_data.



Step 3: Launch the Containers
From inside the project folder:


docker-compose up -d
This will:

Launch Apache NiFi for ETL

Launch PostgreSQL with a pre-configured wattwise database

Launch Apache Superset for reporting



Step 4: Access the Services
Service	URL	Username	Password
NiFi	http://localhost:8080	admin	WattWiseAdmin
Superset	http://localhost:8088		


Step 5: Set Up Superset
If this is your first time running Superset, run the following to initialize:


docker exec -it superset superset fab create-admin
docker exec -it superset superset db upgrade
docker exec -it superset superset init
Follow the prompts to create an admin user

Then log in at http://localhost:8088



Step 6: Configure NiFi ETL Flow
Go to http://localhost:8080

Drag and drop processors to create this flow:

GetFile → ConvertRecord / UpdateRecord → PutDatabaseRecord

Use the mounted path /input_data to read the CSVs.

Configure DB connection to PostgreSQL:

Host: postgres

Port: 5432

DB name: wattwise

User: superset

Password: superset



Step 7: Connect Superset to PostgreSQL
Log in to Superset.

Navigate to Data > Databases > + Database

Use this SQLAlchemy URI:

postgresql+psycopg2://superset:superset@postgres:5432/wattwise

Import the required tables, then build or import dashboards.



You can now explore energy trends, policy data, emissions, and more through your custom dashboard
