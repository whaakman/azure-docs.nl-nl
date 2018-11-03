---
title: De prestaties van een cloudservice testen | Microsoft Docs
description: Test de prestaties van een cloudservice met behulp van de Visual Studio profiler
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: aa8416bb6883a1aa76ddd370bf7061d7013904eb
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969480"
---
# <a name="testing-the-performance-of-a-cloud-service"></a>De prestaties van een cloudservice testen
## <a name="overview"></a>Overzicht
U kunt de prestaties van een cloudservice testen in de volgende manieren:

* Gebruik Azure Diagnostics voor het verzamelen van informatie over aanvragen en verbindingen en site-statistieken die laten zien hoe de service wordt uitgevoerd vanuit het oogpunt van de klant. Als u wilt beginnen met, Zie [diagnostische gegevens configureren voor Azure Cloud Services en Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* De Visual Studio profiler gebruiken om een diepgaande analyse van de rekenkundige aspecten van hoe de service wordt uitgevoerd. Zoals in dit onderwerp wordt beschreven, kunt u de profiler voor het meten van prestaties als een service wordt uitgevoerd in Azure. Zie voor meer informatie over het gebruik van de profiler voor het meten van prestaties als een service wordt lokaal uitgevoerd in een rekenemulator [testen van de prestaties van een Azure Cloud Service lokaal in de Compute-Emulator met behulp van de Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Een methode voor Prestatietesten kiezen
### <a name="use-azure-diagnostics-to-collect"></a>Azure Diagnostics gebruiken om te verzamelen:
* Statistieken op webpagina's of services, zoals aanvragen en verbindingen.
* Statistieken over de functies, zoals hoe vaak een rol opnieuw wordt opgestart.
* Algemene informatie over het gebruik van geheugen, zoals het percentage van de tijd die de garbage collector nodig heeft of de hoeveelheid geheugen instellen van een actieve rol.

### <a name="use-the-visual-studio-profiler-to"></a>De Visual Studio profiler om te gebruiken:
* Bepalen welke functies de meeste tijd kosten.
* Meten hoeveel tijd die nodig is elk onderdeel van een programma rekenintensief zijn.
* Gedetailleerde rapporten voor twee versies van een service met elkaar vergelijken.
* Analyseer de toewijzing van geheugen in meer detail dan het niveau van afzonderlijke geheugentoewijzingen.
* Gelijktijdigheidsproblemen met in code met meerdere threads analyseren.

Wanneer u de profiler gebruikt, kunt u gegevens kunt verzamelen als een cloudservice lokaal of in Azure wordt uitgevoerd.

### <a name="collect-profiling-data-locally-to"></a>Profilering gegevens lokaal te verzamelen:
* Test de prestaties van een deel van een cloudservice, zoals het uitvoeren van specifieke werkrol, waarvoor een realistische gesimuleerde belasting.
* Test de prestaties van een cloudservice in isolatie, gecontroleerd.
* De prestaties van een cloudservice testen voordat u deze naar Azure implementeren.
* Test de prestaties van een cloudservice privé, zonder de bestaande implementaties verstoord.
* Test de prestaties van de service zonder dat er kosten in rekening gebracht voor het uitvoeren in Azure.

### <a name="collect-profiling-data-in-azure-to"></a>Gegevens in Azure te verzamelen:
* Test de prestaties van een cloudservice met een gesimuleerde of echte belasting.
* Gebruik de methode instrumentatie van het verzamelen van gegevens, zoals in dit onderwerp wordt later beschreven.
* Test de prestaties van de service in dezelfde omgeving als wanneer de service wordt uitgevoerd in de productieomgeving.

U doorgaans simuleren een load test cloudservices onder normale of stress voorwaarden.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilering van een cloudservice in Azure
Wanneer u uw cloudservice van Visual Studio publiceert, kunt u het profiel van de service en de profilering instellingen waarmee u de informatie die u wilt opgeven. Een sessie voor profilering is gestart voor elk exemplaar van een rol. Zie voor meer informatie over het publiceren van uw service van Visual Studio [publiceren naar een Azure Cloud Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Zie voor meer informatie over de prestaties van profilering in Visual Studio, [Beginners-handleiding voor prestaties profilering](https://msdn.microsoft.com/library/azure/ms182372.aspx) en [analyseren van de prestaties van toepassingen door hulpprogramma's voor profilering met behulp van](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> U kunt IntelliTrace of wanneer u uw cloudservice publiceert profilering inschakelen. U kunt beide niet inschakelen.
> 
> 

### <a name="profiler-collection-methods"></a>Profiler verzameling methoden
U kunt verschillende methoden gebruiken voor het profileren, op basis van uw prestatieproblemen kunnen voordoen:

* **CPU-sampling** -met deze methode verzamelt toepassingsstatistieken die nuttig voor de eerste analyse van CPU-gebruik problemen zijn. CPU-sampling is de voorgestelde methode voor het starten van de meeste prestaties onderzoeken. Er is een weinig impact op de toepassing die u bij het verzamelen van gegevens van CPU-sampling zijn profilering.
* **Instrumentation** -met deze methode verzamelt gedetailleerde timinggegevens die nuttig is voor gerichte analyse en voor het analyseren van problemen met de i/o-prestaties. De methode instrumentatie registreert elke vermelding, afsluiten en aanroep van de functie van de functies in een module tijdens een profilering wordt uitgevoerd. Deze methode is handig voor het verzamelen van de van gedetailleerde timinginformatie over een sectie van uw code en voor het begrijpen van de impact van de invoer en uitvoer-bewerkingen op de prestaties van toepassingen. Deze methode is uitgeschakeld voor een computer met een 32-bits besturingssysteem. Deze optie is alleen beschikbaar wanneer u de cloudservice in Azure, niet lokaal in de rekenemulator uitvoeren.
* **.NET-geheugentoewijzing** -met deze methode .NET Framework geheugen toegewezen gegevens worden verzameld met behulp van de profilering methode steekproeven. De verzamelde gegevens bevatten het aantal en de grootte van toegewezen objecten.
* **Gelijktijdigheid** -met deze methode resourcegegevens conflicten en proces en thread uitvoeringsgegevens die nuttig zijn bij het analyseren van toepassingen met meerdere threads en meerdere processen is verzameld. De methode gelijktijdigheid verzamelt gegevens voor elke gebeurtenis dat blokken uitvoering van code, zoals wanneer een thread wacht voor toegang tot de toepassingsresource van een om te worden vrijgemaakt vergrendeld. Deze methode is handig voor het analyseren van toepassingen met meerdere threads.
* U kunt ook inschakelen **laag tussenkomst van de profilering**, waarmee u meer informatie over de uitvoertijd van synchrone ADO.NET-aanroepen in de functies van multi-tiered toepassingen die met een of meer databases communiceren. U kunt de laag tussenkomst van de gegevens verzamelen met een van de profilering methoden. Zie voor meer informatie over laag tussenkomst van de profilering [laag Interacties weergave](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profilering instellingen configureren
De volgende afbeelding laat zien hoe het configureren van de profilering instellingen in het dialoogvenster Publish Azure Application.

![Instellingen voor profilering configureren](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> Om in te schakelen de **Schakel profilering** selectievakje, moet u de profiler geïnstalleerd op de lokale computer die u gebruikt voor het publiceren van uw service in de cloud hebben. De profiler is standaard geïnstalleerd tijdens de installatie van Visual Studio.
> 
> 

### <a name="to-configure-profiling-settings"></a>Instellingen voor profilering configureren
1. Open het snelmenu voor uw Azure-project in Solution Explorer en kies vervolgens **publiceren**. Zie voor gedetailleerde stappen over het publiceren van een cloudservice [publiceren van een cloudservice met behulp van de Azure-hulpprogramma's](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. In de **Publish Azure Application** in het dialoogvenster hebt gekozen de **geavanceerde instellingen** tabblad.
3. Als u profilering, schakelt de **Schakel profilering** selectievakje.
4. Als u wilt uw profilering instellingen configureert, kiest u de **instellingen** hyperlink. Het dialoogvenster Instellingen voor profilering wordt weergegeven.
5. Uit de **welke methode van de profilering wilt u gebruiken** keuzerondjes, kies het type van de profilering dat u nodig hebt.
6. Voor het verzamelen van de profileringsgegevens interakce, selecteer de **inschakelen laag tussenkomst van de profilering** selectievakje.
7. Als u wilt de instellingen opslaan, kiest u de **OK** knop.
   
    Wanneer u deze toepassing publiceert, worden deze instellingen worden gebruikt om de profilering sessie voor elke rol te maken.

## <a name="viewing-profiling-reports"></a>Profileringsrapporten kunt weergeven
Een sessie voor profilering is gemaakt voor elk exemplaar van een rol in uw cloudservice. U kunt uw profilering om rapporten te bekijken van elke sessie vanuit Visual Studio, het Server Explorer-venster weergeven en kies vervolgens het Azure Compute-knooppunt een exemplaar van een rol selecteren. U kunt vervolgens de sestavu profilace weergeven, zoals wordt weergegeven in de volgende afbeelding.

![Zobrazit Sestavu Profilace van Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profilering rapporten weergeven
1. Als u wilt weergeven van het Server Explorer-venster in Visual Studio, in het menu weergave kiezen, Server Explorer.
2. Kies het Azure Compute-knooppunt, en kies vervolgens het knooppunt Azure-implementatie voor de cloudservice die u hebt geselecteerd in profiel wanneer u vanuit Visual Studio hebt gepubliceerd.
3. Profilering om rapporten te bekijken voor een exemplaar, kiest u de rol in de service, open het snelmenu voor een specifiek exemplaar in en kies vervolgens **profilering rapport weergeven**.
   
    Het rapport, een bestand in het .vsp formaat, wordt nu gedownload van Azure en de status van de download wordt weergegeven in de Azure Activity Log. Wanneer het downloaden is voltooid, de profilering rapport wordt weergegeven op een tabblad in de editor voor Visual Studio met de naam <Role name> *<Instance Number>* <identifier>.vsp. Gegevens van de samenvatting voor het rapport wordt weergegeven.
4. Om verschillende weergaven van het rapport in de lijst met huidige weergave, kies het type weergave die u wilt. Zie voor meer informatie, [extra rapportweergaven profilering](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Volgende stappen
[Fouten in Cloudservices opsporen](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publiceren naar een Azure-Cloud-Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

