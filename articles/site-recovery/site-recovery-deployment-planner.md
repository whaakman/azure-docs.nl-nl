---
title: Azure Site Recovery-implementatieplanner voor VMware naar Azure | Microsoft Docs
description: Dit is de gebruikershandleiding voor de Azure Site Recovery-implementatieplanner.
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
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: 60b0641076c2fa8ed2feb5c64e7b119519f46cf4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery-implementatieplanner
Dit artikel is de gebruikershandleiding voor de Azure Site Recovery-implementatieplanner voor productie-installaties van het type VMware-naar-Azure.

## <a name="overview"></a>Overzicht

Voordat u een virtuele VMware-machine gaat beveiligen met behulp van Site Recovery, moet u voldoende bandbreedte toewijzen om de gewenste RPO (Recovery Point Objective) te kunnen halen. Bepaal de bandbreedte op basis van de dagelijkse veranderingssnelheid van gegevens. Zorg ervoor dat on-premises het juiste aantal configuratieservers en processervers wordt geïmplementeerd.

Daarnaast is het belangrijk dat u het juiste type en aantal opslagaccounts maakt in Azure. U kunt kiezen tussen Standard- of Premium Storage-accounts, waarbij u rekening moet houden met groei van uw productieservers door toegenomen gebruik na verloop van tijd. U bepaalt per virtuele machine het opslagtype, op basis van kenmerken van de werkbelasting (zoals I/O-bewerkingen voor lezen/schrijven per seconde [IOPS] of gegevensverloop) en limieten van Site Recovery.

De openbare preview van de Site Recovery-implementatieplanner is een opdrachtregelprogramma dat op dit moment alleen beschikbaar is voor een scenario van VMware naar Azure. U kunt uw virtuele VMware-machines met dit hulpprogramma op afstand profileren (zonder enige invloed op de productie) om inzicht te krijgen in de vereisten voor bandbreedte en Azure Storage waaraan moet worden voldaan voor een succesvolle replicatie en testfailover. U kunt het hulpprogramma uitvoeren zonder dat u on-premises onderdelen van Site Recovery hoeft te installeren. Voor nauwkeurige doorvoerresultaten wordt echter aangeraden de planner uit te voeren op een Windows-server die voldoet aan de minimale vereisten van de Site Recovery-configuratieserver die u uiteindelijk gaat implementeren als een van de eerste stappen van de productie-implementatie.

Het hulpprogramma levert de volgende gegevens:

**Beoordeling van compatibiliteit**

* Beoordeling van geschiktheid van een VM op basis van het aantal schijven, schijfgrootte, IOPS, verloop en opstarttype (EFI/BIOS)
* De geschatte vereiste bandbreedte in het netwerk voor replicatie van verschillen

**Vereiste netwerkbandbreedte versus RPO-evaluatie**

* De geschatte vereiste bandbreedte in het netwerk voor replicatie van verschillen
* De doorvoer die Site Recovery kan verwerken van on-premises naar Azure
* Het aantal virtuele machines in een batch, op basis van de geschatte bandbreedte die nodig is om initiële replicatie in een bepaalde tijd te voltooien

**Vereisten voor Azure-infrastructuur**

* Het vereiste type opslag (Standard- of Premium Storage-account) voor elke virtuele machine
* Het totale aantal Standard- en Premium Storage-accounts dat moet worden ingericht voor replicatie
* Suggesties voor naamgeving van opslagaccounts op basis van Azure Storage-richtlijnen
* De plaatsing van het opslagaccount voor alle virtuele machines
* Het aantal Azure-kerngeheugens dat moet worden ingericht voor testfailover of failover van het abonnement
* De aanbevolen grootte voor virtuele machines van Azure voor elke on-premises VM

**On-premises infrastructuurvereisten**
* Het vereiste aantal configuratieservers en processervers dat on-premises moet worden geïmplementeerd

>[!IMPORTANT]
>
>Gezien de verwachte toename van het gebruik in de loop van de tijd, wordt bij alle hierboven genoemde berekeningen een groeifactor van 30 procent gehanteerd voor kenmerken van de werkbelasting en wordt de 95e-percentielwaarde van alle metrische profileringsgegevens (IOPS voor lezen/schrijven, verloop,enzovoort) gebruikt. Beide elementen (groeifactor en percentielberekening) kunnen worden geconfigureerd. Zie de sectie 'Overwegingen voor groeifactor' voor meer informatie over de groeifactor. Zie de sectie 'Percentielwaarde gebruikt voor de berekening' voor meer informatie over de percentielwaarde.
>

## <a name="requirements"></a>Vereisten
Het hulpprogramma heeft twee belangrijke fasen: profileren en rapporten genereren. Er is ook een derde optie, waarmee alleen doorvoer wordt berekend. Raadpleeg de volgende tabel voor de vereisten voor de server waarop de profilering en meting van de doorvoersnelheid worden uitgevoerd:

| Serververeiste | Beschrijving|
|---|---|
|Profileren en meten van doorvoer| <ul><li>Besturingssysteem: Microsoft Windows Server 2012 R2<br>(bij voorkeur met minimaal de [aanbevolen waarden voor de configuratieserver](https://aka.ms/asr-v2a-on-prem-components))</li><li>Machineconfiguratie: 8 vCPU's, 16 GB RAM, 300 GB harde schijf</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable voor Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Internettoegang tot Azure vanaf deze server</li><li>Azure Storage-account</li><li>Beheerderstoegang op de server</li><li>Minimaal 100 GB vrije schijfruimte (uitgaande van 1000 virtuele machines met een gemiddelde van elk drie schijven, geprofileerd voor 30 dagen)</li><li>Het niveau voor VMware vCenter-statistieken moet worden ingesteld op niveau 2 of hoger</li><li>Poort 443 toestaan: de ASR-implementatieplanner gebruikt deze poort voor verbinding met de vCenter-server/ESXi-host</ul></ul>|
| Rapporten genereren | Een Windows-pc of Windows-server met Microsoft Excel 2013 of hoger |
| Gebruikersmachtigingen | Alleen-lezen-machtiging voor het gebruikersaccount dat tijdens het profileren wordt gebruikt voor toegang tot de VMware vCenter-server of VMware vSphere ESXi-host |

> [!NOTE]
>
>Het hulpprogramma kan alleen worden gebruikt voor het profileren van virtuele machines met VMDK- en RDM-schijven. Profilering van virtuele machines met iSCSI- of NFS-schijven is niet mogelijk. Hoewel iSCSI- en NFS-schijven in Site Recovery wel worden ondersteund voor VMware-servers, kan het hulpprogramma deze schijftypen niet zien. Dit komt doordat de implementatieplanner zich niet in de gast bevindt en voor de profilering alleen prestatiemeteritems van vCenter worden gebruikt.
>

## <a name="download-and-extract-the-public-preview"></a>De openbare preview downloaden en uitpakken
1. Download de nieuwste versie van de [openbare preview van de Site Recovery-implementatieplanner](https://aka.ms/asr-deployment-planner).  
Het hulpprogramma bevindt zich in een gecomprimeerde map. De huidige versie van het hulpprogramma ondersteunt alleen het scenario van VMware naar Azure.

2. Kopieer de gecomprimeerde map naar de Windows-server waarop u het hulpprogramma wilt uitvoeren.  
U kunt het hulpprogramma uitvoeren vanuit Windows Server 2012 R2 als de server netwerktoegang heeft voor verbinding met de vCenter-server of vSphere ESXi-host met de virtuele machines die u wilt profileren. We raden u echter aan het hulpprogramma uit te voeren op een server waarvan de hardwareconfiguratie voldoet aan de [aanbevelingen voor de configuratieserver](https://aka.ms/asr-v2a-on-prem-components). Als u al onderdelen van Site Recovery on-premises hebt geïmplementeerd, moet u het hulpprogramma uitvoeren vanaf de configuratieserver.

 Het is raadzaam om voor de server waarop u het hulpprogramma uitvoert, de hardwareconfiguratie te gebruiken die overeenkomt met de configuratie van de configuratieserver (die een ingebouwde processerver heeft). Een dergelijke configuratie zorgt ervoor dat de behaalde doorvoer die het hulpprogramma aangeeft, overeenkomt met de werkelijke doorvoer die Site Recovery kan bereiken tijdens replicatie. De berekening van de doorvoer is afhankelijk van de beschikbare netwerkbandbreedte op de server en de hardwareconfiguratie van de server (CPU, opslag, enzovoort). Als u het hulpprogramma vanaf een andere server uitvoert, wordt de doorvoer berekend vanaf die server naar Microsoft Azure. Aangezien de hardwareconfiguratie van de server kan verschillen van die van de configuratieserver, is het bovendien mogelijk dat de berekende doorvoer onjuist is.

3. Pak de gecomprimeerde map uit.  
De map bevat meerdere bestanden en submappen. Het uitvoerbare bestand is ASRDeploymentPlanner.exe in de map op het hoogste niveau.

    Voorbeeld:  
    Kopieer het ZIP-bestand naar station E:\ en pak het uit.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Functionaliteit
Het opdrachtregelprogramma (ASRDeploymentPlanner.exe) kunt u uitvoeren in een van de volgende drie modi:

1. Profileren  
2. Rapporten genereren
3. Doorvoer bepalen

Voer het hulpprogramma eerst uit in de profileringsmodus om gegevensverloop en IOPS te verzamelen voor de virtuele machine. Voer het hulpprogramma daarna uit om het rapport te genereren en de netwerkbandbreedte en opslagvereisten te bepalen.

## <a name="profiling"></a>Profileren
In de profileringsmodus maakt de implementatieplanner verbinding met de vCenter-server of de vSphere ESXi-host om prestatiegegevens te verzamelen over de virtuele machine.

* Profileren heeft geen invloed op de prestaties van de virtuele machines in de productieomgeving omdat er geen rechtstreekse verbinding met de machines wordt gemaakt. Alle prestatiegegevens worden verzameld van de vCenter-server of vSphere ESXi-host.
* Om ervoor te zorgen dat het profileren een minimaal effect heeft op de server, wordt de vCenter-server of vSphere ESXi-host slechts om de 15 minuten bevraagd. Dit heeft geen gevolgen voor de nauwkeurigheid van de profilering omdat het hulpprogramma alle prestatiemeteritemgegevens per minuut opslaat.

### <a name="create-a-list-of-vms-to-profile"></a>Een lijst maken met de virtuele machines die u wilt profileren
U moet eerst een lijst maken met de virtuele machines die u wilt profileren. U kunt alle namen van virtuele machines op een vCenter-server of vSphere ESXi-host opvragen met behulp van de opdrachten van VMware vSphere PowerCLI in de volgende procedure. Een andere manier is om de beschrijvende namen of IP-adressen van de virtuele machines die u wilt profileren handmatig toe te voegen aan een bestand.

1. Meld u aan bij de virtuele machine waarop VMware vSphere PowerCLI is geïnstalleerd in.
2. Open de console van VMware vSphere PowerCLI.
3. Zorg ervoor dat het uitvoeringsbeleid is ingeschakeld voor het script. Als het beleid is uitgeschakeld, start u de VMware vSphere PowerCLI-console in de beheerdersmodus en voert u de volgende opdracht uit om het beleid in te schakelen:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. U kunt desgewenst de volgende opdracht uitvoeren als Connect VIServer niet wordt herkend als de naam van de cmdlet.
 
            Add-PSSnapin VMware.VimAutomation.Core 

5. Als u alle namen wilt ophalen van de virtuele machines op een vCenter-server of vSphere ESXi-host en de lijst wilt opslaan in een TXT-bestand, voert u de volgende twee opdrachten uit.
Vervang &lsaquo;servernaam&rsaquo;, &lsaquo;gebruikersnaam&rsaquo;, &lsaquo;wachtwoord&rsaquo;, &lsaquo;outputfile.txt&rsaquo;; door uw invoer.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

6. Open het uitvoerbestand in Kladblok en kopieer de namen van alle virtuele machines die u wilt profileren naar een ander bestand (bijvoorbeeld ProfileVMList.txt). Zet elke naam op een afzonderlijke regel. Dit bestand gebruikt u als invoer voor de parameter *-VMListFile* van het opdrachtregelprogramma.

    ![Lijst met namen van virtuele machines in de implementatieplanner](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Profileren starten
Als u de lijst met te profileren virtuele machines hebt opgesteld, kunt u het hulpprogramma uitvoeren in de profileringsmodus. Hier volgt een lijst met verplichte en optionele parameters van het hulpprogramma die u in de profileringsmodus kunt uitvoeren.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Parameternaam | Beschrijving |
|---|---|
| -Operation | StartProfiling |
| -Server | De FQDN-naam of het IP-adres van de vCenter-server of vSphere ESXi-host waarvan de virtuele machines moeten worden geprofileerd.|
| -User | De naam van de gebruiker die verbinding maakt met de vCenter-server of vSphere ESXi-host. De gebruiker moet ten minste alleen-lezen-toegang hebben.|
| -VMListFile | Het bestand met de lijst met virtuele machines die u wilt profileren. Het bestandspad kan absoluut of relatief zijn. Het bestand moet per regel één naam of IP-adres van een virtuele machine bevatten. De namen van de virtuele machines in het bestand moeten identiek zijn met de namen van de virtuele machine op de vCenter-server of vSphere ESXi-host.<br>Het bestand ProfileVMList bevat bijvoorbeeld de volgende virtuele machines:<ul><li>virtual_machine_A</li><li>10.150.29.110</li><li>virtual_machine_B</li><ul> |
| -NoOfDaysToProfile | Het aantal dagen dat profilering moet worden uitgevoerd. Het is raadzaam om profilering langer dan 15 dagen uit te voeren om ervoor te zorgen dat het workloadpatroon in uw omgeving in de opgegeven periode goed wordt waargenomen, zodat er een nauwkeurige aanbeveling kan worden gedaan. |
| -Directory | (Optioneel) Het UNC-pad (Universal Naming Convention) of het pad naar de lokale directory voor het opslaan van gegevens die tijdens de profilering worden gegenereerd. Als u geen directorynaam opgeeft, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory. |
| -Password | (Optioneel) Het wachtwoord om verbinding te maken met de vCenter-server of vSphere ESXi-host. Als u nu geen wachtwoord opgeeft, wordt u erom gevraagd wanneer de opdracht wordt uitgevoerd.|
| -StorageAccountName | (Optioneel) De naam van het opslagaccount dat wordt gebruikt om de bereikbare doorvoer te vinden voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de doorvoer te berekenen.|
| -StorageAccountKey | (Optioneel) De sleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > <*naam van opslagaccount*> > Instellingen > Toegangssleutels > Key1 (of primaire toegangssleutel voor klassieke opslagaccount). |
| -Environment | (optioneel) Dit is uw doelomgeving voor het Azure Storage-account. Dit kan een van de volgende drie waarden zijn: AzureCloud, AzureUSGovernment, AzureChinaCloud. De standaardwaarde is AzureCloud. Gebruik de parameter wanneer uw Azure-doelregio de clouds voor Azure van de Amerikaanse overheid of Azure voor China is. |


Het is raadzaam om de virtuele machines gedurende ten minste 15 tot 30 dagen te profileren. Gedurende de periode voor profilering blijft ASRDeploymentPlanner.exe actief. Het hulpprogramma noteert de invoer van de profileringstijd in dagen. Als u enkele uren of minuten wilt profileren om het hulpprogramma even te testen, moet u in de openbare preview de tijd omzetten in een corresponderende meting in dagen. Als u bijvoorbeeld 30 minuten wilt profileren, moet de invoer 30/(60*24) = 0,021 dagen zijn. De minimaal toegestane tijd voor profilering is 30 minuten.

Tijdens het profileren kunt u eventueel de naam en sleutel van een opslagaccount doorgeven om vast te stellen wat de doorvoer is die Site Recovery kan behalen op het moment van replicatie vanaf de configuratieserver of processerver naar Azure. Als tijdens de profilering geen accountnaam en -sleutel wordt doorgegeven, wordt er geen bereikbare doorvoer berekend.

U kunt meerdere exemplaren van het hulpprogramma uitvoeren voor verschillende sets virtuele machines. Zorg ervoor dat de namen van de virtuele machines niet worden herhaald in een van de profileringssets. Als u bijvoorbeeld tien virtuele machines hebt geprofileerd (VM1 tot en met VM10) en na een paar dagen vijf andere virtuele machines wilt profileren (VM11 tot en met VM15), kunt u voor de tweede set virtuele machines (VM11 tot en met VM15) het hulpprogramma uitvoeren vanuit een andere opdrachtregelconsole. Zorg er wel voor dat de tweede set virtuele machines geen namen bevat van de eerste reeks geprofileerde virtuele machines of gebruik voor de tweede set een andere uitvoerdirectory. Als er voor het profileren van dezelfde virtuele machines twee exemplaren van het hulpprogramma worden gebruikt en dezelfde uitvoerdirectory wordt gebruikt, is het gegenereerde rapport niet correct.

De configuraties van de virtuele machines worden eenmaal aan het begin van de profilering vastgelegd en opgeslagen in een bestand met de naam VMDetailList.xml. Deze informatie wordt gebruikt bij het genereren van het rapport. Eventuele wijzigingen in de configuratie van een virtuele machine (bijvoorbeeld een groter aantal kerngeheugens, schijven of NIC's) vanaf het begin tot het einde van de profilering worden niet vastgelegd. Als de configuratie van een geprofileerde virtuele machine tijdens het profileren is gewijzigd, kunt u in de openbare preview de volgende oplossing gebruiken om de laatste gegevens van de virtuele machine te achterhalen wanneer u het rapport genereert:

* Maak een back-up van VMdetailList.xml en verwijder het bestand van de huidige locatie.
* Geef argumenten voor -User en -Password door wanneer u het rapport genereert.

Met de profileringsopdracht worden meerdere bestanden gegenereerd in de directory voor profilering. Verwijder deze bestanden niet, aangezien dit gevolgen heeft voor het genereren van de rapporten.

#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Voorbeeld 1: virtuele machines 30 dagen profileren en de doorvoer van on-premises naar Azure bepalen
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Voorbeeld 2: virtuele machines 15 dagen profileren

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-the-tool"></a>Voorbeeld 3: virtuele machines 1 uur profileren om het hulpprogramma kort te testen
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Als de server waarop het hulpprogramma wordt uitgevoerd, opnieuw wordt opgestart of is gecrasht, of als u het hulpprogramma afsluit met Ctrl + C, blijven de profileringsgegevens behouden. De kans bestaat echter wel dat de laatste 15 minuten aan geprofileerde gegevens ontbreekt. In dat geval voert u het hulpprogramma opnieuw uit in de profileringsmodus nadat de server opnieuw is opgestart.
>* Wanneer de accountnaam en -sleutel worden doorgegeven, meet het hulpprogramma de doorvoer tijdens de laatste stap van de profilering. Als het hulpprogramma wordt beëindigd voordat de profilering is voltooid, wordt de doorvoer niet berekend. Als u de doorvoer wilt weten voordat het rapport wordt gegenereerd, kunt u de bewerking GetThroughput uitvoeren vanuit de opdrachtregelconsole. Anders wordt bevat het gegenereerde rapport geen informatie over de doorvoer.


## <a name="generate-a-report"></a>Een rapport genereren
Het hulpprogramma genereert een Microsoft Excel-bestand met ingeschakelde macro's (XLSM-bestand) als de rapportuitvoer, met daarin een overzicht van alle aanbevelingen voor de implementatie. Het rapport heeft de naam DeploymentPlannerReport_<*unieke numerieke id*>.xlsm en wordt in de opgegeven directory geplaatst.

Nadat de profilering is voltooid, kunt u het hulpprogramma uitvoeren in de modus voor het genereren van een rapport. Hieronder ziet u een tabel met verplichte en optionele parameters van het hulpprogramma die u kunt uitvoeren in de profileringsmodus.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Parameternaam | Beschrijving |
|-|-|
| -Operation | GenerateReport |
| -Server |  De FQDN-naam of het IP-adres van de vCenter- of vSphere-server (gebruik dezelfde naam of hetzelfde IP-adres als op het moment van profilering) waar de geprofileerde virtuele machines zich bevinden waarvan een rapport wordt gegenereerd. Let op: als u ten tijde van de profilering een vCenter-server hebt gebruikt, kunt u geen vSphere-server gebruiken voor het genereren van rapporten en omgekeerd.|
| -VMListFile | Het bestand met de lijst met geprofileerde virtuele machines waarvoor het rapport moet worden gegenereerd. Het bestandspad kan absoluut of relatief zijn. Het bestand moet per regel één naam of IP-adres van een virtuele machine bevatten. De namen van de virtuele machines die in het bestand zijn opgegeven, moeten gelijk zijn aan de namen van de virtuele machines op de vCenter-server of vSphere ESXi-host en overeenkomen met de namen die tijdens de profilering zijn gebruikt.|
| -Directory | (Optioneel) De UNC-directory of de lokale directory waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Als u deze parameter niet opgeeft, wordt de directory ProfiledData gebruikt. |
| -GoalToCompleteIR | (Optioneel) Het aantal uren waarin de initiële replicatie van de geprofileerde virtuele machines moet worden voltooid. Het gegenereerde rapport bevat het aantal virtuele machines waarvoor de initiële replicatie in de opgegeven tijd kan worden voltooid. Standaard is dit 72 uur. |
| -User | (Optioneel) De naam van de gebruiker die verbinding maakt met de vCenter-/vSphere-server. De naam wordt gebruikt voor het ophalen van de meest recente configuratiegegevens van de virtuele machines, zoals het aantal schijven, kerngeheugens en NIC's, die in het rapport moeten worden gebruikt. Als u geen naam opgeeft, worden de configuratiegegevens gebruikt die aan het begin van de profilering zijn verzameld. |
| -Password | (Optioneel) Het wachtwoord om verbinding te maken met de vCenter-server of vSphere ESXi-host. Als u hier geen wachtwoord opgeeft, wordt u erom gevraagd wanneer de opdracht wordt uitgevoerd. |
| -DesiredRPO | (Optioneel) Het gewenste beoogde herstelpunt (Recovery Point Objective [RPO]) in minuten. Standaard is dit 15 minuten.|
| -Bandwidth | De bandbreedte in Mbps. De parameter voor het berekenen van de RPO die voor de opgegeven bandbreedte kan worden bereikt. |
| -StartDate | (Optioneel) De begindatum en -tijd in de notatie MM-DD-JJJJ:UU:MM (in 24-uursindeling). Als u *StartDate* opgeeft, moet u ook *EndDate* opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| -EndDate | (Optioneel) De einddatum en -tijd in de notatie MM-DD-JJJJ:UU:MM (in 24 uursindeling). Als u *EndDate* opgeeft, moet u ook *StartDate* opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| -GrowthFactor | (Optioneel) De groeifactor, uitgedrukt als een percentage. De standaardwaarde is 30 procent. |
| -UseManagedDisks | (Optioneel) UseManagedDisks - Ja/Nee. Standaard is Ja. Het aantal virtuele machines dat kan worden ondergebracht in een enkel opslagaccount, wordt berekend afgaande op het feit of de Failover/Test-testfailover van virtuele machines is uitgevoerd op een beheerde schijf in plaats van op een niet-beheerde schijf. |

#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Voorbeeld 1: een rapport met standaardwaarden genereren wanneer de geprofileerde gegevens zich op de lokale schijf bevinden
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Voorbeeld 2: een rapport genereren wanneer de geprofileerde gegevens zich op een externe server bevinden
De gebruiker moet lees-/schrijftoegang hebben voor de externe directory.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Voorbeeld 3: een rapport genereren met specifieke bandbreedte en het doel IR te voltooien binnen de opgegeven periode
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Voorbeeld 4: een rapport genereren met een groeifactor van 5% in plaats van de standaardwaarde 30%
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Voorbeeld 5: een rapport genereren met een subset geprofileerde gegevens
U hebt gedurende 30 dagen gegevens geprofileerd en u wilt een rapport genereren voor slechts 20 van de 30 dagen.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Voorbeeld 6: een rapport genereren voor 5 minuten RPO
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-the-calculation"></a>Percentielwaarde die voor de berekening wordt gebruikt
**Welke standaardpercentielwaarde met betrekking tot de prestatiegerelateerde metrische gegevens die tijdens de profilering zijn verzameld, wordt tijdens het genereren van het rapport gebruikt?**

Het hulpprogramma wordt standaard ingesteld op de 95e-percentielwaarde van de IOPS voor lezen/schrijven, de IOPS voor schrijven en de gegevensverloop die tijdens het profileren van de virtuele machines zijn verzameld. Deze waarde zorgt ervoor dat een piek (100e-percentielwaarde) die vanwege tijdelijke gebeurtenissen kan ontstaan op virtuele machines, niet wordt gebruikt om de vereisten te bepalen van het doel-opslagaccount en de bron-bandbreedte. Voorbeelden van tijdelijke gebeurtenissen zijn het één keer per dag uitvoeren van een back-up, het periodiek indexeren van een database, het genereren van analyserapporten en andere vergelijkbare activiteiten die op een bepaald moment actief zijn.

Het gebruik van de 95e-percentielwaarde biedt een waarheidsgetrouw beeld van de werkelijke workloadkenmerken en verzekert u van optimale prestaties wanneer deze workloads in Azure worden uitgevoerd. We verwachten niet dat u deze waarde hoeft te wijzigen. Als de waarde toch wilt wijzigen (bijvoorbeeld op het 90e percentiel), kunt u het configuratiebestand *ASRDeploymentPlanner.exe.config* in de standaardmap bijwerken en opslaan om een nieuw rapport te genereren op basis van de bestaande geprofileerde gegevens.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Overwegingen voor groeifactor
**Waarom moet ik rekening houden met een groeifactor bij het plannen van implementaties?**

Het is essentieel dat u in uw workloadkenmerken rekening houdt met groei (mogelijke toename in gebruik na verloop van tijd). Als de beveiliging eenmaal is ingeschakeld en uw workloadkenmerken vervolgens veranderen, is het namelijk niet mogelijk om over te schakelen naar een ander opslagaccount voor beveiliging zonder eerst de beveiliging uit te schakelen en opnieuw in te schakelen.

Stel dat uw virtuele machine op dit moment voldoende heeft aan een Standard Storage-replicatie-account. De kans is echter groot dat in de volgende drie maanden verschillende wijzigingen optreden:

* Het aantal gebruikers van de toepassing die wordt uitgevoerd op de virtuele machine neemt toe.
* Het toegenomen verloop op de virtuele machine dat hiervan het gevolg is, betekent dat de virtuele machine moet worden overgezet naar een Premium Storage-account om ervoor te zorgen dat Site Recovery de replicatie kan blijven uitvoeren.
* Dit betekent dat u de beveiliging moet uitschakelen en vervolgens opnieuw moet inschakelen met een Premium Storage-account.

We raden het dan ook sterk aan om rekening te houden met groei tijdens het plannen van de implementatie. U kent het gebruikspatroon van uw toepassingen als geen ander en aan de hand daarvan kunt u de verwachte groei inschatten. De standaardwaarde van 30 procent kunt u tijdens het genereren van een rapport aanpassen. U kunt zelfs meerdere rapporten met verschillende groeifactoren genereren met dezelfde geprofileerde gegevens en zo nagaan welke aanbevelingen voor doel-opslagaccount en bron-bandbreedte het beste aansluiten bij uw situatie.

Het gegenereerde Microsoft Excel-rapport bevat de volgende informatie:

* [Invoer](site-recovery-deployment-planner.md#input)
* [Aanbevelingen](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Aanbevelingen bandbreedte](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [VM<->Opslagplaatsing](site-recovery-deployment-planner.md#vm-storage-placement)
* [Compatibele VM's](site-recovery-deployment-planner.md#compatible-vms)
* [Niet-compatibele VM's](site-recovery-deployment-planner.md#incompatible-vms)

![Implementatieplanner](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Doorvoer bepalen

Als u een schatting wilt maken van de doorvoer die Site Recovery tijdens de replicatie van on-premises naar Azure kan behalen, moet u het hulpprogramma uitvoeren in de GetThroughput-modus. Het hulpprogramma berekent de doorvoer vanaf de server waarop het hulpprogramma wordt uitgevoerd. Deze server voldoet bij voorkeur aan de aanbevelingen voor de configuratieserver. Als u al infrastructuuronderdelen van Site Recovery on-premises hebt geïmplementeerd, moet u het hulpprogramma uitvoeren op de configuratieserver.

Open een opdrachtregelconsole en ga naar de map met het hulpprogramma voor de Site Recovery-implementatieplanner. Voer ASRDeploymentPlanner.exe uit met de volgende parameters.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Parameternaam | Beschrijving |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Optioneel) De UNC-directory of de lokale directory waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Als u geen directory opgeeft, wordt de directory 'ProfiledData' gebruikt. |
| -StorageAccountName | De naam van het opslagaccount dat wordt gebruikt om de bandbreedte te bepalen die wordt gebruikt voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de gebruikte bandbreedte te bepalen. |
| -StorageAccountKey | De sleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > <*naam van het opslagaccount*> > Instellingen > Toegangssleutels > Key1 (of primaire toegangssleutel voor klassieke opslagaccount). |
| -VMListFile | Het bestand met de lijst met virtuele machines die moeten worden geprofileerd voor het berekenen van de gebruikte bandbreedte. Het bestandspad kan absoluut of relatief zijn. Het bestand moet per regel één naam of IP-adres van een virtuele machine bevatten. De namen van de virtuele machines in het bestand moeten identiek zijn met de namen van de virtuele machine op de vCenter-server of vSphere ESXi-host.<br>Het bestand ProfileVMList bevat bijvoorbeeld de volgende virtuele machines:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (optioneel) Dit is uw doelomgeving voor het Azure Storage-account. Dit kan een van de volgende drie waarden zijn: AzureCloud, AzureUSGovernment, AzureChinaCloud. De standaardwaarde is AzureCloud. Gebruik de parameter wanneer uw Azure-doelregio de clouds voor Azure van de Amerikaanse overheid of Azure voor China is. |

Het hulpprogramma maakt in de opgegeven directory verschillende asrvhdfile<#>.vhd-bestanden van 64 MB (# is het volgnummer). Deze bestanden worden geüpload naar het opslagaccount om de doorvoer te bepalen. Nadat de doorvoer is gemeten, worden al deze bestanden verwijderd uit het opslagaccount en van de lokale server. Als het hulpprogramma om wat voor reden dan ook wordt beëindigd tijdens het berekenen van de doorvoer, worden de bestanden niet verwijderd uit het account of van de lokale server. U moet de bestanden dan handmatig verwijderen.

De doorvoer wordt op een gegeven moment gemeten. Dit is de maximale doorvoer die Site Recovery tijdens de replicatie kan behalen, mits alle andere factoren gelijk blijven. Als een bepaalde toepassing bijvoorbeeld op hetzelfde netwerk meer bandbreedte gaat gebruiken, varieert de werkelijke doorvoer tijdens de replicatie. Als u GetThroughput uitvoert vanaf een configuratieserver, is het hulpprogramma niet op de hoogte van beveiligde virtuele machines en actieve replicatie. Het resultaat van de gemeten doorvoer verschilt als de GetThroughput-bewerking wordt uitgevoerd terwijl er sprake is van een groot gegevensverloop op de beveiligde virtuele machines. Het verdient daarom aanbeveling het hulpprogramma tijdens de profilering op verschillende tijdstippen uit te voeren om na te gaan welke doorvoersnelheden op verschillende momenten kunnen worden behaald. Het rapport bevat de laatst gemeten doorvoer in het hulpprogramma.

### <a name="example"></a>Voorbeeld
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Voer het hulpprogramma uit op een server die de dezelfde opslag- en CPU-kenmerken heeft als de configuratieserver.
>
> Voor replicatie moet u de aanbevolen bandbreedte gebruiken om de RPO 100 procent van de tijd te halen. Als u na het instellen van de juiste bandbreedte geen toename ziet in de gerapporteerde behaalde doorvoer, doet u het volgende:
>
>  1. Controleer of er een netwerk-QoS (Quality of Service) is die de doorvoer van Site Recovery beperkt.
>
>  2. Controleer of de kluis van Site Recovery zich in de dichtstbijzijnde fysieke ondersteunde Microsoft Azure-regio bevindt voor minimale netwerklatentie.
>
>  3. Controleer de kenmerken van de lokale opslag om te bepalen of u de hardware kunt verbeteren (bijvoorbeeld SSD in plaats van HDD).
>
>  4. Wijzig de Site Recovery-instellingen op de processerver om [de voor replicatie gebruikte netwerkbandbreedte te vergroten](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Aanbevelingen met gewenste RPO als invoer

### <a name="profiled-data"></a>Geprofileerde gegevens

![De weergave met geprofileerde gegevens in de implementatieplanner](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Periode geprofileerde gegevens**: de periode waarin de profilering is uitgevoerd. Standaard gebruikt het hulpprogramma voor de berekening alle geprofileerde gegevens, tenzij het rapport wordt gegenereerd voor een bepaalde periode (tijdens het genereren van het rapport zijn dan waarden opgegeven voor StartDate en EndDate).

**Servernaam**: de naam of het IP-adres van de VMware vCenter of ESXi-host met de virtuele machines waarvoor het rapport wordt gegenereerd.

**Gewenste RPO**: het gewenste beoogde herstelpunt (RPO) voor uw implementatie. Standaard wordt de vereiste netwerkbandbreedte berekend voor de RPO-waarden van 15, 30 en 60 minuten. Op basis van de selectie worden de betrokken waarden bijgewerkt op het blad. Als u tijdens het genereren van het rapport de parameter *DesiredRPOinMin* hebt gebruikt, wordt de betreffende waarde weergegeven in deze vervolgkeuzelijst.

### <a name="profiling-overview"></a>Overzicht van profilering

![Resultaten van profilering in de implementatieplanner](./media/site-recovery-deployment-planner/profiling-overview.png)

**Totale aantal geprofileerde virtuele machines**: het totale aantal virtuele machines waarvan de geprofileerde gegevens beschikbaar zijn. Als het bestand dat is opgegeven bij VMListFile namen van virtuele machines bevat die niet zijn geprofileerd, worden deze virtuele machines niet meegenomen in het rapport en uitgesloten van het totale aantal geprofileerde virtuele machines.

**Compatibele virtuele machines**: het aantal virtuele machines dat op Azure kan worden beveiligd met Site Recovery. Dit is het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het aantal opslagaccounts, het aantal Azure-kerngeheugens, en het aantal configuratieservers en aanvullende processervers wordt berekend. De details van elke compatibele virtuele machine zijn beschikbaar in de sectie 'Compatibele VM's'.

**Niet-compatibele virtuele machines**: het aantal geprofileerde virtuele machines dat niet compatibel is met beveiliging met Site Recovery. De redenen voor incompatibiliteit worden vermeld in de sectie 'Niet-compatibele VM's'. Als het bestand dat is opgegeven bij VMListFile namen bevat van virtuele machines die niet zijn geprofileerd, worden die virtuele machines uitgesloten van het aantal niet-compatibele virtuele machines. Deze virtuele machines worden weergegeven als 'Gegevens niet gevonden' aan het einde van de sectie 'Niet-compatibele VM's'.

**Gewenste RPO**: het gewenste beoogde herstelpunt (Recovery Point Objective [RPO]) in minuten. Het rapport wordt gegenereerd voor drie RPO-waarden: 15 (standaard), 30 en 60 minuten. De aanbeveling voor bandbreedte in het rapport wordt gewijzigd op basis van uw selectie in de vervolgkeuzelijst Gewenste RPO in de rechterbovenhoek van het blad. Als u het rapport hebt gegenereerd met een aangepaste waarde voor de parameter *-DesiredRPO*, wordt deze aangepaste waarde als standaardwaarde weergegeven in de vervolgkeuzelijst Gewenste RPO.

### <a name="required-network-bandwidth-mbps"></a>Vereiste netwerkbandbreedte (Mbps)

![Vereiste netwerkbandbreedte in de implementatieplanner](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**Altijd aan de RPO voldoen:** de aanbevolen bandbreedte in Mbps die moet worden toegewezen om altijd te voldoen aan uw gewenste RPO. Deze bandbreedte moet worden toegewezen voor onveranderlijke replicatie van verschillen bij al uw compatibele virtuele machines om eventuele RPO-schendingen te voorkomen.

**90% van de tijd aan de RPO voldoen**: als u vanwege de kosten voor een breedbandverbinding of om een andere reden niet de benodigde bandbreedte kunt toepassen om altijd aan de RPO te voldoen, kunt u een lager bedrag voor bandbreedte kiezen waarmee u 90% van de tijd aan de RPO kunt voldoen. Voor inzicht in de gevolgen van het instellen van deze lagere bandbreedte bevat het rapport een wat-als-analyse van het aantal en de duur van de RPO-schendingen die u kunt verwachten.

**Behaalde doorvoer:** de doorvoer van de server waar u de GetThroughput-opdracht hebt uitgevoerd naar de Microsoft Azure-regio waar het opslagaccount zich bevindt. Deze waarde geeft het geschatte niveau aan dat kan worden behaald als u de compatibele virtuele machines beveiligt met Site Recovery, op voorwaarde dat de kenmerken voor opslag en netwerk van uw configuratieserver of processerver hetzelfde blijven als die van de server waarop u het hulpprogramma hebt uitgevoerd.

Voor replicatie moet u de aanbevolen bandbreedte gebruiken om de RPO 100 procent van de tijd te halen. Als u na het instellen van de bandbreedte geen toename ziet in de gerapporteerde behaalde doorvoer, doet u het volgende:

1. Controleer of er een netwerk-QoS (Quality of Service) is die de doorvoer van Site Recovery beperkt.

2. Controleer of de kluis van Site Recovery zich in de dichtstbijzijnde fysieke ondersteunde Microsoft Azure-regio bevindt voor minimale netwerklatentie.

3. Controleer de kenmerken van de lokale opslag om te bepalen of u de hardware kunt verbeteren (bijvoorbeeld SSD in plaats van HDD).

4. Wijzig de Site Recovery-instellingen op de processerver om [de voor replicatie gebruikte netwerkbandbreedte te vergroten](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Als u het hulpprogramma uitvoert op een configuratieserver of processerver die al virtuele machines beveiligt, voert u het hulpprogramma een paar keer uit. De bereikte doorvoer verandert afhankelijk van het gegevensverloop op dat moment.

Voor alle Enterprise-implementaties van Site Recovery wordt het gebruik van [ExpressRoute](https://aka.ms/expressroute) aanbevolen.

### <a name="required-storage-accounts"></a>Vereiste opslagaccounts
De volgende grafiek geeft het totale aantal opslagaccounts aan (Standard Storage en Premium Storage) dat is vereist voor het beveiligen van alle compatibele virtuele machines. Zie de sectie 'VM-opslagplaatsing' voor meer informatie over welk opslagaccount u moet gebruiken voor elke virtuele machine.

![Vereiste opslagaccounts in de implementatieplanner](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Vereiste aantal Azure-kerngeheugens
Dit is het totale aantal kerngeheugens dat moet worden ingesteld vóór failover of testfailover van de compatibele virtuele machines. Als er in het abonnement onvoldoende kerngeheugens beschikbaar zijn, kan Site Recovery geen virtuele machines maken op het moment van een failover of testfailover.

![Vereiste aantal Azure-kerngeheugens in de implementatieplanner](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Vereiste on-premises infrastructuur
Het totale aantal configuratieservers en aanvullende processervers dat moet worden geconfigureerd voor het beveiligen van alle compatibele virtuele machines. Afhankelijk van de ondersteunde [aanbevelingen voor de configuratieserver](https://aka.ms/asr-v2a-on-prem-components), kan het hulpprogramma extra servers aanbevelen. De aanbeveling wordt gebaseerd op wat het grootst is: het gegevensverloop per dag of het maximum aantal beveiligde virtuele machines (uitgaande van gemiddeld drie schijven per virtuele machine), ongeacht wat het eerste wordt bereikt op de configuratieserver of de aanvullende processerver. De details van het totale verloop per dag en het totale aantal beveiligde schijven vindt u in de sectie Invoer.

![Vereiste on-premises infrastructuur in de implementatieplanner](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Wat als-analyse
Deze analyse beschrijft hoeveel schendingen zich kunnen voordoen tijdens de profileringsperiode wanneer u een lagere bandbreedte instelt om 90% van de tijd aan de RPO te voldoen. Op elke dag kunnen er een of meer RPO-schendingen optreden. De grafiek toont de piek-RPO van de dag.
Op basis van deze analyse kunt u besluiten of het aantal RPO-schendingen voor alle dagen en de hoogste RPO per dag acceptabel zijn in combinatie met de opgegeven lagere bandbreedte. Als u dat vindt, kunt u de lagere bandbreedte toewijzen voor replicatie. Zo niet, dan stelt u de hogere bandbreedte in zoals voorgesteld om altijd aan de RPO te voldoen.

![Wat-als analyse in de implementatieplanner](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Aanbevolen VM-batchgrootte voor de initiële replicatie
In deze sectie ziet u een aanbeveling voor het aantal virtuele machines die gelijktijdig kunnen worden beveiligd om de initiële replicatie binnen 72 uur met de voorgestelde bandbreedte af te ronden om gedurende de ingestelde tijd altijd te voldoen aan de gewenste RPO. Deze waarde is kunt u aanpassen. Gebruik hiervoor de parameter *GoalToCompleteIR* tijdens het genereren van het rapport.

In de grafiek ziet u het bereik met bandbreedtewaarden en de berekende grootte voor de batch virtuele machines waarmee de initiële replicatie binnen 72 uur kan worden voltooid, uitgaande van de gemiddelde grootte van de gedetecteerde virtuele machines binnen de volledige set compatibele virtuele machines.

In de openbare preview bevat het rapport geen informatie over de virtuele machines die in een batch moeten worden opgenomen. U kunt de schijfgrootte uit de sectie 'Compatibele VM's' gebruiken om de grootte van elke virtuele machine te bepalen en vervolgens virtuele machines voor een batch te selecteren. U kunt ook virtuele machines selecteren op basis van bekende workloadkenmerken. De tijd die een initiële replicatie kost, wijzigt proportioneel op basis van de daadwerkelijke schijfgrootte van de virtuele machine, de gebruikte schijfruimte en de beschikbare netwerkdoorvoer.

![Aanbevolen VM-batchgrootte](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Groeifactor en gebruikte percentielwaarden
Deze sectie onder aan het werkblad toont de percentielwaarde die voor alle prestatiemeteritems van de geprofileerde virtuele machines wordt gebruikt (standaard is dit het 95e percentiel), plus de groeifactor in procenten die in alle berekeningen wordt gebruikt (standaard 30%).

![Groeifactor en gebruikte percentielwaarden](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Aanbevelingen met beschikbare bandbreedte als invoer

![Aanbevelingen met beschikbare bandbreedte als invoer](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Het is mogelijk dat u om wat voor reden dan ook niet meer dan x Mbps bandbreedte kunt instellen voor Site Recovery-replicatie. Met het hulpprogramma kunt u de beschikbare bandbreedte opgeven (dit doet u tijdens het genereren van het rapport met behulp van de parameter -Bandwidth) en zo de haalbare RPO in minuten achterhalen. Aan de hand van deze haalbare RPO-waarde kunt u beslissen of u extra bandbreedte wilt instellen of bereid bent u te beperken tot een noodhersteloplossing voor deze RPO.

![Haalbare RPO voor 500 Mbps bandbreedte](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Invoer
Het werkblad Invoer biedt een overzicht van de geprofileerde VMware-omgeving.

![Overzicht van de geprofileerde VMware-omgeving](./media/site-recovery-deployment-planner/Input.png)

**Begindatum** en **Einddatum** zijn de begin- en einddatums van de profileringsgegevens die in aanmerking komen voor het genereren van rapporten. De begindatum is standaard de datum waarop de profilering start en de einddatum de datum waarop de profilering stopt. Dit kunnen de waarden voor StartDate en EndDate zijn als het rapport met deze parameters wordt gegenereerd.

**Totale aantal dagen van profilering**: het totale aantal dagen van profilering tussen de begin- en einddatum waarvoor het rapport wordt gegenereerd.

**Aantal compatibele virtuele machine**: het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het vereiste aantal Azure-opslagaccounts, Microsoft Azure-kerngeheugens, configuratieservers en aanvullende processervers wordt berekend.

**Totale aantal schijven voor alle compatibele virtual machines**: het aantal dat wordt gebruikt als een van de invoerwaarden om het aantal configuratieservers en aanvullende processenservers te bepalen dat in de implementatie moet worden gebruikt.

**Gemiddeld aantal schijven per compatibele virtuele machine**: het gemiddelde aantal schijven dat voor alle compatibele virtuele machines is berekend.

**Gemiddelde schijfgrootte (GB)**: de gemiddelde schijfgrootte die voor alle compatibele virtuele machines is berekend.

**Gewenste RPO (minuten)**: de standaard-RPO-waarde of de waarde die tijdens het genereren van het rapport voor de parameter DesiredRPO wordt doorgegeven om de vereiste bandbreedte in te schatten.

**Gewenste bandbreedte (Mbps)**: de waarde die u tijdens het genereren van het rapport voor de parameter Bandwidth hebt doorgegeven om de haalbare RPO in te schatten.

**Waargenomen typisch gegevensverloop per dag (GB)**: het gemiddelde gegevensverloop dat voor alle profileringsdagen is vastgesteld. Dit aantal wordt gebruikt als een van de invoerwaarden om het aantal configuratieservers en aanvullende processenservers te bepalen dat in de implementatie moet worden gebruikt.


## <a name="vm-storage-placement"></a>VM-opslagplaatsing

![VM-opslagplaatsing](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Type schijfopslag**: het type opslagaccount (Standard Storage of Premium Storage) dat wordt gebruikt voor het repliceren van alle bijbehorende virtuele machines in de kolom **Te plaatsen VM's**.

**Voorgestelde voorvoegsel**: het voorgestelde voorvoegsel, bestaande uit drie tekens, dat kan worden gebruikt voor de naam van het opslagaccount. U kunt uw eigen voorvoegsel gebruiken, maar de suggestie van het hulpprogramma volgt de [naamgevingsregels voor partities voor opslagaccounts](https://aka.ms/storage-performance-checklist).

**Voorgestelde accountnaam**: de naam van het opslagaccount na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Logboekopslagaccount**: alle replicatielogboeken worden opgeslagen in een opslagaccount van het type Standard. In het geval van virtuele machines die repliceren naar een Premium Storage-account moet u een extra opslagaccount van het type Standard instellen voor het opslaan van logboeken. Eén Standard-account voor het opslaan van logboeken kan worden gebruikt door meerdere Premium-opslagaccounts voor replicatie. Virtuele machines die worden gerepliceerd naar een Standard-opslagaccount gebruiken hetzelfde account voor het opslaan van logboeken.

**Voorgestelde naam logboekaccount**: de naam van het account voor het opslaan van logboeken na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Plaatsing samenvatting**: een samenvatting van de belasting van het totale aantal virtuele machines in het opslagaccount tijdens replicatie en testfailover of failover. Hier ziet u het totale aantal virtuele machines dat is toegewezen aan het opslagaccount, de totale lezen/schrijven-IOPS voor alle virtuele machines die in dit opslagaccount worden geplaatst, totale schrijven-IOPS (replicatie), totale ingestelde grootte voor alle schijven en het totale aantal schijven.

**Te plaatsen virtuele machines**: een lijst van alle virtuele machines die in het opgegeven opslagaccount moeten worden geplaatst voor optimale prestaties en optimaal gebruik.

## <a name="compatible-vms"></a>Compatibele VM's
![Excel-werkblad met compatibele VM's](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM-naam**: de naam of het IP-adres van de virtuele machine dat wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de schijven (VMDK's) die aan de virtuele machines zijn gekoppeld. Om onderscheid te maken tussen virtuele machines van vCenter met dubbele namen of IP-adressen, bevatten de namen de naam van de ESXi-host. De vermelde ESXi-host is de host waar de virtuele machine werd geplaatst op het moment van detectie door het hulpprogramma tijdens de profileringsperiode.

**VM-compatibiliteit**: de mogelijke waarden zijn **Ja** en **Ja**\*. **Ja**\* verwijst naar gevallen waarin de virtuele machine in aanmerking komt voor [Azure Premium Storage](https://aka.ms/premium-storage-workload). Het hoge geprofileerde verloop of de IOPS-schijf komt overeen met categorie P20 of P30, maar de grootte van de schijf zorgt ervoor dat P10 of P20 wordt toegewezen. Het opslagaccount bepaalt aan welk schijftype voor Premium Storage een schijf wordt toegewezen, op basis van de grootte. Bijvoorbeeld:
* <128 GB is een P10.
* 128 GB tot 512 GB is een P20.
* 512 GB tot 1024 GB is een P30.
* 1025 GB tot 2048 GB is een P40.
* 2049 GB tot 4095 GB is een P50.

Als de kenmerken van de workload van een schijf overeenkomen met de categorie P20 of P30, maar de schijf door grootte aan een lagere categorie schijftype voor Premium Storage wordt gekoppeld, markeert het hulpprogramma die virtuele machine als **Ja**\*. Het hulpprogramma adviseert ook om ofwel de grootte van de bronschijf te wijzigen zodat deze overeenkomt met het schijftype voor Premium Storage of de post-failover van het doelschijftype te wijzigen.

**Opslagtype**: Standard of Premium.

**Voorgestelde voorvoegsel**: het voorvoegsel van drie tekens van het opslagaccount.

**Opslagaccount**: de naam die gebruikmaakt van het voorgestelde voorvoegsel voor het opslagaccount.

**R/W IOPS (met groeifactor)**: de piekworkload-IOPS voor lezen/schrijven op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). De totale IOPS voor lezen/schrijven van een virtuele machine is niet altijd de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven van de virtuele machine. De piek-IOPS voor lezen/schrijven van de virtuele machine is namelijk de piek van de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven voor elke minuut van de profileringsperiode.

**Gegevensverloop in Mbps (met groeifactor)**: het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Azure VM-grootte**: de ideale toegewezen grootte voor virtuele machines van Azure Cloud Services voor deze on-premises virtuele machine. De toewijzing is gebaseerd op het geheugen, het aantal schijven/kerngeheugens/NIC’s en de IOPS voor lezen/schrijven van de on-premises virtuele machine. De aanbeveling is altijd de laagste Azure VM-grootte die overeenkomt met alle kenmerken van de on-premises virtuele machine.

**Aantal schijven**: het totale aantal schijven (VMDK's) van de virtuele machine.

**Schijfgrootte (GB)**: de totale ingestelde grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: het RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Opstarttype**: het opstarttype van de virtuele machine. Dit kan BIOS of EFI zijn. Op dit moment ondersteunt Azure Site Recovery alleen BIOS-opstarttypen. Alle virtuele machines van het EFI-opstarttype worden vermeld op het werkblad Incompatibele virtuele machines.

**Type besturingssysteem**: het type besturingssysteem van de virtuele machine. Dit kan Windows, Linux of een ander besturingssysteem zijn.

## <a name="incompatible-vms"></a>Niet-compatibele VM's

![Excel-werkblad met niet-compatibele VM's](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM-naam**: de naam of het IP-adres van de virtuele machine dat wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de VMDK's die aan de virtuele machines zijn gekoppeld. Om onderscheid te maken tussen virtuele machines van vCenter met dubbele namen of IP-adressen, bevatten de namen de naam van de ESXi-host. De vermelde ESXi-host is de host waar de virtuele machine werd geplaatst op het moment van detectie door het hulpprogramma tijdens de profileringsperiode.

**VM-compatibiliteit**: geeft aan waarom de virtuele machine niet compatibel is voor gebruik met Site Recovery. De redenen worden voor elke niet-compatibele schijf van de virtuele machine beschreven. Op basis van gepubliceerde [opslaglimieten](https://aka.ms/azure-storage-scalbility-performance) kan dit een van de volgende redenen zijn:

* De schijf is groter dan 4095 GB. Azure Storage biedt momenteel geen ondersteuning voor gegevensschijven groter dan 4095 GB.
* De besturingssysteemschijf is groter dan 2048 GB. Azure Storage biedt momenteel geen ondersteuning voor besturingssysteemschijven groter dan 2048 GB.
* Opstarttype is EFI. Azure Site Recovery ondersteunt op dit moment alleen virtuele machines met het BIOS-opstarttype.

* Totale grootte van virtuele machine (replicatie + TFO) overschrijdt de ondersteunde limiet voor opslagaccounts (35 TB). Dit probleem treedt meestal op wanneer één schijf in de virtuele machine een prestatiekenmerk heeft dat groter is dan de maximaal ondersteunde limieten voor Standard-opslag van Azure of Site Recovery. De virtuele machine komt dan in aanmerking voor Premium Storage. De maximaal ondersteunde grootte van een Premium Storage-account is echter 35 TB en één beveiligde virtuele machine kan niet worden beveiligd via meerdere opslagaccounts. Houd er ook rekening mee dat bij het uitvoeren van een testfailover op een beveiligde virtuele machine, deze wordt uitgevoerd in het opslagaccount waarin ook de replicatie plaatsvindt. Stel in dit geval 2 x de grootte van de schijf in om replicatie mogelijk te maken en het testen van failover parallel uit te voeren.
* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor opslag van 5000 per schijf.
* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor opslag van 80.000 per schijf.
* Het gemiddelde gegevensverloop overschrijdt de ondersteunde Site Recovery-limiet voor gegevensverloop van 10 MBps voor de gemiddelde IO-grootte voor de schijf.
* Het totale gegevensverloop voor alle schijven in de virtuele machine overschrijdt de maximaal ondersteunde limiet voor Site Recovery-gegevensverloop van 54 MBps per virtuele machine.
* De gemiddelde effectieve schrijf-IOPS overschrijdt de ondersteunde IOPS-limiet van Site Recovery van 840 voor schijven.
* De berekende opslag voor momentopnamen overschrijdt de ondersteunde opslaglimiet van 10 TB voor momentopnamen.

**R/W IOPS (met groeifactor)**: de piekworkload-IOPS op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). De totale IOPS voor lezen/schrijven van een virtuele machine is niet altijd de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven van de virtuele machine. De piek-IOPS voor lezen/schrijven van de virtuele machine is namelijk de piek van de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven voor elke minuut van de profileringsperiode.

**Gegevensverloop in Mbps (met groeifactor)**: het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Aantal schijven**: het totale aantal VMDK's van de virtuele machine.

**Schijfgrootte (GB)**: de totale ingestelde grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: de hoeveelheid RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Opstarttype**: het opstarttype van de virtuele machine. Dit kan BIOS of EFI zijn. Op dit moment ondersteunt Azure Site Recovery alleen BIOS-opstarttypen. Alle virtuele machines van het EFI-opstarttype worden vermeld op het werkblad Incompatibele virtuele machines.

**Type besturingssysteem**: het type besturingssysteem van de virtuele machine. Dit kan Windows, Linux of een ander besturingssysteem zijn.


## <a name="site-recovery-limits"></a>Site Recovery-limieten

**Beoogde replicatieopslag** | **Gemiddelde I/O-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 Mbps | 168 GB per schijf
Premium P10-schijf | 8 kB | 2 Mbps | 168 GB per schijf
Premium P10-schijf | 16 kB | 4 Mbps | 336 GB per schijf
Premium P10-schijf | 32 kB of meer | 8 MBps | 672 GB per schijf
Premium P20- of P30-schijf | 8 kB  | 5 Mbps | 421 GB per schijf
Premium P20- of P30-schijf | 16 kB of meer |10 Mbps | 842 GB per schijf

Dit zijn gemiddelden uitgaande van een I/O-overlapping van 30%. Site Recovery kan een hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag. De bovenstaande waarden zijn gebaseerd op een typische backlog van ongeveer vijf minuten. Dat wil zeggen dat de gegevens na het uploaden binnen vijf minuten worden verwerkt en er een herstelpunt is gemaakt.

Deze limieten zijn gebaseerd op onze tests, maar dekken niet alle mogelijke toepassings-I/O-combinaties. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. Voor optimale resultaten, zelfs na het plannen van de implementatie, is het altijd beter om toepassingen uitgebreid te testen met behulp van een testfailover. Zo krijgt u een nauwkeurig inzicht in de prestaties.

## <a name="updating-the-deployment-planner"></a>De implementatieplanner bijwerken
Ga als volgt te werk om de implementatieplanner bij te werken:

1. Download de nieuwste versie van de [Azure Site Recovery-implementatieplanner](https://aka.ms/asr-deployment-planner).

2. Kopieer de gecomprimeerde map naar de server waarop u de planner wilt uitvoeren.

3. Pak de gecomprimeerde map uit.

4. Voer een van de volgende bewerkingen uit:
 * Als de nieuwste versie geen bijgewerkte profileringsvoorziening bevat en er al een profilering wordt uitgevoerd in uw huidige versie van de planner, laat u de profilering gewoon doorgaan.
 * Als de nieuwste versie wel een bijgewerkte profileringsvoorziening bevat, adviseren we om de profilering met de huidige versie te stoppen en opnieuw te starten met de nieuwe versie.

  >[!NOTE]
  >
  >Wanneer u gaat profileren met de nieuwe versie, geef dan dezelfde uitvoerdirectory op zodat het hulpprogramma profielgegevens kan toevoegen aan de bestaande bestanden. Er wordt een volledige set geprofileerde gegevens gebruikt om het rapport te genereren. Als u een andere uitvoerdirectory opgeeft, worden er nieuwe bestanden gemaakt en worden oude profileringsgegevens niet gebruikt bij het genereren van het rapport.
  >
  >Elke nieuwe implementatieplanner is een cumulatieve update van het ZIP-bestand. U hoeft dus niet de nieuwste bestanden naar de vorige map te kopiëren. U kunt een nieuwe map maken en deze gebruiken.


## <a name="version-history"></a>Versiegeschiedenis

### <a name="131"></a>1.3.1
Bijgewerkt: 19 juli 2017

De volgende nieuwe functie is toegevoegd:

* Er is ondersteuning toegevoegd voor grote schijven (> 1 TB) bij het genereren van rapporten. Voortaan kunt u met de Implementatieplanner ook replicatie plannen voor virtuele machines met een schijfgrootte van meer dan 1 TB (maximaal 4095 GB).
Meer informatie over [ondersteuning voor grote schijven in Azure Site Recovery](https://azure.microsoft.com/en-us/blog/azure-site-recovery-large-disks/)


### <a name="13"></a>1.3
Bijgewerkt: 9 mei 2017

De volgende nieuwe functie is toegevoegd:

* Toegevoegd is ondersteuning voor beheerde schijven bij het genereren van rapporten. Het aantal virtuele machines dat kan worden geplaatst in een enkel opslagaccount, wordt berekend op basis van het feit om een beheerde schijf is geselecteerd voor Failover/Test-failover.        


### <a name="12"></a>1.2
Bijgewerkt: 7 april 2017

De volgende oplossingen zijn toegevoegd:

* Toegevoegd is de controle van het opstarttype (BIOS of EFI) voor elke virtuele machine om te bepalen of de virtuele machine compatibel is met de beveiliging.
* Toegevoegd is informatie over het type besturingssysteem voor elke virtuele machine op de werkbladen Compatibele virtuele machines en Incompatibele virtuele machines.
* De bewerking GetThroughput wordt nu ondersteund in de Microsoft Azure-regio's van de Amerikaanse overheid en China.
* Er zijn meer controles op vereisten toegevoegd voor vCenter en ESXi Server.
* Er werd een onjuist rapport gegenereerd als de landinstellingen stonden ingesteld op een andere taal dan Engels.


### <a name="11"></a>1.1
Bijgewerkt: 9 maart 2017

De volgende problemen zijn verholpen:

* Het hulpprogramma kan geen virtuele machines profileren als het vCenter twee of meer virtuele machines met dezelfde naam of hetzelfde IP-adres bevat voor verschillende ESXi-hosts.
* Kopiëren en zoeken is uitgeschakeld voor de werkbladen Compatibele VM's en Incompatibele VM's.

### <a name="10"></a>1.0
Bijgewerkt: 23 februari 2017

De openbare preview 1.0 van de Azure Site Recovery-implementatieplanner heeft de volgende bekende problemen (die in toekomstige updates worden verholpen):

* Het hulpprogramma werkt alleen voor VMware-naar-Azure-implementaties, niet voor Hyper-V-naar-Azure-implementaties. Gebruik voor Hyper-V naar Azure de [capaciteitsplanner voor Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* De bewerking GetThroughput wordt niet ondersteund in de Microsoft Azure-regio's US Government en China.
* Het hulpprogramma kan geen virtuele machines profileren als de vCenter-server twee of meer virtuele machines met dezelfde naam of hetzelfde IP-adres bevat voor verschillende ESXi-hosts. In deze versie slaat het hulpprogramma profilering over bij namen of IP-adressen van virtuele machines die dubbel voorkomen in het bestand dat is opgegeven voor VMListFile. Een tijdelijke oplossing is de virtuele machines te profileren met behulp van een ESXi-host in plaats van de vCenter-server. U moet voor elke ESXi-host één exemplaar uitvoeren.
