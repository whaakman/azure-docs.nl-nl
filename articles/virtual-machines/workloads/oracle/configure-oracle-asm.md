---
title: Oracle Asm installeren op een Azure Linux-machine instellen | Microsoft Docs
description: Snel gebruiksklaar Oracle Asm installeren om in uw Azure-omgeving.
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
ms.openlocfilehash: 236809336975eec94d7decd9822fc9143ae19bfb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981015"
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Oracle ASM installeren op een virtuele Azure Linux-machine  

Virtuele machines in Azure bieden een volledig geconfigureerde en flexibele computeromgeving. In deze zelfstudie bevat informatie over de implementatie van virtuele Azure-machines in combinatie met de installatie en configuratie van Oracle geautomatiseerde Storage Management (ASM).  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maken en verbinding maken met een Oracle-Database-VM
> * Installeren en configureren van Oracle geautomatiseerd beheer van opslag
> * Installeren en configureren van de infrastructuur voor Oracle-raster
> * Een Oracle ASM-installatie initialiseren
> * Maak een Oracle DB beheerd door ASM


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="prepare-the-environment"></a>De omgeving voorbereiden

### <a name="create-a-resource-group"></a>Een resourcegroep maken

U kunt een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure resources worden geïmplementeerd en beheerd. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* regio.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine op basis van de installatiekopie met Oracle-Database maken en deze voor het gebruik van Oracle ASM configureren, gebruikt u de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. 

Het volgende voorbeeld wordt een virtuele machine met de naam myVM die een groot Standard_DS2_v2 met vier gekoppelde gegevensschijven van 50 GB. Als ze nog niet bestaan in de standaardsleutellocatie, maakt het ook SSH-sleutels.  Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

Nadat u de virtuele machine maakt, geeft Azure CLI informatie die vergelijkbaar is met het volgende voorbeeld. Noteer de waarde voor `publicIpAddress`. U gebruikt dit adres voor toegang tot de virtuele machine.

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

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de virtuele machine en aanvullende instellingen configureren. Vervang het IP-adres met de `publicIpAddress` waarde voor uw virtuele machine.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM installeren

Oracle ASM installeren, de volgende stappen. 

Zie voor meer informatie over het installeren van Oracle ASM [Oracle ASMLib Downloads voor Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. U moet zich aanmelden als basis om door te gaan met de ASM-installatie:

   ```bash
   sudo su -
   ```
   
2. Voer deze aanvullende opdrachten om Oracle ASM-onderdelen te installeren:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Controleer of Oracle ASM is geïnstalleerd:

   ```bash
   rpm -qa |grep oracleasm
   ```

    De uitvoer van deze opdracht moet bevatten de volgende onderdelen:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM vereist specifieke gebruikers en rollen om te kunnen laten functioneren. De volgende opdrachten maakt de vereiste gebruikersaccounts en groepen: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Controleer of gebruikers en groepen correct zijn gemaakt:

   ```bash
   id grid
   ```

    De uitvoer van deze opdracht moet een lijst met de volgende gebruikers en groepen:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Maak een map voor de gebruiker *raster* en de eigenaar niet wijzigen:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle Asm installeren

Voor deze zelfstudie de standaardwaarde is *raster* en de standaard-groep is *asmadmin*. Zorg ervoor dat de *oracle* gebruiker deel uitmaakt van de groep asmadmin. Als u de installatie van de Oracle ASM instelt, moet u de volgende stappen uitvoeren:

1. Instellen van het stuurprogramma van Oracle ASM-bibliotheek omvat het definiëren van de standaardgebruiker (raster) en de standaardgroep (asmadmin), evenals het configureren van het station om te beginnen bij het opstarten (Kies y) en om te zoeken naar schijven op opstarten (y te kiezen). U moet de hierom wordt gevraagd van de volgende opdracht uit:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   De uitvoer van deze opdracht ziet er vergelijkbaar met het volgende moet worden gestopt met vraagt moeten worden beantwoord.

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

2. De schijfconfiguratie van de weergeven:
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

3. Schijf formatteren */dev/sdc* door te voeren van de volgende opdracht en beantwoord de prompts met:
   - *n* voor nieuwe partitie
   - *p* voor primaire partitie
   - *1* om de eerste partitie te selecteren
   - Druk op `enter` voor de standaard eerste 3D-cilinder
   - Druk op `enter` voor de standaard laatste 3D-cilinder
   - Druk op *w* de wijzigingen naar de partitietabel schrijven  

   ```bash
   fdisk /dev/sdc
   ```
   
   Met behulp van de antwoorden die hierboven, uitzien de uitvoer voor de opdracht fdisk als volgt:

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

   De uitvoer van de opdracht ziet er als volgt uit:

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

6. Controleer de status van de Oracle ASM-service en start de service Oracle Asm installeren:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   De uitvoer van de opdracht ziet er als volgt uit:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing the Oracle ASMLib driver:                     [  OK  ]
   Scanning the system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Oracle ASM-schijven maken:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   De uitvoer van de opdracht ziet er als volgt uit:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Oracle ASM-schijven vermelden:

   ```bash
   service oracleasm listdisks
   ```   

   De uitvoer van de opdracht moet uit de volgende Oracle ASM-schijven vermelden:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. De wachtwoorden voor de basis-, oracle en raster gebruikers wijzigen. **Maak notitie van deze nieuwe wachtwoorden** als u deze later tijdens de installatie gebruikt.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Wijzig de map machtiging:

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

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Download en Oracle Grid infrastructuur voorbereiden

Om te downloaden en bereidt u de infrastructuur voor het raster van Oracle-software, voert u de volgende stappen uit:

1. Downloaden van Oracle Grid infrastructuur niet vanuit de [Oracle ASM-downloadpagina](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Bij het downloaden met de titel **Oracle Database 12c Release 1 Grid infrastructuur (12.1.0.2.0) voor Linux x86-64**, de twee ZIP-bestanden te downloaden.

2. Nadat u het ZIP-bestanden naar uw client-computer downloaden, kunt u Secure Copy Protocol (SCP) gebruiken de bestanden naar uw virtuele machine te kopiëren:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH in uw Oracle-VM in Azure om te verplaatsen van het ZIP-bestanden in de / opt-map. Wijzig de eigenaar van de bestanden:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Pak de bestanden. (Installatie van de Linux unzip hulpprogramma als deze nog niet geïnstalleerd.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. De machtiging wijzigen:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Update geconfigureerd wisselruimte. Oracle Grid onderdelen moeten ten minste 6,8 GB aan wisselruimte voor het installeren van de Grid. Standaardgrootte van het wisselbestand voor Oracle Linux-installatiekopieën in Azure is alleen 2048MB. U wilt verhogen `ResourceDisk.SwapSizeMB` in de `/etc/waagent.conf` -bestand en de WALinuxAgent-service opnieuw starten om de bijgewerkte instellingen toe te passen. Omdat het een alleen-lezen-bestand is, moet u bestandsmachtigingen om in te schakelen toegang voor schrijven wijzigen.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Zoeken naar `ResourceDisk.SwapSizeMB` en wijzig de waarde in **8192**. U moet druk op `insert` invoegmodus, typt u in de waarde van **8192** en druk vervolgens op `esc` om terug te keren naar de opdrachtmodus. Voor het schrijven van de wijzigingen en sluit het bestand, typ `:wq` en druk op `enter`.
   
   > [!NOTE]
   > Is het raadzaam dat u altijd gebruikt `WALinuxAgent` wisselruimte configureren zodat deze altijd op de lokale tijdelijke schijf (tijdelijke schijf) voor de beste prestaties wordt gemaakt. Zie voor meer informatie over [toevoegen van een wisselbestand in Linux Azure virtuele machines](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-to-run-x11"></a>Uw lokale client en de virtuele machine om uit te voeren x11 voorbereiden
Oracle ASM configureren, is een grafische interface voor het voltooien van de installatie en configuratie vereist. We gebruiken de x11 protocol om deze installatie mogelijk te maken. Als u een clientsysteem (Mac of Linux) die al X11 heeft mogelijkheden ingeschakeld en geconfigureerd - kunt u deze configuratie en installatie exclusieve verdergaan met Windows-machines. 

1. [Download PuTTY](http://www.putty.org/) en [Xming downloaden](https://xming.en.softonic.com/) op uw Windows-computer. U moet de installatie van beide van deze toepassingen met de standaardwaarden voordat u doorgaat.

2. Nadat u PuTTY, open een opdrachtprompt, in de PuTTY-map (bijvoorbeeld C:\Program Files\PuTTY) wijzigen en uitvoeren `puttygen.exe` om een sleutel te genereren.

3. In de PuTTY Serversleutelgenerator:
   
   1. Genereer een sleutel door het selecteren van de `Generate` knop.
   2. Kopieer de inhoud van de sleutel (Ctrl + C).
   3. Selecteer de knop `Save private key`.
   4. Negeer de waarschuwing over het beveiligen van de sleutel met een wachtwoordzin, en selecteer vervolgens `OK`.

   ![Schermafbeelding van PuTTY Serversleutelgenerator](./media/oracle-asm/puttykeygen.png)

4. Voer deze opdrachten uit in uw VM:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Maak een bestand met de naam `authorized_keys`. Plak de inhoud van de sleutel in dit bestand en sla het bestand.

   > [!NOTE]
   > De sleutel moet de tekenreeks bevatten `ssh-rsa`. De inhoud van de sleutel moet bovendien één regel tekst.
   >  

6. Start op het clientsysteem PuTTY. In de **categorie** deelvenster, Ga naar **verbinding** > **SSH** > **Auth**. In de **bestand met persoonlijke sleutel voor verificatie** vak, blader naar de sleutel die u eerder hebt gegenereerd.

   ![Schermafbeelding van de SSH-verificatie-opties](./media/oracle-asm/setprivatekey.png)

7. In de **categorie** deelvenster, Ga naar **verbinding** > **SSH** > **X11**. Selecteer de **inschakelen X11 doorsturen** selectievakje.

   ![Schermafbeelding van de SSH-X11 opties doorsturen](./media/oracle-asm/enablex11.png)

8. In de **categorie** deelvenster, Ga naar **sessie**. Voer uw Oracle ASM VM `<publicIPaddress>` Vul in het dialoogvenster de naam van de host in een nieuwe `Saved Session` een naam en klik vervolgens op `Save`.  Als u hebt opgeslagen, klikt u op `open` verbinding maken met uw Oracle ASM virtuele machine.  De eerste keer dat u verbinding maakt u een waarschuwing dat het externe systeem is niet in de cache opgeslagen in het register. Klik op `yes` toe te voegen en te blijven.

   ![Schermafbeelding van de PuTTY-sessie-opties](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle Grid infrastructuur installeren

Voor het installeren van Oracle Grid infrastructuur, voert u de volgende stappen uit:

1. Meld u als **raster**. (U moet aanmelden zonder een wachtwoord wordt gevraagd.) 

   > [!NOTE]
   > Als u Windows uitvoert, zorg er dan voor dat u hebt Xming gestart voordat u begint met de installatie.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle Grid infrastructuur 12c Release 1 installatieprogramma wordt geopend. (Het duurt een paar minuten voor het installatieprogramma te starten.)

2. Op de **installatie-optie selecteren** pagina, selecteert u **installeren en configureren van Oracle Grid infrastructuur voor een zelfstandige Server**.

   ![Schermafbeelding van pagina van het installatieprogramma met installatie-optie selecteren](./media/oracle-asm/install01.png)

3. Op de **Product talen selecteren** pagina, controleert u of **Engels** of de taal die u wilt dat is geselecteerd.  Klik op `next`.

4. Op de **ASM-schijfgroep maken** pagina:
   - Voer een naam voor de schijfgroep.
   - Onder **redundantie**, selecteer **externe**.
   - Onder **clustergrootte**, selecteer **4**.
   - Onder **schijven toevoegen**, selecteer **ORCLASMSP**.
   - Klik op `next`.

5. Op de **ASM-wachtwoord opgeven** weergeeft, schakelt de **dezelfde wachtwoorden gebruiken voor deze accounts** optie en voer een wachtwoord.

   ![Schermafbeelding van pagina van de ASM-wachtwoord opgeven van het installatieprogramma](./media/oracle-asm/install04.png)

6. Op de **beheeropties opgeven** pagina, hebt u de optie voor het beheer van EM Cloud configureren. Deze optie wordt overgeslagen, klikt u `next` om door te gaan. 

7. Op de **bevoorrechte besturingssysteem** pagina, gebruikt u de standaardinstellingen. Klik op `next` om door te gaan.

8. Op de **installatielocatie opgeven** pagina, gebruikt u de standaardinstellingen. Klik op `next` om door te gaan.

9. Op de **inventaris maken** pagina, wijzig de map-inventarisatie in `/u01/app/grid/oraInventory`. Klik op `next` om door te gaan.

   ![Schermafbeelding van pagina van de voorraad maken van het installatieprogramma](./media/oracle-asm/install08.png)

10. Op de **hoofdmap script uitvoering configuratie** weergeeft, schakelt de **automatisch uitgevoerd configuratiescripts** selectievakje. Selecteer vervolgens de **"root" gebruikersreferenties gebruiken** optie en voer het hoofdwachtwoord van de gebruiker.

    ![Schermafbeelding van pagina met van het installatieprogramma hoofdmap script uitvoering configuratie](./media/oracle-asm/install09.png)

11. Op de **Vereistencontroles uitgevoerd** pagina, de huidige installatie mislukt met fouten. Dit is verwacht gedrag. Selecteer `Fix & Check Again`.

12. In de **correctie Script** in het dialoogvenster, klikt u op `OK`.

13. Op de **samenvatting** pagina, Controleer de geselecteerde instellingen en klik vervolgens op `Install`.

    ![Schermafbeelding van overzichtspagina van het installatieprogramma](./media/oracle-asm/install12.png)

14. Een dialoogvenster weergegeven waarin u configuration scripts moeten worden uitgevoerd als een bevoegde gebruiker. Klik op `Yes` om door te gaan.

15. Op de **voltooien** pagina, klikt u op `Close` om de installatie te voltooien.

## <a name="set-up-your-oracle-asm-installation"></a>Instellen van de installatie van de Oracle Asm installeren

Als u de installatie van de Oracle ASM instelt, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat u nog steeds bent aangemeld als **raster**, van uw X11 sessie. U moet mogelijk bereikt `enter` naar de terminal terughalen. Start vervolgens de Oracle geautomatiseerde Storage Management-configuratie-assistent:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM-configuratie-assistent wordt geopend.

2. In de **ASM configureren: schijfgroepen** in het dialoogvenster, klikt u op de `Create` knop en klik vervolgens op `Show Advanced Options`.

3. In de **schijfgroep maken** in het dialoogvenster:

   - Voer de naam van de schijf **gegevens**.
   - Onder **lidschijven selecteren**, selecteer **ORCL_DATA** en **ORCL_DATA1**.
   - Onder **clustergrootte**, selecteer **4**.
   - Klik op `ok` om de schijfgroep te maken.
   - Klik op `ok` om de bevestigingsvenster te sluiten.

   ![Schermopname van het dialoogvenster groep maken](./media/oracle-asm/asm02.png)

4. In de **ASM configureren: schijfgroepen** in het dialoogvenster, klikt u op de `Create` knop en klik vervolgens op `Show Advanced Options`.

5. In de **schijfgroep maken** in het dialoogvenster:

   - Voer de naam van de schijf **FRA**.
   - Onder **redundantie**, selecteer **extern (geen)**.
   - Onder **lidschijven selecteren**, selecteer **ORCL_FRA**.
   - Onder **clustergrootte**, selecteer **4**.
   - Klik op `ok` om de schijfgroep te maken.
   - Klik op `ok` om de bevestigingsvenster te sluiten.

   ![Schermopname van het dialoogvenster groep maken](./media/oracle-asm/asm04.png)

6. Selecteer **afsluiten** sluiten ASM-configuratie-assistent.

   ![Schermafbeelding van de ASM configureren: in het dialoogvenster schijfgroepen met knop Afsluiten](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>De database maken

Het Oracle database-software is al geïnstalleerd op de Azure Marketplace-installatiekopie. Voor het maken van een database, voert u de volgende stappen uit:

1. Schakelen tussen gebruikers op de Oracle-beheerder, en deze vervolgens initialiseren van de listener voor logboekregistratie:

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
   - Voor **locatie van databasebestanden**, gebruikt u de standaard ASM voorgesteld locatie.
   - Voor **snel herstel gebied**, gebruikt u de standaard ASM voorgesteld locatie.
   - Typ in een **beheerderswachtwoord** en **wachtwoord bevestigen**.
   - Zorg ervoor dat `create as container database` is geselecteerd.
   - Typ in een `pluggable database name` waarde.

4. Op de **samenvatting** pagina, Controleer de geselecteerde instellingen en klik vervolgens op `Finish` om de database te maken.

   ![Schermafbeelding van de pagina overzicht](./media/oracle-asm/createdb03.png)

5. De Database is gemaakt. Op de **voltooien** pagina, hebt u de optie voor het ontgrendelen van extra accounts voor het gebruik van deze database en de wachtwoorden wijzigen. Als u doen wilt, selecteert u **wachtwoordbeheer** -anders klikt u op `close`.

## <a name="delete-the-vm"></a>De VM verwijderen

U hebt Oracle geautomatiseerd beheer van opslag geconfigureerd op de installatiekopie van het Oracle DB van de Azure Marketplace.  Wanneer u deze virtuele machine niet meer nodig hebt, kunt u de volgende opdracht uit de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Oracle DataGuard configureren](configure-oracle-dataguard.md)

[Zelfstudie: Oracle GoldenGate configureren](Configure-oracle-golden-gate.md)

Beoordeling [een Oracle-database ontwerpen](oracle-design.md)
