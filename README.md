# FaxNoLogs Database Initial Setup

Assuming Docker is installed on  machine that hosts the containers for database and app, we need to run the following command:

  docker exec faxnologs_webapp bash -c "apt-get update && apt-get -y install wget && wget --no-check-certificate https://github.com/kparginos/faxnologs-dbsetup/raw/main/DBSetup.tar && mkdir dbsetup && tar xf DBSetup.tar -C dbsetup && cd dbsetup && sed -i 's/localhost,1433/db/g' appsettings.json && dotnet FaxNoLogs.Migrations.dll"
  
The above command, should it runn correctly, must apply the following:

  1. Update OS
  2. Install wget utility
  3. Use wget to download the DBSetup.tar
  4. Create a dbsetup folder
  5. Extract to above folder the contents of DBSetup.tar
  6. Switch to dbsetup folder
  7. Change DBSetup app configuration to target the MSSql server of the Database container
  8. Run the DB initialization script
