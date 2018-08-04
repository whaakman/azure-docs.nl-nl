---
title: Back-up en herstellen van een Oracle Database 12c-database op een virtuele Azure Linux-machine | Microsoft Docs
description: Leer hoe u back-up en herstellen van een Oracle Database 12c-database in uw Azure-omgeving.
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
ms.openlocfilehash: 93fbd5bbba91b45e1afd123a2466b249302e2354
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492837"
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Back-up en herstellen van een Oracle Database 12c-database op een virtuele Azure Linux-machine

U kunt Azure CLI gebruiken om te maken en beheren van Azure-resources bij een opdrachtprompt of scripts gebruiken. In dit artikel gebruiken we Azure CLI-scripts voor het implementeren van een Oracle Database 12c-database van de installatiekopie van een Azure Marketplace-galerie.

Voordat u begint, zorg ervoor dat Azure CLI is geïnstalleerd. Zie voor meer informatie de [Azure CLI-installatiehandleiding](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>De omgeving voorbereiden

### <a name="step-1-prerequisites"></a>Stap 1: vereisten

*   Als u het proces van back-up en herstel, moet u eerst een Linux-VM met een geïnstalleerd exemplaar van Oracle Database 12c maken. De Marketplace-installatiekopie die u kunt de virtuele machine maken met de naam *Oracle: Oracle-Database-Ee:12.1.0.2:latest*.

    Zie voor meer informatie over het maken van een Oracle-database, de [Oracle database-snelstartgids maken](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-to-the-vm"></a>Stap 2: Verbinding maken met de virtuele machine

*   Voor het maken van een Secure Shell (SSH)-sessie met de virtuele machine, gebruikt u de volgende opdracht. Vervang het IP-adres en de host-naam-combinatie met de `publicIpAddress` waarde voor uw virtuele machine.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a>Stap 3: De database voorbereiden

1.  Deze stap wordt ervan uitgegaan dat u hebt een Oracle-exemplaar (cdb1) dat wordt uitgevoerd op een virtuele machine met de naam *myVM*.

    Voer de *oracle* superuser hoofdmap en vervolgens worden de listener geïnitialiseerd:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
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

2.  (Optioneel) Zorg ervoor dat de database zich in de modus voor het logboek van archief:

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
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (Optioneel) Maak een tabel als u wilt testen van het doorvoeren:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Controleren of de back-upbestand locatie en grootte wijzigen:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Gebruik Oracle Recovery Manager (RMAN) voor back-up van de database:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Stap 4: Toepassingsconsistente back-up voor virtuele Linux-machines

Toepassingsconsistente back-ups is een nieuwe functie in Azure Backup. U kunt maken en scripts uit te voeren voor en na de VM-momentopname (voorafgaat aan momentopnamen en volgt op momentopnamen) selecteren.

1. Download het JSON-bestand.

    Download VMSnapshotScriptPluginConfig.json van https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig. Inhoud van het bestand er ongeveer als volgt uit:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Maak de map etc op de virtuele machine:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. Kopieer het JSON-bestand.

    Kopieer VMSnapshotScriptPluginConfig.json naar de map etc.

4. Bewerk het JSON-bestand.

    Bewerk het bestand VMSnapshotScriptPluginConfig.json om op te nemen de `PreScriptLocation` en `PostScriptlocation` parameters. Bijvoorbeeld:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Maak de voorafgaat aan momentopnamen en volgt op momentopnamen scriptbestanden.

    Hier volgt een voorbeeld van scripts voor voorafgaat aan momentopnamen en volgt op momentopnamen voor een 'koude back-up' (een offline back-up, met afsluiten en opnieuw starten):

    Voor /etc/azure/pre_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    Voor /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    Hier volgt een voorbeeld van scripts voor voorafgaat aan momentopnamen en volgt op momentopnamen voor een 'hot back-up' (een online back-up):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Voor /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Voor /etc/azure/pre_script.sql, wijzigt u de inhoud van het bestand overeenkomstig uw vereisten:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    Voor /etc/azure/post_script.sql, wijzigt u de inhoud van het bestand overeenkomstig uw vereisten:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Machtigingen voor bestanden wijzigen:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Test de scripts.

    Als u wilt testen van de scripts, eerst, meld u als root. Controleer vervolgens of dat er geen fouten zijn:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Zie voor meer informatie, [toepassingsconsistente back-up voor virtuele Linux-machines](https://azure.microsoft.com/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a>Stap 5: Gebruik Azure Recovery Services-kluizen om het back-up van de virtuele machine

1.  Zoek in de Azure-portal **Recovery Services-kluizen**.

    ![Pagina Recovery Services-kluizen](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Op de **Recovery Services-kluizen** blade, om toe te voegen een nieuwe kluis, klikt u op **toevoegen**.

    ![Recovery Services-kluizen toevoegen pagina](./media/oracle-backup-recovery/recovery_service_02.png)

3.  Als u wilt doorgaan, klikt u op **myVault**.

    ![Recovery Services-kluizen Details pagina](./media/oracle-backup-recovery/recovery_service_03.png)

4.  Op de **myVault** blade, klikt u op **back-up**.

    ![Recovery Services-kluizen back-up maken van de pagina](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Op de **back-updoel** blade, gebruikt u de standaardwaarden van **Azure** en **virtuele machine**. Klik op **OK**.

    ![Recovery Services-kluizen Details pagina](./media/oracle-backup-recovery/recovery_service_05.png)

6.  Voor **back-upbeleid**, gebruikt u **DefaultPolicy**, of selecteer **nieuw beleid**. Klik op **OK**.

    ![Recovery Services-kluizen back-up van de detailpagina van beleid](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Op de **virtuele machines selecteren** blade, selecteer de **myVM1** selectievakje en klik vervolgens op **OK**. Klik op de **back-up inschakelen** knop.

    ![Recovery Services-kluizen items naar de detailpagina van het back-up](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Nadat u op **back-up inschakelen**, het back-upproces start niet totdat het geplande tijdstip is verlopen. Als u een onmiddellijke back-up instelt, voert u de volgende stap.

8.  Op de **myVault - back-upitems** blade onder **aantal back-UPITEMS**, selecteert u het aantal back-upitems.

    ![Recovery Services-kluizen myVault-detailpagina](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Op de **back-Upitems (Azure virtuele Machine)** blade aan de rechterkant van de pagina, klikt u op het weglatingsteken (**...** ) en klik vervolgens op **back-up nu**.

    ![Recovery Services-kluizen de opdracht nu back-up](./media/oracle-backup-recovery/recovery_service_09.png)

10. Klik op de **back-up** knop. Wacht tot de back-proces is voltooid. Ga vervolgens naar [stap 6: verwijderen van de databasebestanden](#step-6-remove-the-database-files).

    Als u wilt de status van de back-uptaak weergeven, klikt u op **taken**.

    ![Recovery Services-kluizen taak pagina](./media/oracle-backup-recovery/recovery_service_10.png)

    De status van de back-uptaak wordt weergegeven in de volgende afbeelding:

    ![Recovery Services-kluizen pagina met de status van taken](./media/oracle-backup-recovery/recovery_service_11.png)

11. Los eventuele fouten in het logboekbestand voor een toepassingsconsistente back-up. Het logboekbestand bevindt zich op /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.

### <a name="step-6-remove-the-database-files"></a>Stap 6: De databasebestanden verwijderen 
Verderop in dit artikel leert u hoe u kunt het herstelproces te testen. Voordat u het herstelproces testen kunt, moet u de databasebestanden verwijderen.

1.  Verwijder de bestanden tabelruimte en back-up:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (Optioneel) Het Oracle-exemplaar afsluiten:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a>De verwijderde bestanden herstellen vanaf de Recovery Services-kluizen
Als u de verwijderde bestanden herstellen, voert u de volgende stappen uit:

1. Zoek in de Azure-portal, de *myVault* Recovery Services-kluizen item. Op de **overzicht** blade onder **back-up items**, selecteert u het aantal items.

    ![Recovery Services-kluizen myVault back-upitems](./media/oracle-backup-recovery/recovery_service_12.png)

2. Onder **aantal back-up-items**, selecteert u het aantal items.

    ![Recovery Services-kluizen aantal voor virtuele Azure-Machine-back-upitems](./media/oracle-backup-recovery/recovery_service_13.png)

3. Op de **myvm1** blade, klikt u op **Bestandsherstel (Preview)**.

    ![Schermafbeelding van de Recovery Services-kluizen pagina van de herstel-bestand](./media/oracle-backup-recovery/recovery_service_14.png)

4. Op de **Bestandsherstel (Preview)** deelvenster, klikt u op **downloadscript**. Sla het downloadbestand (.sh) naar een map op de clientcomputer.

    ![Opties voor opgeslagen-script-bestand downloaden](./media/oracle-backup-recovery/recovery_service_15.png)

5. Kopieer het .sh-bestand naar de virtuele machine.

    Het volgende voorbeeld ziet hoe u een beveiligde kopie (scp) gebruiken opdracht voor het verplaatsen van het bestand naar de virtuele machine. U kunt de inhoud ook kopiëren naar het Klembord en plak de inhoud in een nieuw bestand dat is ingesteld op de virtuele machine.

    > [!IMPORTANT]
    > Zorg ervoor dat u de IP-adres en de map waarden bijwerken in het volgende voorbeeld. De waarden moeten worden toegewezen aan de map waarin het bestand is opgeslagen.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Het bestand wijzigen zodat deze eigendom van de hoofdmap.

    In het volgende voorbeeld wordt het bestand te wijzigen zodat deze eigendom van de hoofdmap. Wijzig machtigingen.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    Het volgende voorbeeld ziet wat u ziet nadat u hebt dit script wordt uitgevoerd. Wanneer u wordt gevraagd om door te gaan, voert u **Y**.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. Toegang tot de gekoppelde volumes is bevestigd.

    Als u wilt afsluiten, voer **q**, en zoek vervolgens de gekoppelde volumes. Voor het maken van een lijst met de toegevoegde volumes, bij een opdrachtprompt, voer **df -k**.

    ![De df -k-opdracht](./media/oracle-backup-recovery/recovery_service_16.png)

8. Het volgende script gebruiken om te kopiëren van de ontbrekende bestanden terug naar de mappen:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. Gebruik RMAN om de database te herstellen in het volgende script:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Ontkoppel de schijf.

    In de Azure-portal op de **Bestandsherstel (Preview)** blade, klikt u op **schijven ontkoppelen**.

    ![Opdracht schijven ontkoppelen](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a>De volledige VM herstellen

In plaats van de verwijderde bestanden terugzetten vanuit de Recovery Services-kluizen, kunt u de volledige virtuele machine herstellen.

### <a name="step-1-delete-myvm"></a>Stap 1: Verwijderen myVM

*   In de Azure-portal, gaat u naar de **myVM1** kluis en selecteer vervolgens **verwijderen**.

    ![Opdracht voor verwijderen van kluis](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a>Stap 2: De virtuele machine herstellen

1.  Ga naar **Recovery Services-kluizen**, en selecteer vervolgens **myVault**.

    ![myVault vermelding](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Op de **overzicht** blade onder **back-up items**, selecteert u het aantal items.

    ![myVault maakt u een back-up van items](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Op de **back-Upitems (Azure virtuele Machine)** Selecteer **myvm1**.

    ![Pagina voor herstel-VM](./media/oracle-backup-recovery/recover_vm_04.png)

4.  Op de **myvm1** blade, klik op het weglatingsteken (**...** ) en klik vervolgens op **VM terugzetten**.

    ![Opdracht van de virtuele machine herstellen](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Op de **herstelpunt selecteren** blade, selecteer het item dat u wilt herstellen en klik vervolgens op **OK**.

    ![Selecteer het herstelpunt](./media/oracle-backup-recovery/recover_vm_06.png)

    Als u toepassingsconsistente back-up hebt ingeschakeld, wordt er een verticale blauwe balk weergegeven.

6.  Op de **configuratie terugzetten** blade, selecteert u de naam van de virtuele machine, selecteer de resourcegroep en klik vervolgens op **OK**.

    ![Herstellen-configuratiewaarden](./media/oracle-backup-recovery/recover_vm_07.png)

7.  Als u de virtuele machine herstellen, klikt u op de **herstellen** knop.

8.  Klik op om de status van het herstelproces **taken**, en klik vervolgens op **back-uptaken**.

    ![Opdracht voor back-uptaken-status](./media/oracle-backup-recovery/recover_vm_08.png)

    De volgende afbeelding toont de status van het herstelproces:

    ![Status van het herstelproces](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a>Stap 3: Het openbare IP-adres instellen
Nadat de virtuele machine is hersteld, instellen van het openbare IP-adres.

1.  Voer in het zoekvak **openbaar IP-adres**.

    ![Lijst met openbare IP-adressen](./media/oracle-backup-recovery/create_ip_00.png)

2.  Op de **openbare IP-adressen** blade, klikt u op **toevoegen**. Op de **openbare IP-adres maken** blade voor **naam**, selecteert u het openbare IP-naam. Voor **Resourcegroep** selecteert u **Bestaande gebruiken**. Klik vervolgens op **Maken**.

    ![IP-adres maken](./media/oracle-backup-recovery/create_ip_01.png)

3.  Als u wilt het openbare IP-adres koppelen aan de netwerkinterface voor de virtuele machine, zoek en selecteer **myVMip**. Klik vervolgens op **koppelen**.

    ![IP-adres koppelen](./media/oracle-backup-recovery/create_ip_02.png)

4.  Voor **resourcetype**, selecteer **netwerkinterface**. Selecteer de netwerkinterface die wordt gebruikt door de myVM-exemplaar en klik vervolgens op **OK**.

    ![Resourcetype en NIC waarden selecteren](./media/oracle-backup-recovery/create_ip_03.png)

5.  Zoeken en openen van het exemplaar van myVM die is geïmplementeerd vanaf de portal. Het IP-adres dat is gekoppeld aan de virtuele machine wordt weergegeven op de myVM **overzicht** blade.

    ![IP-adreswaarde](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a>Stap 4: Verbinding maken met de virtuele machine

*   Voor verbinding met de virtuele machine, moet u het volgende script gebruiken:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a>Stap 5: Controleren of de database toegankelijk is
*   Als u wilt testen toegankelijkheid, moet u het volgende script gebruiken:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Als de database **opstarten** opdracht een fout wordt gegenereerd, waarnaar de database hersteld, Zie [stap 6: RMAN gebruiken om de database te herstellen](#step-6-optional-use-rman-to-recover-the-database).

### <a name="step-6-optional-use-rman-to-recover-the-database"></a>Stap 6: (Optioneel) gebruik RMAN om de database te herstellen
*   Als u wilt herstellen van de database, moet u het volgende script gebruiken:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

Het back-up en herstel van de Oracle Database 12c-database op een Azure Linux-VM is nu voltooid.

## <a name="delete-the-vm"></a>De VM verwijderen

Wanneer u de virtuele machine niet meer nodig hebt, kunt u de volgende opdracht uit de resourcegroep, de virtuele machine en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Een maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)

[Azure CLI-voorbeelden voor VM-implementatie verkennen](../../linux/cli-samples.md)



