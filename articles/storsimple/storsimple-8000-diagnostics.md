---
title: Hulpprogramma voor diagnostische gegevens naar StorSimple 8000-apparaat oplossen | Microsoft Docs
description: Beschrijving van de StorSimple-apparaat-modi en wordt uitgelegd hoe u Windows PowerShell voor StorSimple gebruiken om te wijzigen van de modus van het apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 5cce4337e3ef95c6407d46d9b8b6401fe4f6600b
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497737"
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>Gebruik het diagnostisch hulpprogramma voor StorSimple 8000 series apparaat problemen oplossen

## <a name="overview"></a>Overzicht

Het hulpprogramma StorSimple Diagnostics diagnoses problemen met betrekking tot het systeem, prestaties, netwerk en hardware Onderdeelstatus voor een StorSimple-apparaat. Het hulpprogramma diagnostics kan worden gebruikt in verschillende scenario's. Deze scenario's omvatten werkbelasting plannen, implementeren van een StorSimple-apparaat, beoordeling van de netwerkomgeving en bepalen van de prestaties van een operationeel-apparaat. In dit artikel biedt een overzicht van het hulpprogramma voor diagnostische gegevens en wordt beschreven hoe het hulpprogramma kan worden gebruikt met een StorSimple-apparaat.

Het hulpprogramma diagnostics is voornamelijk bedoeld voor StorSimple 8000-serie on-premises apparaten (8100 en 8600).

## <a name="run-diagnostics-tool"></a>Hulpprogramma voor diagnostische gegevens worden uitgevoerd

Dit hulpprogramma kan worden uitgevoerd via de Windows PowerShell-interface van uw StorSimple-apparaat. Er zijn twee manieren toegang krijgen tot de lokale interface van uw apparaat:

* [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Extern toegang verkrijgen tot het hulpprogramma via de Windows PowerShell voor StorSimple](storsimple-8000-remote-connect.md).

In dit artikel nemen we aan dat u verbinding hebt gemaakt met de seriële console van het apparaat via de PuTTY.

#### <a name="to-run-the-diagnostics-tool"></a>Het hulpprogramma diagnostics uit te voeren

Nadat u verbinding hebt gemaakt met de Windows PowerShell-interface van het apparaat, moet u de volgende stappen voor het uitvoeren van de cmdlet uitvoeren.
1. Meld u aan bij de seriële console van het apparaat met de volgende stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Typ de volgende opdracht:

    `Invoke-HcsDiagnostics`

    Als de bereikparameter niet opgegeven is, wordt de diagnostische tests uitgevoerd door de cmdlet. Deze tests omvatten system, status voor hardware-onderdeel, netwerk en prestaties. 
    
    Om te worden uitgevoerd alleen een specifieke test, moet u de scope-parameter opgeven. Bijvoorbeeld, om alleen de Netwerktest uitvoert, typ

    `Invoke-HcsDiagnostics -Scope Network`

3. Selecteer en kopieer de uitvoer van de PuTTY-venster in een tekstbestand voor verdere analyse.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Scenario's voor het hulpprogramma diagnostics gebruiken

Het hulpprogramma diagnostics gebruiken om op te lossen van de status van het netwerk, prestaties, systeem en hardware van het systeem. Hier volgen enkele mogelijke scenario's:

* **Apparaat offline** -apparaat uit de StorSimple 8000-serie is offline. Echter, via de Windows PowerShell-interface, het lijkt erop dat beide controllers actief en werkend zijn.
    * Dit hulpprogramma kunt u vervolgens kunt u de status van het netwerk bepalen.
         
         > [!NOTE]
         > Gebruik dit hulpprogramma niet om instellingen voor prestaties en het netwerk op een apparaat voordat u de registratie (of configureren via de wizard setup) vast te stellen. Een geldig IP-adres is toegewezen aan het apparaat tijdens de setup-wizard en de registratie. U kunt deze cmdlet uitvoeren op een apparaat dat niet is geregistreerd, voor de gezondheid van hardware en het systeem. Gebruik de bereikparameter, bijvoorbeeld:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Problemen met de permanente apparaat** -ondervindt u problemen met het apparaat die lijken om vast te leggen. Bijvoorbeeld, de registratie is mislukt. Er kan ook worden sprake van problemen met het apparaat wanneer het apparaat is geregistreerd en operationeel even is.
    * In dit geval moet u dit hulpprogramma gebruiken voor het voorlopig oplossen voordat u zich een serviceaanvraag met Microsoft Support aanmeldt. U wordt aangeraden dat u dit hulpprogramma uitvoeren en vastleggen van de uitvoer van dit hulpprogramma. Vervolgens kunt u deze uitvoer bieden aan ondersteuning voor het oplossen van problemen versnellen.
    * Als er problemen met hardware-onderdeel of het cluster, moet u zich aanmeldt een ondersteuningsaanvraag.

* **Lage Apparaatprestaties** -uw StorSimple-apparaat traag is.
    * In dit geval moet u deze cmdlet uitvoeren met bereikparameter ingesteld op prestaties. Analyseer de uitvoer. U krijgt de cloud alleen-lezen-latenties. De gerapporteerde latenties gebruiken als het doel van maximale haalbare, factor in enige overhead voor de interne gegevensverwerking en implementeer vervolgens de werkbelasting op het systeem. Ga voor meer informatie naar [de Netwerktest gebruiken om op te lossen Apparaatprestaties](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Diagnostische test en voorbeeld-uitvoer

### <a name="hardware-test"></a>Hardwaretest

Deze test bepaalt de status van de hardware-onderdelen, de firmware voor Gebruikersbeheer en de schijffirmware die wordt uitgevoerd op uw systeem.

* De hardwareonderdelen die vermeld zijn de onderdelen die de test is mislukt of zijn niet aanwezig in het systeem.
* De firmwareversies voor Gebruikersbeheer firmware en de schijf worden gerapporteerd voor de Controller 0, 1 van de domeincontroller, en gedeelde onderdelen in uw systeem. Voor een volledige lijst van hardware-onderdelen, gaat u naar:

    * [Onderdelen van primaire behuizing](storsimple-8000-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [Onderdelen van EBOD behuizing](storsimple-8000-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Als de hardwaretest niet functionerende onderdelen, rapporteert [zich in een serviceaanvraag met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Voorbeeld van de uitvoer van de hardwaretest uitvoeren op een 8100-apparaat

Hier volgt een voorbeeld van uitvoer van een StorSimple 8100-apparaat. De EBOD-behuizing is niet aanwezig in het model 8100-apparaat. Daarom worden de onderdelen van EBOD-controller niet gerapporteerd.

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a>Systeem-test

Deze test rapporteert de systeemgegevens, de updates die beschikbaar zijn, de clustergegevens en de service-informatie voor uw apparaat.

* De systeemgegevens bevat het model, serienummer van apparaat, tijdzone, de status van de domeincontroller en de gedetailleerde softwareversie op het systeem. Voor meer informatie over de verschillende parameters voor het gerapporteerd als de uitvoer, gaat u naar [interpreteren systeemgegevens](#appendix-interpreting-system-information).

* De beschikbaarheid van de rapporten of de modi normaal en onderhoud beschikbaar zijn en hun bijbehorende pakketnamen. Als `RegularUpdates` en `MaintenanceModeUpdates` zijn `false`, dit geeft aan dat de updates niet beschikbaar zijn. Uw apparaat is up-to-date.
* De cluster-informatie bevat de informatie over verschillende logische onderdelen van alle groepen van de HCS-cluster en de bijbehorende status. Als u een offline clustergroep in deze sectie van het rapport ziet [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* De service-informatie bevat de namen en de status van alle de HCS en configuratie-items services die worden uitgevoerd op uw apparaat. Deze informatie is nuttig voor het Microsoft Support bij het oplossen van het probleem van het apparaat.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Voorbeeld van de uitvoer van systeem test uitvoeren op een 8100-apparaat

Hier volgt een voorbeeld van de uitvoer van de systeem-test uitvoeren op een 8100-apparaat.

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_service        Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a>Netwerk testen

Deze test controleert de status van de netwerkinterfaces, poorten, DNS en NTP server verbinding, SSL-certificaat, opslagaccountreferenties, verbinding met de Update-servers en web proxy connectiviteit op uw StorSimple-apparaat.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Voorbeeld van de uitvoer van netwerk testen wanneer alleen DATA0 is ingeschakeld

Hier volgt een voorbeeld van de uitvoer van het 8100-apparaat. U kunt zien in de uitvoer die:
* Alleen DATA 0 en 1 van DATA-netwerkinterfaces zijn ingeschakeld en geconfigureerd.
* DATA 2-5 zijn niet ingeschakeld in de portal.
* De configuratie van de DNS-server is geldig en het apparaat verbinding kan maken via de DNS-server.
* De connectiviteit van de server NTP is ook prima.
* Poorten 80 en 443 zijn geopend. Echter, poort 9354 is geblokkeerd. Op basis van de [netwerk systeemvereisten](storsimple-system-requirements.md), moet u deze poort voor de service bus-communicatie te openen.
* Het SSL-certificaat is geldig.
* Het apparaat verbinding kan maken met de storage-account: _myss8000storageacct_.
* De verbinding met de Update-servers is geldig.
* De webproxy is niet geconfigureerd op dit apparaat.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Voorbeeld van de uitvoer van Netwerktest wanneer DATA0 en bestand1 zijn ingeschakeld

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a>Prestatietest

Deze test rapporteert de prestaties van de cloud via de cloud alleen-lezen-latenties voor uw apparaat. Dit hulpprogramma kan worden gebruikt om een basislijn van de prestaties van de cloud dat kan worden behaald met StorSimple stand te brengen. Het hulpprogramma rapporteert de maximale prestaties (beste scenario voor alleen-lezen-latenties) die u voor uw verbinding kan verwerken.

Als het hulpprogramma de maximale haalbare prestaties rapporteert, kunnen we de gerapporteerde latenties voor lezen / schrijven gebruiken als doelen bij het implementeren van de werkbelastingen.

De test simuleert de blob-opslag die is gekoppeld aan de verschillende typen op het apparaat. Normale gelaagde en back-ups van lokaal vastgemaakte volumes met een grootte van 64 KB-blob. Gelaagde volumes met de archive-optie is ingeschakeld gebruik gegevensgrootte van 512 KB blob. Als het apparaat heeft gelaagde en lokaal vastgemaakte volumes geconfigureerd, alleen de test die overeenkomt met de blob van 64 KB gegevensgrootte wordt uitgevoerd.

Voor het gebruik van dit hulpprogramma, moet u de volgende stappen uitvoeren:

1.  Maak eerst een combinatie van gelaagde volumes en gelaagde volumes met gearchiveerde optie is ingeschakeld. Deze actie zorgt ervoor dat het hulpprogramma de tests uit voor zowel 64 KB en 512 KB blob-opslag voert.

2. De cmdlet uitvoeren nadat u hebt gemaakt en de volumes geconfigureerd. Type:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Noteer de alleen-lezen-latenties gemeld door het hulpprogramma. Deze test kan enige tijd duren om uit te voeren voordat deze de resultaten rapporteert.

4. Als de verbinding latenties allemaal op het verwachte bereik zijn, kunnen klikt u vervolgens de latenties gemeld door het hulpprogramma worden gebruikt als maximale haalbare doel bij het implementeren van de werkbelastingen. Factor in enige overhead voor interne gegevensverwerking.

    Als de alleen-lezen-latenties gemeld door het hulpprogramma diagnostics hoog zijn:

    1. Storage Analytics voor blob-services configureren en analyseren van de uitvoer voor meer informatie over de latentie voor het Azure storage-account. Ga voor gedetailleerde instructies naar [inschakelen en configureren van Storage Analytics](../storage/common/storage-enable-and-view-metrics.md). Als deze latenties ook zijn hoog en vergelijkbaar is met de getallen die u hebt ontvangen van het hulpprogramma StorSimple Diagnostics, moet u een serviceaanvraag registreren met Azure storage.

    2. Als de storage-account latenties laag zijn, neem dan contact op met de netwerkbeheerder voor het onderzoeken van latentieproblemen met in uw netwerk.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Voorbeeld van de uitvoer van de prestatietest uitvoeren op een 8100-apparaat

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a>Bijlage: interpreteren systeemgegevens

Hier volgt een tabel met een beschrijving van wat de verschillende Windows PowerShell-parameters in het systeem gegevens toegewezen aan. 

| PowerShell Parameter    | Description  |
|-------------------------|------------------|
| Exemplaar-id             | Elke domeincontroller heeft een unieke id of een GUID die ermee verbonden zijn.|
| Name                    | De beschrijvende naam van het apparaat geconfigureerd via Azure portal tijdens de implementatie van het apparaat. De beschrijvende naam die standaard is het serienummer van het apparaat. |
| Model                   | Het model van de StorSimple 8000-apparaat. Het model is 8100 of 8600.|
| Serienummer            | Het serienummer van het apparaat is toegewezen in de fabriek en 15 tekens lang is. Bijvoorbeeld, 8600-SHX0991003G44HT geeft aan dat:<br> 8600 – is het Apparaatmodel.<br>VX – Is de productie-site.<br> 0991003 - is een specifiek product. <br> G44HT:-de laatste 5 cijfers voor het maken van de unieke serienummers worden verhoogd. Dit kan niet een opeenvolgende reeks zijn.|
| TimeZone                | De apparaat-tijdzone zoals geconfigureerd in de Azure portal tijdens de implementatie van het apparaat.|
| CurrentController       | De controller die u aan zijn gekoppeld via de Windows PowerShell-interface van uw StorSimple-apparaat.|
| ActiveController        | De controller die actief is op uw apparaat en wordt alle netwerk- en schijfconfiguratie bewerkingen beheren. Dit kan Controller 0 of 1 van de domeincontroller zijn.  |
| Controller0Status       | De status van Controller 0 op uw apparaat. De status van de domeincontroller kan worden normaal, in de herstelmodus of niet bereikbaar is.|
| Controller1Status       | De status van Controller 1 op uw apparaat.  De status van de domeincontroller kan worden normaal, in de herstelmodus of niet bereikbaar is.|
| SystemMode              | De algehele status van uw StorSimple-apparaat. Status van het apparaat kan worden normaal, onderhoud, of uit bedrijf genomen (komt overeen met gedeactiveerd in Azure portal).|
| FriendlySoftwareVersion | De beschrijvende tekenreeks die overeenkomt met de versie van de apparaat-software. Voor een systeem waarop Update 4 wordt uitgevoerd, is de beschrijvende softwareversie StorSimple 8000 serie Update 4.0.|
| HcsSoftwareVersion      | De versie van de HCS software is uitgevoerd op uw apparaat. Bijvoorbeeld, is de versie van de HCS-software overeenkomt met de StorSimple 8000 serie Update 4.0 6.3.9600.17820. |
| ApiVersion              | De softwareversie van de Windows PowerShell-API van de HCS-apparaat.|
| VhdVersion              | De softwareversie van de installatiekopie van het factory die het apparaat is geleverd met. Als u uw apparaat op Fabrieksinstellingen terugzetten, vervolgens wordt deze uitgevoerd deze softwareversie.|
| OSVersion               | De softwareversie van het besturingssysteem Windows Server wordt uitgevoerd op het apparaat. De Windows Server 2012 R2 die overeenkomt met 6.3.9600 is gebaseerd op het StorSimple-apparaat.|
| CisAgentVersion         | De versie voor de agent van uw configuratie-items die wordt uitgevoerd op uw StorSimple-apparaat. Deze agent kan communiceren met de StorSimple Manager-service die wordt uitgevoerd in Azure.|
| MdsAgentVersion         | De versie overeenkomt met de Mds-agent die wordt uitgevoerd op uw StorSimple-apparaat. Deze agent worden gegevens verplaatst naar de bewaking en diagnostische gegevens voor de Service (MDS).|
| Lsisas2Version          | De versie overeenkomt met de LSI-stuurprogramma's op uw StorSimple-apparaat.|
| Capaciteit                | De totale capaciteit van het apparaat in bytes.|
| RemoteManagementMode    | Geeft aan of het apparaat op afstand kan worden beheerd via de Windows PowerShell-interface. |
| FipsMode                | Geeft aan of de Verenigde Staten Federal Information Processing Standard (FIPS)-modus is ingeschakeld op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door de Amerikaanse federale overheid computersystemen voor de beveiliging van gevoelige gegevens. Voor apparaten met Update 4 of hoger, is FIPS-modus standaard ingeschakeld. |

## <a name="next-steps"></a>Volgende stappen

* Informatie over de [syntaxis van de cmdlet Invoke-HcsDiagnostics](https://technet.microsoft.com/library/mt795371.aspx).

* Meer informatie over het [probleemoplossing voor implementatieproblemen](storsimple-troubleshoot-deployment.md) op uw StorSimple-apparaat.
