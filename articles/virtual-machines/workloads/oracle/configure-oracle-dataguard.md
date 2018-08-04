---
title: Oracle Data Guard implementeren op een virtuele Azure Linux-machine | Microsoft Docs
description: Snel gebruiksklaar Oracle Data Guard omhoog in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: 08420be7171df78babf62b262fef84fd29fb34ab
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495060"
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a>Oracle Data Guard implementeren op een virtuele Azure Linux-machine 

Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In dit artikel wordt beschreven hoe u Azure CLI gebruiken voor het implementeren van een Oracle Database 12c-database uit de Azure Marketplace-installatiekopie. Dit artikel laat u stap voor stap vervolgens installeren en configureren van Data Guard op een Azure-machine (VM).

Voordat u begint, zorg ervoor dat Azure CLI is geïnstalleerd. Zie voor meer informatie de [Azure CLI-installatiehandleiding](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>De omgeving voorbereiden
### <a name="assumptions"></a>Veronderstellingen

Voor het installeren van Oracle Data Guard, moet u in dezelfde beschikbaarheidsset twee virtuele machines van Azure maken:

- De primaire virtuele machine (myVM1) heeft een actief exemplaar van Oracle.
- De stand-by-VM (myVM2) heeft de Oracle-software alleen worden geïnstalleerd.

De Marketplace-installatiekopie die u gebruiken voor het maken van de virtuele machines is Oracle: Oracle-Database-Ee:12.1.0.2:latest.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Aanmelden bij uw Azure-abonnement met behulp van de [az login](/cli/azure/reference-index#az_login) opdracht en volgt u de op het scherm aanwijzingen.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met behulp van de [az-groep maken](/cli/azure/group#az_group_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure resources worden geïmplementeerd en beheerd. 

Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westus` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

Het maken van een beschikbaarheidsset is optioneel, maar wordt u aangeraden deze. Zie voor meer informatie, [Azure-beschikbaarheidssets richtlijnen](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. 

Het volgende voorbeeld maakt u twee virtuele machines met de naam `myVM1` en `myVM2`. SSH-sleutels, maakt het ook als ze niet al bestaan op een standaardsleutellocatie. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

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

Nadat u de virtuele machine hebt gemaakt, toont Azure CLI informatie die vergelijkbaar is met het volgende voorbeeld. Noteer de waarde van `publicIpAddress`. U gebruikt dit adres voor toegang tot de virtuele machine.

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

### <a name="open-the-tcp-port-for-connectivity"></a>Open de TCP-poort voor verbinding

Deze stap configureert u externe eindpunten, waarmee u externe toegang tot de Oracle-database.

Open de poort voor myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

Het resultaat ziet er vergelijkbaar met het volgende antwoord:

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

Gebruik de volgende opdracht om een SSH-sessie te starten voor de virtuele machine. Vervang het IP-adres met de `publicIpAddress` waarde voor uw virtuele machine.

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a>Maken van de database op myVM1 (primair)

De Oracle-software is al geïnstalleerd op de Marketplace-installatiekopie, zodat de volgende stap is het installeren van de database. 

Schakel over naar de Oracle-beheerder:

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
Uitvoer ziet er vergelijkbaar met het volgende antwoord:

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

Stel de variabelen ORACLE_SID en ORACLE_HOME:

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

(Optioneel) kunt u toevoegen ORACLE_HOME en ORACLE_SID naar het bestand /home/oracle/.bashrc, zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a>Data Guard configureren

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>Modus voor het logboek archiveren op myVM1 (primaire) inschakelen

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
Force logboekregistratie inschakelen en zorg ervoor dat ten minste één logboekbestand is aanwezig:

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

Logboeken van de stand-by opnieuw maken:

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

Schakel Flashback (wat recovery veel gemakkelijker maakt) en stel de stand-by\_bestand\_MANAGEMENT automatisch. Sluit SQL * Plus hierna.

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a>Instellen van de service op myVM1 (primair)

Bewerken of maken van het bestand tnsnames.ora, die zich in de map $ORACLE_HOME\network\admin.

De volgende vermeldingen toe te voegen:

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

Bewerken of maken van het bestand listener.ora, die zich in de map $ORACLE_HOME\network\admin.

De volgende vermeldingen toe te voegen:

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

Data Guard Broker inschakelen:
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

SSH naar myVM2:

```bash 
$ ssh azureuser@<publicIpAddress>
```

Meld u aan als Oracle:

```bash
$ sudo su - oracle
```

Bewerken of maken van het bestand tnsnames.ora, die zich in de map $ORACLE_HOME\network\admin.

De volgende vermeldingen toe te voegen:

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

Bewerken of maken van het bestand listener.ora, die zich in de map $ORACLE_HOME\network\admin.

De volgende vermeldingen toe te voegen:

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


### <a name="restore-the-database-to-myvm2-standby"></a>De database herstellen naar myVM2 (stand-by)

De parameter-bestand /tmp/initcdb1_stby.ora maken met de volgende inhoud:
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

Maak een wachtwoordbestand:

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

De database herstellen met behulp van het hulpprogramma RMAN:

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

U ziet de volgende strekking weergegeven wanneer de opdracht is voltooid. RMAN afsluiten.
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

(Optioneel) kunt u toevoegen ORACLE_HOME en ORACLE_SID naar het bestand /home/oracle/.bashrc, zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

Data Guard Broker inschakelen:
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a>Data Guard Broker configureren op myVM1 (primair)

Data Guard Manager start en zich aanmeldt met behulp van SYS en een wachtwoord. (Gebruik geen OS-verificatie.) Het volgende doen:

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

U kunt de Oracle Data Guard-instellingen hebt voltooid. De volgende sectie wordt beschreven hoe u de connectiviteit testen en schakel over.

### <a name="connect-the-database-from-the-client-machine"></a>Verbinding maken met de database van de client-computer

Bijwerken of maak het bestand tnsnames.ora op de clientcomputer. Dit bestand wordt meestal $ORACLE_HOME\network\admin.

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
## <a name="test-the-data-guard-configuration"></a>De configuratie van de Data Guard testen

### <a name="switch-over-the-database-on-myvm1-primary"></a>Schakel over de database op myVM1 (primair)

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

U kunt nu verbinding maken met de stand-by-database.

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

### <a name="switch-over-the-database-on-myvm2-standby"></a>Schakel over de database op myVM2 (stand-by)

Als u wilt overschakelen, voert u het volgende op myVM2:
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

Nogmaals, zou u nu moeten kunnen verbinding maken met de primaire database.

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

U hebt de installatie en configuratie van Data Guard voor Oracle Linux.


## <a name="delete-the-virtual-machine"></a>Verwijder de virtuele machine

Wanneer u de virtuele machine niet meer nodig hebt, kunt u de volgende opdracht uit de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)

[Azure CLI-voorbeelden voor VM-implementatie verkennen](../../linux/cli-samples.md)
