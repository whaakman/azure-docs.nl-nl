---
title: Diagnostische hulpprogramma om op te lossen StorSimple 8000 apparaat | Microsoft Docs
description: Beschrijft de modus voor de StorSimple-apparaat en wordt uitgelegd hoe u Windows PowerShell voor StorSimple om de apparatuurmodus te wijzigen.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 8fae7bb357f8e5e8eff249edfe3a2aaafe04283c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>Gebruik het diagnostisch hulpprogramma van StorSimple 8000 series apparaat problemen kunt oplossen

## <a name="overview"></a>Overzicht

Het hulpprogramma StorSimple diagnostische gegevens diagnoses problemen met de status van de onderdeel-systeem, prestaties, netwerk en hardware voor een StorSimple-apparaat. Het diagnostisch hulpprogramma kan worden gebruikt in verschillende scenario's. Deze scenario's omvatten werkbelasting planning, implementatie van een StorSimple-apparaat, beoordeling van de netwerkomgeving en bepalen van de prestaties van een operationeel apparaat. In dit artikel biedt een overzicht van het hulpprogramma Diagnostische gegevens en beschrijft hoe het hulpprogramma kan worden gebruikt met een StorSimple-apparaat.

Het diagnostisch hulpprogramma is voornamelijk bedoeld voor StorSimple 8000 series on-premises apparaten (8100 en 8600).

## <a name="run-diagnostics-tool"></a>Diagnostische hulpprogramma uitvoeren

Dit hulpprogramma kan worden uitgevoerd via de Windows PowerShell-interface van uw StorSimple-apparaat. Er zijn twee manieren toegang krijgen tot de lokale interface van uw apparaat:

* [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Externe toegang tot het hulpprogramma via de Windows PowerShell voor StorSimple](storsimple-remote-connect.md).

In dit artikel gaan we ervan uit dat u hebt verbonden met de seriële console van het apparaat via de PuTTY.

#### <a name="to-run-the-diagnostics-tool"></a>Het diagnostisch hulpprogramma uitvoeren

Zodra u hebt gekoppeld aan de Windows PowerShell-interface van het apparaat, moet u de volgende stappen om te worden uitgevoerd van de cmdlet uitvoeren.
1. Meld u aan bij de seriële console van het apparaat door de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Typ de volgende opdracht:

    `Invoke-HcsDiagnostics`

    Als de scope-parameter niet is opgegeven, wordt de diagnostische tests uitgevoerd door de cmdlet. Deze tests omvatten system, status voor hardware-onderdeel, netwerk en prestaties. 
    
    Geef voor het uitvoeren van alleen specifieke test, de scope-parameter. Bijvoorbeeld, u alleen de Netwerktest uitvoeren te typen

    `Invoke-HcsDiagnostics -Scope Network`

3. Selecteer en kopieer de uitvoer van het venster PuTTY naar een tekstbestand voor verdere analyse.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Scenario's met het hulpprogramma voor diagnostische gegevens

Gebruik het diagnostisch hulpprogramma oplossen van problemen met de status van het netwerk, prestaties, systeem en hardware van het systeem. Hier volgen enkele mogelijke scenario's:

* **Apparaat offline** -uw StorSimple 8000 series apparaat offline is. Echter, uit de Windows PowerShell-interface het lijkt erop dat beide domeincontrollers actief en werkend zijn.
    * Dit hulpprogramma kunt u vervolgens de netwerk-status te bepalen.
         
         > [!NOTE]
         > Gebruik dit hulpprogramma niet om u te evalueren prestatie- en instellingen op een apparaat voordat u de registratie (of configureren via de wizard setup). Een geldig IP-adres is toegewezen aan het apparaat tijdens de wizard setup en registratie. U kunt deze cmdlet uitvoeren op een apparaat dat niet is geregistreerd voor de status van de hardware- en systeem. Gebruik bijvoorbeeld de bereikparameter:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Problemen met de permanente apparaat** -u ondervindt problemen met apparaten die goed lijken te behouden. Bijvoorbeeld, de registratie is mislukt. U kan ook worden ondervindt problemen met apparaat nadat het apparaat is geregistreerd en operationeel een tijdje is.
    * In dit geval moet u dit hulpprogramma gebruiken voor voorlopige probleemoplossing voordat u zich een serviceaanvraag met Microsoft Support aanmeldt. U wordt aangeraden dat u dit hulpprogramma uitvoeren en vastleggen van de uitvoer van dit hulpprogramma. U kunt vervolgens deze uitvoer bieden ondersteuning voor snellere het oplossen van problemen.
    * Als er een onderdeel of het cluster hardwarefouten, moet u een ondersteuningsaanvraag aanmelden.

* **Lage Apparaatprestaties** -uw StorSimple-apparaat traag is.
    * In dit geval moet u deze cmdlet uitvoeren met bereikparameter ingesteld op prestaties. Analyseer de uitvoer. Krijgt u de cloud lezen-schrijven latenties. De gerapporteerde latenties gebruiken als het doel van maximale haalbare, rekening te houden in enige overhead voor het verwerken van de interne gegevens en vervolgens implementeert u de werkbelasting op het systeem. Ga voor meer informatie naar [de Netwerktest gebruiken om op te lossen Apparaatprestaties](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Diagnostische test en voorbeeld-uitvoer

### <a name="hardware-test"></a>Hardwaretest

Deze test bepaalt de status van de hardware-onderdelen, het USM firmware en de firmware van de schijf op het systeem uitgevoerd.

* De hardwareonderdelen die zijn gerapporteerd, zijn deze onderdelen die de test mislukt of zijn niet aanwezig in het systeem.
* De USM firmware- en firmware-versies worden gerapporteerd voor de Controller 0, 1 van de domeincontroller, en gedeelde onderdelen in uw systeem. Voor een volledige lijst van hardware-onderdelen, gaat u naar:

    * [Primaire behuizing-onderdelen](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [EBOD behuizing-onderdelen](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Als de hardwaretest niet functionerende onderdelen, rapporteert [Meld u bij een serviceaanvraag met Microsoft Support](storsimple-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Voorbeeld van uitvoer van de hardwaretest uitgevoerd op een 8100-apparaat

Hier volgt een voorbeeld van uitvoer van een StorSimple 8100-apparaat. De behuizing EBOD is niet aanwezig in het model 8100-apparaat. Daarom worden de onderdelen van de domeincontroller EBOD niet gemeld.

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

Deze test rapporteert de systeeminformatie, de updates die beschikbaar zijn, de clustergegevens en de gegevens van de service voor uw apparaat.

* De systeeminformatie omvat het model, apparaatserienummer tijdzone, status van de domeincontroller en de versie van de gedetailleerde software op het systeem. Voor informatie over de verschillende parameters voor het gerapporteerd als de uitvoer, gaat u naar [systeemgegevens interpreteren](#appendix-interpreting-system-information).

* De beschikbaarheid van de rapporten of de reguliere en onderhoud modi beschikbaar zijn en de namen van de bijbehorende pakket. Als `RegularUpdates` en `MaintenanceModeUpdates` zijn `false`, dit geeft aan dat de updates niet beschikbaar zijn. Uw apparaat is bijgewerkt.
* De cluster-informatie bevat de informatie over de verschillende logische onderdelen van de groepen voor de cluster HCS en hun respectieve statussen. Als u een offline clustergroep in deze sectie van het rapport ziet [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).
* De service-informatie bevat de namen en de status van alle HCS en CiS services uitgevoerd op uw apparaat. Deze informatie is nuttig voor het Microsoft Support bij het oplossen van het probleem van het apparaat.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Voorbeeld van uitvoer van systeem test uitgevoerd op een 8100-apparaat

Hier volgt een voorbeeld van uitvoer van de systeem-test uitvoeren op een 8100-apparaat.

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
hcs_management_servic         Online HCS Cluster Group
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

### <a name="network-test"></a>Netwerktest

Deze test controleert de status van de netwerkinterfaces, poorten, DNS- en NTP serververbindingen, SSL-certificaat, opslagaccountreferenties, verbinding met de Update-servers en web proxy connectiviteit op uw StorSimple-apparaat.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Voorbeeld van uitvoer van het netwerk testen wanneer alleen DATA0 is ingeschakeld

Hier volgt een voorbeeld van uitvoer van het 8100-apparaat. U kunt zien in de uitvoer die:
* Alleen DATA 0 en 1 gegevens netwerkinterfaces zijn ingeschakeld en geconfigureerd.
* GEGEVENS-2-5 zijn niet ingeschakeld in de portal.
* De configuratie van DNS-server is geldig en het apparaat verbinding kan maken via de DNS-server.
* De connectiviteit NTP-server is ook geen probleem.
* Poorten 80 en 443 zijn geopend. Poort 9354 is geblokkeerd. Op basis van de [netwerk systeemvereisten](storsimple-system-requirements.md), moet u deze poort voor de service bus-communicatie te openen.
* Het SSL-certificaat is ongeldig.
* Het apparaat verbinding kan maken met de opslagaccount: _myss8000storageacct_.
* De verbinding met de Update-servers is ongeldig.
* De webproxy is niet geconfigureerd op dit apparaat.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Voorbeeld van uitvoer van Netwerktest wanneer DATA0 en bestand1 zijn ingeschakeld

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

Deze test rapporteert de prestaties van de cloud via de cloud lezen-schrijven latenties voor uw apparaat. Dit hulpprogramma kan worden gebruikt om een basislijn voor de prestaties van de cloud die u met StorSimple bereiken kunt stand te brengen. Het hulpprogramma rapporteert de maximale prestaties (beste scenario voor lezen-schrijven latenties) die u voor de verbinding ophalen kunt.

Als het hulpprogramma de maximale haalbare prestaties meldt, kunnen we de gerapporteerde latenties voor lezen-schrijven gebruiken als doelen bij het implementeren van de werkbelastingen.

De test simuleert de grootte van de blob die is gekoppeld aan de typen ander volume op het apparaat. Gewone lagen en back-ups van lokaal vastgemaakte volumes met een blob-grootte van 64 KB. Gelaagde volumes met archief-optie ingeschakeld gebruiken blob-gegevensgrootte 512 KB. Als het apparaat heeft gelaagde en lokaal vastgemaakte volumes geconfigureerd, alleen de test die overeenkomt met de blob van 64 KB gegevensgrootte wordt uitgevoerd.

Om dit hulpprogramma, moet u de volgende stappen uitvoeren:

1.  Maak eerst een combinatie van gelaagde volumes en gelaagde volumes met gearchiveerde optie ingeschakeld. Deze actie zorgt ervoor dat het hulpprogramma voor de tests voor 64 KB en 512 KB grootten van de blob.

2. De cmdlet uitvoeren nadat u hebt gemaakt en de volumes geconfigureerd. Type:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Noteer de latenties lezen-schrijven is gemeld door het hulpprogramma. Deze test kan enige tijd duren om uit te voeren voordat het rapporteert de resultaten.

4. Als de verbinding latenties allemaal onder het verwachte bereik zijn, kunnen klikt u vervolgens de latenties gemeld door het hulpprogramma worden gebruikt als maximale haalbare doel bij het implementeren van de werkbelastingen. Rekening te houden in enige overhead voor interne gegevensverwerking.

    Als de latenties lezen-schrijven is gemeld door het diagnostisch hulpprogramma hoog zijn:

    1. Storage Analytics voor blob-services configureren en analyseren van de uitvoer voor inzicht in de latenties voor de Azure storage-account. Ga voor gedetailleerde instructies naar [inschakelen en configureren van opslag Analytics](../storage/common/storage-enable-and-view-metrics.md). Als deze latenties ook hoge en vergelijkbaar is met de getallen die u hebt ontvangen van het hulpprogramma StorSimple diagnostische gegevens, moet u een serviceaanvraag registreren met Azure storage.

    2. De storage-account latenties lage neemt contact op met de netwerkbeheerder om te onderzoeken latentieproblemen in uw netwerk.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Voorbeeld van uitvoer van de prestatietest uitvoeren op een 8100-apparaat

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

## <a name="appendix-interpreting-system-information"></a>Bijlage: systeemgegevens interpreteren

Hier volgt een tabel met een beschrijving van wat de verschillende Windows PowerShell-parameters in het systeem informatie toegewezen aan. 

| PowerShell-Parameter    | Beschrijving  |
|-------------------------|------------------|
| Exemplaar-id             | Elke domeincontroller heeft een unieke id of een GUID die is gekoppeld aan.|
| Naam                    | De beschrijvende naam van het apparaat zoals geconfigureerd via de Azure portal tijdens de implementatie van het apparaat. De beschrijvende naam van de standaard is het serienummer van het apparaat. |
| Model                   | Het model van uw StorSimple 8000 serie-apparaat. Het model is 8100 of 8600.|
| Serienummer            | Het serienummer van het apparaat in de fabriek is toegewezen en 15 tekens lang is. Bijvoorbeeld: 8600 SHX0991003G44HT Hiermee wordt aangegeven:<br> 8600 – is het model.<br>SHX – Is de productie-site.<br> 0991003 - is een specifiek product. <br> G44HT--de laatste 5 cijfers voor het maken van unieke serienummers worden verhoogd. Dit kan niet een sequentiële set zijn.|
| Tijdzone                | De apparaat-tijdzone zoals geconfigureerd in de Azure-portal tijdens de implementatie van het apparaat.|
| CurrentController       | De controller waaraan u via de Windows PowerShell-interface van uw StorSimple-apparaat verbonden bent.|
| ActiveController        | De domeincontroller die actief is op uw apparaat en alle bewerkingen van de netwerk- en beheerd. Dit kan zijn Controller 0 of 1 van de domeincontroller.  |
| Controller0Status       | De status van de Controller 0 op uw apparaat. De status van de domeincontroller kan normale in herstelmodus is of niet bereikbaar zijn.|
| Controller1Status       | De status van de Controller 1 op uw apparaat.  De status van de domeincontroller kan normale in herstelmodus is of niet bereikbaar zijn.|
| SystemMode              | De algehele status van uw StorSimple-apparaat. Status van het apparaat kan worden normaal, onderhoud, of buiten gebruik gestelde (komt overeen met gedeactiveerd in de Azure portal).|
| FriendlySoftwareVersion | De beschrijvende tekenreeks die overeenkomt met de versie van de software apparaat. Voor een systeem met Update 4, is de beschrijvende softwareversie StorSimple 8000 Series Update 4.0.|
| HcsSoftwareVersion      | De versie van de HCS software is uitgevoerd op uw apparaat. Bijvoorbeeld, wordt de softwareversie HCS die overeenkomt met de StorSimple 8000 Series Update 4.0 6.3.9600.17820. |
| ApiVersion              | De softwareversie van de Windows PowerShell-API van het apparaat HCS.|
| VhdVersion              | De softwareversie van de factory-installatiekopie die met het apparaat is verzonden. Als u uw apparaat opnieuw op de standaardwaarden, voert deze de softwareversie van deze.|
| OSVersion               | De softwareversie van het besturingssysteem Windows Server wordt uitgevoerd op het apparaat. Het StorSimple-apparaat is gebaseerd op Windows Server 2012 R2 die overeenkomt met 6.3.9600.|
| CisAgentVersion         | De versie voor uw configuratie-items agent wordt uitgevoerd op uw StorSimple-apparaat. Deze agent kan communiceren met de StorSimple Manager-service worden uitgevoerd in Azure.|
| MdsAgentVersion         | De versie die overeenkomt met de Mds-agent uitgevoerd op uw StorSimple-apparaat. Deze agent verplaatst gegevens naar de controle en diagnostische gegevens Service (MDS).|
| Lsisas2Version          | De versie die overeenkomt met de LSI stuurprogramma's op uw StorSimple-apparaat.|
| Capaciteit                | De totale capaciteit van het apparaat in bytes.|
| RemoteManagementMode    | Hiermee wordt aangegeven of het apparaat kan extern worden beheerd via de Windows PowerShell-interface. |
| FipsMode                | Hiermee wordt aangegeven of de Verenigde Staten Federal Information Processing Standard (FIPS)-modus is ingeschakeld op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door ons Federal government computersystemen voor de bescherming van gevoelige gegevens. FIPS-modus is standaard ingeschakeld voor apparaten met Update 4 of hoger is. |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [syntaxis van de cmdlet Invoke-HcsDiagnostics](https://technet.microsoft.com/library/mt795371.aspx).

* Meer informatie over het [implementatieproblemen oplossen](storsimple-troubleshoot-deployment.md) op uw StorSimple-apparaat.
