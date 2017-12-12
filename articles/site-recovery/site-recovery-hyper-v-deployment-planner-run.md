---
title: Azure Site Recovery-implementatieplanner voor Hyper-V naar Azure | Microsoft Docs
description: In dit artikel wordt het scenario beschreven voor het gebruik van de Azure Site Recovery-implementatieplanner voor Hyper-V naar Azure.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: bb4ec5cfd455ab0cc22ab693c2a07eed9883dc76
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="run-azure-site-recovery-deployment-planner-for-hyper-v-to-azure"></a>De Azure Site Recovery-implementatieplanner voor Hyper-V naar Azure uitvoeren

## <a name="modes-of-running-deployment-planner"></a>Modi waarin de implementatieplanner kan worden uitgevoerd
U kunt het opdrachtregelprogramma (ASRDeploymentPlanner.exe) in de volgende vier modi uitvoeren: 
1.  [VM-lijst ophalen](#get-vm-list-for-profiling-hyper-v-vms)
2.  [Profileren](#profile-hyper-v-vms)
3.  [Rapporten genereren](#generate-report)
4.  [Doorvoer bepalen](#get-throughput)

Voer eerst het hulpprogramma uit voor het ophalen van de lijst met virtuele machines van een of meer Hyper-V-hosts.  Voer daarna het hulpprogramma uit in de profileringsmodus om gegevensverloop en IOPS te verzamelen voor de virtuele machine. Voer het hulpprogramma daarna uit om het rapport te genereren en de netwerkbandbreedte en opslagvereisten te bepalen.

## <a name="get-vm-list-for-profiling-hyper-v-vms"></a>VM-lijst ophalen voor het profileren van virtuele Hyper-V-machines
U moet eerst een lijst maken met de virtuele machines die u wilt profileren. Gebruik de GetVMList-modus van de implementatieplanner om met een enkele opdracht de lijst te genereren met VM's die aanwezig zijn op meerdere Hyper-V-hosts. Zodra u de volledige lijst hebt gegenereerd, kunt u de virtuele machines die u niet wilt profileren uit het uitvoerbestand verwijderen. Gebruik het uitvoerbestand vervolgens voor alle overige bewerkingen, zoals profileren, rapporten genereren en het bepalen van de doorvoer.

U kunt de VM-lijst genereren waarmee het hulpprogramma wordt verwezen naar een Hyper-V-cluster, naar een zelfstandige Hyper-V-host of naar een combinatie daarvan.

### <a name="command-line-parameters"></a>Opdrachtregelparameters
Hieronder ziet u een tabel met verplichte en optionele parameters van het hulpprogramma, die u kunt uitvoeren in de GetVMList-modus. 
```
ASRDeploymentPlanner.exe -Operation GetVMList /?
```
| Parameternaam | Beschrijving |
|---|---|
| -Operation | GetVMList |
| -User | De naam van de gebruiker om verbinding te maken met de Hyper-V-host of het Hyper-V-cluster. De gebruiker moet beheerderstoegang hebben.|
|-ServerListFile | Het bestand met de lijst met servers met de virtuele machines die geprofileerd moeten worden. Het bestandspad kan absoluut of relatief zijn. Dit bestand moet op elke regel een van de volgende items bevatten:<ul><li>Naam of IP-adres van Hyper-V-host</li><li>Naam of IP-adres van Hyper-V-cluster</li></ul><br>Het bestand ServerList.txt bevat bijvoorbeeld de volgende servers:<ul><li>Host_1</li><li>10.8.59.27</li><li>Cluster_1</li><li>Host_2</li>|
| -Directory|(Optioneel) Het UNC-pad (Universal Naming Convention) of het pad naar de lokale directory voor het opslaan van gegevens die tijdens deze bewerking worden gegenereerd. Als er geen directory wordt opgegeven, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory.|
|-OutputFile| (Optioneel) Het bestand waarin de lijst met virtuele machines die zijn opgehaald uit de opgegeven Hyper-V-servers, wordt opgeslagen. Als de naam niet wordt vermeld, worden de details opgeslagen in VMList.txt.  Gebruik het bestand om met de profilering te starten nadat u de virtuele machines hebt verwijderd die niet hoeven te worden geprofileerd.|
|-Password|(Optioneel) Het wachtwoord om verbinding te maken met de Hyper-V-host.   Als u hier geen wachtwoord opgeeft, wordt u erom gevraagd wanneer de opdracht wordt uitgevoerd.|

### <a name="how-does-getvmlist-discovery-work"></a>Hoe werkt de detectie van GetVMList?
**Hyper-V-cluster**: als de naam van het Hyper-V-cluster wordt vermeld in het bestand met de serverlijst, vindt het hulpprogramma alle Hyper-V-knooppunten van het cluster en worden de virtuele machines opgehaald die aanwezig zijn op elk van de Hyper-V-hosts.

**Hyper-V-host**: als er een naam voor de Hyper-V-host wordt vermeld, controleert het hulpprogramma eerst of deze tot een cluster behoort. Zo ja, dan worden de knooppunten opgehaald die tot het cluster behoren. Vervolgens worden de virtuele machines van elke Hyper-V-host opgehaald. 

U kunt er ook voor kiezen om de beschrijvende namen of IP-adressen van de virtuele machines die u wilt profileren, handmatig toe te voegen aan een bestand.

Open het uitvoerbestand in Kladblok en kopieer de namen van alle virtuele machines die u wilt profileren naar een ander bestand (bijvoorbeeld ProfileVMList.txt). Zet elke naam op een afzonderlijke regel. Dit bestand wordt gebruikt als invoer voor de parameter -VMListFile voor alle overige bewerkingen, zoals profileren, rapporten genereren en het bepalen van de doorvoer.

### <a name="example-1-to-store-the-list-of-vms-in-a-file"></a>Voorbeeld 1: de lijst met virtuele machines opslaan in een bestand
```
ASRDeploymentPlanner.exe -Operation GetVMlist -ServerListFile “E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1 -OutputFile "E:\Hyper-V_ProfiledData\VMListFile.txt"
```

### <a name="example-2-to-store-the-list-of-vms-at-the-default-location-ie--directory-path"></a>Voorbeeld 2: de lijst met virtuele machines opslaan op de standaardlocatie d.w.z. het pad -Directory
```
ASRDeploymentPlanner.exe -Operation GetVMList -Directory "E:\Hyper-V_ProfiledData" -ServerListFile "E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1
```

## <a name="profile-hyper-v-vms"></a>Virtuele Hyper-V-machines profileren
In de profileringsmodus maakt de implementatieplanner verbinding met elk van de Hyper-V hosts om prestatiegegevens te verzamelen over de virtuele machines. 

* Profileren heeft geen invloed op de prestaties van de virtuele machines in de productieomgeving omdat er geen rechtstreekse verbinding met de machines wordt gemaakt. Alle prestatiegegevens worden verzameld vanaf de Hyper-V-host.
* Het hulpprogramma verzendt elke 15 seconden een query naar de Hyper-V-host zodat de profilering nauwkeurig verloopt, en slaat voor elke minuut de gemiddelde waarde op van de prestatiemeteritems.
* Het hulpprogramma handelt naadloos de VM-migratie af van het ene knooppunt naar een ander in het cluster, en de opslagmigratie binnen een host.

### <a name="get-vm-list-to-profile"></a>VM-lijst voor profilering ophalen
Raadpleeg de bewerking [GetVMList](#get-vm-list-for-profiling-hyper-v-vms) als u een lijst wilt maken met virtuele machines die geprofileerd moeten worden.

Als u de lijst met te profileren virtuele machines hebt opgesteld, kunt u het hulpprogramma uitvoeren in de profileringsmodus. 

### <a name="command-line-parameters"></a>Opdrachtregelparameters
Hieronder ziet u een tabel met verplichte en optionele parameters van het hulpprogramma die u kunt uitvoeren in de profileringsmodus. Het hulpprogramma wordt veel gebruikt in VMware-naar-Azure- en Hyper-V-naar-Azure-scenario's. De volgende parameters zijn van toepassing op Hyper-V. 
```
ASRDeploymentPlanner.exe -Operation StartProfiling /?
```
| Parameternaam | Beschrijving |
|---|---|
| -Operation | StartProfiling |
| -User | De naam van de gebruiker om verbinding te maken met de Hyper-V-host of het Hyper-V-cluster. De gebruiker moet beheerderstoegang hebben.|
| -VMListFile | Het bestand met de lijst met virtuele machines die moeten worden geprofileerd. Het bestandspad kan absoluut of relatief zijn. Voor Hyper-V is dit het uitvoerbestand voor de bewerking GetVMList. Als u de voorbereiding handmatig uitvoert, moet het bestand een servernaam of IP-adres bevatten, gevolgd door de naam van de VM, gescheiden door een \ per regel. De naam van de virtuele machine die in het bestand is opgegeven, moet gelijk zijn aan de naam van de virtuele machine op de Hyper-V host.<ul>Het bestand VMList.txt bevat bijvoorbeeld de volgende virtuele machines:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-NoOfMinutesToProfile|Het aantal minuten dat profilering moet worden uitgevoerd. Minimaal is 30 minuten.|
|-NoOfHoursToProfile|Het aantal uur dat profilering moet worden uitgevoerd.|
|-NoOfDaysToProfile |Het aantal dagen dat profilering moet worden uitgevoerd. U doet er verstandig aan om profilering langer dan 7 dagen uit te voeren om ervoor te zorgen dat het workloadpatroon in uw omgeving in de opgegeven periode goed wordt aangehouden, zodat er een nauwkeurige aanbeveling kan worden gedaan.|
|-Virtualization|Geef het type virtualisatie op (VMware of Hyper-V).|
|-Directory|(Optioneel) Het UNC-pad (Universal Naming Convention) of het pad naar de lokale directory voor het opslaan van gegevens die tijdens de profilering worden gegenereerd. Als er geen directory wordt opgegeven, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory.|
|-Password|(Optioneel) Het wachtwoord om verbinding te maken met de Hyper-V-host. Als u dit nu niet opgeeft, wordt u er later om gevraagd wanneer de opdracht wordt uitgevoerd.|
|-StorageAccountName|(Optioneel) De naam van het opslagaccount dat wordt gebruikt om de bereikbare doorvoer te vinden voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de doorvoer te berekenen.|
|-StorageAccountKey|(Optioneel) De sleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > <Storage account name> > Instellingen > Toegangssleutels > Key1 (of primaire toegangssleutel voor klassieke opslagaccount).|
|-Environment|(Optioneel) Dit is uw doelomgeving voor het Azure Storage-account. Dit kan een van de volgende drie waarden zijn: AzureCloud, AzureUSGovernment, AzureChinaCloud. De standaardwaarde is AzureCloud. Gebruik de parameter wanneer uw Azure-doelregio de clouds voor Azure van de Amerikaanse overheid of Azure voor China is.|

Het is raadzaam om de virtuele machines gedurende meer dan 7 dagen te profileren. Als het verlooppatroon varieert binnen een maand, raden wij u aan de profilering door de week uit te voeren wanneer zich het maximale verloop voordoet. Het beste is het om de profilering gedurende 31 dagen uit te voeren, omdat u dan een betere aanbeveling krijgt. Gedurende de periode voor profilering blijft ASRDeploymentPlanner.exe actief. Het hulpprogramma noteert de invoer van de profileringstijd in dagen. Voor het uitvoeren van een snelle test of concepttest van het hulpprogramma, kunt u een profilering van een paar uur of van een paar minuten uitvoeren. De minimaal toegestane tijd voor profilering is 30 minuten. 

Tijdens het profileren kunt u eventueel de naam en sleutel van een opslagaccount doorgeven om vast te stellen wat de doorvoer is die Azure Site Recovery kan behalen op het moment van replicatie vanaf de Hyper-V server naar Azure. Als tijdens de profilering geen accountnaam en -sleutel worden doorgegeven, wordt er geen bereikbare doorvoer berekend.

U kunt meerdere exemplaren van het hulpprogramma uitvoeren voor verschillende sets virtuele machines. Zorg ervoor dat de namen van de virtuele machines niet worden herhaald in een van de profileringssets. Als u bijvoorbeeld tien virtuele machines hebt geprofileerd (VM1 tot en met VM10) en na een paar dagen vijf andere virtuele machines wilt profileren (VM11 tot en met VM15), kunt u voor de tweede set virtuele machines (VM11 tot en met VM15) het hulpprogramma uitvoeren vanuit een andere opdrachtregelconsole. Zorg er wel voor dat de tweede set virtuele machines geen namen bevat van de eerste reeks geprofileerde virtuele machines of gebruik voor de tweede set een andere uitvoerdirectory. Als er voor het profileren van dezelfde virtuele machines twee exemplaren van het hulpprogramma worden gebruikt en dezelfde uitvoerdirectory wordt gebruikt, is het gegenereerde rapport niet correct. 

De configuraties van de virtuele machines worden eenmaal aan het begin van de profilering vastgelegd en opgeslagen in een bestand met de naam VMDetailList.xml. Deze informatie wordt gebruikt bij het genereren van het rapport. Eventuele wijzigingen in de configuratie van een virtuele machine (bijvoorbeeld een groter aantal kerngeheugens, schijven of NIC's) vanaf het begin tot het einde van de profilering worden niet vastgelegd. Als de configuratie van een geprofileerde virtuele machine tijdens het profileren is gewijzigd, kunt u de volgende oplossing gebruiken om de laatste gegevens van de virtuele machine te achterhalen wanneer u het rapport genereert:

* Maak een back-up van VMdetailList.xml en verwijder het bestand van de huidige locatie.
* Geef argumenten voor -User en -Password door wanneer u het rapport genereert.

Met de profileringsopdracht worden meerdere bestanden gegenereerd in de directory voor profilering. Verwijder deze bestanden niet, aangezien dit gevolgen heeft voor het genereren van de rapporten.

### <a name="examples"></a>Voorbeelden
#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Voorbeeld 1: virtuele machines 30 dagen profileren en de doorvoer van on-premises naar Azure bepalen
```
ASRDeploymentPlanner.exe -Operation StartProfiling -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile 30 -User Contoso\HyperVUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Voorbeeld 2: virtuele machines 15 dagen profileren
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User contoso\HypreVUser1
```

#### <a name="example-3-profile-vms-for-60-minutes-for-a-quick-test-of-the-tool"></a>Voorbeeld 3: virtuele machines 60 minuten profileren om het hulpprogramma kort te testen
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfMinutesToProfile 60 -User Contoso\HyperVUser1
```

#### <a name="example-4-profile-vms-for-2-hours-for-a-proof-of-concept"></a>Voorbeeld 4: virtuele machines 2 uur profileren om het hulpprogramma kort aan een concepttest te onderwerpen
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfHoursToProfile 2 -User Contoso\HyperVUser1
```

>[OPMERKING]
>
* Als de server waarop het hulpprogramma wordt uitgevoerd, opnieuw wordt opgestart of is gecrasht, of als u het hulpprogramma afsluit met Ctrl + C, blijven de profileringsgegevens behouden. De kans bestaat echter wel dat de laatste 15 minuten aan geprofileerde gegevens ontbreekt. In dergelijke gevallen voert u het hulpprogramma opnieuw uit in de profileringsmodus nadat de server opnieuw is opgestart.
* Wanneer de accountnaam en -sleutel worden doorgegeven, meet het hulpprogramma de doorvoer tijdens de laatste stap van de profilering. Als het hulpprogramma wordt beëindigd voordat de profilering is voltooid, wordt de doorvoer niet berekend. Als u de doorvoer wilt weten voordat het rapport wordt gegenereerd, kunt u de bewerking GetThroughput uitvoeren vanuit de opdrachtregelconsole. Anders bevat het gegenereerde rapport geen informatie over de doorvoer.
* Azure Site Recovery biedt geen ondersteuning voor virtuele machines met iSCSI- en passthrough-schijven. Het hulpprogramma kan echter iSCSI en passthrough-schijven die zijn gekoppeld aan virtuele machines, niet detecteren.

## <a name="generate-report"></a>Rapport genereren
Het hulpprogramma genereert een Microsoft Excel-bestand met ingeschakelde macro's (XLSM-bestand) als de rapportuitvoer, met daarin een overzicht van alle aanbevelingen voor de implementatie. Het rapport heeft de naam DeploymentPlannerReport_<unique numeric identifier>.xlsm en wordt in de opgegeven directory geplaatst.
Nadat de profilering is voltooid, kunt u het hulpprogramma uitvoeren in de modus voor het genereren van een rapport. 

### <a name="command-line-parameters"></a>Opdrachtregelparameters
Hieronder ziet u een tabel met verplichte en optionele parameters van het hulpprogramma die u kunt uitvoeren in de profileringsmodus. Het hulpprogramma wordt veel gebruikt in VMware-naar-Azure- en Hyper-V-naar-Azure-scenario's. De volgende parameters zijn van toepassing op Hyper-V.
```
ASRDeploymentPlanner.exe -Operation GenerateReport /?
```
| Parameternaam | Beschrijving |
|---|---|
| -Operation | GenerateReport |
|-VMListFile | Het bestand met de lijst met geprofileerde virtuele machines waarvoor het rapport moet worden gegenereerd. Het bestandspad kan absoluut of relatief zijn. Voor Hyper-V is dit het uitvoerbestand voor de bewerking GetVMList. Als u de voorbereiding handmatig uitvoert, moet het bestand een servernaam of IP-adres bevatten, gevolgd door de naam van de VM, gescheiden door een \ per regel. De naam van de virtuele machine die in het bestand is opgegeven, moet gelijk zijn aan de naam van de virtuele machine op de Hyper-V host.<ul>Het bestand VMList.txt bevat bijvoorbeeld de volgende virtuele machines:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Virtualization|Geef het type virtualisatie op (VMware of Hyper-V).|
|-Directory|(Optioneel) De UNC-directory (universal naming convention) of de lokale directory waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Als er geen naam wordt opgegeven, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory.|
| -User | (Optioneel) De naam van de gebruiker om verbinding te maken met de Hyper-V-host of het Hyper-V-cluster. De gebruiker moet beheerderstoegang hebben.<br>De gebruikersnaam en het wachtwoord worden gebruikt voor het ophalen van de meest recente configuratiegegevens van de virtuele machines, zoals het aantal schijven, kernen, NIC's enz. die in het rapport moeten worden gebruikt. Indien niet opgegeven, worden de configuratiegegevens gebruikt die tijdens de profilering zijn verzameld.|
|-Password|(Optioneel) Het wachtwoord om verbinding te maken met de Hyper-V-host. Als u dit nu niet opgeeft, wordt u er later om gevraagd wanneer de opdracht wordt uitgevoerd.|
| -DesiredRPO | (Optioneel) Het gewenste beoogde herstelpunt (Recovery Point Objective [RPO]) in minuten. Standaard is dit 15 minuten.|
| -Bandwidth | (Optioneel) De bandbreedte in Mbps. De parameter voor het berekenen van de RPO die voor de opgegeven bandbreedte kan worden bereikt. |
| -StartDate | (Optioneel) De begindatum en -tijd in de notatie MM-DD-JJJJ:UU:MM (in 24-uursindeling). Als u *StartDate* opgeeft, moet u ook *EndDate* opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| -EndDate | (Optioneel) De einddatum en -tijd in de notatie MM-DD-JJJJ:UU:MM (in 24 uursindeling). Als u *EndDate* opgeeft, moet u ook *StartDate* opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| -GrowthFactor | (Optioneel) De groeifactor, uitgedrukt als een percentage. De standaardwaarde is 30 procent. |
| -UseManagedDisks | (Optioneel) UseManagedDisks - Ja/Nee. Standaard is Ja. Het aantal virtuele machines dat kan worden ondergebracht in een enkel opslagaccount, wordt berekend afgaande op het feit of de Failover/Test-testfailover van virtuele machines is uitgevoerd op een beheerde schijf in plaats van op een niet-beheerde schijf. |
|-SubscriptionId |(Optioneel) De abonnement-GUID. Gebruik deze parameter om het kostenramingsrapport te genereren met de meest recente prijzen op basis van uw abonnement, de aanbieding die aan uw abonnement is gekoppeld en voor de opgegeven Azure-doelregio in de opgegeven valuta.|
|-TargetRegion|(Optioneel) De Azure-regio waarop de replicatie is gericht. Omdat er voor Azure verschillende kosten per regio worden berekend, moet u voor het genereren van dit rapport deze parameter met een specifieke Azure-doelregio gebruiken.<br>De standaardwaarde is WestUS2, of de laatst gebruikte doelregio.<br>Raadpleeg de lijst met [ondersteunde doelregio's](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions).|
|-OfferId|(Optioneel) De aanbieding is gekoppeld aan het vermelde abonnement. De standaardwaarde is MS-AZR-0003P (betalen naar gebruik).|
|-Currency|(Optioneel) De valuta waarin de kosten in het gegenereerde rapport worden weergegeven. Standaard is Amerikaanse Dollar ($) of de laatst gebruikte valuta.<br>Raadpleeg de lijst met [ondersteunde valuta's](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).|

### <a name="examples"></a>Voorbeelden
#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Voorbeeld 1: een rapport met standaardwaarden genereren wanneer de geprofileerde gegevens zich op de lokale schijf bevinden
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Voorbeeld 2: een rapport genereren wanneer de geprofileerde gegevens zich op een externe server bevinden
De gebruiker moet lees-/schrijftoegang hebben voor de externe directory.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V - -Directory “\\PS1-W2K12R2\Hyper-V_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-that-you-will-be-provision-for-the-replication"></a>Voorbeeld 3: een rapport genereren met een specifieke bandbreedte die u voor de replicatie gaat inrichten
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -Bandwidth 100
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Voorbeeld 4: een rapport genereren met een groeifactor van 5% in plaats van de standaardwaarde 30% 
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Voorbeeld 5: een rapport genereren met een subset geprofileerde gegevens
U hebt gedurende 30 dagen gegevens geprofileerd en u wilt een rapport genereren voor slechts 20 van de 30 dagen.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minutes-rpo"></a>Voorbeeld 6: een rapport genereren voor 5 minuten RPO
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

#### <a name="example-7-generate-a-report-for-south-india-azure-region-with-indian-rupee-and-specific-offer-id"></a>Voorbeeld 7: een rapport genereren voor de Azure-regio Zuid-India met de Indiase roepie en een specifieke aanbiedings-ID
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -SubscriptionID 4d19f16b-3e00-4b89-a2ba-8645edf42fe5 -OfferID MS-AZR-0148P -TargetRegion southindia -Currency INR
```

## <a name="percentile-value-used-for-the-calculation"></a>Percentielwaarde die voor de berekening wordt gebruikt
**Welke standaardpercentielwaarde met betrekking tot de prestatiegerelateerde metrische gegevens die tijdens de profilering zijn verzameld, wordt tijdens het genereren van het rapport gebruikt?**

Het hulpprogramma wordt standaard ingesteld op de 95e-percentielwaarde van de IOPS voor lezen/schrijven, de IOPS voor schrijven en de gegevensverloop die tijdens het profileren van de virtuele machines zijn verzameld. Deze waarde zorgt ervoor dat een piek (100e-percentielwaarde) die vanwege tijdelijke gebeurtenissen kan ontstaan op virtuele machines, niet wordt gebruikt om de vereisten te bepalen van het doel-opslagaccount en de bron-bandbreedte. Voorbeelden van tijdelijke gebeurtenissen zijn het één keer per dag uitvoeren van een back-up, het periodiek indexeren van een database, het genereren van analyserapporten en andere vergelijkbare activiteiten die op een bepaald moment actief zijn.

Het gebruik van de 95e-percentielwaarde biedt een waarheidsgetrouw beeld van de werkelijke workloadkenmerken en verzekert u van optimale prestaties wanneer deze workloads in Azure worden uitgevoerd. We verwachten niet dat u deze waarde hoeft te wijzigen. Als u de waarde toch wilt wijzigen (bijvoorbeeld op het 90e percentiel), kunt u het configuratiebestand ASRDeploymentPlanner.exe.config in de standaardmap bijwerken en opslaan om een nieuw rapport te genereren op basis van de bestaande geprofileerde gegevens.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Overwegingen met betrekking tot de groeifactor
**Waarom moet ik rekening houden met een groeifactor bij het plannen van implementaties?**
Het is essentieel dat u in uw workloadkenmerken rekening houdt met groei (mogelijke toename in gebruik na verloop van tijd). Als de beveiliging eenmaal is ingeschakeld en uw workloadkenmerken vervolgens veranderen, is het namelijk niet mogelijk om over te schakelen naar een ander opslagaccount voor beveiliging zonder eerst de beveiliging uit te schakelen en opnieuw in te schakelen.

Stel dat uw virtuele machine op dit moment voldoende heeft aan een Standard Storage-replicatie-account. De kans is echter groot dat in de volgende drie maanden verschillende wijzigingen optreden:

* Het aantal gebruikers van de toepassing die wordt uitgevoerd op de virtuele machine neemt toe.
* Het toegenomen verloop op de virtuele machine dat hiervan het gevolg is, betekent dat de virtuele machine moet worden overgezet naar een Premium Storage-account om ervoor te zorgen dat Azure Site Recovery de replicatie kan blijven uitvoeren.
* Dit betekent dat u de beveiliging moet uitschakelen en vervolgens opnieuw moet inschakelen met een Premium Storage-account.

Het wordt ten zeerste aangeraden om bij het plannen van een implementatie rekening houdt met eventuele groei. Hoewel de standaardwaarde 30 procent is, bent u de expert wat betreft het gebruikspatroon en de geschatte groei van uw toepassing. U kunt deze waarde dan ook wijzigen als u een rapport genereert. U kunt zelfs meerdere rapporten met verschillende groeifactoren genereren met dezelfde geprofileerde gegevens, en zo nagaan welke aanbevelingen voor doel-opslagaccount en bron-bandbreedte het beste aansluiten bij uw situatie. 

Het gegenereerde Microsoft Excel-rapport bevat de volgende informatie:

* [Samenvatting on-premises](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-summary)
* [Aanbevelingen](site-recovery-hyper-v-deployment-planner-analyze-report.md#recommendations)
* [VM<->Opslagplaatsing](site-recovery-hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation)]
* [Compatibele VM's](site-recovery-hyper-v-deployment-planner-analyze-report.md#compatible-vms)
* [Niet-compatibele VM's](site-recovery-hyper-v-deployment-planner-analyze-report.md#incompatible-vms)
* [On-premises opslagvereiste](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-storage-requirement)
* [IR-batchverwerking](site-recovery-hyper-v-deployment-planner-analyze-report.md#initial-replication-batching)
* [Kostenraming](site-recovery-hyper-v-deployment-planner-cost-estimation.md)

![Rapport van de implementatieplanner](media/site-recovery-hyper-v-deployment-planner-run/deployment-planner-report-h2a.png)


## <a name="get-throughput"></a>Doorvoer bepalen
Als u een schatting wilt maken van de doorvoer die Azure Site Recovery tijdens de replicatie van on-premises naar Azure kan behalen, moet u het hulpprogramma uitvoeren in de GetThroughput-modus. Het hulpprogramma berekent de doorvoer vanaf de server waarop het hulpprogramma wordt uitgevoerd. Deze server is in het ideale geval de Hyper-V-server met de VM's die moeten worden beveiligd. 

### <a name="command-line-parameters"></a>Opdrachtregelparameters 
Open een opdrachtregelconsole en ga naar de map met het hulpprogramma voor de Azure Site Recovery-implementatieplanner. Voer ASRDeploymentPlanner.exe uit met de volgende parameters
```
ASRDeploymentPlanner.exe -Operation GetThroughput /?
```
 Parameternaam | Beschrijving |
|---|---|
| -Operation | GetThroughtput |
|-Virtualization|Geef het type virtualisatie op (VMware of Hyper-V).|
|-Directory|(Optioneel) De UNC-directory (universal naming convention) of de lokale directory waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Als er geen naam wordt opgegeven, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory.|
| -StorageAccountName | De naam van het opslagaccount dat wordt gebruikt om de bandbreedte te bepalen die wordt gebruikt voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de gebruikte bandbreedte te bepalen. |
| -StorageAccountKey | De sleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > <*naam van opslagaccount*> > Instellingen > Toegangssleutels > Key1.|
| -VMListFile | Het bestand met de lijst met virtuele machines die moeten worden geprofileerd voor het berekenen van de gebruikte bandbreedte. Het bestandspad kan absoluut of relatief zijn. Voor Hyper-V is dit het uitvoerbestand voor de bewerking GetVMList. Als u de voorbereiding handmatig uitvoert, moet het bestand een servernaam of IP-adres bevatten, gevolgd door de naam van de VM, gescheiden door een \ per regel. De naam van de virtuele machine die in het bestand is opgegeven, moet gelijk zijn aan de naam van de virtuele machine op de Hyper-V host.<ul>Het bestand VMList.txt bevat bijvoorbeeld de volgende virtuele machines:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Environment|(Optioneel) Dit is uw doelomgeving voor het Azure Storage-account. Dit kan een van de volgende drie waarden zijn: AzureCloud, AzureUSGovernment, AzureChinaCloud. De standaardwaarde is AzureCloud. Gebruik de parameter wanneer uw Azure-doelregio de clouds voor Azure van de Amerikaanse overheid of Azure voor China is.|


Het hulpprogramma maakt in de opgegeven directory verschillende asrvhdfile<#>.vhd-bestanden van 64 MB (# is het volgnummer). Deze bestanden worden geüpload naar het opslagaccount om de doorvoer te bepalen. Nadat de doorvoer is gemeten, worden al deze bestanden verwijderd uit het opslagaccount en van de lokale server. Als het hulpprogramma om wat voor reden dan ook wordt beëindigd tijdens het berekenen van de doorvoer, worden de bestanden niet verwijderd uit het account of van de lokale server. U moet de bestanden dan handmatig verwijderen.

De doorvoer wordt op een gegeven moment gemeten. Dit is de maximale doorvoer die Azure Site Recovery tijdens de replicatie kan behalen, mits alle andere factoren gelijk blijven. Als een bepaalde toepassing bijvoorbeeld op hetzelfde netwerk meer bandbreedte gaat gebruiken, varieert de werkelijke doorvoer tijdens de replicatie. Het resultaat van de gemeten doorvoer verschilt als de GetThroughput-bewerking wordt uitgevoerd terwijl er sprake is van een groot gegevensverloop op de beveiligde virtuele machines. Het verdient daarom aanbeveling het hulpprogramma tijdens de profilering op verschillende tijdstippen uit te voeren om na te gaan welke doorvoersnelheden op verschillende momenten kunnen worden behaald. Het rapport bevat de laatst gemeten doorvoer in het hulpprogramma.
    
### <a name="example"></a>Voorbeeld
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Virtualization Hyper-V -Directory E:\Hyp-erV_ProfiledData -VMListFile E:\Hyper-V_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

[OPMERKING]
>
> Voer het hulpprogramma uit op een server die de dezelfde opslag- en CPU-kenmerken heeft als de Hyper-V-server.
>
> Voor replicatie moet u de aanbevolen bandbreedte gebruiken om de RPO 100 procent van de tijd te halen. Als u na het instellen van de juiste bandbreedte geen toename ziet in de gerapporteerde behaalde doorvoer, doet u het volgende:
>
>  1. Controleer of er een netwerk-QoS (Quality of Service) is die de doorvoer van Azure Site Recovery beperkt.
>
>  2. Controleer of de kluis van Azure Site Recovery zich in de dichtstbijzijnde fysieke ondersteunde Microsoft Azure-regio bevindt voor minimale netwerklatentie.
>
>  3. Controleer de kenmerken van de lokale opslag om te bepalen of u de hardware kunt verbeteren (bijvoorbeeld SSD in plaats van HDD).
>

## <a name="next-steps"></a>Volgende stappen
* [Het gegenereerde rapport analyseren](site-recovery-hyper-v-deployment-planner-analyze-report.md).