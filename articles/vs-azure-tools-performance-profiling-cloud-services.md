---
title: Testen van de prestaties van een service in de cloud | Microsoft Docs
description: Test de prestaties van een cloudservice met behulp van de profiler Visual Studio
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: ghogen
editor: 
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: 483b8b1c7c75c407cb55a1b3b027ae043c506ebb
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="testing-the-performance-of-a-cloud-service"></a>De prestaties van een cloudservice testen
## <a name="overview"></a>Overzicht
U kunt de prestaties van een cloudservice testen in de volgende manieren:

* Gebruik Azure Diagnostics verzamelen van informatie over aanvragen en verbindingen en site-statistieken die laten zien hoe de service uitvoert vanuit het oogpunt van de klant van bekijken. Als u wilt beginnen met, Zie [diagnostische gegevens configureren voor Azure Cloud Services en virtuele Machines](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* De profiler Visual Studio gebruiken om een gedetailleerde analyse van de rekenkundige aspecten van hoe de service wordt uitgevoerd. Zoals in dit onderwerp wordt beschreven, kunt u de profiler om prestaties te meten als een service wordt uitgevoerd in Azure. Zie voor meer informatie over het gebruik van de profiler om prestaties te meten als een service wordt lokaal uitgevoerd in een rekenemulator [testen van de prestaties van een Azure Cloud Service lokaal in de Compute-Emulator met behulp van de Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Een methode voor Prestatietesten kiezen
### <a name="use-azure-diagnostics-to-collect"></a>Gebruik diagnostische Azure-gegevens te verzamelen:
* Statistieken op webpagina's of services, zoals aanvragen en verbindingen.
* Statistieken over de functies, zoals hoe vaak een rol opnieuw wordt opgestart.
* Algemene informatie over geheugengebruik, zoals het percentage tijd die de garbage collector nodig heeft of de hoeveelheid geheugen instellen van een actieve rol.

### <a name="use-the-visual-studio-profiler-to"></a>Gebruik de profiler wordt Visual Studio:
* Bepalen welke functies de meeste tijd in beslag nemen.
* Bepaal hoe lang duurt van elk deel uitmaakt van een rekenkracht programma.
* Vergelijk de van gedetailleerde prestatierapporten voor twee versies van een service.
* Analyseer geheugentoewijzing in meer detail dan het niveau van afzonderlijke geheugentoewijzingen.
* Gelijktijdigheidsproblemen in meerdere threads code analyseren.

Wanneer u de profiler gebruikt, kunt u gegevens verzamelen als een cloudservice wordt uitgevoerd, lokaal of in Azure.

### <a name="collect-profiling-data-locally-to"></a>Profileringsgegevens lokaal te verzamelen:
* Test de prestaties van een deel van een cloudservice, zoals de uitvoering van specifieke werkrol die een realistische gesimuleerde belasting niet nodig.
* Test de prestaties van een cloudservice in isolatie, gecontroleerd.
* Test de prestaties van een cloudservice vóór u ze naar Azure implementeren.
* Test de prestaties van een cloudservice privé, zonder de bestaande implementaties verstoren.
* Test de prestaties van de service zonder kosten voor het uitvoeren in Azure.

### <a name="collect-profiling-data-in-azure-to"></a>Profileringsgegevens in Azure om te verzamelen:
* Test de prestaties van een cloudservice een gesimuleerde of echte wordt belast.
* Gebruik de methode instrumentation profilering gegevens te verzamelen zoals in dit onderwerp wordt beschreven later.
* Test de prestaties van de service in dezelfde omgeving als wanneer de service wordt uitgevoerd in de productieomgeving.

U doorgaans simuleren een werklast voor cloud-services testen onder normaal of belasting.

## <a name="profiling-a-cloud-service-in-azure"></a>Profileren van een cloudservice in Azure
Wanneer u uw cloudservice vanuit Visual Studio publiceert, kunt u de service-profiel en de profileringsoptimalisaties instellingen waarmee u de informatie die u wilt opgeven. Een profilering sessie is voor elk exemplaar van een rol gestart. Zie voor meer informatie over het publiceren van uw service vanuit Visual Studio [publiceren naar een Azure Cloud Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Zie voor meer informatie over prestaties profilering in Visual Studio, [beginnende gebruikers van handleiding voor prestaties profilering](https://msdn.microsoft.com/library/azure/ms182372.aspx) en [toepassingsprestaties analyseren by Using Tools voor profilering](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> U kunt IntelliTrace of wanneer u uw cloudservice publiceert profilering inschakelen. U kunt beide niet inschakelen.
> 
> 

### <a name="profiler-collection-methods"></a>Profiler verzameling methoden
U kunt verschillende methoden gebruiken voor profilering, op basis van uw prestatieproblemen:

* **CPU-steekproeven** -statistieken die handig voor de eerste analyse van CPU-gebruik problemen zijn door deze methode worden verzameld. CPU steekproeven is de voorgestelde methode voor het starten van de meeste prestaties onderzoeken. Er is een lage impact op de toepassing die profileren van wanneer u CPU steekproeven gegevens verzamelt.
* **Instrumentation** -deze methode verzamelt gedetailleerde timinggegevens die nuttig is voor gerichte analyse en voor het analyseren van i/o-prestaties. De methode instrumentation registreert elke vermelding, afsluiten en functie-aanroep van de functies in een module tijdens een profilering uitvoeren. Deze methode is handig voor het verzamelen van de timing van gedetailleerde informatie over een gedeelte van uw code en om de invloed van de invoer en uitvoer bewerkingen op de prestaties van toepassingen. Deze methode is uitgeschakeld voor een computer met een 32-bits besturingssysteem. Deze optie is beschikbaar, alleen wanneer u de cloudservice in Azure, niet lokaal in de rekenemulator uitvoeren.
* **.NET-geheugentoewijzing** -deze methode toewijzingsgegevens van .NET Framework geheugen met behulp van de methode profilering steekproeven verzameld. De verzamelde gegevens bevatten het aantal en de grootte van toegewezen objecten.
* **Gelijktijdigheid** -deze methode brongegevens conflicten en proces en thread uitvoeringsgegevens die nuttig is bij het analyseren van toepassingen met meerdere threads en meerdere processen worden verzameld. De methode gelijktijdigheid verzamelt gegevens voor elke gebeurtenis dat blokken uitvoering van code, zoals wanneer een thread wacht tot toegang tot de bron van een toepassing om te worden vrijgemaakt vergrendeld. Deze methode is handig voor het analyseren van toepassingen met meerdere threads.
* U kunt ook inschakelen **laag interactie profilering**, waarmee u meer informatie over de uitvoeringstijden van synchrone ADO.NET-aanroepen in de functies van multi-tiered toepassingen die met een of meer databases communiceren. Gegevens van de interactie van de laag kunt u met een van de methoden profilering verzamelen. Zie voor meer informatie over laag interactie profilering [laag Interacties weergave](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profileringsoptimalisaties instellingen configureren
De volgende afbeelding laat zien hoe uw profilering instellingen in het dialoogvenster Publish Azure Application configureren.

![Profileren van de instellingen configureren](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> Om in te schakelen de **inschakelen profilering** selectievakje, moet u de profiler die is geïnstalleerd op de lokale computer die u gebruikt voor het publiceren van uw cloudservice hebben. De profiler is standaard geïnstalleerd tijdens de installatie van Visual Studio.
> 
> 

### <a name="to-configure-profiling-settings"></a>Profileringsoptimalisaties instellingen configureren
1. Open het snelmenu voor uw Azure-project in Solution Explorer en kies vervolgens **publiceren**. Zie voor gedetailleerde stappen over het publiceren van een cloudservice [publiceren van een cloudservice met de Azure-hulpprogramma's](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. In de **Publish Azure Application** in het dialoogvenster hebt gekozen het **geavanceerde instellingen** tabblad.
3. Als u profilering, schakelt de **inschakelen profilering** selectievakje.
4. Kies uw profileringsoptimalisaties om instellingen te configureren, de **instellingen** hyperlink. Het dialoogvenster Instellingen voor het samenstellen van profiel wordt weergegeven.
5. Van de **welke methode voor het samenstellen van profiel dat u wilt gebruiken** keuzerondjes, kies het type van het profiel dat u nodig hebt.
6. Voor het verzamelen van de laag interactie profileringsgegevens, selecteer de **inschakelen laag interactie profilering** selectievakje.
7. Sla de instellingen, kies de **OK** knop.
   
    Wanneer u deze toepassing publiceert, worden deze instellingen gebruikt om de sessie profileringsoptimalisaties voor elke rol te maken.

## <a name="viewing-profiling-reports"></a>Profileren van rapporten weergeven
Een profilering sessie wordt gemaakt voor elk exemplaar van een rol in uw cloudservice. U kunt uw profileringsoptimalisaties om rapporten te bekijken van elke sessie vanuit Visual Studio, het Server Explorer-venster weergeven en kies vervolgens het knooppunt Azure Compute een exemplaar van een rol selecteren. Vervolgens kunt u de profilering rapport weergeven, zoals wordt weergegeven in de volgende afbeelding.

![Profileren van rapport van Azure weergeven](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profileringsoptimalisaties rapporten weer te geven
1. Als u wilt weergeven van het Server Explorer-venster in Visual Studio, in het menu weergave kiezen, Server Explorer.
2. Kies het Azure Compute-knooppunt en kies vervolgens het knooppunt van de Azure-implementatie voor de cloudservice die u hebt geselecteerd voor het profiel wanneer u gepubliceerd vanuit Visual Studio.
3. Profileringsoptimalisaties om rapporten te bekijken voor een exemplaar, selecteer de rol in de service, open het snelmenu voor een specifiek exemplaar en kies vervolgens **profilering rapport weergeven**.
   
    Het rapport, een bestand .vsp wordt nu gedownload van Azure en de status van de download wordt weergegeven in de Azure Activity Log. Wanneer het downloaden is voltooid, het profilering rapport wordt weergegeven op een tabblad in de editor voor Visual Studio met de naam <Role name>  *<Instance Number>*  <identifier>.vsp. Gegevens van de samenvatting voor het rapport wordt weergegeven.
4. Om verschillende weergaven van het rapport in de lijst met huidige weergave, selecteer het type van de gewenste weergave. Zie voor meer informatie [extra rapportweergaven profilering](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Volgende stappen
[Foutopsporing van Cloud-Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publiceren naar een Azure Cloudservice vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

