---
title: MPIO op StorSimple Linux-host configureren | Microsoft Docs
description: MPIO configureren op verbonden met een Linux-host CentOS 6.6 met StorSimple
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: add539351066f9ff94febeebfd5334773b360e8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>MPIO configureren op een StorSimple-host waarop van CentOS
Dit artikel wordt uitgelegd hoe u Multipath I/O (MPIO) configureren op de hostserver Centos 6.6. De host-server is verbonden met uw Microsoft Azure StorSimple-apparaat voor hoge beschikbaarheid via iSCSI-initiators. Deze beschrijving gedetailleerde van de automatische detectie van multipath-apparaten en de specifieke instellingen alleen van StorSimple-volumes.

Deze procedure is van toepassing op alle modellen van apparaten voor StorSimple 8000-serie.

> [!NOTE]
> Deze procedure kan niet worden gebruikt voor een virtueel StorSimple-apparaat. Zie voor meer informatie het configureren van hostservers voor uw virtuele apparaat.
> 
> 

## <a name="about-multipathing"></a>Over het gebruik van meerdere paden
De functie multipath kunt u meerdere i/o-paden tussen een hostserver en een opslagapparaat configureren. Deze i/o-paden zijn fysieke SAN-verbindingen die afzonderlijke kabels, switches, netwerkinterfaces en domeincontrollers kunnen opnemen. Gebruik van meerdere paden cumuleert de i/o-paden voor het configureren van een nieuw apparaat dat is gekoppeld aan alle geaggregeerde paden.

Het doel van het gebruik van meerdere paden is tweeledig:

* **Hoge beschikbaarheid**: biedt een alternatief pad als elk element van het i/o-pad (zoals een kabel, switch, netwerkinterface of domeincontroller) is mislukt.
* **Taakverdeling**: afhankelijk van de configuratie van uw opslagapparaat, kan deze de prestaties verbeteren door de belasting op de i/o-paden te detecteren en dynamisch herverdeling de belasting.

### <a name="about-multipathing-components"></a>Over het gebruik van meerdere paden onderdelen
Gebruik van meerdere paden in Linux bestaat uit de kernel als gebruikersruimte componenten zoals hieronder in een tabel.

* **Kernel**: het belangrijkste onderdeel is het *apparaat mapper* die wordt omgeleid i/o en failover voor paden en pad groepen ondersteunt.

* **Gebruikersruimte innemen**: dit zijn *multipath-tools* die multipathed apparaten beheren door de multipath apparaat mapper-module wat te doen. De hulpprogramma's bestaan uit:
   
   * **Multipath**: geeft een lijst van en multipathed apparaten configureert.
   * **Multipathd**: daemon die wordt uitgevoerd multipath en bewaakt de paden.
   * **Naam van de Devmap**: biedt een zinvolle apparaatnaam naar udev voor devmaps.
   * **Kpartx**: lineaire devmaps wordt toegewezen aan de partities die apparaten kunnen multipath maps partitioneerbare.
   * **Multipath.conf**: configuratiebestand voor multipath-daemon die wordt gebruikt voor de configuratietabel van de ingebouwde overschreven.

### <a name="about-the-multipathconf-configuration-file"></a>Over het configuratiebestand multipath.conf
Het configuratiebestand `/etc/multipath.conf` kunt u veel van de functies van het gebruik van meerdere paden gebruiker worden geconfigureerd. De `multipath` opdracht en de kernel-daemon `multipathd` gebruik informatie gevonden in dit bestand. Het bestand is alleen tijdens de configuratie van de apparaten multipath geraadpleegd. Zorg ervoor dat alle wijzigingen worden aangebracht voordat u de `multipath` opdracht. Als u het bestand later wijzigt, moet u stoppen en starten multipathd opnieuw om de wijzigingen van kracht te laten worden.

De multipath.conf heeft vijf secties:

- **Niveau standaardsysteemwaarden** *(standaardwaarden)*: U kunt niveau standaardwaarden van het systeem onderdrukken.
- **Apparaten gebeurd** *(blacklist)*: U kunt de lijst met apparaten die niet moeten worden beheerd door apparaat mapper opgeven.
- **Uitzonderingen afgekeurde** *(blacklist_exceptions)*: U specifieke apparaten moet worden behandeld als multipath apparaten, zelfs als die worden vermeld in de zwarte lijst kunt identificeren.
- **Specifieke instellingen voor de controller opslag** *(apparaten)*: U kunt configuratieinstellingen die worden toegepast op apparaten met de leverancier-en productinformatie opgeven.
- **Specifieke apparaatinstellingen** *(multipaths)*: U kunt in deze sectie verfijnen van de configuratie-instellingen voor afzonderlijke LUN's.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Gebruik van meerdere paden op StorSimple die zijn verbonden met het Linux-host configureren
Een StorSimple-apparaat is verbonden met een Linux-host kan worden geconfigureerd voor hoge beschikbaarheid en taakverdeling. Bijvoorbeeld, als de Linux-host twee interfaces zijn verbonden met het SAN heeft en het apparaat twee interfaces zijn verbonden met het SAN heeft, dat deze interfaces op hetzelfde subnet bevinden worden, wordt er 4 paden beschikbaar. Echter, als elke interface gegevens op het apparaat en host-interface op een ander IP-subnet (en niet-routeerbaar), vervolgens alleen 2 paden zijn beschikbaar. U kunt meerdere paden voor het automatisch detecteren van de beschikbare paden, kiest u een taakverdelingsalgoritme voor die paden, toepassen specifieke configuratie-instellingen voor alleen-StorSimple-volumes, en vervolgens inschakelen en controleren van meerdere paden configureren.

De volgende procedure beschrijft het gebruik van meerdere paden configureren wanneer een StorSimple-apparaat met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces.

## <a name="prerequisites"></a>Vereisten
Deze sectie beschrijft de configuratievereisten voor CentOS-server en het StorSimple-apparaat.

### <a name="on-centos-host"></a>Op de host CentOS
1. Zorg ervoor dat uw CentOS-host 2 netwerkinterfaces ingeschakeld heeft. Type:
   
    `ifconfig`
   
    Het volgende voorbeeld ziet u de uitvoer wanneer twee netwerkinterfaces (`eth0` en `eth1`) aanwezig zijn op de host.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Installeer *iSCSI-initiator-utils* op uw server CentOS. Voer de volgende stappen uit voor het installeren van *iSCSI-initiator-utils*.
   
   1. Meld u aan als `root` in uw CentOS-host.
   2. Installeer de *iSCSI-initiator-utils*. Type:
      
       `yum install iscsi-initiator-utils`
   3. Na de *iSCSI-Initiator-utils* met succes is geïnstalleerd, start de iSCSI-service. Type:
      
       `service iscsid start`
      
       In gevallen `iscsid` mogelijk niet daadwerkelijk wordt gestart en de `--force` optie zijn mogelijk vereist
   4. Om ervoor te zorgen dat uw iSCSI-initiator tijdens het opstarten is ingeschakeld, gebruikt u de `chkconfig` opdracht uit om de service.
      
       `chkconfig iscsi on`
   5. Om te verifiëren dat het goed is ingesteld, moet u de opdracht uitvoeren:
      
       `chkconfig --list | grep iscsi`
      
       Hieronder ziet u een voorbeeld van de uitvoer.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       In het bovenstaande voorbeeld ziet u dat uw omgeving iSCSI op opstarten op uitvoeren niveaus 2, 3, 4 en 5 wordt uitgevoerd.
3. Installeer *apparaat-mapper-multipath*. Type:
   
    `yum install device-mapper-multipath`
   
    De installatie wordt gestart. Type **Y** om door te gaan wanneer u om bevestiging wordt gevraagd.

### <a name="on-storsimple-device"></a>Op het StorSimple-apparaat
Uw StorSimple-apparaat moet hebben:

* Minimaal twee interfaces zijn ingeschakeld voor iSCSI. Om te controleren dat twee interfaces ingeschakeld voor iSCSI op uw StorSimple-apparaat zijn, moet u de volgende stappen uitvoeren in de klassieke Azure portal voor uw StorSimple-apparaat:
  
  1. Meld u aan bij de klassieke portal voor uw StorSimple-apparaat.
  2. Selecteer uw StorSimple Manager-service, klikt u op **apparaten** en kies de specifieke StorSimple-apparaat. Klik op **configureren** en controleer of de netwerkinterface-instellingen. Een schermafbeelding met twee netwerkinterfaces zijn ingeschakeld voor iSCSI-worden hieronder weergegeven. Hier DATA 2 en DATA 3, beide 10 GbE interfaces zijn ingeschakeld voor iSCSI.
     
      ![Configuratie van MPIO StorsSimple DATA 2](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorSimple gegevens 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      In de **configureren** pagina
     
     1. Zorg ervoor dat beide netwerkinterfaces iSCSI-functionaliteit. De **iSCSI ingeschakeld** veld moet worden ingesteld op **Ja**.
     2. Zorg dat de netwerkinterfaces hebben dezelfde snelheid, of beide moet 1 GbE- of 10 GbE.
     3. Noteer de IPv4-adressen van de interfaces iSCSI-functionaliteit en opslaan voor toekomstig gebruik op de host.
* De iSCSI-interfaces op uw StorSimple-apparaat moet bereikbaar is vanaf de CentOS-server.
      Om dit te controleren, moet u de IP-adressen van uw StorSimple iSCSI-functionaliteit netwerkinterfaces op uw host-server opgeven. De opdrachten die worden gebruikt en de bijbehorende uitvoer met DATA2 (10.126.162.25) en DATA3 (10.126.162.26) worden hieronder weergegeven:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Hardwareconfiguratie
Het is raadzaam dat u verbinding de twee iSCSI-netwerkinterfaces op afzonderlijke paden voor de redundantie maakt. De onderstaande afbeelding ziet u de aanbevolen hardwareconfiguratie voor hoge beschikbaarheid en taakverdeling gebruik van meerdere paden voor uw CentOS-server en het StorSimple-apparaat.

![MPIO hardwareconfiguratie voor StorSimple Linux-host](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Zoals u in de voorgaande afbeelding:

* Uw StorSimple-apparaat zich in een actief / passief-configuratie met twee domeincontrollers.
* Twee SAN-switches zijn verbonden met uw apparaat-domeincontrollers.
* Twee iSCSI-initiators zijn ingeschakeld op uw StorSimple-apparaat.
* Twee netwerkinterfaces zijn ingeschakeld op uw host CentOS.

De bovenstaande configuratie resulteert 4 afzonderlijke paden tussen het apparaat en de host als de host- en interfaces routeerbaar zijn.

> [!IMPORTANT]
> * Het is raadzaam dat u niet door elkaar 1 GbE- en 10 GbE-netwerkinterfaces voor gebruik van meerdere paden. Wanneer u twee netwerkinterfaces, moet de interfaces die zowel het identieke type.
> * Op uw StorSimple-apparaat DATA0, bestand1, DATA4 en DATA5 zijn 1 GbE-interfaces DATA2 en DATA3 10 GbE-netwerkinterfaces zijn. |
> 
> 

## <a name="configuration-steps"></a>Configuratiestappen
De configuratiestappen voor gebruik van meerdere paden betrekking hebben op voor het configureren van de beschikbare paden voor automatische detectie, geven de taakverdelingsalgoritme moet worden gebruikt, gebruik van meerdere paden inschakelen en ten slotte de configuratie te controleren. Elk van deze stappen wordt uitgebreid beschreven in de volgende secties.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Stap 1: Configureer meerdere paden voor automatische detectie
De multipath-ondersteunde apparaten kunnen automatisch worden gedetecteerd en geconfigureerd.

1. Initialiseren `/etc/multipath.conf` bestand. Type:
   
     `mpathconf --enable`
   
    De bovenstaande opdracht maakt een `sample/etc/multipath.conf` bestand.
2. Multipath-service starten. Type:
   
    `service multipathd start`
   
    Hier ziet u de volgende uitvoer:
   
    `Starting multipathd daemon:`
3. Automatische detectie van multipaths inschakelen. Type:
   
    `mpathconf --find_multipaths y`
   
    Dit wordt wijzigt u de sectie standaardinstellingen van uw `multipath.conf` zoals hieronder wordt weergegeven:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Stap 2: Configureer meerdere paden voor StorSimple-volumes
Standaard worden alle apparaten zwart zijn vermeld in het bestand multipath.conf en zal worden overgeslagen. U moet een zwarte lijst uitzonderingen zodat meerdere paden voor volumes van StorSimple-apparaten maken.

1. Bewerk de `/etc/mulitpath.conf` bestand. Type:
   
    `vi /etc/multipath.conf`
2. Zoek de blacklist_exceptions-sectie in het bestand multipath.conf. Uw StorSimple-apparaat moet worden weergegeven als een zwarte lijst uitzondering in deze sectie. U kunt opmerkingen bij relevante regels in dit bestand te wijzigen zoals hieronder (Gebruik alleen het specifieke model van het apparaat dat u gebruikt):
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Stap 3: Gebruik van meerdere paden round robin configureren
Deze load balancing-algoritme maakt gebruik van alle beschikbare multipaths met de actieve controller taakverdeling, round-robin toewijst.

1. Bewerk de `/etc/multipath.conf` bestand. Type:
   
    `vi /etc/multipath.conf`
2. Onder de `defaults` sectie, stelt u de `path_grouping_policy` naar `multibus`. De `path_grouping_policy` Hiermee geeft u het standaardpad beleid toepassen op niet-gespecificeerde multipaths groeperen. De sectie standaardwaarden wordt gezocht, zoals hieronder wordt weergegeven.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> De meest voorkomende waarden van `path_grouping_policy` omvatten:
> 
> * failover = 1 pad per groep prioriteit
> * multibus = alle geldige paden in de groep 1 prioriteit
> 
> 

### <a name="step-4-enable-multipathing"></a>Stap 4: Enable meerdere paden
1. Start opnieuw op de `multipathd` daemon. Type:
   
    `service multipathd restart`
2. De uitvoer zijn zoals hieronder wordt weergegeven:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>Stap 5: Controleren of gebruik van meerdere paden
1. Eerst voor zorgen dat dat iSCSI-verbinding tot stand is gebracht met de StorSimple-apparaat als volgt:
   
   a. Detecteren van uw StorSimple-apparaat. Type:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    De uitvoer wanneer IP-adres voor DATA0 10.126.162.25 en poort 3260 wordt geopend op het StorSimple-apparaat voor uitgaande iSCSI-verkeer is zoals hieronder wordt weergegeven:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Kopieer het IQN van uw StorSimple-apparaat `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, uit de bovenstaande uitvoer.

   b. Verbinding maken met het apparaat doel IQN gebruikt. Het StorSimple-apparaat is het iSCSI-doel. Type:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Het volgende voorbeeld ziet u uitvoer met een doel IQN van `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. De uitvoer geeft aan dat u hebt is verbonden met de twee netwerkinterfaces voor iSCSI-functionaliteit op uw apparaat.

    ```
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Als u slechts één host-interface en twee paden Hier ziet, moet u zowel de interfaces op de host voor iSCSI-inschakelen. U kunt volgen de [gedetailleerde instructies in de documentatie van Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. Een volume wordt blootgesteld aan de server CentOS van het StorSimple-apparaat. Zie voor meer informatie [stap 6: een volume maken](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via de klassieke Azure portal op uw StorSimple-apparaat.

3. Controleer of de beschikbare paden. Type:

      ```
      multipath –l
      ```

      Het volgende voorbeeld ziet u de uitvoer voor twee netwerkinterfaces op een StorSimple-apparaat is verbonden met een netwerkinterface één host met twee beschikbare paden.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        The following example shows the output for two network interfaces on a StorSimple device connected to two host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After the paths are configured, refer to the specific instructions on your host operating system (Centos 6.6) to mount and format this volume.

## <a name="troubleshoot-multipathing"></a>Problemen met meerdere paden
Deze sectie bevat enkele nuttige tips als u problemen ondervindt tijdens het gebruik van meerdere paden configureren uitvoert.

Q. Zie ik niet de wijzigingen in `multipath.conf` bestand die van kracht.

A. Als u aangebrachte wijzigingen in de `multipath.conf` -bestand, moet u het gebruik van meerdere paden-service opnieuw starten. Typ de volgende opdracht:

    service multipathd restart

Q. Ik heb twee netwerkinterfaces op de StorSimple-apparaat en twee netwerkinterfaces op de host ingeschakeld. Wanneer ik de beschikbare paden, zie ik slechts twee paden. Ik verwachtte vier beschikbare paden.

A. Zorg ervoor dat de twee paden zich op hetzelfde subnet en routeerbaar. Als de netwerkinterfaces zich op verschillende VLAN's en niet routeerbaar te maken, u slechts twee paden ziet. Er is een manier om dit te controleren om ervoor te zorgen dat u zowel de hostinterfaces van de netwerkinterface op het StorSimple-apparaat kunt bereiken. U moet [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) zoals deze verificatie kan alleen worden uitgevoerd via een ondersteuningssessie voor.

Q. Wanneer ik de lijst beschikbare paden, zie ik niet geen uitvoer.

A. Normaal gesproken niet ziet multipathed paden stelt een probleem met het gebruik van meerdere paden daemon en het is zeer waarschijnlijk dat er probleem hier ligt in de `multipath.conf` bestand.

Het is ook waard controleren dat u daadwerkelijk een aantal schijven zien kunt nadat u het doel als er geen reactie van de lijsten van multipath kan ook betekenen er geen schijven.

* Gebruik de volgende opdracht om te scannen van de SCSI-bus:
  
    `$ rescan-scsi-bus.sh `(onderdeel van het pakket sg3_utils)
* Typ de volgende opdrachten:
  
    `$ dmesg | grep sd*`
     
     of
  
    `$ fdisk –l`
  
    Deze wordt details van onlangs toegevoegde schijven geretourneerd.
* Om te bepalen of het een StorSimple-schijf is, gebruik de volgende opdrachten:
  
    `cat /sys/block/<DISK>/device/model`
  
    Hiermee wordt een tekenreeks, die bepaalt of het een StorSimple-schijf is geretourneerd.

Een minder waarschijnlijk maar mogelijke oorzaak kan ook worden de verlopen iscsid pid. Gebruik de volgende opdracht af te melden bij de iSCSI-sessies:

    iscsiadm -m node --logout -p <Target_IP>

Herhaal deze opdracht voor de verbonden netwerkinterfaces op de iSCSI-doel, uw StorSimple-apparaat is. Nadat u afgemeld bij alle iSCSI-sessies bent, gebruikt u de iSCSI-doel IQN te hervatten van de iSCSI-sessie. Typ de volgende opdracht:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Ik wil niet zeker weet of het apparaat wilt plaatsen is.

A. Gebruik de volgende opdracht voor probleemoplossing interactief om te controleren of uw apparaat wilt plaatsen:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Ga voor meer informatie naar [probleemoplossing interactief opdracht voor het gebruik van meerdere paden gebruiken](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Lijst met opdrachten nuttig
| Type | Opdracht | Beschrijving |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |ISCSI-service starten |
| &nbsp; |`service iscsid stop` |ISCSI-service stoppen |
| &nbsp; |`service iscsid restart` |ISCSI-service opnieuw starten |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Beschikbare doelen op het opgegeven adres detecteren |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |Aanmelden bij de iSCSI-doel |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |Meld u af van de iSCSI-doel |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |Naam van de iSCSI-initiator afdrukken |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Controleer de status van de iSCSI-sessie en het volume dat is gedetecteerd op de host |
| &nbsp; |`iscsi –m session` |Bevat alle iSCSI-sessies tot stand gebracht tussen de host en het StorSimple-apparaat |
|  | | |
| **Gebruik van meerdere paden** |`service multipathd start` |Multipath-daemon starten |
| &nbsp; |`service multipathd stop` |Multipath-daemon stoppen |
| &nbsp; |`service multipathd restart` |Multipath-daemon starten |
| &nbsp; |`chkconfig multipathd on` </br> OF </br> `mpathconf –with_chkconfig y` |Schakel multipath-daemon te starten tijdens het opstarten |
| &nbsp; |`multipathd –k` |Start de interactieve console voor probleemoplossing |
| &nbsp; |`multipath –l` |Lijst met meerdere paden verbindingen en apparaten |
| &nbsp; |`mpathconf --enable` |Maken van een voorbeeldbestand mulitpath.conf in`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Volgende stappen
Als u de MPIO op Linux-host configureert, moet u mogelijk ook om te verwijzen naar de volgende documenten voor CentoS 6.6:

* [Instellen van MPIO op CentOS](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Linux Training handleiding](http://linux-training.be/files/books/LinuxAdm.pdf)

