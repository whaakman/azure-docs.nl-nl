---
title: De diagnostische gegevens voor prestaties voor virtuele machines van Azure | Microsoft Docs
description: Introduceert de prestaties van Azure Diagnostics voor Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: przlplx
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 9/20/2018
ms.author: anandh
ms.openlocfilehash: f79be44d33a327f3ead8d950885cb5d47ce3a015
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163799"
---
# <a name="performance-diagnostics-for-azure-virtual-machines"></a>De diagnostische gegevens voor prestaties voor virtuele machines van Azure

Het hulpprogramma voor prestatieverbetering diagnostics helpt bij het oplossen van prestatieproblemen die invloed kunnen zijn op een Windows virtuele machine (VM). Ondersteunde scenario's voor het oplossen van problemen bevatten snelle controle van de bekende problemen en aanbevolen procedures en complexe problemen die betrekking hebben op trage prestaties van de virtuele machine of intensief gebruik van CPU, schijfruimte of geheugen. 

U kunt prestatiediagnoses uitvoeren rechtstreeks vanuit de Azure portal waarin u kunt ook bekijken inzichten en een rapport op verschillende logboeken, geavanceerde configuratie en diagnostische gegevens. U wordt aangeraden dat u de diagnostische gegevens van prestaties en de inzichten en diagnostische gegevens bekijken voordat u contact op met Microsoft Support.

> [!NOTE]
> Prestaties van diagnostische gegevens wordt momenteel ondersteund op Windows-VM's met .NET SDK-versie 4.5 of hoger geïnstalleerd. Zie voor de stappen uit te voeren van diagnostische gegevens voor prestaties op klassieke virtuele machines [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
Windows 10, Windows 8, Windows 8 Enterprise, Windows 8 Pro, Windows 8.1, WindowsServer 2016, Windows Server 2012, Windows Server 2012 Datacenter, Windows Server 2012 R2, Windows Server 2012 R2 Datacenter, Windows Server 2012 R2 Standard, WindowsServer 2012 Standard, Windows Server 2008 R2, Windows Server 2008 R2 Datacenter, Windows Server 2008 R2 Enterprise, Windows Server 2008 R2 Foundation, Windows Server 2008 R2 SP1, Windows Server 2008 R2 Standard.

## <a name="install-and-run-performance-diagnostics-on-your-vm"></a>Installeren en uitvoeren van diagnostische gegevens voor prestaties op de virtuele machine
Prestaties van diagnostische gegevens wordt geïnstalleerd voor een VM-extensie die wordt uitgevoerd een hulpprogramma voor diagnostische gegevens met de naam [PerfInsights](https://aka.ms/perfinsights). Als u wilt installeren en uitvoeren van diagnostische gegevens over prestaties, de volgende stappen uit:
1.  Selecteer in de linkerkolom van opdrachten, **virtuele machines**.
1.  Selecteer de virtuele machine die u uitvoeren wilt op in de lijst met namen van de virtuele machine.
1.  Selecteer in de rechterkolom van opdrachten, **prestatiediagnoses**.

    ![Schermopname van het Azure-portal met installatie performance diagnostics knop gemarkeerd](media/performance-diagnostics/performance-diagnostics-install.png)

    > [!NOTE]
    > In deze schermafbeelding, is de blade van de namen van de virtuele machine verborgen.
1. Selecteer een opslagaccount (optioneel)

    Als u een enkel opslagaccount gebruiken voor het opslaan van de prestatieresultaten van de diagnostische gegevens voor meerdere virtuele machines wilt, kunt u een opslagaccount selecteren door te klikken op de **instellingen** knop op de werkbalk. Klik op de **OK** knop nadat u de storage-account selecteren.

    Als u een storage-account niet opgeeft, wordt er standaard een nieuw opslagaccount gemaakt.

    ![Blade diagnostische gegevens over schermafbeelding van de prestaties, met de knop Instellingen gemarkeerd](media/performance-diagnostics/settings-button.png)

    ![Schermopname van het storage-account selecteren uit de blade van instellingen voor diagnostische gegevens over prestaties](media/performance-diagnostics/select-storage-account.png)

1. Selecteer de **prestatiediagnoses installeren** knop.
1. Selecteer de **diagnose uitvoeren** selectievakje in als u een diagnose uitvoeren wilt nadat de installatie is voltooid. Als u deze selectie maakt, moet u kunnen de prestaties analysis scenario en de verwante opties te kiezen.

    ![Schermafbeelding van de prestatiecontrole installeren knop](media/performance-diagnostics/install-diagnostics-button.png)

## <a name="select-an-analysis-scenario-to-run"></a>Selecteer een scenario analyse om uit te voeren

De volgende scenario's voor prestatieanalyse zijn beschikbaar via de Azure-portal. Selecteer een analyse, afhankelijk van het prestatieprobleem dat u ondervindt. Selecteer de opties voor de duur en tracering die nodig zijn voor de analyse.

* **Analyse van snelle prestaties**  
    Controleert voor bekende problemen, analyseert de aanbevolen procedures en diagnostische gegevens worden verzameld. Deze analyse duurt enkele minuten om uit te voeren. [Meer informatie](https://aka.ms/perfinsights/quick)

* **Analyse van prestaties**  
    Bevat alle controles in de analyse van de snelle prestaties en hoge resourceverbruik bewaakt. Gebruik deze versie met algemene prestaties oplossen, zoals hoge CPU-, geheugen- en schijfgebruik. Deze analyse kan 30 seconden tot 15 minuten, afhankelijk van de geselecteerde periode. [Meer informatie](https://aka.ms/perfinsights/vmslow) 
    
* **Geavanceerde analyse**  
    Bevat alle controles in de analyse van prestaties en een of meer van de traceringen worden verzameld zoals vermeld in de volgende secties. In dit scenario om complexe problemen waarvoor extra traceringen te gebruiken. Met dit scenario voor langere tijd, wordt de totale grootte van de uitvoer van de diagnostische gegevens, afhankelijk van de grootte van de virtuele machine en de traceeropties die zijn geselecteerd verhogen. Deze analyse duurt 30 seconden tot 15 minuten uit te voeren, afhankelijk van de geselecteerde periode. [Meer informatie](https://aka.ms/perfinsights/advanced) 
    
* **Azure bestanden analysis**  
    Bevat alle controles in de analyse van prestaties, en een netwerktracering en SMB-prestatiemeteritems wordt vastgelegd. In dit scenario gebruiken om op te lossen van de prestaties van Azure files. Deze analyse duurt 30 seconden tot 15 minuten uit te voeren, afhankelijk van de geselecteerde periode. [Meer informatie](https://aka.ms/perfinsights/azurefiles)


![Schermafbeelding van het uitvoeren van diagnostische gegevens deelvenster in de blade diagnostische gegevens over prestaties](media/performance-diagnostics/run-diagnostics-pane.png)

### <a name="provide-symptoms-optional"></a>Geef de symptomen (optioneel)
Vooraf geselecteerde symptomen in de lijst selecteren of toevoegen van nieuwe problemen. Dit helpt ons de analyse in de toekomst te verbeteren. 

### <a name="provide-support-request-number-if-available-optional"></a>Bieden ondersteuning voor aanvraag getal, indien beschikbaar (optioneel)
Als u met een ondersteuningsmedewerker van Microsoft op een bestaande ondersteuningsticket werkt, geeft u het ticketnummer ondersteuning. 

### <a name="review-the-privacy-policy-and-legal-terms-and-select-the-check-box-to-acknowledge-required"></a>Controleer het privacybeleid en de juridische voorwaarden en schakel het selectievakje in om te bevestigen (vereist)
Als u wilt uitvoeren van de diagnostische gegevens, moet u akkoord gaat met de juridische voorwaarden en privacybeleid accepteren.

### <a name="select-ok-to-run-the-diagnostics"></a>Klik op OK om uit te voeren van de diagnostische gegevens 
Een melding wordt weergegeven als prestatiediagnoses begint te installeren. Nadat de installatie is voltooid, ziet u een melding die aangeeft dat de installatie voltooid is. De geselecteerde analyse wordt vervolgens uitgevoerd voor de opgegeven duur. Dit is een goed moment om het prestatieprobleem reproduceren, zodat de diagnostische gegevens kunnen worden vastgelegd op het juiste moment. 

Nadat de analyse voltooid is, worden de volgende items zijn geüpload naar Azure-tabellen en een binaire grote object (BLOB)-container in het opgegeven opslagaccount:

*   Alle inzichten en verwante informatie over de uitvoering
*   Een uitvoer gecomprimeerd bestand (.zip) (met de naam **PerformanceDiagnostics_yyyy-MM-dd_hh-mm-ss-fff.zip**) die logboekbestanden bevat
*   Een HTML-rapport

Na het uploaden, wordt een nieuw rapport voor diagnostische gegevens vermeld in de Azure-portal.

![Schermafbeelding van een lijst van rapport van de diagnostische gegevens in de blade diagnostische gegevens over prestaties](media/performance-diagnostics/diagnostics-report-list.png)

## <a name="how-to-change-performance-diagnostics-settings"></a>Het wijzigen van prestatie-instellingen voor diagnostische gegevens
Gebruik de **instellingen** werkbalkknop te wijzigen van het opslagaccount waar de diagnostische gegevens over inzichten en de uitvoer kunnen worden opgeslagen. U kunt hetzelfde opslagaccount gebruiken voor meerdere virtuele machines die gebruikmaken van diagnostische gegevens voor prestaties. Wanneer u het opslagaccount wijzigt, worden de oude rapporten en inzichten worden niet verwijderd. Echter, wordt ze niet meer worden weergegeven in de lijst met diagnostische gegevens over rapporten. 

## <a name="review-insights-and-performance-diagnostics-report"></a>Inzichten en diagnostische gegevens Prestatierapport bekijken
Elke diagnose uitvoeren bevat een lijst met inzichten en aanbevelingen, betrokken resources, logboekbestanden en andere geavanceerde diagnostische gegevens die worden verzameld, plus een rapport voor offline te bekijken. Zie voor een volledige lijst van alle verzamelde diagnostische gegevens, [wat voor soort informatie wordt verzameld door PerfInsights?](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/how-to-use-perfinsights#what-kind-of-information-is-collected-by-perfinsights) 

### <a name="select-a-performance-diagnostics-report"></a>Selecteer een rapport van de diagnostische gegevens over prestaties
De lijst met diagnostische gegevens van rapporten kunt u zoeken naar alle diagnostische rapporten die zijn uitgevoerd. De lijst bevat informatie over de analyse die is gebruikt, inzichten die zijn gevonden en hun niveaus van impact. Selecteer een rij om meer details weer te geven.

![Schermafbeelding van het selecteren van een rapport diagnostische gegevens van de blade diagnostische gegevens over prestaties](media/performance-diagnostics/select-report.png)

### <a name="review-a-performance-diagnostics-report"></a>Bekijk een rapport van de diagnostische gegevens over prestaties
Elk rapport van de diagnostische gegevens over prestaties kan bevatten verschillende inzichten en duiden op een niveau van impact van hoog, Gemiddeld of laag. Elke inzicht bevat ook aanbevelingen voor het verkleinen van de bezorgdheid. Inzichten worden voor het filteren van eenvoudig gegroepeerd. 

Niveaus van impact vertegenwoordigen de kans op prestatieproblemen, op basis van factoren zoals onjuiste configuratie, bekende problemen, of problemen die worden gerapporteerd door andere gebruikers. Er kan niet nog worden sprake van een of meer van de vermelde problemen. Bijvoorbeeld, u mogelijk SQL-logboekbestanden en databasebestanden op dezelfde gegevensschijf als. Dit probleem heeft een grote kans voor knelpunten en andere prestatieproblemen als het Databasegebruik hoog, is dat niet u een probleem ziet als het gebruik laag is.

![Schermafbeelding van de prestatiecontrole rapporteren overzichtsblade](media/performance-diagnostics/performance-diagnostics-report-overview.png)

### <a name="reviewing-performance-diagnostics-insights-and-recommendations"></a>Prestatie-inzichten voor diagnostische gegevens en aanbevelingen te controleren
U kunt een inzicht om meer informatie over de betrokken resources, voorgestelde oplossingen en koppelingen met naslaginformatie weer te geven. 

![Schermafbeelding van een insight-details van de prestatie-diagnostische gegevens](media/performance-diagnostics/insight-detail.png)

### <a name="download-and-review-the-full-performance-diagnostics-report"></a>Downloaden en bekijken van het rapport van de diagnostische gegevens over de volledige prestaties
U kunt de **rapport downloaden** knop voor het downloaden van een HTML-rapport met gegevens over de aanvullende uitgebreide diagnostische gegevens, zoals opslag en configuratie van het netwerk, prestatiemeteritems, traceringen, overzicht van de processen, en zich aanmeldt. De inhoud, is afhankelijk van de geselecteerde analyse. Voor geavanceerde probleemoplossing, bevat het rapport mogelijk aanvullende informatie en interactieve grafieken die gerelateerd zijn aan hoge CPU-gebruik, -intensief gebruik en -processen die excessief veel geheugen gebruiken. Zie voor meer informatie over het rapport van de diagnostische gegevens over prestaties, [diagnostisch rapport bekijken](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/how-to-use-perfinsights#review-the-diagnostics-report).

## <a name="manage-performance-diagnostics-reports"></a>Diagnostische rapporten beheren
U kunt een of meer prestaties diagnostische rapporten verwijderen met behulp van de **rapport verwijderen** knop.

## <a name="how-to-uninstall-performance-diagnostics"></a>Het verwijderen van diagnostische gegevens over prestaties
U kunt prestaties diagnostische gegevens verwijderen van een virtuele machine. Deze actie verwijdert u de VM-extensie, maar heeft geen invloed op een diagnostische gegevens die in de storage-account is. 

![Schermafbeelding van de werkbalk prestaties diagnostische gegevens van blade met de knop verwijderen gemarkeerd](media/performance-diagnostics/uninstal-button.png)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="where-is-the-diagnostics-data-from-my-vm-stored"></a>Waar is de diagnostische gegevens van mijn virtuele machine opgeslagen? 
Alle inzichten van diagnostische gegevens en rapporten worden opgeslagen in uw eigen opslagaccount. Inzicht zijn in Azure-tabellen opgeslagen. Het gecomprimeerde bestand rapporten is opgeslagen in een binaire grote object (BLOB)-container met de naam azdiagextnresults.

U kunt de gegevens over het opslagaccount weergeven met behulp van de knop instellingen op de werkbalk. 

### <a name="how-do-i-share-this-data-with-microsoft-customer-support"></a>Hoe moet ik deze gegevens delen met de klantondersteuning van Microsoft? 
Er zijn meerdere manieren om het rapport diagnostische gegevens delen met Microsoft.

**Optie 1:** automatisch de meest recente rapport delen  
Wanneer u een ondersteuningsticket met Microsoft opent, is het belangrijk dat u het prestatierapport diagnostische gegevens delen. Als u ervoor hebt gekozen voor het delen van deze gegevens met Microsoft tijdens het uitvoeren van de diagnostische gegevens (door het selecteren van de '**ik ga akkoord met de diagnostische gegevens delen met Microsoft**"selectievakje), Microsoft mogelijk toegang tot het rapport van uw opslag een account met behulp van een SAS-koppeling naar het zip-bestand voor uitvoer voor 30 dagen na het uitvoeren. Alleen het laatste rapport is beschikbaar voor de ondersteuningstechnicus. 

**Optie 2:** een Shared Access Signature genereren voor de diagnostische gegevens gecomprimeerd bestand rapporteren  
U kunt een koppeling naar het gecomprimeerde bestand rapporten delen met behulp van handtekeningen voor gedeelde toegang. Voer de volgende stappen uit om dit te doen: 
1.  Blader naar het opslagaccount waarin de diagnostische gegevens worden opgeslagen in de Azure-portal.
1.  Selecteer **Blobs** onder de **Blob-service** sectie. 
1.  Selecteer de **azdiagextnresults** container.
1.  Selecteer het prestaties diagnostische uitvoer gecomprimeerde bestand dat u wilt delen.
1.  Op de **SAS genereren** tabblad, selecteert u de criteria voor het delen van. 
1.  Klik op **blob SAS-token en URL genereren**.
1.  Kopieer de **Blob SAS-URL**, en deze delen met de ondersteuningstechnicus. 

**Optie 3:** downloaden van het rapport van de storage-account

U kunt ook de prestaties van diagnostische gegevens rapport gecomprimeerd bestand vinden met behulp van de stappen 1 tot 4 in optie 2. Selecteer om te downloaden van het bestand en vervolgens via e-mail of vraag de ondersteuningstechnicus voor instructies om het bestand te uploaden.  

### <a name="how-do-i-capture-the-diagnostics-data-at-the-correct-time"></a>Hoe ik de diagnostische gegevens vastleggen op het juiste moment?
Elke prestatiediagnoses uitvoeren bestaat uit twee fasen: 
1.  Installeren of bijwerken van de prestaties van diagnostische gegevens over VM-extensie.
1.  De diagnostische gegevens voor de opgegeven duur uitvoeren.

Er is momenteel geen eenvoudige manier om te weten precies zien wanneer de installatie van de VM-extensie voltooid is. Het duurt meestal ongeveer 45 seconden tot 1 minuut voor het installeren van de VM-extensie. Nadat de VM-extensie is geïnstalleerd, kunt u de stappen om te reproduceren als u wilt dat de prestaties van diagnostische gegevens vastleggen van de juiste set gegevens voor het oplossen van problemen kunt uitvoeren. 

## <a name="next-steps"></a>Volgende stappen
Nadat u de prestatie-inzichten voor diagnostische gegevens en het rapport, bekijken als u nog steeds niet kan de oorzaak van het probleem en meer hulp nodig hebt, kunt u een ondersteuningsticket openen met de klantondersteuning van Microsoft. 

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/), en selecteer **ondersteuning krijgen**. Voor meer informatie over het gebruik van Azure-ondersteuning, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
