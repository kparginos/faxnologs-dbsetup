# FaxNoLogs Database Initial Setup


## How to create the containers(db and app) on a host machine
1. Setup the containers on a Windows host machine:

	 i. Download https://github.com/kparginos/faxnologs-dbsetup/blob/main/FaxNoLogs-Containers-WinSetup.yml
	
	ii. From a command prompt run the following:

```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml up -d
```

The above will get all neccessary images from the remote repo and start the containers. Will also create on host the folder
C:\faxnologs\data which will be the volume for the database container to store it databases.

To stop the containers run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml stop
```
To start the containers run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml start
```

To remove the containers run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml down
```
Note: This will not erase the volumes stored at the host machine

2. Setup the containers on a Linux host machine:

	 i. Download https://github.com/kparginos/faxnologs-dbsetup/blob/main/FaxNoLogs-Containers-LinuxSetup.yml
	
	ii. From a shell run the following:
```
docker-compose -f FaxNoLogs-Containers-LinuxSetup.yml up -d
```

The above will get all neccessary images from the remote repo and start the containers. Will also create on host the folder
C:\faxnologs\data which will be the volume for the database container to store it databases.

To stop the containers from running run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml stop
```

To start the containers run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml start
```
To remove the containers run:
```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml down
```
Note: This will not erase the volumes stored at the host machine

>## How to apply the DB Initial setup

At the host machine run the following:
```
docker exec faxnologs_webapp bash -c "apt-get update && apt-get -y install wget && wget --no-check-certificate https://github.com/kparginos/faxnologs-dbsetup/raw/main/DBSetup.tar && mkdir dbsetup && tar xf DBSetup.tar -C dbsetup && cd dbsetup && sed -i 's/localhost,1433/db/g' appsettings.json && dotnet FaxNoLogs.Migrations.dll"
```
The above command, should it run correctly, must apply the following:

  1. Update the container's OS
  2. Install **wget** utility
  3. Use wget to download the **DBSetup.tar**
  4. Create a dbsetup folder
  5. Extract to above folder the contents of DBSetup.tar
  6. Switch to dbsetup folder
  7. Change DBSetup app configuration to target the MSSql server of the Database container
  8. Run the DB initialization script
