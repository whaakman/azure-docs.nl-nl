---
title: Maken van een Oracle-database in een virtuele machine in Azure | Microsoft Docs
description: Snel gebruiksklaar een Oracle-Database 12c-database in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 8683b016c4db2c66fb1dd994405b70c3d137a7fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Maak een Oracle-Database in een Azure VM

Deze handleiding details met de Azure CLI voor het implementeren van een Azure virtuele machine van de [Oracle marketplace afbeelding](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) om te kunnen maken van een 12-c Oracle-database. Zodra de server is geïmplementeerd, maakt u verbinding via SSH om te configureren van de Oracle-database. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a>Virtuele machine maken

Gebruik voor het maken van een virtuele machine (VM) de [az vm maken](/cli/azure/vm#create) opdracht. 

In het volgende voorbeeld wordt een virtuele machine met de naam `myVM` gemaakt. SSH-sleutels wordt ook gemaakt als deze niet al bestaan op de standaardlocatie van de sleutel. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

Nadat u de virtuele machine hebt gemaakt, ziet Azure CLI er ongeveer als volgt uitzien. Noteer de waarde voor `publicIpAddress`. U kunt dit adres gebruiken voor toegang tot de virtuele machine.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Maakt een SSH-sessie met de virtuele machine, moet u de volgende opdracht gebruiken. Vervang de IP-adres met de `publicIpAddress` waarde voor uw virtuele machine.

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a>De database maken

De Oracle-software is al geïnstalleerd op de Marketplace-installatiekopie. Als volgt te werk om een voorbeelddatabase te maken. 

1.  Overschakelen naar de *oracle* supergebruiker en vervolgens de initialisatie van de listener voor logboekregistratie:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    De uitvoer lijkt op het volgende:

    ```bash
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    The listener supports no services
    The command completed successfully
    ```

2.  De database maken:

    ```bash
    dbca -silent \
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

    Het duurt enkele minuten om de database te maken.

3. Oracle-variabelen instellen

Voordat u verbinding maakt, moet u twee omgevingsvariabelen worden ingesteld: *ORACLE_HOME* en *ORACLE_SID*.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
U kunt ook variabelen ORACLE_HOME en ORACLE_SID toevoegen aan het bestand .bashrc. Hierdoor zou de omgevingsvariabelen voor toekomstige aanmeldingen opgeslagen. Controleer de volgende instructies zijn toegevoegd aan de `~/.bashrc` bestand met behulp van de editor van uw keuze.

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Oracle EM snelle verbindingen

Voor een GUI-beheerprogramma waarmee u kunt de database verkennen, Oracle EM Express instellen. Als u wilt verbinding maken met Oracle EM Express, moet u eerst de poort in Oracle instellen. 

1. Verbinding maken met de database met de sqlplus:

    ```bash
    sqlplus / as sysdba
    ```

2. Eenmaal zijn verbonden, de poort 5502 instellen voor snelle EM

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Open de container PDB1 als dat niet al is geopend, maar het eerste Controleer de status:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    De uitvoer lijkt op het volgende:

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. Als de OPEN_MODE voor `PDB1` is niet gelezen SCHRIJFT, voer de volgende opdrachten PDB1 openen:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

U moet zelf typen `quit` naar einde van de sessie sqlplus en het type `exit` af te melden van de oracle-gebruiker.

## <a name="automate-database-startup-and-shutdown"></a>Database opstarten en afsluiten automatiseren

De Oracle-database standaard niet automatisch wordt gestart wanneer u de virtuele machine opnieuw opstart. Als u de Oracle-database instelt op automatisch starten, zich eerst aanmelden als hoofdmap. Vervolgens maken en bijwerken van sommige systeembestanden.

1. Aanmelden als hoofdmap
    ```bash
    sudo su -
    ```

2.  Met behulp van uw favoriete editor Bewerk het bestand `/etc/oratab` en wijzigt u de standaard `N` naar `Y`:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Maak een bestand met de naam `/etc/init.d/dbora` en plak de volgende inhoud:

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  Wijzigen van machtigingen voor bestanden met *type chmod* als volgt:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Symbolische koppelingen maken voor opstarten en afsluiten als volgt:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  Start opnieuw op de virtuele machine voor het testen van de wijzigingen:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>Open poorten voor connectiviteit

De laatste taak is het aantal externe eindpunten configureren. Als u de Azure-netwerk-beveiligingsgroep die de virtuele machine beveiligt instelt, moet u eerst uw SSH-sessie in de virtuele machine (moet hebben is volledig buiten SSH wanneer opnieuw opstarten in de vorige stap) sluiten. 

1.  Om te openen van het eindpunt waarmee u kunt extern toegang tot de Oracle-database, maakt u een regel Netwerkbeveiligingsgroep met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) als volgt: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  Om te openen van het eindpunt dat u extern toegang tot Oracle EM Express gebruiken, maakt u een regel Netwerkbeveiligingsgroep met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) als volgt:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. Indien nodig, het openbare IP-adres van uw virtuele machine opnieuw met verkrijgen [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#show) als volgt:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  Verbinding maken met snelle EM vanuit de browser. Controleer of dat uw browser is compatibel met EM Express (Flash installatie is vereist): 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

U kunt aanmelden met behulp van de **SYS** account en controleer de **als sysdba** selectievakje. Het wachtwoord gebruiken **OraPasswd1** die u hebt ingesteld tijdens de installatie. 

![Schermafbeelding van de aanmeldingspagina Oracle OEM Express](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u klaar bent met het verkennen van uw eerste Oracle-database in Azure en de virtuele machine niet meer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht voor het verwijderen van de resourcegroep, VM, en alle bijbehorende resources.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere [Oracle-oplossingen in Azure](oracle-considerations.md). 

Probeer de [installeren en configureren van Oracle geautomatiseerde Storage Management](configure-oracle-asm.md) zelfstudie.