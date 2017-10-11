---
title: Oracle ASM instellen op een virtuele machine van Azure Linux | Microsoft Docs
description: Snel gebruiksklaar Oracle ASM omhoog in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: 117212a2e7e3da7c3e249798eec804a652e0ef58
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Oracle ASM instellen op een virtuele machine van Azure Linux  

Virtuele machines van Azure bieden een volledig worden geconfigureerd en flexibele computeromgeving. Deze zelfstudie bevat informatie over basic virtuele machine van Azure-implementatie in combinatie met de installatie en configuratie van Oracle geautomatiseerde Storage Management (ASM).  Procedures voor:

> [!div class="checklist"]
> * Maken en verbinding maken met een Oracle-Database VM
> * Installeren en configureren van Oracle automatische opslagbeheer
> * Installeren en configureren van Oracle raster infrastructuur
> * De installatie van een Oracle-ASM initialiseren
> * Een beheerd door ASM Oracle-database maken


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="prepare-the-environment"></a>De omgeving voorbereiden

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Voor het maken van een resourcegroep gebruikt de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container in welke Azure resources worden geïmplementeerd en beheerd. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* regio.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>Een virtuele machine maken

Voor een virtuele machine op basis van de installatiekopie van het Oracle-Database maken en configureren voor het gebruik van Oracle ASM, gebruiken de [az vm maken](/cli/azure/vm#create) opdracht. 

Het volgende voorbeeld wordt een virtuele machine met de naam myVM een groot Standard_DS2_v2 met vier bijgesloten gegevensschijven van 50 GB. Als deze niet al bestaan op de standaardlocatie van de sleutel, maakt het ook SSH-sleutels.  Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

Nadat u de virtuele machine hebt gemaakt, ziet Azure CLI er ongeveer als volgt uitzien. Noteer de waarde voor `publicIpAddress`. U kunt dit adres gebruiken voor toegang tot de virtuele machine.

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Aanvullende instellingen wilt maken van een SSH-sessie met de virtuele machine en configureren, moet u de volgende opdracht gebruiken. Vervang de IP-adres met de `publicIpAddress` waarde voor uw virtuele machine.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM installeren

Voltooi de volgende stappen uit voor het installeren van Oracle ASM. 

Zie voor meer informatie over het installeren van Oracle ASM [Oracle ASMLib Downloads voor Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. U moet zich aanmelden als basis om door te gaan met de ASM-installatie:

   ```bash
   sudo su -
   ```
   
2. Voer deze aanvullende opdrachten om Oracle-ASM-onderdelen te installeren:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Controleer of de Oracle-ASM is geïnstalleerd:

   ```bash
   rpm -qa |grep oracleasm
   ```

    De uitvoer van deze opdracht moet de volgende onderdelen weergeven:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM vereist zijn voor specifieke gebruikers en rollen. De volgende opdrachten maken de vereiste gebruikersaccounts en groepen: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Controleer of de gebruikers en groepen correct zijn gemaakt:

   ```bash
   id grid
   ```

    De uitvoer van deze opdracht moet de volgende gebruikers en groepen weergeven:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Maak een map voor de gebruiker *raster* en wijzigt u de eigenaar:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle ASM instellen

Voor deze zelfstudie is het de standaardgebruiker *raster* en de standaardgroep is *asmadmin*. Zorg ervoor dat de *oracle* gebruiker maakt deel uit van de groep asmadmin. Als u de installatie van de Oracle-ASM instelt, moet u de volgende stappen uitvoeren:

1. Instellen van het stuurprogramma van de bibliotheek Oracle ASM omvat het definiëren van de standaardgebruiker (raster) en de standaardgroep (asmadmin), evenals het station te starten op opstarten configureren (Kies y) en om te zoeken naar schijven op opstarten (Kies y). U moet de vragen beantwoorden van de volgende opdracht:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   De uitvoer van deze opdracht ziet er ongeveer als volgt, stoppen met vraagt worden beantwoord.

    ```bash
   Configuring the Oracle ASM library driver.

   This will configure the on-boot properties of the Oracle ASM library
   driver. The following questions will determine whether the driver is
   loaded on boot and what permissions it will have. The current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user to own the driver interface []: grid
   Default group to own the driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. De schijfconfiguratie weergeven:
   ```bash
   cat /proc/partitions
   ```

   De uitvoer van deze opdracht zijn vergelijkbaar met de volgende lijst met beschikbare schijven

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Schijf formatteren */dev/sdc* door de volgende opdracht uit te voeren en de prompts met beantwoorden:
   - *n*voor nieuwe partitie
   - *p* voor primaire partitie
   - *1* om de eerste partitie te selecteren
   - Druk op `enter` voor de eerste cilinder standaard
   - Druk op `enter` voor de laatste cilinder standaard
   - Druk op *w* de wijzigingen naar de partitietabel schrijven  

   ```bash
   fdisk /dev/sdc
   ```
   
   Met behulp van de bovenstaande antwoorden, moet de uitvoer voor de opdracht fdisk eruitzien als in het volgende:

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide to write them.
   After that, of course, the previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   The device presents a logical sector size that is smaller than
   the physical sector size. Aligning to a physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off the mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   The partition table has been altered!

   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```

4. Herhaal de voorgaande opdracht fdisk voor `/dev/sdd`, `/dev/sde`, en `/dev/sdf`.

5. Controleer de schijfconfiguratie:

   ```bash
   cat /proc/partitions
   ```

   De uitvoer van de opdracht moet er als volgt uitzien:

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Controleer de status van de Oracle-ASM-service en start de service Oracle ASM:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   De uitvoer van de opdracht moet er als volgt uitzien:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing the Oracle ASMLib driver:                     [  OK  ]
   Scanning the system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Oracle ASM schijven maken:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   De uitvoer van de opdracht moet er als volgt uitzien:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Lijst met Oracle ASM schijven:

   ```bash
   service oracleasm listdisks
   ```   

   De uitvoer van de opdracht moet de volgende Oracle ASM schijven uit lijst:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. De wachtwoorden voor de basis-, oracle en raster gebruikers wijzigen. **Noteer deze nieuwe wachtwoorden** zoals u ze later tijdens de installatie worden gebruikt.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. De machtiging map wijzigen:

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Download en Oracle raster infrastructuur voorbereiden

Als u wilt downloaden en voorbereiden van de software Oracle raster infrastructuur, moet u de volgende stappen uitvoeren:

1. Downloaden van Oracle raster infrastructuur niet vanuit de [Oracle ASM-downloadpagina](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Onder het downloaden met de titel **Oracle-Database 12c versie 1 raster infrastructuur (12.1.0.2.0) voor Linux x86-64**, de twee ZIP-bestanden downloaden.

2. Nadat u het ZIP-bestanden op uw clientcomputer hebt gedownload, kunt u beveiligde kopie Protocol (SCP) Kopieer de bestanden naar uw virtuele machine:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH terug in uw Oracle-virtuele machine in Azure om het verplaatsen van het ZIP-bestanden in de / opt-map. Wijzig de eigenaar van de bestanden:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Pak de bestanden. (De Linux installeren pak hulpprogramma als deze nog niet is geïnstalleerd.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. De machtiging wijzigen:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Update geconfigureerd wisselruimte. Oracle raster onderdelen moeten ten minste 6,8 GB wisselruimte voor het installeren van raster. Standaardgrootte van het wisselbestand voor Oracle Linux afbeeldingen in Azure is alleen 2048MB. U moet verhogen om `ResourceDisk.SwapSizeMB` in de `/etc/waagent.conf` -bestand en de WALinuxAgent-service opnieuw starten om de bijgewerkte instellingen kracht te laten worden. Omdat het een alleen-lezen bestand, die u wilt wijzigen bestandsmachtigingen om in te schakelen voor toegang voor schrijven.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Zoeken naar `ResourceDisk.SwapSizeMB` en wijzig de waarde in **8192**. Moet u op `insert` invoegmodus, typt u in de waarde van **8192** en druk vervolgens op `esc` om terug te keren naar de opdrachtmodus. Voor het schrijven van de wijzigingen en sluit het bestand, typ `:wq` en druk op `enter`.
   
   > [!NOTE]
   > We raden dat u altijd gebruiken `WALinuxAgent` wisselruimte configureren zodat deze altijd wordt gemaakt op de lokale tijdelijke schijf (tijdelijke schijf) voor de beste prestaties. Zie voor meer informatie over [toevoegen van een wisselbestand in Linux Azure virtual machines](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-to-run-x11"></a>Voorbereiden van uw lokale client en de virtuele machine voor het uitvoeren van x11
Oracle ASM configureert, dient een grafische interface voor het voltooien van de installatie en configuratie. We gebruiken de x11 protocol om deze installatie. Als u een clientsysteem (Mac of Linux) dat al X11 is mogelijkheden ingeschakeld en geconfigureerd - kunt u deze configuratie en installatie exclusieve overslaan voor Windows-computers. 

1. [Download PuTTY](http://www.putty.org/) en [Xming downloaden](https://xming.en.softonic.com/) naar uw Windows-computer. U moet de installatie van beide toepassingen met de standaardwaarden voordat u doorgaat.

2. Nadat u PuTTY hebt geïnstalleerd, open een opdrachtprompt, wijzigen in de PuTTY-map (bijvoorbeeld C:\Program Files\PuTTY) en voer `puttygen.exe` om een sleutel te genereren.

3. In de PuTTY codegenerator:
   
   1. Een sleutel te genereren door de `Generate` knop.
   2. Kopieer de inhoud van de sleutel (Ctrl + C).
   3. Selecteer de `Save private key` knop.
   4. Negeer de waarschuwing over het beveiligen van de sleutel met een wachtwoordzin in en selecteer vervolgens `OK`.

   ![Schermopname van PuTTY codegenerator](./media/oracle-asm/puttykeygen.png)

4. In de virtuele machine, moet u deze opdrachten uitvoeren:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Maak een bestand met de naam `authorized_keys`. Plak de inhoud van de sleutel in dit bestand en sla het bestand.

   > [!NOTE]
   > De sleutel moet de tekenreeks bevatten `ssh-rsa`. De inhoud van de sleutel moet ook een enkele regel tekst.
   >  

6. Start op uw clientsysteem PuTTY. In de **categorie** deelvenster, gaat u naar **verbinding** > **SSH** > **Auth**. In de **persoonlijke sleutelbestand voor verificatie** vak, blader naar de sleutel die u eerder hebt gegenereerd.

   ![Schermafbeelding van de opties voor SSH-verificatie](./media/oracle-asm/setprivatekey.png)

7. In de **categorie** deelvenster, gaat u naar **verbinding** > **SSH** > **X11**. Selecteer de **inschakelen X11 doorsturen** selectievakje.

   ![Schermafbeelding van de SSH-X11 opties doorsturen](./media/oracle-asm/enablex11.png)

8. In de **categorie** deelvenster, gaat u naar **sessie**. Voer uw Oracle ASM VM `<publicIPaddress>` in het dialoogvenster host vult u een nieuwe `Saved Session` name op en klik vervolgens op `Save`.  Wanneer opgeslagen, klik op `open` verbinding maken met de Oracle-ASM virtuele machine.  De eerste keer dat u verbinding maakt u een waarschuwing dat het externe systeem is niet in de cache opgeslagen in het register. Klik op `yes` toe te voegen en doorgaan.

   ![Schermafbeelding van de PuTTY-sessie-opties](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle raster infrastructuur installeren

Als u wilt installeren Oracle raster infrastructuur, moet u de volgende stappen uitvoeren:

1. Meld u aan als **raster**. (U moet zich aanmelden zonder te worden gevraagd om een wachtwoord.) 

   > [!NOTE]
   > Als u Windows uitvoert, zorg er dan voor dat u hebt Xming gestart voordat u de installatie begint.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle raster infrastructuur 12c versie 1 installatieprogramma wordt geopend. (Het kan enkele minuten duren voordat het installatieprogramma starten.)

2. Op de **installatieoptie selecteren** pagina **installeren en configureren van Oracle raster infrastructuur voor een zelfstandige Server**.

   ![Schermafbeelding van pagina met het installatieprogramma de installatie-optie selecteren](./media/oracle-asm/install01.png)

3. Op de **Product talen selecteren** pagina, controleert u **Engels** of de taal die u wilt dat is geselecteerd.  Klik op `next`.

4. Op de **ASM schijfgroep maken** pagina:
   - Voer een naam voor de schijfgroep op.
   - Onder **redundantie**, selecteer **externe**.
   - Onder **clustergrootte**, selecteer **4**.
   - Onder **schijven toevoegen**, selecteer **ORCLASMSP**.
   - Klik op `next`.

5. Op de **ASM wachtwoord opgeven** pagina de **dezelfde wachtwoorden gebruiken voor deze accounts** optie en voer een wachtwoord.

   ![Schermafbeelding van pagina met het installatieprogramma ASM wachtwoord opgeven](./media/oracle-asm/install04.png)

6. Op de **beheeropties opgeven** pagina, hebt u de optie voor het configureren van EM Cloud-besturingselement. Deze optie wordt overgeslagen: klik `next` om door te gaan. 

7. Op de **bevoorrechte besturingssysteem** pagina, gebruikt u de standaardinstellingen. Klik op `next` om door te gaan.

8. Op de **installatielocatie opgeven** pagina, gebruikt u de standaardinstellingen. Klik op `next` om door te gaan.

9. Op de **inventaris maken** pagina, wijzig de map-inventarisatie in `/u01/app/grid/oraInventory`. Klik op `next` om door te gaan.

   ![Schermafbeelding van pagina met het installatieprogramma inventaris maken](./media/oracle-asm/install08.png)

10. Op de **hoofdmap script uitvoering configuratie** pagina de **automatisch uitgevoerd configuratiescripts** selectievakje. Selecteer de **'root' gebruikersreferenties gebruiken** optie en voer het hoofdwachtwoord van de gebruiker.

    ![Schermafbeelding van de configuratiepagina voor het installatieprogramma basis-script uitvoeren](./media/oracle-asm/install09.png)

11. Op de **vereiste controles uitvoeren** pagina, de huidige installatie mislukt met fouten. Dit is verwacht gedrag. Selecteer `Fix & Check Again`.

12. In de **reparatie Script** in het dialoogvenster, klikt u op `OK`.

13. Op de **samenvatting** pagina, Controleer de geselecteerde instellingen en klik vervolgens op `Install`.

    ![Schermafbeelding van overzichtspagina van het installatieprogramma](./media/oracle-asm/install12.png)

14. Een waarschuwingsdialoogvenster wordt geïnformeerd u configuratie scripts moeten worden uitgevoerd als een bevoegde gebruiker weergegeven. Klik op `Yes` om door te gaan.

15. Op de **voltooien** pagina, klikt u op `Close` om de installatie te voltooien.

## <a name="set-up-your-oracle-asm-installation"></a>Instellen van de Oracle-ASM-installatie

Als u de installatie van de Oracle-ASM instelt, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat u nog steeds bent aangemeld als **raster**, van uw X11 sessie. Mogelijk moet u bereikt `enter` naar schud de terminal. Start vervolgens de Oracle geautomatiseerde Storage Management Configuration-assistent:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM configuratie-assistent wordt geopend.

2. In de **ASM configureren: schijfgroepen** in het dialoogvenster, klikt u op de `Create` knop en klik vervolgens op `Show Advanced Options`.

3. In de **schijfgroep maken** in het dialoogvenster:

   - Voer de naam van de schijf **gegevens**.
   - Onder **lidschijven selecteren**, selecteer **ORCL_DATA** en **ORCL_DATA1**.
   - Onder **clustergrootte**, selecteer **4**.
   - Klik op `ok` om de schijfgroep te maken.
   - Klik op `ok` om de bevestigingsvenster te sluiten.

   ![Schermopname van het dialoogvenster schijfgroep maken](./media/oracle-asm/asm02.png)

4. In de **ASM configureren: schijfgroepen** in het dialoogvenster, klikt u op de `Create` knop en klik vervolgens op `Show Advanced Options`.

5. In de **schijfgroep maken** in het dialoogvenster:

   - Voer de naam van de schijf **FRA**.
   - Onder **redundantie**, selecteer **External (geen)**.
   - Onder **lidschijven selecteren**, selecteer **ORCL_FRA**.
   - Onder **clustergrootte**, selecteer **4**.
   - Klik op `ok` om de schijfgroep te maken.
   - Klik op `ok` om de bevestigingsvenster te sluiten.

   ![Schermopname van het dialoogvenster schijfgroep maken](./media/oracle-asm/asm04.png)

6. Selecteer **afsluiten** ASM configuratie-assistent sluiten.

   ![Schermafbeelding van de ASM configureren: schijfgroepen-dialoogvenster met de knop Afsluiten](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>De database maken

De software van Oracle-database is al geïnstalleerd op de Azure Marketplace-installatiekopie. Als u wilt een database maakt, moet u de volgende stappen uitvoeren:

1. Gebruikers overschakelen naar de Oracle-beheerder en vervolgens de listener voor logboekregistratie te initialiseren:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Configuratie-assistent database wordt geopend.

2. Op de **databasebewerking** pagina, klikt u op `Create Database`.

3. Op de **aanmaakmodus** pagina:

   - Voer een naam voor de database.
   - Voor **opslagtype**, zorg ervoor dat **automatische Storage Management (ASM)** is geselecteerd.
   - Voor **bestanden databaselocatie**, gebruikt u de standaard ASM voorgestelde locatie.
   - Voor **snel herstel gebied**, gebruikt u de standaard ASM voorgestelde locatie.
   - Typ in een **beheerderswachtwoord** en **wachtwoord bevestigen**.
   - Zorg ervoor dat `create as container database` is geselecteerd.
   - Typ in een `pluggable database name` waarde.

4. Op de **samenvatting** pagina, Controleer de geselecteerde instellingen en klik vervolgens op `Finish` om de database te maken.

   ![Schermafbeelding van de pagina overzicht](./media/oracle-asm/createdb03.png)

5. De Database is gemaakt. Op de **voltooien** pagina, hebt u de optie voor het ontgrendelen van extra accounts voor deze database en de wachtwoorden wijzigen. Als u doen wilt, selecteert u **wachtwoordbeheer** -anders klikt u op `close`.

## <a name="delete-the-vm"></a>De virtuele machine verwijderen

U hebt Oracle automatisch beheer van opslag geconfigureerd op de installatiekopie Oracle DB vanuit Azure Marketplace.  Wanneer u deze virtuele machine niet meer nodig hebt, kunt u de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Oracle DataGuard configureren](configure-oracle-dataguard.md)

[Zelfstudie: Oracle GoldenGate configureren](Configure-oracle-golden-gate.md)

Bekijk [bouwen van een Oracle DB](oracle-design.md)
