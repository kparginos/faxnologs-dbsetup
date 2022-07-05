# FaxNoLogs Containers Preparation and Database Initial Setup

<details><summary>Installation Pre-requisities</summary>
<p>
	
In order to setup the containers neccessary for the FaxNoLogs application, the host machine must have docker support installed.
According to the type of your OS you can obtain Docker from:
	
1. https://docs.docker.com/desktop/linux/install/ for Linux
2. https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe for Windows
	
It is advisable to read Docker's vendor notes for each OS before proceeding with the installation
</p>
</details>

<details><summary>How to create the containers(db and app) on a host machine</summary>
<p>
	
* Setup the containers on a Windows host machine:
	1. Download https://github.com/kparginos/faxnologs-dbsetup/blob/main/FaxNoLogs-Containers-WinSetup.yml
	2. From a command prompt run the following(must be at the same folder where you've downloaded the above):

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

* Setup the containers on a Linux host machine:
	1. Download https://github.com/kparginos/faxnologs-dbsetup/blob/main/FaxNoLogs-Containers-LinuxSetup.yml
	2. From a shell run the following(must be at the same folder where you've downloaded the above):
```
docker-compose -f FaxNoLogs-Containers-LinuxSetup.yml up -d
```

The above will get all neccessary images from the remote repo and start the containers. Will also create on host the folder
/var/faxnologs/data which will be the volume for the database container to store it databases.

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
</p>
</details>

<details><summary>How to update the containers(db and app) on a host machine</summary>
<p>

In order to update the latest containers you need to do the following:

For the Windows Host run this command:

```
docker-compose -f FaxNoLogs-Containers-WinSetup.yml pull
```

For the Linux Host run this command:

```
docker-compose -f FaxNoLogs-Containers-LinuxSetup.yml pull
```

</p>
</details>

<details><summary>How to apply the DB Initial setup</summary>
<p>

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
	
If the last command that creates the database completes successfully, there should be the following output to console:

```
Start Database Initialization...
Building Database...
Creating Tables...
Creating Tables - OK
Create SPs...
Create SPs - OK
Create Views...
Create Views - OK
Database Initialization Completed
```
</p>
</details>
