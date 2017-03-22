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
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 2/21/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 2575621d72b7db2b090ba923324697b7fa7b8308
ms.lasthandoff: 03/15/2017


---
#<a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery-implementatieplanner
Dit is de gebruikershandleiding voor de Azure Site Recovery-implementatieplanner voor productie-installaties van het type VMware naar Azure.


##<a name="overview"></a>Overzicht

Voordat u een virtuele VMware-machine beveiligt met behulp van Azure Site Recovery, moet u voldoende bandbreedte toewijzen op basis van uw dagelijkse veranderingssnelheid om te voldoen aan de gewenste RPO. U moet on-premises het juiste aantal configuratieservers en processervers implementeren. Ook moet u het juiste type en aantal Azure-doelopslagaccounts maken (Standard of Premium), waarbij u rekening houdt met groei op uw bronproductieservers door toegenomen gebruik na verloop van tijd. Het opslagtype wordt per virtuele machine bepaald op basis van de workloadkenmerken (R/W IOPS, gegevensverloop) en de limieten van Azure Site Recovery.  

De openbare preview van de Azure Site Recovery-implementatieplanner is een opdrachtregelprogramma dat momenteel alleen beschikbaar is voor VMware naar Azure. U kunt uw virtuele VMware-machines met dit hulpprogramma op afstand profileren (zonder enige productie-invloed) om inzicht te krijgen in de bandbreedte en in de Azure-opslagvereisten voor replicatie en testfailover.  U kunt het hulpprogramma uitvoeren zonder on-premises Azure Site Recovery-onderdelen te installeren. Voor nauwkeurige doorvoerresultaten wordt aangeraden de planner uit te voeren op een Windows-server die voldoet aan de minimale vereisten van de Azure Site Recovery-configuratieserver die u uiteindelijk gaat implementeren als een van de eerste stappen van de productie-implementatie.

Het hulpprogramma levert de volgende gegevens:

**Beoordeling van compatibiliteit**<br>
* Beoordeling van geschiktheid van de virtuele machine op basis van het aantal schijven, schijfgrootte, IOPS en verloop

**Vereisten voor netwerkbandbreedte vs. RPO-evaluatie**<br>
* Geschatte vereiste bandbreedte voor replicatie van verschillen<br>
* Doorvoer die Azure Site Recovery van on-premises naar Azure kan sturen<br>
* Aantal virtuele machines naar batch op basis van de geschatte bandbreedte om initiële replicatie in een bepaalde tijd te voltooien<br>

**Vereisten voor Microsoft Azure-infrastructuur**<br>
* Vereisten voor type opslag (Standard of Premium Storage) voor elke virtuele machine<br>
* Totaal aantal Standard en Premium Storage-accounts dat voor replicatie moet worden ingericht<br>
* Suggesties voor naamgeving van opslagaccounts op basis van Azure Storage-richtlijnen<br>
* Plaatsing van het opslagaccount van elke virtuele machine<br>
* Aantal kernen voor Microsoft Azure dat moet worden ingericht voor testfailover/failover op het abonnement<br>
* Aanbevolen Microsoft Azure VM-grootte voor elke on-premises virtuele machine<br>

**On-premises infrastructuurvereisten**<br>
* Het vereiste aantal configuratieservers en processervers dat on-premises moet worden geïmplementeerd<br>

>[!IMPORTANT]
>
>Deze berekeningen worden in het hulpprogramma uitgevoerd uitgaande van een groeifactor van 30% in de kenmerken van uw workload door eventueel groter gebruik in de toekomst en op basis van de 95e percentiel van alle metrische profileringsgegevens (R/W IOPS, verloop enz.) Beide parameters (groeifactor en percentielberekening) kunnen worden geconfigureerd. Meer informatie over de [groeifactor](site-recovery-deployment-planner.md#growth-factor) en de [percentielwaarde die voor de berekening wordt gebruikt](site-recovery-deployment-planner.md#percentile-value-used-for-the-calculation).
>


## <a name="requirements"></a>Vereisten
Het hulpprogramma heeft twee belangrijke fasen: profileren en rapporteren genereren. Er is ook een derde optie, waarmee alleen doorvoer wordt berekend. Hieronder vindt u de vereisten voor de server waarop de profilering/doorvoermeting wordt gestart.

| Vereiste | Beschrijving|
|---|---|
|Profileren en meten van doorvoer| <br>Besturingssysteem: Microsoft Windows Server 2012 R2 <br>In het ideale geval met minimaal de volgende configuratieserver[grootte](https://aka.ms/asr-v2a-on-prem-components)<br>Machineconfiguratie: 8 vCPu, 16 GB aan RAM-geheugen, 300 GB harde schijf<br [Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)<br>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)<br>[Microsoft Visual C++ Redistributable voor Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)<br> Internettoegang tot Microsoft Azure vanaf deze server<br> Microsoft Azure-opslagaccount<Br>Beheerderstoegang op de server<br>Minimale vrije schijfruimte van 100 GB (uitgaande van 1000 virtuele machines met gemiddeld 3 schijven per machine die elke 30 dagen worden geprofileerd)|
| Rapporten genereren| Windows-pc's/Windows-servers met Microsoft Excel 2013 en hoger |
| Gebruikersmachtigingen | Alleen-lezen-machtiging voor het gebruikersaccount dat wordt gebruikt voor toegang tot de VMware vCenter-/vSphere-server tijdens het profileren|


> [!NOTE]
>
> Het hulpprogramma kan alleen virtuele machines met VMDK- en RDM-schijven profileren. Het hulpprogramma kan geen virtuele machines met iSCSI- of NFS-schijven profileren. Hoewel Azure Site Recovery iSCSI- en NFS-schijven voor VMware-servers ondersteunt, kan het hulpprogramma deze schijftypen niet zien. Dit komt doordat de implementatieplanner zich niet in de gast bevindt en de profilering alleen gebruikmaakt van prestatiemeteritems in vCenter.
>


##<a name="download"></a>Downloaden
[Download](https://aka.ms/asr-deployment-planner) de nieuwste versie van de openbare preview van de Azure Site Recovery-implementatieplanner.  Het hulpprogramma bevindt zich in een ZIP-bestand.  De huidige versie van het hulpprogramma ondersteunt alleen het VMware Azure-scenario.

Kopieer het ZIP-bestand naar de Windows-server waarop u het hulpprogramma wilt uitvoeren. U kunt het hulpprogramma uitvoeren op elke Windows Server 2012 R2 met netwerktoegang tot de VMware vCenter-server of de VMware vSphere ESXi-host met de virtuele machines die moeten worden geprofileerd, maar u doet er verstandig aan het hulpprogramma uit te voeren op een server met een hardwareconfiguratie die overeenkomt met de [richtlijnen voor de grootte van de configuratieserver](https://aka.ms/asr-v2a-on-prem-components).  Als u Azure Site Recovery-onderdelen al on-premises hebt geïmplementeerd, moet u het hulpprogramma uitvoeren vanaf de configuratieserver. Dezelfde hardwareconfiguratie als die van de configuratieserver (met een ingebouwde processerver) wordt aanbevolen op de server waarop u het hulpprogramma uitvoert. Dit zorgt ervoor dat de bereikte doorvoer die het hulpprogramma rapporteert, overeenkomt met de daadwerkelijke doorvoer die Azure Site Recovery tijdens de replicatie kan halen. De doorvoerberekening hangt af van de beschikbare netwerkbandbreedte op de server en van de hardwareconfiguratie (CPU, opslag enz.) van de server. Als u het hulpprogramma vanaf een andere server uitvoert, wordt de doorvoer berekend op basis van die server naar Microsoft Azure. Daarnaast is de hardwareconfiguratie van de server mogelijk anders dan die van de configuratieserver. Dit leidt ertoe dat de bereikte doorvoer die door het hulpprogramma wordt gerapporteerd, onjuist is.

Pak de gecomprimeerde map uit. U ziet meerdere bestanden en submappen. Het uitvoerbare bestand is ASRDeploymentPlanner.exe in de bovenliggende map.

Voorbeeld: kopieer het ZIP-bestand naar station E:\ en pak het uit.
E:\ASR Deployment Planner-Preview_v1.1.zip

E:\ASR Deployment Planner-Preview_v1.1\ ASR Deployment Planner-Preview_v1.1\ ASRDeploymentPlanner.exe

##<a name="capabilities"></a>Functionaliteit
Het opdrachtregelprogramma (ASRDeploymentPlanner.exe) kan worden uitgevoerd in een van de volgende drie modi:

1.    Profileren  
2.    Rapporten genereren
3.    Doorvoer bepalen

U moet eerst het hulpprogramma uitvoeren in de profileermodus om gegevensverloop en IOPS van de virtuele machine te verzamelen.  Voer vervolgens het hulpprogramma uit om het rapport te genereren en de netwerkbandbreedte en opslagvereisten te bepalen.

##<a name="profiling"></a>Profileren
In de profileringsmodus maakt het hulpprogramma van de implementatieplanner verbinding met de vCenter-server of de vSphere ESXi-hosts voor het verzamelen van prestatiegegevens over de virtuele machine.

* Profileren heeft geen invloed op de prestaties van de virtuele productiemachines omdat hiermee geen rechtstreekse verbinding wordt gemaakt. Alle prestatiegegevens worden verzameld via de vCenter-server/vSphere ESXi-host.
* De vCenter-server/vSphere EXSi-host wordt slechts om de 15 minuten bevraagd om ervoor te zorgen dat het profileren een minimaal effect heeft op de server. Dit is zonder gevaar voor de nauwkeurigheid van de profilering omdat het hulpprogramma alle prestatiemeteritemgegevens per minuut opslaat.

####<a name="create-a-list-of-virtual-machines-to-profile"></a>Een lijst maken met virtuele machines die moeten worden geprofileerd
Eerst moet u een lijst maken met virtuele machines die u wilt profileren. U kunt de namen van alle virtuele machines op een VMware vCenter of VMware vSphere ESXi-host ophalen met behulp van de volgende VMware vSphere PowerCLI-opdrachten. U kunt ook handmatig de beschrijvende namen/IP-adressen van de virtuele machines die u wilt profileren, in een bestand noteren.

1.    Aanmelden bij de virtuele machine waarop VMware vSphere PowerCLI is geïnstalleerd
2.    VMware vSphere PowerCLI-console openen
3.    Zorg ervoor dat het uitvoeringsbeleid niet is uitgeschakeld voor het script. Als het is uitgeschakeld, start u de VMware vSphere PowerCLI-console in de beheerdersmodus en voert u de volgende opdracht uit om het in te schakelen:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4.    Voer de volgende twee opdrachten uit om de namen van alle virtuele machines op een VMware vCenter of VMware vSphere ESXi op te halen en sla deze op in een TXT-bestand.
Vervang &lsaquo;servernaam&rsaquo;, &lsaquo;gebruikersnaam&rsaquo;, &lsaquo;wachtwoord&rsaquo;, &lsaquo;outputfile.txt&rsaquo;; door uw invoer.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>


5.    Open het uitvoerbestand in Kladblok. Kopieer de namen van alle virtuele machines die u wilt profileren, naar een ander bestand (bijvoorbeeld ProfileVMList.txt). Op elke regel moet één naam van een virtuele machine staan. Dit bestand wordt gebruikt als invoer voor de parameter -VMListFile van het opdrachtregelprogramma

    ![Implementatieplanner](./media/site-recovery-deployment-planner/profile-vm-list.png)


####<a name="start-profiling"></a>Profileren starten
Wanneer u de lijst met te profileren virtuele machines hebt opgesteld, kunt u het hulpprogramma uitvoeren in de profileringsmodus. Hier volgt een lijst met verplichte en optionele parameters van het hulpprogramma die u in de profileringsmodus kunt uitvoeren. Parameters tussen [] zijn optioneel.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Naam van de parameter | Beschrijving |
|---|---|
| -Operation |      StartProfiling |
| -Server | FQDN-naam of IP-adres van de vCenter-server/ESXi-host waarvan de virtuele machines moeten worden geprofileerd.|
| -User | De naam van de gebruiker die verbinding maakt met de vCenter-server/ESXi-host. De gebruiker moet ten minste alleen-lezen-toegang hebben.|
| -VMListFile |    Het bestand met de lijst met virtuele machines die moeten worden geprofileerd. Het bestandspad kan absoluut of relatief zijn. Dit bestand moet per regel één virtuele machinenaam/IP-adres bevatten. De naam van de virtuele machine die in het bestand is opgegeven, moet gelijk zijn aan de naam van de virtuele machine op de vCenter-server of de ESXi-host. <br> Bijvoorbeeld: het bestand VMList.txt bevat de volgende virtuele machines:<br>virtual_machine_A <br>10.150.29.110<br>virtual_machine_B |
| -NoOfDaysToProfile | Aantal dagen dat profilering moet worden uitgevoerd. U doet er verstandig aan om profilering langer dan 15 dagen uit te voeren om ervoor te zorgen dat het workloadpatroon in uw omgeving in de opgegeven periode goed wordt waargenomen, zodat er een nauwkeurige aanbeveling kan worden gedaan. |
| [-Directory] |    UNC of lokale directory voor het opslaan van de gegevens die tijdens profilering zijn gegenereerd. Als er geen directory wordt opgegeven, wordt de directory met de naam 'ProfiledData' onder het huidige pad gebruikt als de standaarddirectory. |
| [-Password ] | Het wachtwoord waarmee verbinding wordt gemaakt met de vCenter-server/ESXi-host. Als u dit nu niet opgeeft, wordt u erom gevraagd wanneer de opdracht wordt uitgevoerd.|
|  [-StorageAccountName]  | Azure Storage-accountnaam om de bereikbare doorvoer te vinden voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de doorvoer te berekenen.|
| [-StorageAccountKey] | Azure Storage-accountsleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > [Naam van het opslagaccount] > Instellingen > Toegangssleutels > Key1 (of primaire toegangssleutel voor klassieke opslagaccount). |

Het verdient aanbeveling uw virtuele machines ten minste 15 tot 30 dagen te profileren. Gedurende de periode voor profilering blijft ASRDeploymentPlanner.exe actief. Het hulpprogramma noteert de invoer van de profileringstijd in dagen. Als u enkele uren of minuten wilt profileren om het hulpprogramma even te testen, moet u in de openbare preview de tijd omzetten in een corresponderende meting in dagen.  Als u bijvoorbeeld 30 minuten wilt profileren, moet de invoer 30 / (60 * 24) = 0,021 dagen zijn.  De minimaal toegestane tijd voor profilering is 30 minuten.

Tijdens het profileren kunt u eventueel een Azure Storage-accountnaam en -sleutel doorgeven om vast te stellen wat de doorvoer is die Azure Site Recovery kan behalen op het moment van replicatie vanaf de configuratieserver/processerver naar Azure. Als de Azure Storage-accountnaam en -sleutel tijdens de profilering niet worden doorgegeven, berekent het hulpprogramma niet de bereikbare doorvoer.


U kunt meerdere exemplaren van het hulpprogramma uitvoeren voor verschillende sets virtuele machines. Zorg ervoor dat de namen van de virtuele machines niet worden herhaald in een van de profileringssets. Voorbeeld: als u tien virtuele machines hebt geprofileerd (VM1 - VM10) en na een paar dagen vijf andere virtuele machines wilt profileren (VM11 - VM15), kunt u voor de tweede set virtuele machines (VM11 - VM15) het hulpprogramma uitvoeren vanaf een andere opdrachtregelconsole. Zorg er wel voor dat de tweede set virtuele machines geen namen bevat van de eerste reeks geprofileerde virtuele machines of gebruik voor de tweede set een andere uitvoerdirectory. Als er voor het profileren van dezelfde virtuele machines twee exemplaren van het hulpprogramma worden gebruikt en dezelfde uitvoermap wordt gebruikt, is het gegenereerde rapport niet correct.

De VM-configuratie wordt eenmaal aan het begin van de profilering vastgelegd en opgeslagen in een bestand met de naam VMDetailList.xml. Deze gegevens worden gebruikt wanneer het rapport wordt gegenereerd. Eventuele wijzigingen in de VM-configuratie (bijvoorbeeld een hoger aantal kernen, schijven, NIC's enz.) vanaf de start van de profilering tot aan de beëindiging hiervan worden niet vastgelegd. In het geval een geprofileerde VM-configuratie in de loop van het profileren is gewijzigd, kunt u in de openbare preview de volgende oplossing gebruiken om de laatste VM-gegevens te achterhalen wanneer u het rapport genereert.   

* Maak een back-up van VMdetailList.xml en verwijder het bestand van de huidige locatie.
* Geef argumenten voor -User en -Password door wanneer u het rapport genereert.

De opdracht voor profilering genereert meerdere bestanden in de profileringsdirectory. Verwijder geen van deze bestanden, omdat dit gevolgen heeft voor het genereren van het rapport.

#####<a name="example-1-to-profile-virtual-machines-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Voorbeeld 1: virtuele machines 30 dagen profileren en de doorvoer van on-premises naar Azure bepalen
ASRDeploymentPlanner.exe **-Operation** StartProfiling -Directory “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  30  **-User** vCenterUser1 **-StorageAccountName**  asrspfarm1 **-StorageAccountKey** Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

#####<a name="example-2-to-profile-virtual-machines-for-15-days"></a>Voorbeeld 2: virtuele machines 15 dagen profileren
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  15  -User vCenterUser1

#####<a name="example-3-to-profile-virtual-machines-for-1-hour-for-a-quick-test-of-the-tool"></a>Voorbeeld 3: virtuele machines 1 uur profileren om het hulpprogramma snel te testen
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-NoOfDaysToProfile**  0.04  **-User** vCenterUser1


>[!NOTE]
>
> * Als de server waarop het hulpprogramma wordt uitgevoerd, opnieuw wordt opgestart of is gecrasht, of als u het hulpprogramma afsluit met Ctrl + C, blijven de profileringsgegevens behouden. De kans bestaat echter wel dat de laatste 15 minuten aan geprofileerde gegevens ontbreekt. U moet het hulpprogramma opnieuw uitvoeren in de profileringsmodus nadat de server opnieuw is gestart.
>
> * Wanneer de Azure Storage-accountnaam en -sleutel zijn doorgegeven, meet het hulpprogramma de doorvoer tijdens de laatste stap van de profilering. Als het hulpprogramma wordt beëindigd voordat de profilering correct is voltooid, wordt de doorvoer niet berekend. U kunt altijd de bewerking GetThroughput uitvoeren vanaf de opdrachtregelconsole om de doorvoer te bepalen voordat het rapport wordt gegenereerd, anders bevat het gegenereerde rapport niet de uiteindelijke doorvoer.
>

##<a name="generate-report"></a>Rapport genereren
Het hulpprogramma genereert een rapport in de vorm van een XLSM-bestand (Microsoft Excel-macrobestand) met daarin een overzicht van de aanbevelingen. Het rapport heet DeploymentPlannerReport_<Unique Numeric Identifier>xlsm en wordt in de opgegeven directory geplaatst.

Nadat de profilering is voltooid, kunt u het hulpprogramma uitvoeren in de modus voor het genereren van een rapport. Hier volgt een lijst met verplichte en optionele parameters van het hulpprogramma die u kunt uitvoeren in de profileringsmodus. Parameters tussen [] zijn optioneel.

ASRDeploymentPlanner.exe -Operation GenerateReport /?

|Naam van de parameter | Beschrijving |
|-|-|
| -Operation | GenerateReport |
| -Server |  De FQDN-naam of het IP-adres van de vCenter/vSphere-server (gebruik exact dezelfde naam of hetzelfde IP-adres als op het moment van profilering) waar de geprofileerde virtuele machines zich bevinden waarvan een rapport wordt gegenereerd. Let op: als u ten tijde van de profilering een vCenter-server hebt gebruikt, kunt u geen vSphere-server gebruiken voor het genereren van rapporten en vice versa.|
| -VMListFile | Het bestand met de lijst van geprofileerde virtuele machines waarvoor een rapport wordt gegenereerd. Het bestandspad kan absoluut of relatief zijn. Dit bestand moet per regel één virtuele machinenaam/IP-adres bevatten. De namen van de virtuele machines die in het bestand zijn opgegeven, moeten gelijk zijn aan de namen van de virtuele machines op de vCenter-server of de ESXi-host en overeenkomen met de namen die ten tijde van de profilering zijn gebruikt.|
| [-Directory] | UNC of het lokale directorypad waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Indien niet opgegeven, wordt de directory ProfiledData gebruikt. |
| [-GoalToCompleteIR] |    Het aantal uren waarin de initiële replicatie van de geprofileerde virtuele machines moet worden voltooid. Het gegenereerde rapport bevat het aantal virtuele machines waarvoor de initiële replicatie in de opgegeven tijd kan worden voltooid. Standaard is dit 72 uur. |
| [-User] | De naam van de gebruiker die verbinding maakt met de vCenter-/vSphere-server. Deze parameter wordt gebruikt voor het ophalen van de meest recente configuratiegegevens van de virtuele machines, zoals het aantal schijven, kernen, NIC's enz. die in het rapport moeten worden gebruikt. Indien niet opgegeven, worden de configuratiegegevens gebruikt die aan het begin van de profilering zijn verzameld. |
| [-Password] | Het wachtwoord waarmee verbinding wordt gemaakt met de vCenter-server/ESXi-host. Als dit niet als parameter is opgegeven, wordt u erom gevraagd wanneer de opdracht wordt uitgevoerd. |
| [-DesiredRPO] | Het gewenste beoogde herstelpunt (Recovery Point Objective (RPO)) in minuten. Standaard is dit 15 minuten.|
| [-Bandwidth] | De bandbreedte in Mbps. Deze parameter wordt gebruikt voor het berekenen van de RPO die voor de opgegeven bandbreedte kan worden bereikt. |
| [-StartDate]  | De begindatum en -tijd in mm-dd-jjjj:uu:mm (in 24-uursindeling). Als u StartDate opgeeft, moet u ook EndDate opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| [-EndDate] | De einddatum en -tijd in mm-dd-jjjj:uu:mm (in 24 uursindeling). Als u EndDate opgeeft, moet u ook StartDate opgeven. Het rapport wordt dan gegenereerd voor de geprofileerde gegevens die zijn verzameld tussen StartDate en EndDate. |
| [-GrowthFactor] |De groeifactor als percentage. Standaard is dit 30%.  |


##### <a name="example-1-to-generate-report-with-default-values-when-profiled-data-is-on-the-local-drive"></a>Voorbeeld 1: een rapport met standaardwaarden genereren wanneer de geprofileerde gegevens zich op de lokale schijf bevinden
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”


##### <a name="example-2-to-generate-report-when-profiled-data-is-on-a-remote-server-user-should-have-readwrite-access-on-the-remote-directory"></a>Voorbeeld 2: een rapport genereren wanneer de geprofileerde gegevens zich op een externe server bevinden. De gebruiker moet lees-/schrijftoegang hebben voor de externe directory.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “\\\\PS1-W2K12R2\vCenter1_ProfiledData” **-VMListFile** “\\\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”

##### <a name="example-3-generate-report-with-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Voorbeeld 3: een rapport genereren met specifieke bandbreedte en het doel IR te voltooien binnen de opgegeven periode
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-Bandwidth** 100 **-GoalToCompleteIR** 24

##### <a name="example-4-generate-report-with-5-growth-factor-instead-of-the-default-30"></a>Voorbeeld 4: een rapport genereren met een groeifactor van 5% in plaats van de standaardwaarde 30%
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-GrowthFactor** 5

##### <a name="example-5-generate-report-with-a-subset-of-profiled-data-say-you-have-30-days-of-profiled-data-and-want-to-generate-the-report-for-only-20-days"></a>Voorbeeld 5: een rapport genereren met een subset geprofileerde gegevens. Stel dat u 30 dagen aan geprofileerde gegevens hebt en een rapport wilt genereren voor slechts 20 van de 30 dagen.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-StartDate**  01-10-2017:12:30 -**EndDate** 01-19-2017:12:30

##### <a name="example-6-generate-report-for-5-minutes-rpo"></a>Voorbeeld 6: een rapport genereren voor 5 minuten RPO.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  **-DesiredRPO** 5

### <a name="percentile-value-used-for-the-calculation"></a>Percentielwaarde die voor de berekening wordt gebruikt
**Welke standaardpercentielwaarde met betrekking tot de prestatiegerelateerde metrische gegevens die tijdens de profilering zijn verzameld, wordt ten tijde van het genereren van het rapport gebruikt?**

Het hulpprogramma wordt standaard ingesteld op de 95e-percentielwaarde van R/W IOPS, schrijf-IOPS en gegevensverloop die tijdens het profileren van de virtuele machines zijn verzameld. Dit zorgt ervoor dat de 100e-percentielpiek die zich vanwege tijdelijke gebeurtenissen mogelijk voordoet in uw virtuele machines (bijvoorbeeld een back-uptaak die één keer per dag wordt uitgevoerd, het periodiek indexeren van een database of genereren van een analyseactiviteit, of een andere gelijksoortige, kortdurende gebeurtenis die gedurende de profileringsperiode plaatsvindt), niet wordt gebruikt om de vereisten met betrekking tot de doel-Azure-opslag en de bronbandbreedte te bepalen. Het gebruik van de 95e-percentielwaarde biedt een waarheidsgetrouw beeld van de werkelijke workloadkenmerken en verzekert u van optimale prestaties wanneer deze workloads in Microsoft Azure worden uitgevoerd. U hoeft dit getal niet vaak te wijzigen, maar als u deze nog lager wilt instellen, bijvoorbeeld op het 90e percentiel, kunt u dit configuratiebestand 'ASRDeploymentPlanner.exe.config' in de standaardmap bijwerken en opslaan om een nieuw rapport te genereren op basis van de bestaande geprofileerde gegevens.

        &lsaquo;add key="WriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="ReadWriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="DataChurnPercentile" value="95" /&rsaquo;

### <a name="growth-factor"></a>Groeifactor
**Waarom moet ik bij het plannen van een implementatie rekening houden met de groeifactor?**

Het is essentieel dat u in uw workloadkenmerken rekening houdt met groei (mogelijke toename in gebruik na verloop van tijd). Als de beveiliging eenmaal is ingeschakeld en uw workloadkenmerken vervolgens veranderen, hebt u momenteel namelijk niet de mogelijkheid om over te schakelen naar een ander Azure-opslagaccount voor beveiliging zonder de beveiliging uit te schakelen en opnieuw in te schakelen. Voorbeeld: als een virtuele machine op dit moment geschikt is voor een Standard Storage-account voor replicatie, maar over drie maanden het verloop op de virtuele machine dusdanig toeneemt (vanwege een toename van het aantal gebruikers van de toepassing op de virtuele machine) dat verhoging naar Premium Storage is vereist om Azure Site Recovery-replicatie in staat te stellen het nieuwe hogere verloop bij te houden, moet u de beveiliging uitschakelen en deze opnieuw inschakelen voor een premium-opslagaccount. Het wordt dus sterk aanbevolen om tijdens de implementatieplanning rekening te houden met groei. De standaardwaarde hiervoor is 30%. U kent het gebruikspatroon van uw toepassingen en de groeivooruitzichten zelf het beste. Pas dit percentage tijdens het genereren van een rapport daarom desgewenst aan. U kunt zelfs meerdere rapporten met verschillende groeifactoren genereren met dezelfde geprofileerde gegevens en zo nagaan welke aanbevelingen voor doel-Azure-opslag en bronbandbreedte het beste aansluiten bij uw situatie.

Het gegenereerde rapport in Microsoft Excel-indeling bevat de volgende bladen

* [Invoer](site-recovery-deployment-planner.md#input)
* [Aanbevelingen](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Aanbevelingen - bandbreedte invoer](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [VM<->Opslagplaatsing](site-recovery-deployment-planner.md#vm-storage-placement)
* [Compatibele VM's](site-recovery-deployment-planner.md#compatible-vms)
* [Niet-compatibele VM's](site-recovery-deployment-planner.md#incompatible-vms)

![Implementatieplanner](./media/site-recovery-deployment-planner/dp-report.png)


##<a name="get-throughput"></a>Doorvoer bepalen
Als u een schatting wilt maken van de doorvoer die Azure Site Recovery tijdens de replicatie van on-premises naar Azure kan behalen, moet u het hulpprogramma uitvoeren in de GetThroughput-modus. Het hulpprogramma berekent de doorvoer van de server waarop het hulpprogramma wordt uitgevoerd (bij voorkeur een server op basis van de richtlijnen voor configuratieservergrootte).  Als u Azure Site Recovery-infrastructuuronderdelen al on-premises hebt geïmplementeerd, moet u het hulpprogramma uitvoeren op de configuratieserver.

Open een opdrachtregelconsole en ga naar de map met het hulpprogramma voor de ASR-implementatieplanner.  Voer ASRDeploymentPlanner.exe uit met de volgende parameters. Parameters tussen [] zijn optioneel.

ASRDeploymentPlanner.exe -Operation GetThroughput /?

|Naam van de parameter | Beschrijving |
|-|-|
| -operation | GetThroughput |
| [-Directory] | UNC of het lokale directorypad waar de geprofileerde gegevens (bestanden die tijdens de profilering zijn gegenereerd) worden opgeslagen. Deze gegevens zijn vereist voor het genereren van het rapport. Indien niet opgegeven, wordt de directory ProfiledData gebruikt.  |
| -StorageAccountName | Azure Storage-accountnaam om de bandbreedte te bepalen die wordt gebruikt voor gegevensreplicatie van on-premises naar Azure. Het hulpprogramma uploadt testgegevens naar dit opslagaccount om de gebruikte bandbreedte te bepalen. |
| -StorageAccountKey | Azure Storage-accountsleutel die wordt gebruikt voor toegang tot het opslagaccount. Ga naar Azure Portal > Opslagaccounts > [Naam van het opslagaccount] > Instellingen > Toegangssleutels > Key1 (of primaire toegangssleutel voor klassieke opslagaccount). |
| -VMListFile | Het bestand met de lijst met virtuele machines die moeten worden geprofileerd voor het berekenen van de gebruikte bandbreedte. Het bestandspad kan absoluut of relatief zijn. Dit bestand moet per regel één virtuele machinenaam/IP-adres bevatten. De namen van de virtuele machines die in het bestand zijn opgegeven, moeten gelijk zijn aan de namen van de virtuele machines op de vCenter-server of de ESXi-host.<br>Voorbeeld: het bestand VMList.txt bevat de volgende virtuele machines:<br>virtuele machine_A <br>10.150.29.110<br>virtuele machine_B|

Het hulpprogramma maakt in de opgegeven directory verschillende asrvhdfile<#>.vhd-bestanden van 64 MB (# is het volgnummer).  Deze bestanden worden geüpload naar het Azure-opslagaccount om de doorvoer te bepalen. Wanneer de doorvoer is gemeten, worden al deze bestanden verwijderd uit het Azure-opslagaccount en van de lokale server. Als het hulpprogramma tijdens het berekenen van de doorvoer halverwege om een willekeurige reden wordt beëindigd, worden deze bestanden niet verwijderd van Azure Storage of van de lokale server. U moet ze dan handmatig verwijderen.

De doorvoer wordt op een bepaald moment gemeten. Dit is de maximale doorvoer die Azure Site Recovery tijdens de replicatie kan behalen, mits alle andere factoren gelijk blijven. Als bijvoorbeeld een bepaalde toepassing op hetzelfde netwerk meer bandbreedte gaat gebruiken, varieert de werkelijke doorvoer tijdens de replicatie. Als u de GetThroughput-opdracht uitvoert vanaf een configuratieserver, is het hulpprogramma niet op de hoogte van beveiligde virtuele machines en continue replicatie. Over het algemeen zullen de meetresultaten verschillen als de GetThroughput-bewerking wordt uitgevoerd op het moment dat de beveiligde virtuele machines een hoog gegevensverloop hebben vs. wanneer het gegevensverloop laag is.  Het verdient aanbeveling het hulpprogramma tijdens de profilering op verschillende tijdstippen uit te voeren om na te gaan welke doorvoersnelheden op verschillende tijdstippen kunnen worden behaald. Het rapport bevat de laatst gemeten doorvoer in het hulpprogramma.


##### <a name="example"></a>Voorbeeld
ASRDeploymentPlanner.exe **-Operation** GetThroughput **-Directory**  E:\vCenter1_ProfiledData **-VMListFile** E:\vCenter1_ProfiledData\ProfileVMList1.txt  **-StorageAccountName**  asrspfarm1 **-StorageAccountKey** by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

>[!NOTE]
>
> * Het hulpprogramma uitvoeren op een server die de dezelfde opslag- en CPU-kenmerken heeft als de configuratieserver
>
> * Pas voor replicatie de aanbevolen bandbreedte toe om te allen tijde aan de RPO te voldoen. Als u zelfs na het toepassen van de juiste bandbreedte geen toename ziet in de door het hulpprogramma gerapporteerde behaalde doorvoer, controleert u het volgende:
>
> a. Controleer of er een netwerk-QoS (Quality of Service) is die de Azure Site Recovery-doorvoer beperkt
>
> b. Controleer of uw Azure Site Recovery-kluis zich in de dichtstbijzijnde fysieke ondersteunde Microsoft Azure-regio bevindt voor minimale netwerklatentie
>
> c. Controleer de kenmerken van uw lokale opslag en probeer de hardware te verbeteren (bijvoorbeeld HDD naar SSD enz.)
>
> d. Wijzig de Azure Site Recovery-instellingen op de processerver om [de voor replicatie gebruikte netwerkbandbreedte te vergroten](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
>

##<a name="recommendations-with-desired-rpo-as-input"></a>Aanbevelingen met gewenste RPO als invoer

###<a name="profiled-data"></a>Geprofileerde gegevens

![Implementatieplanner](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Periode geprofileerde gegevens** is de periode waarin de profilering is uitgevoerd. Standaard gebruikt het hulpprogramma voor de berekening alle geprofileerde gegevens, tenzij het rapport wordt gegenereerd voor een bepaalde periode (tijdens het genereren van het rapport zijn dan waarden opgegeven voor StartDate en EndDate).

**Servernaam** is de naam of het IP-adres van de VMware vCenter of ESXi-host met de virtuele machines waarvoor het rapport is gegenereerd.

**Gewenste RPO** is het gewenste beoogde herstelpunt (RPO) voor uw implementatie. Standaard wordt de vereiste netwerkbandbreedte berekend voor de RPO-waarden van 15, 30 en 60 minuten. Op basis van de selectie worden de betrokken waarden bijgewerkt op het blad. Als u tijdens het genereren van het rapport de parameter DesiredRPOinMin hebt gebruikt, wordt de betreffende waarde weergegeven in deze RPO-vervolgkeuzelijst.

###<a name="profiling-overview"></a>Overzicht van profilering

![Implementatieplanner](./media/site-recovery-deployment-planner/profiling-overview.png)

**Totale aantal geprofileerde virtuele machines** is het totale aantal virtuele machines waarvan de geprofileerde gegevens beschikbaar zijn. Als het bestand VMListFile namen van virtuele machines bevat die niet zijn geprofileerd, worden deze virtuele machines niet meegenomen in het rapport en uitgesloten van het totale aantal geprofileerde virtuele machines.

**Compatibele virtuele machines** is het aantal virtuele machines dat op Azure kan worden beveiligd met Azure Site Recovery. Het is het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het aantal Azure-opslagaccounts, het aantal Microsoft Azure-kernen en het aantal configuratieservers en aanvullende processervers wordt berekend. De details van elke compatibele virtuele machine zijn beschikbaar op het werkblad Compatibele VM's van het rapport.

**Niet-compatibele virtuele machines** is het aantal geprofileerde virtuele machines dat niet compatibel is met beveiliging met Azure Site Recovery. De redenen voor incompatibiliteit worden vermeld in het onderstaande gedeelte over niet-compatibele virtuele machines. Als het bestand VMListFile namen van virtuele machines bevat die niet zijn geprofileerd, worden deze virtuele machines uitgesloten van het aantal niet-compatibele virtuele machines. Deze virtuele machines worden weergegeven als 'Gegevens niet gevonden' aan het einde van het werkblad met niet-compatibele virtuele machines.

**Gewenste RPO** is uw gewenste RPO in minuten. Het rapport wordt gegenereerd voor drie RPO-waarden (15, 30 en 60 minuten), waarbij 15 minuten als standaardwaarde wordt gehanteerd. De aanbeveling voor bandbreedte in het rapport wordt gewijzigd op basis van uw selectie in de vervolgkeuzelijst Gewenste RPO in de rechterbovenhoek van het blad. Als u het rapport hebt gegenereerd met een aangepaste waarde voor de parameter -DesiredRPO, wordt deze aangepaste waarde als standaardwaarde weergegeven in de vervolgkeuzelijst Gewenste RPO.

###<a name="required-network-bandwidth-mbps"></a>Vereiste netwerkbandbreedte (Mbps)

![Implementatieplanner](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**Altijd aan de RPO voldoen:** de aanbevolen bandbreedte in Mbps die moet worden toegewezen om altijd te voldoen aan uw gewenste RPO. Deze bandbreedte moet worden toegewezen voor onveranderlijke replicatie van verschillen bij al uw compatibele virtuele machines om eventuele RPO-schendingen te voorkomen.

**90% van de tijd aan de RPO voldoen**: als u vanwege de kosten voor een breedbandverbinding of om een andere reden niet de benodigde bandbreedte kunt toepassen om altijd aan de RPO te voldoen, kunt u een lager bedrag voor bandbreedte kiezen waarmee u 90% van de tijd aan de RPO kunt voldoen. Voor inzicht in de gevolgen van het toepassen van deze lagere bandbreedte bevat het rapport een what-if-analyse van het aantal en de duur van de RPO-schendingen die u kunt verwachten.

**Behaalde doorvoer:** de doorvoer van de server waar u de GetThroughput-opdracht hebt uitgevoerd naar de Microsoft Azure-regio waar het Azure-opslagaccount zich bevindt. Hier ziet u de geschatte doorvoer die kan worden behaald als u de compatibele virtuele machines beveiligt met Azure Site Recovery, op voorwaarde dat de kenmerken van uw configuratieserver-/processerveropslag gelijk blijven als die van de server waarop u het hulpprogramma hebt uitgevoerd.    

Voor replicatie moet u de aanbevolen bandbreedte toepassen om te allen tijde aan de RPO te voldoen. Als u zelfs na het toepassen van de juiste bandbreedte geen toename ziet in de door het hulpprogramma gerapporteerde behaalde doorvoer, controleert u het volgende:

a.    Controleer of er een netwerk-QoS (Quality of Service) is die de Azure Site Recovery-doorvoer beperkt

b.    Controleer of uw Azure Site Recovery-kluis zich in de dichtstbijzijnde fysieke ondersteunde Microsoft Azure-regio bevindt voor minimale netwerklatentie

c.    Controleer de kenmerken van uw lokale opslag en probeer de hardware te verbeteren (bijvoorbeeld HDD naar SSD enz.)

d. Wijzig de Azure Site Recovery-instellingen op de processerver om [de voor replicatie gebruikte netwerkbandbreedte te vergroten](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Wanneer u het hulpprogramma op een configuratieserver/processerver uitvoert die al beveiligde virtuele machines bevat, voert u het hulpprogramma meerdere keren uit. De behaalde doorvoer varieert namelijk afhankelijk van het verloop dat op dat tijdstip wordt verwerkt.

Voor alle Azure Site Recovery-enterprise-implementaties wordt gebruik van [ExpressRoute](https://aka.ms/expressroute) aanbevolen.

###<a name="required-azure-storage-accounts"></a>Vereiste Azure-opslagaccounts
Deze grafiek geeft het totale aantal Azure-opslagaccounts aan (Standard Storage en Premium Storage) dat is vereist voor het beveiligen van alle compatibele virtuele machines.  Klik op [Aanbevolen VM-plaatsingsplan](site-recovery-deployment-planner.md#vm-storage-placement) om na te gaan welk opslagaccount moet worden gebruikt voor elke virtuele machine.  

![Implementatieplanner](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

###<a name="required-number-of-azure-cores"></a>Het vereiste aantal Azure-kernen
Dit is het totale aantal kernen dat moet worden ingericht vóór failover of testfailover van alle compatibele virtuele machines. Als er in het abonnement onvoldoende kernen beschikbaar zijn, kan Azure Site Recovery geen virtuele machines maken op het moment van een failover of testfailover.

![Implementatieplanner](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

###<a name="required-on-premises-infrastructure"></a>Vereiste on-premises infrastructuur
Het totale aantal configuratieservers en aanvullende processervers dat moet worden geconfigureerd voor het beveiligen van de compatibele virtuele machines. Het hulpprogramma raadt extra servers aan op basis van de ondersteunde [limieten](https://aka.ms/asr-v2a-on-prem-components) op de grootste configuratie: hetzij het verloop per dag, hetzij het maximumaantal beveiligde virtuele machines (uitgaande van gemiddeld drie schijven per virtuele machine), ongeacht welke limiet het eerste wordt bereikt op de configuratieserver of de aanvullende processerver. De details van het totale verloop per dag en het totale aantal beveiligde schijven vindt u op het blad [Invoer](site-recovery-deployment-planner.md#input).

![Implementatieplanner](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

###<a name="what-if-analysis"></a>What-if-analyse
Deze analyse beschrijft hoeveel schendingen zich kunnen voordoen tijdens de profileringsperiode wanneer u een lagere bandbreedte toepast om 90% van de tijd aan de RPO te voldoen. Op een willekeurige dag kunnen zich een of meer RPO-schendingen voordoen. In de grafiek staat de hoogste RPO van de dag.
Op basis van deze analyse kunt u besluiten of het aantal RPO-schendingen voor alle dagen en de hoogste RPO per dag acceptabel zijn in combinatie met de opgegeven lagere bandbreedte. Vindt u deze waarden acceptabel, dan kunt u de lagere bandbreedte toewijzen voor replicatie. Zo niet, dan stelt u de hogere bandbreedte in zoals voorgesteld om altijd aan de RPO te voldoen.

![Implementatieplanner](./media/site-recovery-deployment-planner/what-if-analysis.png)

###<a name="recommended-vm-batch-size-for-initial-replication"></a>Aanbevolen VM-batchgrootte voor de initiële replicatie
In dit gedeelte wordt het aantal virtuele machines aangeraden dat parallel kan worden beveiligd om met de voorgestelde bandbreedte de initiële replicatie binnen 72 uur te voltooien (configureerbare waarde: gebruik bij het genereren van het rapport de parameter GoalToCompleteIR om dit te wijzigen) en hierbij altijd 100% te voldoen aan de gewenste RPO.  In de grafiek ziet u het bereik met bandbreedtewaarden en de berekende grootte voor de batch virtuele machines waarmee de initiële replicatie binnen 72 uur kan worden voltooid, uitgaande van de gemiddelde grootte van de gedetecteerde virtuele machines binnen de volledige set compatibele virtuele machines.  

In de openbare preview specificeert het rapport niet welke virtuele machines in een batch moeten worden opgenomen. U kunt de schijfgrootte die op het werkblad Compatibele VM's wordt weergegeven, gebruiken om de grootte van elke virtuele machine te bepalen en vervolgens virtuele machines voor een batch te selecteren. U kunt ook een selectie maken op basis van de bekende workloadkenmerken.  De tijd die een initiële replicatie kost, wijzigt proportioneel op basis van de daadwerkelijke schijfgrootte van de virtuele machine, de gebruikte schijfruimte en de beschikbare netwerkdoorvoer.

![Implementatieplanner](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

###<a name="growth-factor-and-percentile-values-used"></a>Groeifactor en gebruikte percentielwaarden
Deze sectie onderaan op het werkblad toont de percentielwaarde die voor alle prestatiemeteritems van de geprofileerde virtuele machines wordt gebruikt (standaard is dit het 95e percentiel) en de groeifactor in procenten die in alle berekeningen wordt gebruikt (standaard 30%).

![Implementatieplanner](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

##<a name="recommendations-with-available-bandwidth-as-input"></a>Aanbevelingen met beschikbare bandbreedte als invoer

![Implementatieplanner](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

In bepaalde gevallen weet u mogelijk dat u niet meer dan x Mbps bandbreedte kunt toepassen voor Azure Site Recovery-replicatie. Met het hulpprogramma kunt u de beschikbare bandbreedte opgeven (dit doet u tijdens het genereren van het rapport met behulp van de parameter -Bandwidth) en zo de haalbare RPO in minuten achterhalen. Aan de hand van deze haalbare RPO-waarde kunt u beslissen of u extra bandbreedte wilt toepassen of bereid bent u te beperken tot een noodhersteloplossing voor deze RPO.

![Implementatieplanner](./media/site-recovery-deployment-planner/achievable-rpos.png)

##<a name="input"></a>Invoer
De pagina Invoer biedt een overzicht van de geprofileerde VMware-omgeving.

![Implementatieplanner](./media/site-recovery-deployment-planner/Input.png)

**Begindatum en einddatum** zijn de begin- en einddatums van de profileringsgegevens die in aanmerking komen voor het genereren van rapporten. De begindatum is standaard de datum waarop de profilering start en de einddatum de datum waarop de profilering stopt.  Dit kunnen de waarden voor StartDate en EndDate zijn als het rapport met deze parameters wordt gegenereerd. Begindatum en einddatum zijn de begin- en einddatums van de profileringsgegevens die in aanmerking komen voor het genereren van rapporten. De begindatum is standaard de datum waarop de profilering start en de einddatum de datum waarop de profilering stopt.  Dit kunnen de waarden voor StartDate en EndDate zijn als het rapport met deze parameters wordt gegenereerd.

**Totale aantal dagen van profilering** is het totale aantal dagen van profilering tussen de begin- en einddatum waarvoor het rapport wordt gegenereerd. Totale aantal dagen van profilering is het totale aantal dagen van profilering tussen de begin- en einddatum waarvoor het rapport wordt gegenereerd.

**Aantal compatibele virtuele machine ** is het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het vereiste aantal Azure-opslagaccounts, Microsoft Azure-kernen, configuratieservers en aanvullende processervers wordt berekend.
Totale aantal schijven voor alle compatibele virtuele machines is het totale aantal schijven voor alle compatibele virtuele machines. Dit aantal wordt gebruikt als een van de invoerwaarden om het aantal configuratieservers en aanvullende processenservers te bepalen dat in de implementatie moet worden gebruikt.

**Gemiddeld aantal schijven per compatibele virtuele machine** is het gemiddelde aantal schijven dat voor alle compatibele virtuele machines is berekend.

**Gemiddelde schijfgrootte (GB)** is de gemiddelde schijfgrootte die voor alle compatibele virtuele machines is berekend.

**Gewenste RPO (minuten)** is de standaard-RPO-waarde of de waarde die tijdens het genereren van het rapport voor de parameter DesiredRPO wordt doorgegeven om de vereiste bandbreedte in te schatten.

**Gewenste bandbreedte (Mbps)** is de waarde die u tijdens het genereren van het rapport voor de parameter Bandwidth hebt doorgegeven om de haalbare RPO in te schatten.

**Waargenomen typisch gegevensverloop per dag (GB)** is het gemiddelde gegevensverloop dat voor alle profileringsdagen is vastgesteld. Dit aantal wordt gebruikt als een van de invoerwaarden om het aantal configuratieservers en aanvullende processenservers te bepalen dat in de implementatie moet worden gebruikt.


##<a name="vm-storage-placement"></a>VM-opslagplaatsing

![Implementatieplanner](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Type schijfopslag** is het type Azure-opslagaccount (Standard Storage of Premium Storage) dat wordt gebruikt voor het repliceren van alle bijbehorende virtuele machines in de kolom 'Te plaatsen VM's'.

**Voorgestelde voorvoegsel** is het voorgestelde voorvoegsel, bestaande uit drie tekens, dat kan worden gebruikt voor de naam van het Azure-opslagaccount. U kunt altijd uw eigen voorvoegsel gebruiken, maar de suggesties van het hulpprogramma zijn overeenkomstig de [conventies voor partitienamen van Azure-opslagaccounts](https://aka.ms/storage-performance-checklist).

**Voorgestelde accountnaam** geeft aan hoe de naam van uw Azure-opslagaccount eruitziet nadat het voorgestelde voorvoegsel is bijgevoegd. Vervang de naam in <> door uw aangepaste invoer.

**Logboekopslagaccount:** alle replicatielogboeken worden opgeslagen in een Azure-opslagaccount van het type Standard. Voor virtuele machines die worden gerepliceerd naar een Azure-opslagaccount van het type Premium, moet u een extra Azure-opslagaccount van het type Standard inrichten voor logboekopslag. Eén Standard-account voor het opslaan van logboeken kan worden gebruikt door meerdere Premium-opslagaccounts voor replicatie. Virtuele machines die worden gerepliceerd naar een Standard-opslagaccount, gebruiken hetzelfde account voor het opslaan van logboeken.

**Voorgestelde logboekaccountnaam** geeft aan hoe de naam van uw Azure Storage-logboekaccount eruit moet zien nadat het voorgestelde voorvoegsel is bijgevoegd. Vervang de naam in <> door uw aangepaste invoer.

**Plaatsing samenvatting** bevat een samenvatting van de belasting van het totale aantal virtuele machines in het Azure-opslagaccount tijdens replicatie en failover/testfailover. Hier ziet u het totale aantal virtuele machines dat is toegewezen aan het Azure-opslagaccount, de totale lezen/schrijven-IOPS voor alle virtuele machines die in dit Azure-opslagaccount worden geplaatst, totale schrijven-IOPS (replicatie), totale ingerichte grootte voor alle schijven en het totale aantal schijven.

**Te plaatsen virtuele machines** geeft een lijst van alle virtuele machines die in het opgegeven Azure-opslagaccount moeten worden geplaatst voor optimale prestaties en optimaal gebruik.

## <a name="compatible-vms"></a>Compatibele VM's
![Implementatieplanner](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM-naam** is de naam van de virtuele machine die of het IP-adres dat tijdens het genereren van het rapport is gebruikt in het bestand VMListFile. Deze kolom bevat ook de schijven (VMDK's) die zijn gekoppeld aan de virtuele machines. Virtuele machines op een vCenter met dubbele namen of IP-adressen worden weergegeven met de ESXi-hostnaam om onderscheid te maken tussen de virtuele machines. De vermelde ESXi-host is de host waarop de virtuele machine tijdens de profileringsperiode is geplaatst bij de eerste detectie door het hulpprogramma.

**VM-compatibiliteit** heeft twee waarden: Ja / Ja*. Ja* is bedoeld voor gevallen waarin de virtuele machine dankzij een geprofileerd hoog verloop en een IOPS-schijf die in categorie P20 of P30 past, geschikt is voor [Premium Azure Storage](https://aka.ms/premium-storage-workload), maar de grootte van de schijf ervoor zorgt dat deze als P10 of P20 wordt geclassificeerd. Azure Storage bepaalt op basis van de grootte aan welk type Premium Storage-schijf een schijf moet worden toegewezen. Hierbij geldt het volgende: < 128 GB is P10, 128 tot 512 GB is P20 en 512 GB tot 1023 GB is P30. Dus als een schijf conform de workloadkenmerken in categorie P20 of P30 valt, maar deze in verband met de grootte aan een lager Premium Storage-type wordt toegewezen, markeert het hulpprogramma die virtuele machine als Ja*. Bovendien wordt u aangeraden hetzij de grootte van de bronschijf te wijzigen, zodat deze geschikt is voor het aanbevolen Premium Storage-schijftype, hetzij het type doelschijf na de failover te wijzigen.
Opslagtype is Standard of Premium.

**Voorgestelde voorvoegsel** is het voorvoegsel van drie tekens van het Azure-opslagaccount

**Opslagaccount** is de naam met het voorgestelde voorvoegsel

**R/W IOPS (met groeifactor)** is de piekworkload-IOPS op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale R/W IOPS-waarde van de virtuele machine niet altijd de som is van de afzonderlijke R/W IOPS-schijven van de virtuele machine. De piekwaarde voor R/W IOPS van de virtuele machine is namelijk de piek van de som van de individuele R/W IOPS-schijven voor elke minuut van de profileringsperiode.

**Gegevensverloop in Mbps (met groeifactor)** is het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Azure VM-grootte** is de ideale toegewezen grootte van Azure Compute-VM's voor deze on-premises virtuele machine. Deze toewijzing gebeurt op basis van het geheugen van de on-premises virtuele machine, het aantal schijven/kernen/NIC’s en R/W IOPS - het wordt altijd aanbevolen laagste grootte van virtuele machine van Azure te gebruiken die overeenkomt met al deze kenmerken voor de on-premises virtuele machine.

**Aantal schijven** is het totale aantal schijven (VMDK's) op de virtuele machine

**Schijfgrootte (GB)** is de totale ingerichte grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kernen** is het aantal CPU-kernen van de virtuele machine.

**Geheugen (MB)** is het RAM-geheugen van de virtuele machine.

**NIC's** is het aantal NIC's van de virtuele machine.

##<a name="incompatible-vms"></a>Niet-compatibele VM's

![Implementatieplanner](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM-naam** is de naam van de virtuele machine die of het IP-adres dat tijdens het genereren van het rapport is gebruikt in het bestand VMListFile. Deze kolom bevat ook de schijven (VMDK's) die zijn gekoppeld aan de virtuele machines. Virtuele machines op een vCenter met dubbele namen of IP-adressen worden weergegeven met de ESXi-hostnaam om onderscheid te maken tussen de virtuele machines. De vermelde ESXi-host is de host waarop de virtuele machine tijdens de profileringsperiode is geplaatst bij de eerste detectie door het hulpprogramma.

**VM-compatibiliteit** geeft aan waarom de opgegeven virtuele machine niet compatibel is voor gebruik met Azure Site Recovery. De redenen worden vermeld per incompatibele schijf van de virtuele machine. Hierbij kan het gaan om een van de volgende redenen op basis van de gepubliceerde Azure Storage-[limieten](https://aka.ms/azure-storage-scalbility-performance).

* Grootte > 1023 GB – Azure Storage biedt momenteel geen ondersteuning voor schijfgrootten > 1 TB
* De totale VM-grootte (replicatie + TFO) is groter dan de ondersteunde Azure Storage-accountlimiet (35 TB). Dit gebeurt meestal wanneer de virtuele machine één schijf bevat met bepaalde prestatiekenmerken die de maximaal ondersteunde Microsoft Azure/Azure Site Recovery-limieten voor standaardopslag overschrijden, waardoor de virtuele machine in de Premium Storage-zone terechtkomt. De maximaal ondersteunde grootte van een premium Azure Storage-account is echter 35 TB en één beveiligde virtuele machine kan niet worden beveiligd via meerdere opslagaccounts. Houd er ook rekening mee dat wanneer er een TFO (testfailover) wordt uitgevoerd op een beveiligde virtuele machine, dit gebeurt in hetzelfde opslagaccount als de replicatie. Daarom moeten we 2x de grootte van de schijf inrichten, zodat replicatie en failover parallel kunnen worden uitgevoerd.
* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor Azure Storage van 5000 per schijf
* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor Azure Storage van 80.000 per virtuele machine
* Het gemiddelde gegevensverloop overschrijdt de ondersteunde Azure Site Recovery-limiet voor gegevensverloop van 10 MBps voor de gemiddelde IO-grootte voor de schijf
* Het totale gegevensverloop voor alle schijven in de virtuele machine overschrijdt de maximaal ondersteunde limiet voor Azure Site Recovery-gegevensverloop van 54 MBps per virtuele machine
* De gemiddelde effectieve schrijf-IOPS overschrijdt de ondersteunde IOPS-limiet van Azure Site Recovery van 840 voor schijven
* De berekende opslag voor momentopnamen overschrijdt de ondersteunde opslaglimiet van 10 TB voor momentopnamen

**R/W IOPS (met groeifactor)** is de piekworkload-IOPS op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale R/W IOPS-waarde van de virtuele machine niet altijd de som is van de afzonderlijke R/W IOPS-schijven van de virtuele machine. De piekwaarde voor R/W IOPS van de virtuele machine is namelijk de piek van de som van de individuele R/W IOPS-schijven voor elke minuut van de profileringsperiode.

**Gegevensverloop in Mbps (met groeifactor)** is het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Aantal schijven** is het totale aantal schijven (VMDK's) op de virtuele machine

**Schijfgrootte (GB)** is de totale ingerichte grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kernen** is het aantal CPU-kernen van de virtuele machine.

**Geheugen (MB)** is het RAM-geheugen van de virtuele machine.

**NIC's** is het aantal NIC's van de virtuele machine.


##<a name="azure-site-recovery-limits"></a>Azure Site Recovery-limieten

**Beoogde replicatieopslag** | **Gemiddelde I/O-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB    | 2 MB/s | 168 GB per schijf
Premium P10-schijf | 8 kB    | 2 MB/s | 168 GB per schijf
Premium P10-schijf | 16 kB | 4 MB/s |    336 GB per schijf
Premium P10-schijf | 32 kB of hoger | 8 MB/s | 672 GB per schijf
Premium P20-/P30-schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20-/P30-schijf | 16 kB of hoger |10 MB/s    | 842 GB per schijf


Dit zijn gemiddelden uitgaande van een I/O-overlapping van 30%. Azure Site Recovery kan hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag. De bovenstaande getallen gaan uit van een typische achterstand van ongeveer 5 minuten. D.w.z.: eenmaal geüploade gegevens worden verwerkt en binnen 5 minuten wordt er een herstelpunt gemaakt.

De bovenstaande gepubliceerde limieten zijn gebaseerd op onze tests maar dekken niet alle mogelijke toepassings-I/O-combinaties. De werkelijke resultaten variëren op basis van uw toepassings-I/O-combinatie. Voor optimale resultaten, zelfs na het plannen van de implementatie, is het altijd beter om toepassingen uitgebreid te testen met behulp van een testfailover. Zo krijgt u een nauwkeurig inzicht in de prestaties.

## <a name="how-to-update-the-deployment-planner"></a>De implementatieplanner bijwerken?
[Download](site-recovery-deployment-planner.md#download) de nieuwste versie van de Azure Site Recovery-implementatieplanner. Kopieer het ZIP-bestand naar een server waarop u de toepassing wilt uitvoeren. Pak het gecomprimeerde bestand uit.
Als u een eerdere versie van de implementatieplanner hebt en profilering wordt uitgevoerd, hoeft u de profilering niet te stoppen, tenzij de nieuwe versie profileringsherstel heeft. Als de release verbeteringen in de profileringsonderdelen bevat, verdient het aanbeveling om profilering in de oudere versie te stoppen en de profilering opnieuw te starten met de nieuwe versie. Houd er rekening mee dat wanneer u de profilering start met de nieuwe versie, u hetzelfde pad voor de uitvoermap moet gebruiken, zodat het hulpprogramma profielgegevens van bestaande bestanden kan toevoegen en de volledige set profielgegevens kan worden gebruikt voor het genereren van rapporten. Als u verschillende uitvoermappen doorgeeft, worden er nieuwe bestanden gemaakt en kunnen gegevens van oude profielen niet worden gebruikt bij het genereren van rapporten.<br> Elke update is een cumulatieve update met een ZIP-bestand. U hoeft de nieuwe versiebestanden niet te kopiëren naar de map van de vorige versie om ze te gebruiken. U kunt hier de nieuwe map voor gebruiken.


##<a name="version-history"></a>Versiegeschiedenis
### <a name="11"></a>1.1
Bijgewerkt op: 09 maart 2017 <br>

De volgende problemen zijn verholpen<br>

* Kan geen virtuele machines profileren als het vCenter twee of meer virtuele machines met dezelfde naam/hetzelfde IP-adres voor verschillende ESXi-hosts heeft.<br>
* Kopiëren en zoeken is uitgeschakeld voor de blades Compatibele VM's en Incompatibele VM's.


### <a name="10"></a>1.0 
Bijgewerkt op: 23 februari 2017 

De openbare previewversie 1.0 van de Azure Site Recovery-implementatieplanner heeft de volgende bekende problemen die in toekomstige updates worden verholpen.

* Het programma werkt alleen voor VMware-naar-Azure-implementaties, niet voor Hyper-V-naar-Azure-implementaties. Gebruik voor Hyper-V naar Azure de [Capaciteitsplanner voor Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* De GetThroughput-bewerking wordt niet ondersteund in Microsoft Azure-regio's US Government en China.
* Het hulpprogramma kan geen virtuele machines profileren als het vCenter twee of meer virtuele machines met dezelfde naam/hetzelfde IP-adres voor verschillende ESXi-hosts heeft. In deze versie slaat het hulpprogramma profilering over bij VM-namen/IP-adressen die dubbel voorkomen in VMListFile. Een tijdelijke oplossing is om de virtuele machines met ESXi-host te profileren in plaats van met de vCenter-server. U moet voor elke ESXi-host één exemplaar uitvoeren.

