---
title: Een .NET-Service Fabric-toepassing maken in Azure | Microsoft Docs
description: Maak een .NET-toepassing voor Azure met behulp van de Service Fabric-snel starten-voorbeeld.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: mikhegn
ms.custom: mvc, devcenter
ms.openlocfilehash: 3be8836ae6b877bc4caa98f0467147b008c42aa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a>Een .NET-Service Fabric-toepassing maken in Azure
Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u schaalbare en betrouwbare microservices en containers implementeert en beheert. 

Deze snelstartgids laat zien hoe uw eerste .NET-toepassing voor Service Fabric implementeert. Wanneer u klaar bent, hebt u een stemtoepassing met een ASP.NET Core web-front-die stemmende resultaten worden opgeslagen in een stateful back-end-service in het cluster.

![Schermafbeelding van de toepassing](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

Met behulp van deze toepassing u leert hoe:
> [!div class="checklist"]
> * Een toepassing maken met .NET- en Service Fabric
> * ASP.NET core gebruiken als een webfront-end
> * Opslaan van toepassingsgegevens in een stateful service
> * Fouten opsporen in uw toepassing lokaal
> * De toepassing implementeren naar een cluster in Azure
> * Scale-out van de toepassing op meerdere knooppunten
> * Een rolling upgrade van de toepassing uitvoeren

## <a name="prerequisites"></a>Vereisten
Dit zijn de vereisten voor het voltooien van deze Quickstart:
1. [Installeer Visual Studio 2017](https://www.visualstudio.com/) met de **ontwikkelen van Azure** en **ASP.NET en web ontwikkeling** werkbelastingen.
2. [Git installeren](https://git-scm.com/)
3. [De Microsoft Azure Service Fabric SDK installeren](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. Voer de volgende opdracht om in te schakelen van Visual Studio op de lokale Service Fabric-cluster implementeren:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-the-sample"></a>Het voorbeeld downloaden
Voer de volgende opdracht voor het klonen van de opslagplaats van de voorbeeld-app op uw lokale computer in een opdrachtvenster.
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren
Met de rechtermuisknop op het pictogram Visual Studio in het Menu Start en kies **als administrator uitvoeren**. U moet het foutopsporingsprogramma koppelen aan uw services, Visual Studio als administrator uitvoeren.

Open de **Voting.sln** Visual Studio-oplossing uit de opslagplaats voor u.

Voor het implementeren van de toepassing, drukt u op **F5**.

> [!NOTE]
> De eerste keer dat u uitvoert en implementeer de toepassing in Visual Studio maakt een lokaal cluster voor foutopsporing. Deze bewerking kan enige tijd duren. De status van het maken van het cluster wordt weergegeven in het Visual Studio-uitvoervenster.

Wanneer de implementatie voltooid is, een browser starten en open deze pagina: `http://localhost:8080` -de webfront-end van de toepassing.

![Front-toepassing](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

U kunt nu een set opties voor uw stem toevoegen en begin met het maken van stemmen. De toepassing wordt uitgevoerd en worden alle gegevens in uw Service Fabric-cluster, zonder de noodzaak van een aparte database opgeslagen.

## <a name="walk-through-the-voting-sample-application"></a>Doorloop de stemmende voorbeeldtoepassing
De stemtoepassing bestaat uit twee services:
- Web-front-service (VotingWeb): een ASP.NET Core web-front-service, die de webpagina fungeert en zichtbaar gemaakt web-API's om te communiceren met de back-endservice.
- Back-end-service (VotingData)-Core van een ASP.NET-webservice, die wordt er een API voor het opslaan van de stem resultaten in een betrouwbare woordenlijst persistent op schijf.

![Diagram van de toepassing](./media/service-fabric-quickstart-dotnet/application-diagram.png)

Wanneer u in de toepassing stemmen gebeurt het volgende:
1. Een JavaScript verzendt de stemaanvraag naar de web-API in de web-front-endservice als een HTTP PUT-aanvraag.

2. De web-front-service wordt een proxyserver gebruikt om te zoeken en doorsturen van een HTTP PUT-aanvraag naar de back-endservice.

3. De back-endservice nodig van de binnenkomende aanvraag en slaat de bijgewerkte resultaten in een betrouwbare woordenlijst die wordt gerepliceerd naar meerdere knooppunten binnen het cluster en opgeslagen op schijf. Gegevens van de toepassing wordt opgeslagen in het cluster, zodat er geen database nodig is.

## <a name="debug-in-visual-studio"></a>Fouten opsporen in Visual Studio
Als u fouten opspoort toepassing in Visual Studio, gebruikt u een lokaal cluster van de Service Fabric-ontwikkeling. U hebt de optie voor het aanpassen van uw ervaring foutopsporing op uw scenario. In deze toepassing opgeslagen gegevens in onze back-end-service met behulp van een betrouwbare woordenlijst. Visual Studio verwijdert u de toepassing per standaard, wanneer u het foutopsporingsprogramma stopt. De toepassing is verwijderd, worden de gegevens in de back-end-service ook verwijderd. Om te blijven behouden de gegevens tussen foutopsporingssessies, kunt u de **toepassing foutopsporingsmodus** als eigenschap op de **Voting** -project in Visual Studio.

Als u wilt kijken wat er in de code gebeurt, kunt u de volgende stappen uitvoeren:
1. Open de **VotesController.cs** bestands- en stel een onderbrekingspunt in de web-API's **plaatsen** methode (regel 47) - u kunt zoeken naar het bestand in Solution Explorer in Visual Studio.

2. Open de **VoteDataController.cs** bestands- en stel een onderbrekingspunt in deze web-API **plaatsen** methode (line 50).

3. Ga terug naar de browser en klik op een optie of een nieuwe stemmende optie toevoegen. U onderbrekingspunt de eerste in de web front-end van api-controller.
    - Dit is waar de JavaScript in de browser wordt een aanvraag verzonden naar de web-API-controller in de front-end-service.
    
    ![Stem front-end-Service toevoegen](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - Eerst wordt de URL van de ReverseProxy opstellen voor onze back-endservice **(1)**.
    - Vervolgens wordt de HTTP PUT-aanvraag naar de ReverseProxy verzenden **(2)**.
    - Ten slotte de we het antwoord van de back-end-service naar de client geretourneerd **(3)**.

4. Druk op **F5** om door te gaan
    - U bent nu op het punt onderbreking in de back-endservice.
    
    ![Stem Back-End-Service toevoegen](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - In de eerste regel in de methode **(1)** gebruiken we de `StateManager` ophalen of het toevoegen van een betrouwbare woordenlijst aangeroepen `counts`.
    - Alle interacties met waarden in een betrouwbare woordenlijst vereisen een transactie, deze met de instructie **(2)** die transactie maakt.
    - In de transactie we vervolgens de waarde van de relevante sleutel voor de optie bijwerken en voert de bewerking **(3)**. Als de gegevens van de commit-methode retourneert is bijgewerkt in de woordenlijst en gerepliceerd naar andere knooppunten in het cluster. De gegevens worden nu veilig opgeslagen in het cluster en de back-endservice failover naar andere knooppunten, nog steeds de gegevens beschikbaar.
5. Druk op **F5** om door te gaan

Als u wilt de Foutopsporingssessie stoppen, drukt u op **Shift + F5**.

## <a name="deploy-the-application-to-azure"></a>De toepassing implementeren in Azure
U kunt voor het implementeren van de toepassing naar een cluster in Azure, ofwel uw eigen cluster maken of een Cluster van derden gebruiken.

Clusters van derden zijn gratis, tijdelijke Service Fabric-clusters die worden gehost op Azure en uitgevoerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en meer te weten komen over het platform. [Volg de instructies](http://aka.ms/tryservicefabric) om toegang te krijgen tot een cluster van derden. 

Zie voor meer informatie over het maken van uw eigen cluster [Uw eerste Service Fabric-cluster maken op Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> De web-front-service is geconfigureerd om te luisteren op poort 8080 voor binnenkomend verkeer. Zorg ervoor dat de poort is geopend in het cluster. Als u het Cluster van derden gebruikt, is deze poort is geopend.
>

### <a name="deploy-the-application-using-visual-studio"></a>Implementeer de toepassing met Visual Studio
Nu de toepassing klaar is, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster.

1. Met de rechtermuisknop op **Voting** in Solution Explorer en kiest u **publiceren**. Het dialoogvenster Publiceren wordt weergegeven.

    ![Het dialoogvenster Publiceren](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. Typ het verbindingseindpunt van het cluster in het veld **Verbindingseindpunt** en klik op **Publiceren**. Wanneer u zich registreren voor het Cluster Party, wordt het verbindingseindpunt beschikbaar in de browser. -bijvoorbeeld `winh1x87d1d.westus.cloudapp.azure.com:19000`.

3. Open een browser en typ in het cluster adres foolowed door ': 8080' om te krijgen tot de toepassing in het cluster - bijvoorbeeld `http://winh1x87d1d.westus.cloudapp.azure.com:8080`. U ziet nu de toepassing in het cluster in Azure wordt uitgevoerd.

![Front-toepassing](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Toepassingen en services voor schalen in een cluster
Service Fabric-services kunnen eenvoudig worden geschaald over een cluster te maken voor een wijziging in de belasting van de services. U schaalt een service door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster. U hebt meerdere manieren schalen van uw services, kunt u scripts of opdrachten van PowerShell of CLI van de Fabric-Service (sfctl). In dit voorbeeld gebruiken we Service Fabric Explorer.

Service Fabric Explorer wordt uitgevoerd in alle Service Fabric-clusters en toegankelijk is vanuit een browser door te bladeren naar de clusters HTTP-poort (19080), bijvoorbeeld `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.

Voer de volgende stappen uit om de web-front-endservice te schalen:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klik op het weglatingsteken (drie punten) naast de **fabric: / Voting/VotingWeb** knooppunt in de structuurweergave en kies **Scale Service**.

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    U kunt er nu voor kiezen om het aantal exemplaren van de web-front-endservice te schalen.

3. Wijzig het aantal in **2** en klik op **Service schalen**.
4. Klik op de **fabric: / Voting/VotingWeb** knooppunt in de structuurweergave en het partitie-knooppunt (vertegenwoordigd door een GUID zijn).

    ![Service Fabric Explorer Scale-Service](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    U kunt nu zien dat de service twee exemplaren heeft en in de structuurweergave u welke knooppunten de instanties worden uitgevoerd ziet op.

Met deze eenvoudige beheertaak is het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor onze front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="perform-a-rolling-application-upgrade"></a>Een rolling upgrade van de toepassing uitvoeren
Bij het implementeren van nieuwe updates voor uw toepassing, wordt de Service Fabric van de update op een veilige manier. Rolling upgrades biedt u geen uitvaltijd tijdens het bijwerken of het terugdraaien van de geautomatiseerde moeten fouten optreden.

Upgrade van de toepassing moet het volgende doen:

1. Open de **Index.cshtml** bestand in Visual Studio - u kunt zoeken naar het bestand in Solution Explorer in Visual Studio.
2. De kop op de pagina wijzigen door enkele tekst - bijvoorbeeld toe te voegen.
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. Sla het bestand op.
4. Met de rechtermuisknop op **Voting** in Solution Explorer en kiest u **publiceren**. Het dialoogvenster Publiceren wordt weergegeven.
5. Klik op de **Manifest versie** om te wijzigen van de versie van de service en de toepassing.
6. Wijzigen van de versie van de **Code** element onder **VotingWebPkg** naar '2.0.0' bijvoorbeeld en op **opslaan**.

    ![Dialoogvenster van de versie wijzigen](./media/service-fabric-quickstart-dotnet/change-version.png)
7. In de **Service Fabric-toepassing publiceren** dialoogvenster, Controleer de Upgrade van het selectievakje van de toepassing en klik op **publiceren**.

    ![Dialoogvenster publiceren instelling upgraden](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. Open uw browser en blader naar het adres van het cluster op poort 19080 - bijvoorbeeld `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
9. Klik op de **toepassingen** knooppunt in de structuurweergave en vervolgens **Upgrades uitgevoerd** in het rechterdeelvenster. U ziet hoe de upgrade wordt via de upgradedomeinen in uw cluster ervoor te zorgen dat elk domein is in orde voordat u doorgaat naar de volgende.
    ![Upgrade weergeven in Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/upgrading.png)

    Service Fabric maakt upgrades veilig door Wacht twee minuten na het bijwerken van de service op elk knooppunt in het cluster. Verwacht dat de volledige update van ongeveer 8 minuten duren.

10. Terwijl de upgrade wordt uitgevoerd, kunt u nog steeds de toepassing. Omdat u twee exemplaren van de service wordt uitgevoerd in het cluster hebt, krijgt enkele van de aanvragen mogelijk een bijgewerkte versie van de toepassing, terwijl anderen mogelijk nog steeds de oude versie.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u de volgende zaken geleerd:

> [!div class="checklist"]
> * Een toepassing maken met .NET- en Service Fabric
> * ASP.NET core gebruiken als een webfront-end
> * Opslaan van toepassingsgegevens in een stateful service
> * Fouten opsporen in uw toepassing lokaal
> * De toepassing implementeren naar een cluster in Azure
> * Scale-out van de toepassing op meerdere knooppunten
> * Een rolling upgrade van de toepassing uitvoeren

Voor meer informatie over Service Fabric en .NET, kijk eens naar deze zelfstudie:
> [!div class="nextstepaction"]
> [.NET-toepassing op Service Fabric](service-fabric-tutorial-create-dotnet-app.md)