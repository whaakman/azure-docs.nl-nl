---
title: Uw toepassing werklast testen met behulp van Visual Studio Team Services | Microsoft Docs
description: Ontdek hoe testen uw Azure Service Fabric-toepassingen met behulp van Visual Studio Team Services.
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: e8e270ce865d4da3ee219958b308db2c1c89b11b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Uw toepassing werklast testen met behulp van Visual Studio Team Services
In dit artikel laat zien hoe Microsoft Visual Studio load test functies gebruiken om te testen een toepassing. Maakt gebruik van een Azure Service Fabric stateful service back-end en een webfront-end staatloze service. De voorbeeldtoepassing gebruikt hier is een locatie in een vliegtuig simulator. U opgeven dat een vliegtuig-ID, de vertrektijd en de bestemming. De back-end van de toepassing de verzoeken verwerkt en de front-endserver wordt weergegeven op een kaart het vliegtuig die overeenkomt met de criteria.

Het volgende diagram illustreert de Service Fabric-toepassing die u gaat testen.

![Diagram van de toepassing voorbeeld vliegtuig locatie][0]

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag, moet u het volgende doen:

* Maak een Visual Studio Team Services-account. U kunt een gratis verkrijgen op [Visual Studio Team Services](https://www.visualstudio.com).
* Ontvangen en Visual Studio 2013 of Visual Studio 2015 te installeren. Dit artikel wordt Visual Studio 2015 Enterprise edition gebruikt, maar Visual Studio 2013 en andere edities moeten op dezelfde manier werken.
* Uw toepassing implementeren naar een testomgeving. Zie [toepassingen naar een externe cluster met Visual Studio implementeren](service-fabric-publish-app-remote-cluster.md) voor informatie over deze.
* Inzicht in uw toepassing gebruikspatroon. Deze informatie wordt gebruikt om te simuleren van de load-patroon.
* Het doel voor het testen van uw belasting te begrijpen. Dit helpt u bij het interpreteren en de load-testresultaten analyseert.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Maken en de prestaties van de Web- en belastingstest-project uitvoeren
### <a name="create-a-web-performance-and-load-test-project"></a>Een Web-prestatie- en belastingstest-project maken
1. Open Visual Studio 2015. Kies **bestand** > **nieuw** > **Project** in het menu openen de **nieuw Project** in het dialoogvenster.
2. Vouw de **Visual C#** knooppunt en kies **Test** > **Webprestatie- en belastingstest project**. Geef een naam op voor het project en kies vervolgens de **OK** knop.

    ![Schermopname van het dialoogvenster Nieuw Project][1]

    Hier ziet u een nieuw Webprestatie- en belastingstest project in Solution Explorer.

    ![Schermopname van Solution Explorer toont het nieuwe project][2]

### <a name="record-a-web-performance-test"></a>Neem een WebTest prestaties
1. Open het .webtest-project.
2. Kies de **toevoegen Recording** pictogram opnamesessie voor in uw browser te starten.

    ![Schermopname van het pictogram opname toevoegen in een browser][3]

    ![Schermopname van de knop opnemen in een browser][4]
3. Blader naar de Service Fabric-toepassing. De webaanvragen moet worden weergegeven in het deelvenster opnemen.

    ![Schermopname van webaanvragen in het deelvenster opnemen][5]
4. Uitvoeren van een reeks acties die u verwacht dat de gebruikers om uit te voeren. Deze acties worden gebruikt als een patroon voor het genereren van de belasting.
5. Als u bent klaar, kiest u de **stoppen** knop opname gestopt.

    ![Schermopname van de knop stoppen][6]

    Het .webtest-project in Visual Studio moet een reeks aanvragen hebt vastgelegd. Dynamische parameters worden automatisch vervangen. Op dit moment kunt u extra, herhaalde afhankelijkheid verzoeken die geen deel uitmaken van uw testscenario verwijderen.
6. Sla het project en kies vervolgens de **uitvoeren testen** opdracht voor het lokaal uitvoeren van de WebTest prestaties en zorg ervoor dat alles correct werkt.

    ![Schermopname van de opdracht Test uitvoeren][7]

### <a name="parameterize-the-web-performance-test"></a>Parameter van de prestaties van WebTest
U kunt de prestaties WebTest parameter door te converteren naar een gecodeerde prestaties van de WebTest en vervolgens de code te bewerken. Als alternatief kunt u de prestaties WebTest binden aan een lijst met zodat de test is doorlopen van de gegevens. Zie [genereren en voer een gecodeerde WebTest prestaties](https://msdn.microsoft.com/library/ms182552.aspx) voor meer informatie over het converteren van de webprestaties met een gecodeerde test testen. Zie [een gegevensbron toevoegen aan een WebTest prestaties](https://msdn.microsoft.com/library/ms243142.aspx) voor informatie over het binden van gegevens aan een WebTest prestaties.

In dit voorbeeld hebt we de prestaties WebTest converteren naar een gecodeerde test zodat u kunt de ID in een vliegtuig door een gegenereerde GUID vervangen en voeg meer aanvragen vlucht verzenden naar verschillende locaties.

### <a name="create-a-load-test-project"></a>Een load-test-project maken
Een load-testproject bestaat uit een of meer scenario's beschreven door de WebTest prestaties en het testen van eenheden, samen met de instellingen testen extra belasting van de opgegeven. De volgende stappen laten zien hoe een load-test-project maken:

1. Kies in het snelmenu van uw project Webprestatie- en belastingstest **toevoegen** > **belastingstest**. In de **Load testen** wizard kiest u de **volgende** knop om de test-instellingen te configureren.
2. In de **patroon laden** sectie, kies of u wilt een constante gebruikersbelasting of een stap belasting, die begint met een paar gebruikers en verhoogt de gebruikers gedurende een bepaalde periode.

    Als u een goede schatting van de hoeveelheid gebruikersbelasting hebt en wilt zien hoe het huidige systeem uitvoert, kiest u **constante laden**. Als het doel is te leren of het systeem worden uitgevoerd consistent onder verschillende belasting, kiest u **stap Load**.
3. In de **testen combinatie** sectie, kiest u de **toevoegen** knop en selecteer vervolgens de test die u wilt opnemen in de load-test. U kunt de **distributie** kolom voor het percentage van totale tests uitvoeren voor elke test opgeven.
4. In de **instellingen uitvoeren** sectie, de duur van de test load opgeven.

   > [!NOTE]
   > De **testen iteraties** optie is beschikbaar alleen bij het uitvoeren van een load-test lokaal met Visual Studio.
   >
   >
5. In de **locatie** sectie van **instellingen uitvoeren**, geef de locatie waar load test aanvragen worden gegenereerd. De wizard mogelijk gevraagd u aan te melden met uw Team Services-account. Meld u bij en kies vervolgens een geografische locatie. Als u bent klaar, kiest u de **voltooien** knop.
6. Nadat de load-test is gemaakt, opent u het project .loadtest en kies de huidige instelling, zoals uitvoeren **instellingen uitvoeren** > **Settings1 uitvoeren [actieve]**. Hiermee opent u de instellingen voor uitvoeren in de **eigenschappen** venster.
7. In de **resultaten** sectie van de **instellingen uitvoeren** van het eigenschappenvenster de **Timing Details opslag** instelling hebt **geen** als standaardwaarde. Wijzig deze waarde in **alle afzonderlijke Details** voor meer informatie over de belasting testresultaten. Zie [laden testen](https://www.visualstudio.com/load-testing.aspx) voor meer informatie over hoe u verbinding maken met Visual Studio Team Services en voer een belastingtest uit.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>De load-test uitvoeren met behulp van Visual Studio Team Services
Kies de **Load testen uitvoeren** opdracht om de test uitvoeren te starten.

![Schermopname van de opdracht Load Test uitvoeren][8]

## <a name="view-and-analyze-the-load-test-results"></a>Weergeven en analyseren van de load-testresultaten
Als de belasting test de voortgang van, de informatie over de prestaties grafiek moeten worden weergegeven. U ziet er ongeveer uit als aan de volgende grafiek.

![Schermopname van Prestatiegrafiek voor load testresultaten][9]

1. Kies de **Download rapport** koppeling aan de bovenkant van de pagina. Nadat het rapport wordt gedownload, kiest u de **rapport weergeven** knop.

    Op de **grafiek** tabblad kunt u diagrammen voor verschillende prestatiemeteritems zien. Op de **samenvatting** tabblad de algehele testresultaten worden weergegeven. De **tabellen** tabblad ziet u het totale aantal geslaagde en mislukte load tests.
2. Kies de aantal koppelingen op de **Test** > **mislukt** en de **fouten** > **aantal** kolommen Raadpleeg de foutdetails.

    De **Detail** tabblad ziet u virtuele gebruikers- en informatie over scenario's voor mislukte aanvragen. Deze gegevens kan nuttig zijn als de load-test meerdere scenario's bevat.

Zie [resultaten voor het laden van testen analyseren in de weergave grafieken van de laden testen Analyzer](https://www.visualstudio.com/load-testing.aspx) resultaten voor meer informatie over het weergeven van de belasting te testen.

## <a name="automate-your-load-test"></a>Uw test load automatiseren
Visual Studio Team Services laden Test biedt API's waarmee u load tests beheren en analyseren van resultaten in een Team Services-account. Zie [Cloud Load testen Rest-API's](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* [Controle en diagnose van services in de instellingen voor een lokale computer-ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
