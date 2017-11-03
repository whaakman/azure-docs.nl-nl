---
title: Oracle Golden Gate implementeren op een Azure Linux VM | Microsoft Docs
description: Snel gebruiksklaar een Gate Oracle Golden omhoog in uw Azure-omgeving.
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
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: a05711357d345267647c02e42336fd37c09e1bff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Oracle Golden Gate implementeren op een Azure Linux VM 

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding wordt het gebruik van de Azure CLI voor het implementeren van een Oracle-12-c-database uit de galerie van Azure Marketplace installatiekopie. 

Dit document leest u stapsgewijs hoe maken, installeren en configureren van Oracle Golden Gate op een virtuele machine van Azure.

Voordat u begint, moet u controleren of de Azure-CLI is geïnstalleerd. Zie voor meer informatie de [Installatiehandleiding van de Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>De omgeving voorbereiden

Als u wilt de installatie van de Oracle Golden Gate uitvoeren, moet u twee virtuele Azure-machines op dezelfde beschikbaarheidsset maken. De Marketplace-installatiekopie die u kunt maken van de virtuele machines is **Oracle: Oracle-Database-Ee:12.1.0.2:latest**.

Ook moet u bekend zijn met Unix-editor vi en basiskennis van x11 (Windows X).

Hier volgt een samenvatting van de configuratie van de omgeving:
> 
> |  | **Primaire site** | **Replicatie van site** |
> | --- | --- | --- |
> | **Oracle-release** |Oracle 12c versie 2: (12.1.0.2) |Oracle 12c versie 2: (12.1.0.2)|
> | **Computernaam** |myVM1 |myVM2 |
> | **Besturingssysteem** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle-SID** |CDB1 |CDB1 |
> | **Schema voor replicatie** |TEST|TEST |
> | **Golden Gate eigenaar/repliceren** |C ##GGADMIN |REPUSER |
> | **Golden Gate-proces** |EXTORA |REPORA|


### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Aanmelden bij uw Azure-abonnement met de [az aanmelding](/cli/azure/#login) opdracht. Volg de aanwijzingen op het scherm richtingen.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container in welke Azure-resources zijn geïmplementeerd en die ze kunnen worden beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westus`.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

De volgende stap is optioneel maar aanbevolen. Zie voor meer informatie [Azure handleiding voor beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm#create). 

Het volgende voorbeeld maakt twee virtuele machines met de naam `myVM1` en `myVM2`. SSH-sleutels maken als deze niet al bestaan op de standaardlocatie van de sleutel. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

#### <a name="create-myvm1-primary"></a>Maak myVM1 (primaire):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Nadat de virtuele machine is gemaakt, geeft de Azure CLI informatie weer zoals in het volgende voorbeeld. (Noteer de `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.)

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

#### <a name="create-myvm2-replicate"></a>MyVM2 maken (repliceren):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Noteer de `publicIpAddress` ook nadat deze is gemaakt.

### <a name="open-the-tcp-port-for-connectivity"></a>Open de TCP-poort voor connectiviteit

De volgende stap is het configureren van externe eindpunten, waarmee u kunt extern toegang tot de Oracle-database. Voer de volgende opdrachten voor het configureren van de externe eindpunten.

#### <a name="open-the-port-for-myvm1"></a>Open de poort voor myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

De resultaten moeten er ongeveer als het volgende antwoord:

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

#### <a name="open-the-port-for-myvm2"></a>Open de poort voor myVM2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om een SSH-sessie te starten voor de virtuele machine. Vervang het IP-adres door het `publicIpAddress` van de virtuele machine.

```bash 
ssh <publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a>De database maken op myVM1 (primair)

De Oracle-software is al geïnstalleerd op de Marketplace-installatiekopie, zodat de volgende stap is het installeren van de database. 

Voer de software als supergebruiker 'oracle':

```bash
sudo su - oracle
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
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

De variabelen ORACLE_SID en ORACLE_HOME ingesteld.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Eventueel, kunt u toevoegen ORACLE_HOME en ORACLE_SID naar het bestand .bashrc, zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle-listener starten
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-the-database-on-myvm2-replicate"></a>De database maken op myVM2 (repliceren)

```bash
sudo su - oracle
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
De variabelen ORACLE_SID en ORACLE_HOME ingesteld.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Desgewenst kunt u ORACLE_HOME en ORACLE_SID zijn toegevoegd aan het bestand .bashrc zodat deze instellingen worden opgeslagen voor toekomstige aanmeldingen.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle-listener starten
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Golden Gate configureren 
Voer de stappen in deze sectie voor het configureren van Golden-Gate.

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
Force logboekregistratie inschakelen en controleer of ten minste één logboekbestand aanwezig is.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Golden Gate-software downloaden
Als u wilt downloaden en voorbereiden van de Oracle Golden Gate-software, moet u de volgende stappen uitvoeren:

1. Download de **fbo_ggs_Linux_x64_shiphome.zip** bestand van de [Oracle Golden Gate downloadpagina](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html). Onder de titel downloaden **Oracle GoldenGate 12.x.x.x voor Oracle Linux x86-64**, moet er een reeks ZIP-bestanden te downloaden.

2. Nadat u het ZIP-bestanden op uw clientcomputer hebt gedownload, Kopieer de bestanden naar uw virtuele machine met gebruik van Secure kopie Protocol (SCP):

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. Verplaats de ZIP-bestanden naar de **/ opt** map. Wijzig de eigenaar van de bestanden als volgt:

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. Pak de bestanden (de Linux installeren pak hulpprogramma als deze nog niet is geïnstalleerd):

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. De machtiging wijzigen:

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a>Voorbereiden van de client en de virtuele machine voor uit te voeren x11 (alleen voor Windows-clients)
Dit is een optionele stap. U kunt deze stap overslaan als u een voor Linux client of al x11 hebt setup.

1. Download PuTTY en Xming naar uw Windows-computer:

  * [Download PuTTY](http://www.putty.org/)
  * [Xming downloaden](https://xming.en.softonic.com/)

2.  Nadat u PuTTY, in de PuTTY-map (bijvoorbeeld C:\Program Files\PuTTY) hebt geïnstalleerd, voert u puttygen.exe (Generator PuTTY sleutel).

3.  In de PuTTY codegenerator:

  - Als een sleutel worden gegenereerd, selecteer de **genereren** knop.
  - Kopieer de inhoud van de sleutel (**Ctrl + C**).
  - Selecteer de **persoonlijke sleutel opslaan** knop.
  - Negeer de waarschuwing die wordt weergegeven, en selecteer vervolgens **OK**.

    ![Schermafbeelding van de pagina PuTTY codegenerator](./media/oracle-golden-gate/puttykeygen.png)

4.  In de virtuele machine, moet u deze opdrachten uitvoeren:

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. Maak een bestand met de naam **authorized_keys**. Plak de inhoud van de sleutel in dit bestand en sla het bestand.

  > [!NOTE]
  > De sleutel moet de tekenreeks bevatten `ssh-rsa`. De inhoud van de sleutel moet ook een enkele regel tekst.
  >  

6. Start PuTTY. In de **categorie** deelvenster **verbinding** > **SSH** > **Auth**. In de **persoonlijke sleutelbestand voor verificatie** vak, blader naar de sleutel die u eerder hebt gegenereerd.

  ![Schermafbeelding van de pagina persoonlijke sleutel instellen](./media/oracle-golden-gate/setprivatekey.png)

7. In de **categorie** deelvenster **verbinding** > **SSH** > **X11**. Selecteer vervolgens de **inschakelen X11 doorsturen** vak.

  ![Schermafbeelding van de pagina X11 inschakelen](./media/oracle-golden-gate/enablex11.png)

8. In de **categorie** deelvenster, gaat u naar **sessie**. Geef hostinformatie op de en selecteer vervolgens **Open**.

  ![Schermafbeelding van de sessie-pagina](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Golden Gate software installeren

Voor installatie Oracle Golden poort, voert u de volgende stappen uit:

1. Meld u aan als oracle. (U moet zich aanmelden zonder te worden gevraagd om een wachtwoord.) Zorg ervoor dat Xming voordat u begint met de installatie wordt uitgevoerd.
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. Selecteer 'Oracle GoldenGate voor Oracle-Database 12c'. Selecteer vervolgens **volgende** om door te gaan.

  ![Schermopname van het installatieprogramma op de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Wijzig de locatie van de software. Selecteer vervolgens de **Manager Start** in en voert u de locatie van de database. Selecteer **volgende** om door te gaan.

  ![Schermafbeelding van de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Wijzig de directory inventaris en selecteer vervolgens **volgende** om door te gaan.

  ![Schermafbeelding van de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_03.png)

5. Op de **samenvatting** Schakel in het scherm **installeren** om door te gaan.

  ![Schermopname van het installatieprogramma op de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_04.png)

6. U wordt mogelijk gevraagd een script als 'root' uit te voeren. Zo ja, open een afzonderlijke sessie ssh naar de virtuele machine, sudo aan basis-, en voer het script. Selecteer **OK** gaan.

  ![Schermafbeelding van de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_05.png)

7. Wanneer de installatie is voltooid, selecteert u **sluiten** om het proces te voltooien.

  ![Schermafbeelding van de pagina installatie selecteren](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>Instellen van de service op myVM1 (primair)

1. Maken of bijwerken van het bestand tnsnames.ora:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. De Golden Gate eigenaar en gebruikersaccounts maken.

  > [!NOTE]
  > De eigenaarsaccount moet C ## voorvoegsel hebben.
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA to C##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. De gebruikersaccount Golden Gate test maken:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. Configureer het parameterbestand uitpakken.

 Start de Gouden gate-opdrachtregelinterface (ggsci):

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. De volgende toevoegen aan het parameterbestand EXTRACT (via vi opdrachten). Druk op Esc-toets ': wq!' bestand op te slaan. 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. Registratie uit te pakken--geïntegreerde uitpakken.

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. Extract controlepunten ingesteld en realtime extract te starten:

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request to MANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
In deze stap maakt vinden u de eerste SCN die later wordt gebruikt in een andere sectie:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a>Instellen van de service op myVM2 (repliceren)


1. Maken of bijwerken van het bestand tnsnames.ora:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Een repliceren account maken:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba to repuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. Maak een gebruikersaccount voor Golden Gate-test:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. Het parameterbestand REPLICAT om wijzigingen te repliceren: 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  De inhoud van het parameterbestand REPORA:

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. Een controlepunt replicat instellen:

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-the-replication-myvm1-and-myvm2"></a>Instellen van de replicatie (myVM1 en myVM2)

#### <a name="1-set-up-the-replication-on-myvm2-replicate"></a>1. Instellen van de replicatie op myVM2 (repliceren)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
Het bestand bijwerken met de volgende opties:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
Start de service Manager opnieuw:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-the-replication-on-myvm1-primary"></a>2. Instellen van de replicatie op myVM1 (primair)

Start de initiële laden en het controleren op fouten:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-the-replication-on-myvm2-replicate"></a>3. Instellen van de replicatie op myVM2 (repliceren)

Wijzig de SCN number met het nummer dat u eerder hebt verkregen:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
De replicatie is gestart en u kunt het testen door het invoegen van nieuwe records aan TEST tabellen.


### <a name="view-job-status-and-troubleshooting"></a>Status van de taak weergeven en problemen oplossen

#### <a name="view-reports"></a>Rapporten weergeven
Om rapporten te bekijken op myVM1, voer de volgende opdrachten:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
Om rapporten te bekijken op myVM2, voer de volgende opdrachten:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Weergave status en geschiedenis
U kunt status en geschiedenis op myVM1, voer de volgende opdrachten:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

U kunt status en geschiedenis op myVM2, voer de volgende opdrachten:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Dit voltooit de installatie en configuratie van Golden Gate op Oracle linux.


## <a name="delete-the-virtual-machine"></a>Verwijder de virtuele machine

Wanneer deze niet langer nodig is, kan de volgende opdracht worden gebruikt om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor het maken van virtuele machines met een hoge beschikbaarheid](../../linux/create-cli-complete.md)

[CLI-voorbeelden voor VM-implementatie verkennen](../../linux/cli-samples.md)
