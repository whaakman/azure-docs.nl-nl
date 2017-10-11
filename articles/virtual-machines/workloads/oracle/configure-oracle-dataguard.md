---
title: Oracle Data Guard implementeren op een virtuele machine van Azure Linux | Microsoft Docs
description: Snel gebruiksklaar Oracle Data Guard omhoog in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: rclaus
ms.openlocfilehash: 11492b85e95ddb39489e36c572af2a168b4c7af8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a>Oracle Data Guard implementeren op een virtuele machine van Azure Linux 

Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Dit artikel wordt beschreven hoe u Azure CLI gebruiken voor het implementeren van een database van de Oracle-Database 12c vanaf de Azure Marketplace-installatiekopie. In dit artikel ziet u stap voor stap, klikt u vervolgens het installeren en configureren van Data Guard op Azure een virtuele machine (VM).

Voordat u begint, zorg ervoor dat de Azure CLI is geïnstalleerd. Zie voor meer informatie de [Azure CLI installatiehandleiding](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>De omgeving voorbereiden
### <a name="assumptions"></a>Veronderstellingen

Als u wilt installeren Oracle Data Guard, moet u twee virtuele Azure-machines op dezelfde beschikbaarheidsset maken:

- De primaire virtuele machine (myVM1) heeft een actief Oracle-exemplaar.
- De stand-by VM (myVM2) heeft de Oracle-software alleen worden geïnstalleerd.

De Marketplace-installatiekopie die u gebruikt voor het maken van de virtuele machines is Oracle: Oracle-Database-Ee:12.1.0.2:latest.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Aanmelden bij uw Azure-abonnement met behulp van de [az aanmelding](/cli/azure/#login) opdracht in en volg de op het scherm richtingen.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met behulp van de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container in welke Azure resources worden geïmplementeerd en beheerd. 

Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westus` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

Maken van een beschikbaarheidsset is optioneel, maar wordt u aangeraden deze. Zie voor meer informatie [Azure beschikbaarheidssets richtlijnen](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#create) opdracht. 

Het volgende voorbeeld maakt twee virtuele machines met de naam `myVM1` en `myVM2`. SSH-sleutels wordt ook gemaakt als deze niet al bestaan op de standaardlocatie van de sleutel. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

Maak myVM1 (primaire):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

Nadat u de virtuele machine hebt gemaakt, geeft Azure CLI informatie weer zoals in het volgende voorbeeld. Noteer de waarde van `publicIpAddress`. U kunt dit adres gebruiken voor toegang tot de virtuele machine.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

MyVM2 maken (stand-by):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

Noteer de waarde van `publicIpAddress` nadat u myVM2 hebt gemaakt.

### <a name="open-the-tcp-port-for-connectivity"></a>Open de TCP-poort voor connectiviteit

Deze stap configureert u externe eindpunten die externe toegang tot de Oracle-database.

Open de poort voor myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

Het resultaat moet er ongeveer als het volgende antwoord:

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

Open de poort voor myVM2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om een SSH-sessie te starten voor de virtuele machine. Vervang de IP-adres met de `publicIpAddress` waarde voor de virtuele machine.

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a>De database maken op myVM1 (primair)

De Oracle-software is al geïnstalleerd op de Marketplace-installatiekopie, zodat de volgende stap is het installeren van de database. 

Overschakelen naar de Oracle-beheerder:

```bash
$ sudo su - oracle
```

De database maken:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Uitvoer moeten er ongeveer als het volgende antwoord:

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

De variabelen ORACLE_SID en ORACLE_HOME instellen:

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

Eventueel, kunt u toevoegen ORACLE_HOME en ORACLE_SID naar het bestand /home/oracle/.bashrc, zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a>Bescherming van gegevens configureren

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>Bij myVM1 (primaire) archief logboek-modus inschakelen

```bash
$ sqlplus / as sysdba
SQL> SELECT log_mode FROM v$database;

LOG_MODE
------------
NOARCHIVELOG

SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
SQL> ALTER DATABASE ARCHIVELOG;
SQL> ALTER DATABASE OPEN;
```
Force logboekregistratie inschakelen en zorg ervoor dat ten minste één logboekbestand aanwezig is:

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

Stand-by opnieuw logboeken maken:

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

Flashback (waardoor herstel veel eenvoudiger) inschakelen en stand-by ingesteld\_bestand\_MANAGEMENT automatisch. Afsluiten van SQL * Plus daarna.

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a>Instellen van de service op myVM1 (primair)

Bewerk of het bestand tnsnames.ora, dat zich in de map $ORACLE_HOME\network\admin bevindt maken.

De volgende vermeldingen toevoegen:

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

Bewerk of het bestand listener.ora, dat zich in de map $ORACLE_HOME\network\admin bevindt maken.

De volgende vermeldingen toevoegen:

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

Schakel Data Guard Broker:
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
De listener starten:

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a>Instellen van de service op myVM2 (stand-by)

SSH kunt uitvoeren naar myVM2:

```bash 
$ ssh azureuser@<publicIpAddress>
```

Meld u aan als Oracle:

```bash
$ sudo su - oracle
```

Bewerk of het bestand tnsnames.ora, dat zich in de map $ORACLE_HOME\network\admin bevindt maken.

De volgende vermeldingen toevoegen:

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

Bewerk of het bestand listener.ora, dat zich in de map $ORACLE_HOME\network\admin bevindt maken.

De volgende vermeldingen toevoegen:

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

De listener starten:

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-the-database-to-myvm2-standby"></a>Herstel de database naar myVM2 (stand-by)

De parameter bestand /tmp/initcdb1_stby.ora maken met de volgende inhoud:
```bash
*.db_name='cdb1'
```

Mappen maken:

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

Maak een wachtwoord:

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
Start de database op myVM2:

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

De database herstellen met het hulpprogramma RMAN:

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

Voer de volgende opdrachten in RMAN:
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

U ziet de volgende strekking weergegeven als de opdracht is voltooid. RMAN af te sluiten.
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

Eventueel, kunt u toevoegen ORACLE_HOME en ORACLE_SID naar het bestand /home/oracle/.bashrc, zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

Schakel Data Guard Broker:
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a>Data Guard Broker configureren op myVM1 (primair)

Data Guard Manager start en aanmelden met SYS en een wachtwoord. (Gebruik geen OS-verificatie.) Het volgende doen:

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

Controleer de configuratie:
```bash
DGMGRL> SHOW CONFIGURATION;

Configuration - my_dg_config

  Protection Mode: MaxPerformance
  Members:
  cdb1      - Primary database
    cdb1_stby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS   (status updated 26 seconds ago)
```

U kunt de installatie van de Oracle Data Guard hebt voltooid. De volgende sectie leest u hoe de connectiviteit testen en overschakelen via.

### <a name="connect-the-database-from-the-client-machine"></a>Verbinding maken met de database van de clientcomputer

Bijwerken of maken van het bestand tnsnames.ora op de clientcomputer. Dit bestand bevindt zich doorgaans in $ORACLE_HOME\network\admin.

Vervang de IP-adressen met uw `publicIpAddress` waarden voor myVM1 en myVM2:

```bash
cdb1=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM1 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1)
    )
  )

cdb1_stby=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM2 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1_stby)
    )
  )
```

Start SQL * Plus:

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-the-data-guard-configuration"></a>Test de configuratie van Data Guard

### <a name="switch-over-the-database-on-myvm1-primary"></a>Overschakelen op de database op myVM1 (primair)

Overschakelen van primaire naar de stand-by (cdb1 naar cdb1_stby):

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER TO cdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection to instance "cdb1" on database "cdb1_stby"
Connecting to instance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

U kunt nu verbinding naar de stand-by-database.

Start SQL * Plus:

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-the-database-on-myvm2-standby"></a>Overschakelen op de database op myVM2 (stand-by)

Als u wilt overschakelen, voer het volgende op myVM2:
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome to DGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER TO cdb1;
Performing switchover NOW, please wait...
Operation requires a connection to instance "cdb1" on database "cdb1"
Connecting to instance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

Nogmaals: nu moet u verbinding maken met de primaire database.

Start SQL * Plus:

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

U hebt de installatie en configuratie van Data Guard op Oracle Linux.


## <a name="delete-the-virtual-machine"></a>Verwijder de virtuele machine

Wanneer u de virtuele machine niet meer nodig hebt, kunt u de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)

[VM-implementatie Azure CLI voorbeelden verkennen](../../linux/cli-samples.md)
