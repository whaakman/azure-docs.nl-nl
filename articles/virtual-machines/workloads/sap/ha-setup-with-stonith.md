---
title: Hoge beschikbaarheid instellen met STONITH voor SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Hoge beschikbaarheid voor SAP HANA in Azure (grote exemplaren) in met behulp van de STONITH SUSE tot stand brengen
services: virtual-machines-linux
documentationcenter: 
author: saghorpa
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d710fe24673c6ddc581d36e4f0cacdb750ff74f9
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="high-availability-set-up-in-suse-using-the-stonith"></a>Hoge beschikbaarheid in SUSE met behulp van de STONITH instellen
Dit document bevat de gedetailleerde stapsgewijze instructies voor het instellen van de hoge beschikbaarheid op SUSE-besturingssysteem met behulp van het apparaat STONITH.

**Disclaimer:** *in deze handleiding wordt berekend door het testen van de set up in de omgeving Microsoft HANA grote instanties, die correct werkt. Als het besturingssysteem biedt geen ondersteuning voor Microsoft-Service Management-team voor grote exemplaren HANA, moet u mogelijk contact opnemen met SUSE voor eventuele problemen op te lossen of de informatie op de laag van het besturingssysteem. Service management-team van Microsoft STONITH apparaat ingesteld en volledig ondersteunt en kan worden betrokken bij voor het oplossen van problemen met STONITH apparaat.*
## <a name="overview"></a>Overzicht
Als u de hoge beschikbaarheid met behulp van SUSE clustering instelt, moeten aan de volgende vereisten voldoen.
### <a name="pre-requisites"></a>Vereisten
- Grote HANA-exemplaren zijn ingericht.
- Besturingssysteem is geregistreerd
- Grote exemplaren HANA servers zijn verbonden met server SMT patches/pakketten ophalen
- Besturingssysteem hebben de nieuwste patches die zijn geïnstalleerd
- NTP (tijdserver) is ingesteld
- Lees en begrijp de nieuwste versie van SUSE documentatie over HA instellen

### <a name="set-up-details"></a>Details instellen
- In deze handleiding gebruikt we het volgende instellen:
- Besturingssysteem: SLES 12 SP1 voor SAP
- Grote exemplaren HANA: 2xS192 (4-sockets, 2 TB)
- HANA versie: HANA 2.0 SP1
- Servernamen: sapprdhdb95 (knooppunt1) en sapprdhdb96 (Knooppunt2)
- Apparaat STONITH: STONITH apparaat op basis van iSCSI
- NTP instellen op een van de knooppunten HANA grote exemplaar

Wanneer u grote exemplaren met HSR HANA instelt, kunt u Microsoft Service Management-team voor het instellen van STONITH vragen. Als u al een bestaande klant die HANA grote exemplaren die worden ingericht en STONITH apparaat instellen voor uw bestaande blades nodig heeft, moet u de volgende informatie aan Microsoft Service Management-team in het formulier in de service-aanvraag (SRF). U kunt aanvragen SRF formulier via het Technical Account Manager of uw Microsoft-contactpersoon voor de voorbereiding op grote HANA-exemplaar. De nieuwe klanten kunnen STONITH apparaat op het moment van de inrichting aanvragen. De invoer is beschikbaar in de inrichting aanvraagformulier.

- Servernaam en Server IP-adres (bijvoorbeeld myhanaserver1, 10.35.0.1)
- Locatie (bijvoorbeeld VS-Oost)
- Naam van de klant (bijvoorbeeld Microsoft)
- SID - id van het systeem HANA (bijvoorbeeld H11)

Zodra het apparaat STONITH is geconfigureerd, Microsoft Service Management-team biedt u de naam van het apparaat SBD en IP-adres van de iSCSI-opslag, dat u gebruiken kunt voor het configureren van STONITH instellen. 

Als u de end-to-end HA met STONITH instelt, moet de volgende stappen worden gevolgd:

1.  Identificatie van het apparaat SBD
2.  Het apparaat SBD initialiseren
3.  Configureren van het Cluster
4.  Instellen van de Softdog Watchdog
5.  Het knooppunt toevoegen aan het cluster
6.  Het cluster valideren
7.  Configureer de bronnen aan het cluster
8.  Het failoverproces van testen

## <a name="1---identify-the-sbd-device"></a>1.   Identificatie van het apparaat SBD
Deze sectie wordt beschreven op het apparaat SBD voor uw set up bepalen wanneer de management-team van Microsoft-service heeft de STONITH geconfigureerd. **Deze sectie is alleen van toepassing op de bestaande klant**. Als u een nieuwe klant, biedt Microsoft service management-team SBD de naam van het apparaat bij u en u kunt deze sectie overslaan.

1.1 wijzigen */etc/iscsi/initiatorname.isci* naar 
``` 
iqn.1996-04.de.suse:01:<Tenant><Location><SID><NodeNumber> 
```

Beheer van Microsoft-service biedt deze tekenreeks. Wijzigen van het bestand op **beide** de knooppunten echter het knooppunt getal is op elk knooppunt.

![initiatorname.PNG](media/HowToHLI/HASetupWithStonith/initiatorname.png)

1.2 wijzigen */etc/iscsi/iscsid.conf*: Stel *node.session.timeo.replacement_timeout=5* en *node.startup = automatische*. Wijzigen van het bestand op **beide** de knooppunten.

1.3 Voer de opdracht detectie, ziet u vier sessies. Het uitvoeren op beide knooppunten.

```
iscsiadm -m discovery -t st -p <IP address provided by Service Management>:3260
```

![iSCSIadmDiscovery.png](media/HowToHLI/HASetupWithStonith/iSCSIadmDiscovery.png)

1.4 uitvoeren van de opdracht zich aanmelden bij de iSCSI-apparaat, ziet u vier sessies. Uitvoeren op **beide** de knooppunten.

```
iscsiadm -m node -l
```
![iSCSIadmLogin.png](media/HowToHLI/HASetupWithStonith/iSCSIadmLogin.png)

1.5 voert u het script opnieuw scannen: *opnieuw scannen-scsi-bus.sh*.  Dit script ziet u de nieuwe schijven die voor u gemaakt.  Het uitvoeren op beide knooppunten. U ziet een LUN getal dat groter is dan nul (bijvoorbeeld: 1, 2 enz.)

```
rescan-scsi-bus.sh
```
![rescanscsibus.PNG](media/HowToHLI/HASetupWithStonith/rescanscsibus.png)

1.6 te halen van de naam van het apparaat de opdracht uitvoert *fdisk – l*. Het uitvoeren op beide knooppunten. Kies het apparaat met de grootte van **178 MiB**.

```
  fdisk –l
```

![Fdisk l.png](media/HowToHLI/HASetupWithStonith/fdisk-l.png)

## <a name="2---initialize-the-sbd-device"></a>2.   Het apparaat SBD initialiseren

2.1 op het apparaat SBD geïnitialiseerd **beide** knooppunten

```
sbd -d <SBD Device Name> create
```
![sbdcreate.PNG](media/HowToHLI/HASetupWithStonith/sbdcreate.png)

2.2 controleren wat is geschreven naar het apparaat. Het **beide** knooppunten

```
sbd -d <SBD Device Name> dump
```

## <a name="3---configuring-the-cluster"></a>3.   Configureren van het Cluster
Deze sectie beschrijft de stappen voor het instellen van het SUSE HA-cluster.
### <a name="31-package-installation"></a>3.1 pakketinstallatie
3.1.1 Controleer ha_sles en SAPHanaSR doc patronen zijn geïnstalleerd. Als dit niet is geïnstalleerd, installeert u ze. Installeer het op **beide** de knooppunten.
```
zypper in -t pattern ha_sles
zypper in SAPHanaSR SAPHanaSR-doc
```
![zypperpatternha_sles.PNG](media/HowToHLI/HASetupWithStonith/zypperpatternha_sles.png)
![zypperpatternSAPHANASR doc.png](media/HowToHLI/HASetupWithStonith/zypperpatternSAPHANASR-doc.png)

### <a name="32-setting-up-the-cluster"></a>3.2 instellen van het cluster
3.2.1 u kunt gebruiken *ha-cluster-init* opdracht of het gebruik van de wizard yast2 voor het instellen van het cluster. In dit geval we yast2 wizard gebruikt. U deze stap uitvoeren **alleen op het primaire knooppunt**.

Ga als volgt yast2 > hoge beschikbaarheid > Cluster ![yast-besturingselement-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)
![yast-hawk-install.png](media/HowToHLI/HASetupWithStonith/yast-hawk-install.png)

Klik op **annuleren** zoals we al de halk2 pakket is geïnstalleerd hebben.

![yast-hawk-continue.png](media/HowToHLI/HASetupWithStonith/yast-hawk-continue.png)

Klik op **gaan**

Verwachte waarde aantal knooppunten die zijn geïmplementeerd (in dit geval 2) = ![yast-Cluster-Security.png](media/HowToHLI/HASetupWithStonith/yast-Cluster-Security.png) klikt u op **volgende**
![yast-cluster-configureren-csync2.png](media/HowToHLI/HASetupWithStonith/yast-cluster-configure-csync2.png) toevoegen node names en klik vervolgens op Add' voorgestelde bestanden'

Klik op 'Inschakelen csync2'

Klik op 'Pre-Shared-sleutels genereren' hieronder pop te zien

![yast-sleutel-file.png](media/HowToHLI/HASetupWithStonith/yast-key-file.png)

Klik op **OK**

De verificatie wordt uitgevoerd met de IP-adressen en pre-shared-sleutels in Csync2. Het sleutelbestand wordt gegenereerd met csync2 -k /etc/csync2/key_hagroup. Het bestand key_hagroup moeten worden gekopieerd op alle leden van het cluster handmatig nadat deze gemaakt. **Ervoor zorgen dat het bestand kopiëren vanuit het knooppunt 1 naar knooppunt2**.

![yast-cluster-conntrackd.png](media/HowToHLI/HASetupWithStonith/yast-cluster-conntrackd.png)

Klik op **volgende**
![yast-cluster-service.png](media/HowToHLI/HASetupWithStonith/yast-cluster-service.png)

In de standaardoptie opgestart is uitgeschakeld, op 'aan' wijzigen zodat pacemaker heeft bij het opstarten is gestart. U kunt kiezen op basis van de instellingen van de vereisten.
Klik op **volgende** en configuratie van het cluster is voltooid.

## <a name="4---setting-up-the-softdog-watchdog"></a>4.   Instellen van de Softdog Watchdog
Deze sectie beschrijft de configuratie van de watchdog (softdog).

4.1 de volgende regel toe te voegen */etc/init.d/boot.local* op **beide** de knooppunten.
```
modprobe softdog
```
![modprobe softdog.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog.png)

4.2 update van het bestand */etc/sysconfig/sbd* op **beide** de knooppunten die u als volgt:
```
SBD_DEVICE="<SBD Device Name>"
```
![SBD device.png](media/HowToHLI/HASetupWithStonith/sbd-device.png)

4.3 de kernel-module niet laden op **beide** knooppunten door de volgende opdracht uit te voeren
```
modprobe softdog
```
![modprobe-softdog-command.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog-command.png)

4.4 controleren en ervoor te zorgen dat softdog wordt uitgevoerd als de volgende handelingen uit op **beide** knooppunten:
```
lsmod | grep dog
```
![lsmod-grep-dog.png](media/HowToHLI/HASetupWithStonith/lsmod-grep-dog.png)

4.5 start het apparaat SBD op **beide** knooppunten
```
/usr/share/sbd/sbd.sh start
```
![SBD-v-start.png](media/HowToHLI/HASetupWithStonith/sbd-sh-start.png)

4.6 de daemon SBD testen op **beide** de knooppunten. U twee items ziet nadat u op configureren **beide** knooppunten
```
sbd -d <SBD Device Name> list
```
![SBD list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.7 verzendt een testbericht naar **één** van uw knooppunten
```
sbd  -d <SBD Device Name> message <node2> <message>
```
![SBD list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.8 op de **tweede** knooppunt (Knooppunt2) kunt u de berichtstatus controleren
```
sbd  -d <SBD Device Name> list
```
![SBD-lijst-message.png](media/HowToHLI/HASetupWithStonith/sbd-list-message.png)

4.9 vast te stellen de sbd config, het bestand bijwerken */etc/sysconfig/sbd* als volgt. Bijwerken van het bestand op **beide** knooppunten
```
SBD_DEVICE=" <SBD Device Name>" 
SBD_WATCHDOG="yes" 
SBD_PACEMAKER="yes" 
SBD_STARTMODE="clean" 
SBD_OPTS=""
```
4.10 start de service pacemaker heeft op de **primaire knooppunt** (knooppunt1)
```
systemctl start pacemaker
```
![Start pacemaker.png](media/HowToHLI/HASetupWithStonith/start-pacemaker.png)

Als de service pacemaker heeft *mislukt*, verwijzen naar *Scenario 5: pacemaker heeft service mislukt*

## <a name="5---joining-the-cluster"></a>5.   Lid worden van het cluster
Deze sectie beschrijft over het toevoegen van het knooppunt aan het cluster.

### <a name="51-add-the-node"></a>5.1 het knooppunt toevoegen
Voer de volgende opdracht op **Knooppunt2** te laten Knooppunt2 toegevoegd aan het cluster.
```
ha-cluster-join
```
Als u krijgt een *fout* tijdens het lid wordt van het cluster, Raadpleeg *Scenario 6: knooppunt 2 kan niet deelnemen aan het cluster*.

## <a name="6---validating-the-cluster"></a>6.   De cluster te valideren

### <a name="61-start-the-cluster-service"></a>6.1 start de clusterservice
Om te controleren en desgewenst het cluster voor het eerst starten op **beide** knooppunten.
```
systemctl status pacemaker
systemctl start pacemaker
```
![systemctl-status-pacemaker.png](media/HowToHLI/HASetupWithStonith/systemctl-status-pacemaker.png)
### <a name="62-monitor-the-status"></a>6.2 de status controleren
Voer de opdracht *crm_mon* om ervoor te zorgen **beide** de knooppunten online zijn. U kunt uitvoeren op **een van de knooppunten** van het cluster
```
crm_mon
```
![CRM-mon.png](media/HowToHLI/HASetupWithStonith/crm-mon.png) u ook kunt aanmelden bij hawk om te controleren van de clusterstatus van de *https://<node IP>: 7630*. De standaardgebruiker hacluster is en het wachtwoord is linux. Indien nodig, kunt u het wachtwoord met behulp van *passwd* opdracht.

## <a name="7-configure-cluster-properties-and-resources"></a>7. De clustereigenschappen en Resources configureren 
Deze sectie beschrijft de stappen voor het configureren van de clusterresources.
In dit voorbeeld wordt de volgende resource hebt ingesteld, de rest kan worden geconfigureerd (indien nodig) door te verwijzen naar de SUSE HA-handleiding. Uitvoeren van de configuratie in **een van de knooppunten** alleen. Doet op het primaire knooppunt.

- Cluster bootstrap
- STONITH apparaat
- Het virtuele IP-adres


### <a name="71-cluster-bootstrap-and-more"></a>7.1 cluster bootstrap en meer
Bootstrap cluster toevoegen. Het bestand maken en toevoegen van de tekst als volgt:
```
sapprdhdb95:~ # vi crm-bs.txt
# enter the following to crm-bs.txt
property $id="cib-bootstrap-options" \
no-quorum-policy="ignore" \
stonith-enabled="true" \
stonith-action="reboot" \
stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
resource-stickiness="1000" \
migration-threshold="5000"
op_defaults $id="op-options" \
timeout="600"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-bs.txt
```
![CRM-configureren crmbs.png](media/HowToHLI/HASetupWithStonith/crm-configure-crmbs.png)

### <a name="72-stonith-device"></a>7.2 STONITH apparaat
Resource STONITH toevoegen. Het bestand maken en toevoegen van de tekst als volgt.
```
# vi crm-sbd.txt
# enter the following to crm-sbd.txt
primitive stonith-sbd stonith:external/sbd \
params pcmk_delay_max="15" \
op monitor interval="15" timeout="15"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-sbd.txt
```

### <a name="73-the-virtual-ip-address"></a>7.3 het virtuele IP-adres
Virtueel IP-adres van bron toevoegen. Maak het bestand en voeg tekst toe zoals hieronder.
```
# vi crm-vip.txt
primitive rsc_ip_HA1_HDB10 ocf:heartbeat:IPaddr2 \
operations $id="rsc_ip_HA1_HDB10-operations" \
op monitor interval="10s" timeout="20s" \
params ip="10.35.0.197"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-vip.txt
```

### <a name="74-validate-the-resources"></a>7.4 resources valideren

Wanneer u de opdracht uitvoert *crm_mon*, ziet u de twee resources er.
![crm_mon_command.PNG](media/HowToHLI/HASetupWithStonith/crm_mon_command.png)

Ook ziet u de status op *https://<node IP address>: 7630 cib/live/status*

![hawlk-status-page.png](media/HowToHLI/HASetupWithStonith/hawlk-status-page.png)

## <a name="8-testing-the-failover-process"></a>8. Het failoverproces testen
Om het failoverproces van testen, stopt u de service pacemaker heeft op knooppunt1 en de bronnen failover naar knooppunt2.
```
Service pacemaker stop
```
Nu, stop de service pacemaker heeft op **Knooppunt2** en een failover naar resources **knooppunt1**

**Vóór de failover**
![voordat failover.png](media/HowToHLI/HASetupWithStonith/Before-failover.png)
**na een failover**
![na failover.png](media/HowToHLI/HASetupWithStonith/after-failover.png)
 ![ CRM mon na failover.png](media/HowToHLI/HASetupWithStonith/crm-mon-after-failover.png)


## <a name="9-troubleshooting"></a>9. Problemen oplossen
Deze sectie beschrijft de enkele fout scenario's, die tijdens de instellen optreden kunnen. U kan deze problemen niet per se geconfronteerd.

### <a name="scenario-1-cluster-node-not-online"></a>Scenario 1: Clusterknooppunt niet online
Als een van de knooppunten wordt niet weergegeven in het cluster manager online, kunt u proberen om online brengen te volgen.

De iSCSI-service starten
```
service iscsid start
```

En nu moet u kunnen zich aanmelden bij de iSCSI-knooppunt
```
iscsiadm -m node -l
```
De verwachte uitvoer lijkt erop volgende
```
sapprdhdb45:~ # iscsiadm -m node -l
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] (multiple)
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] successful.
```
### <a name="scenario-2-yast2-does-not-show-graphical-view"></a>Scenario 2: yast2 wordt niet weergegeven voor grafische weergave
We het yast2 grafische scherm gebruikt voor het instellen van het cluster met hoge beschikbaarheid in dit document. Als yast2 niet met de grafische venster, openen zoals wordt weergegeven en Qt fout genereert, voert u de stappen uit als de volgende. Als u deze met de grafische venster wordt geopend, kunt u de stappen overslaan.

**Fout**

![YaST2-qt-gui-error.png](media/HowToHLI/HASetupWithStonith/yast2-qt-gui-error.png)

**Verwachte uitvoer**

![yast-besturingselement-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)

Als de yast2 niet wordt geopend met de grafische weergave, volgt u de volgende stappen.

Installeer de vereiste pakketten. U moet zijn aangemeld als gebruiker "root" en SMT instellen op de pakketten downloaden/installeren.

U kunt de pakketten installeren met yast > Software > softwarebeheer > afhankelijkheden > optie 'Install aanbevolen pakketten...'. De volgende schermafbeelding ziet u de verwachte schermen.
>[!NOTE]
>U moet de stappen uitvoeren op beide knooppunten, zodat u de grafische weergave van yast2 vanaf beide knooppunten openen kunt.

![yast sofwaremanagement.png](media/HowToHLI/HASetupWithStonith/yast-sofwaremanagement.png)

Selecteer onder afhankelijkheden, 'Installeren aanbevolen Packages' ![yast dependencies.png](media/HowToHLI/HASetupWithStonith/yast-dependencies.png)

Controleer de wijzigingen en klik op OK

![yast](media/HowToHLI/HASetupWithStonith/yast-automatic-changes.png)

Pakket-installatie voortgezet ![yast-uitvoeren installation.png](media/HowToHLI/HASetupWithStonith/yast-performing-installation.png)

Klik op volgende

![yast-installatie-report.png](media/HowToHLI/HASetupWithStonith/yast-installation-report.png)

Klik op Voltooien

U moet ook de libqt4 en libyui qt pakketten installeren.
```
zypper -n install libqt4
```
![zypper-install-libqt4.png](media/HowToHLI/HASetupWithStonith/zypper-install-libqt4.png)
```
zypper -n install libyui-qt
```
![zypper-install-ligyui.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui.png)
![zypper-install-ligyui_part2.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui_part2.png) Yast2 moet kunnen hier opent u de grafische weergave nu zoals weergegeven.
![YaST2-besturingselement-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

### <a name="scenario-3-yast2-does-not-high-availability-option"></a>Scenario 3: yast2 biedt geen optie voor hoge beschikbaarheid
Voor de optie voor hoge beschikbaarheid moet zichtbaar zijn in het besturingselement yast2 center, moet u de extra pakketten installeren.

Met behulp van Yast2 > Software > softwarebeheer > selecteert u de volgende patronen

- Base SAP HANA-server
- C/C++-Compiler en hulpprogramma 's
- Hoge beschikbaarheid
- SAP-toepassingsserver base

Het volgende scherm bevat de stappen voor het installeren van de patronen.

Met behulp van yast2 > Software > beheer van Software

![YaST2-besturingselement-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

Selecteer de patronen

![yast pattern1.png](media/HowToHLI/HASetupWithStonith/yast-pattern1.png)
![yast pattern2.png](media/HowToHLI/HASetupWithStonith/yast-pattern2.png)

Klik op **accepteren**

![yast-gewijzigd packages.png](media/HowToHLI/HASetupWithStonith/yast-changed-packages.png)

Klik op **gaan**

![YaST2-uitvoeren installation.png](media/HowToHLI/HASetupWithStonith/yast2-performing-installation.png)

Klik op **volgende** wanneer de installatie is voltooid

![YaST2-installatie-report.png](media/HowToHLI/HASetupWithStonith/yast2-installation-report.png)

### <a name="scenario-4-hana-installation-fails-with-gcc-assemblies-error"></a>Scenario 4: HANA installatie mislukt met fout voor gcc-assembly 's
De HANA-installatie mislukt met de volgende fout is opgetreden.

![Hana-installatie-error.png](media/HowToHLI/HASetupWithStonith/Hana-installation-error.png)

U kunt het probleem oplossen, moet u bibliotheken installeren (libgcc_sl en libstdc ++ 6) als volgt.

![zypper-install-lib.png](media/HowToHLI/HASetupWithStonith/zypper-install-lib.png)

### <a name="scenario-5-pacemaker-service-fails"></a>Scenario 5: Pacemaker heeft service mislukt

Het volgende probleem opgetreden tijdens het starten van de service pacemaker heeft.

```
sapprdhdb95:/ # systemctl start pacemaker
A dependency job for pacemaker.service failed. See 'journalctl -xn' for details.
```
```
sapprdhdb95:/ # journalctl -xn
-- Logs begin at Thu 2017-09-28 09:28:14 EDT, end at Thu 2017-09-28 21:48:27 EDT. --
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration map
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration ser
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster closed pr
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster quorum se
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync profile loading s
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [MAIN  ] Corosync Cluster Engine exiting normally
Sep 28 21:48:27 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager
-- Subject: Unit pacemaker.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pacemaker.service has failed.
--
-- The result is dependency.
```
```
sapprdhdb95:/ # tail -f /var/log/messages
2017-09-28T18:44:29.675814-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.676023-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster closed process group service v1.01
2017-09-28T18:44:29.725885-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.726069-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster quorum service v0.1
2017-09-28T18:44:29.726164-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync profile loading service
2017-09-28T18:44:29.776349-04:00 sapprdhdb95 corosync[57600]:   [MAIN  ] Corosync Cluster Engine exiting normally
2017-09-28T18:44:29.778177-04:00 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager.
2017-09-28T18:44:40.141030-04:00 sapprdhdb95 systemd[1]: [/usr/lib/systemd/system/fstrim.timer:8] Unknown lvalue 'Persistent' in section 'Timer'
2017-09-28T18:45:01.275038-04:00 sapprdhdb95 cron[57995]: pam_unix(crond:session): session opened for user root by (uid=0)
2017-09-28T18:45:01.308066-04:00 sapprdhdb95 CRON[57995]: pam_unix(crond:session): session closed for user root
```

Om dit te corrigeren, kunt u de volgende regel verwijderen uit het bestand */usr/lib/systemd/system/fstrim.timer*

```
Persistent=true
```

![Persistent.PNG](media/HowToHLI/HASetupWithStonith/Persistent.png)

### <a name="scenario-6-node-2-unable-to-join-the-cluster"></a>Scenario 6: Knooppunt 2 kan niet deelnemen aan het cluster

Wanneer u de Knooppunt2 toevoegt aan de bestaande cluster met behulp van *ha-cluster-join* opdracht, de volgende fout is opgetreden.

```
ERROR: Can’t retrieve SSH keys from <Primary Node>
```

![ha-cluster-join-error.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-error.png)

Als u wilt oplossen, voert u de volgende op beide knooppunten

```
ssh-keygen -q -f /root/.ssh/id_rsa -C 'Cluster Internal' -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

![SSH-keygen-knooppunt1. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node1.PNG)

![SSH-keygen-Knooppunt2. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node2.PNG)

Na de vorige oplossing ophalen Knooppunt2 toegevoegd aan het cluster

![ha-cluster-join-fix.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-fix.png)

## <a name="10-general-documentation"></a>10. Algemene documentatie
U vindt meer informatie over SUSE HA ingesteld in de volgende artikelen: 

- [Scenario voor optimale SAP HANA SR prestaties](https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf )
- [Hekwerk op basis van opslag](https://www.suse.com/documentation/sle-ha-2/book_sleha/data/sec_ha_storage_protect_fencing.html)
- [Blog - pacemaker heeft Cluster gebruikt voor SAP HANA - deel 1](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-1-basics/)
- [Blog - pacemaker heeft Cluster gebruikt voor SAP HANA - deel 2](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-2-failure-of-both-nodes/)