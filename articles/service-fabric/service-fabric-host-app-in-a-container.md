---
title: Een .NET-toepassing in een container implementeren in Azure Service Fabric | Microsoft Docs
description: "Leert u hoe u een .NET-app in Visual Studio in een Docker-container moet inpakken. Deze nieuwe 'container'-app wordt vervolgens geïmplementeerd op een Service Fabric-cluster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: cd1c3b063132ae549bfbf1e059667c5056c91046
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Een .NET-toepassing in een Windows-container implementeren in Azure Service Fabric

Deze zelfstudie laat zien hoe u een bestaande ASP.NET-toepassing in een Windows-container in Azure kunt implementeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Docker-project in Visual Studio maken
> * Een bestaande toepassing in een container plaatsen
> * Doorlopende integratie met Visual Studio en VSTS instellen

## <a name="prerequisites"></a>Vereisten

1. Installeer [Docker CE voor Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) zodat u containers op Windows 10 kunt uitvoeren.
2. Maak uzelf vertrouwd met de [Windows 10 Containers-quickstart][link-container-quickstart].
3. Download de voorbeeldtoepassing [Fabrikam Fiber CallCenter][link-fabrikam-github].
4. Installeer [Azure PowerShell][link-azure-powershell-install]
5. Installeer de [extensie Continuous Delivery Tools voor Visual Studio 2017][link-visualstudio-cd-extension]
6. Maak een [Azure-abonnement][link-azure-subscription] en een [Visual Studio Team Services-account][link-vsts-account]. 
7. [Een cluster maken in Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md)

## <a name="create-a-cluster-on-azure"></a>Een cluster maken op Azure
Service Fabric-toepassingen worden uitgevoerd op een cluster, een set virtuele of fysieke machines die verbonden zijn over een netwerk. [Stel een Azure Service Fabric-cluster in dat wordt uitgevoerd op Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md) voordat u uw toepassing maakt en implementeert. Wanneer u het cluster maakt, kiest u een SKU die ondersteuning biedt voor actieve containers (zoals Windows Server 2016 Datacenter met Containers).

## <a name="containerize-the-application"></a>De toepassing in een container plaatsen

Nu u een Service Fabric-cluster in Azure uitvoert, bent u klaar om een beperkte toepassing te maken en implementeren. Om onze toepassing in een container te starten, moeten we **Docker Support** toevoegen aan het project in Visual Studio. Bij het toevoegen van **Docker Support** aan de toepassing gebeuren er twee dingen. Eerst wordt een _Dockerfile_ toegevoegd aan het project. Dit nieuwe bestand beschrijft hoe de containerinstallatiekopie moet worden opgebouwd. Als tweede wordt een nieuw _docker-compose_-project toegevoegd aan de oplossing. Het nieuwe project bevat enkele docker-compose-bestanden. Docker-compose-bestanden kunnen worden gebruikt om te beschrijven hoe de container wordt uitgevoerd.

Meer informatie over het werken met [Visual Studio Container Tools][link-visualstudio-container-tools].

>[!NOTE]
>Als dit de eerste keer is dat u Windows-containerinstallatiekopieën op uw computer uitvoert, moet Docker CE eerst de basisinstallatiekopieën voor uw containers ophalen. De afbeeldingen die in deze zelfstudie worden gebruikt zijn 14 GB. Ga aan de slag en voer de volgende terminalopdracht in voor het ophalen van de basisinstallatiekopieën:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Docker-ondersteuning toevoegen

Open het bestand [FabrikamFiber.CallCenter.sln][link-fabrikam-github] in Visual Studio.

Klik met de rechtermuisknop op het project **FabrikamFiber.Web** > **Toevoegen van**  > **Docker ondersteuning**.

### <a name="add-support-for-sql"></a>Ondersteuning voor SQL toevoegen

Deze toepassing gebruikt SQL als de gegevensprovider zodat een SQL-server vereist is voor het uitvoeren van de toepassing. Verwijs naar een SQL Server-containerinstallatiekopie in ons docker-compose.override.yml-bestand.

Open in Visual Studio **Solution Explorer**, zoek **docker-compose**, en open het bestand **docker-compose.override.yml**.

Navigeer naar het knooppunt `services:`, voeg een knooppunt toe met de naam `db:` die de SQL Server-vermelding voor de container definieert.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>U een SQL Server naar keuze gebruiken voor lokale foutopsporing, zolang deze bereikbaar is vanaf de host. **localdb** biedt echter geen ondersteuning voor `container -> host`-communicatie.

>[!WARNING]
>Het uitvoeren van SQL Server in een container biedt geen ondersteuning voor het vastleggen van gegevens. Wanneer de container wordt gestopt, worden uw gegevens gewist. Gebruik deze configuratie niet voor productie.

Navigeer naar het knooppunt `fabrikamfiber.web:` en voeg een onderliggend knooppunt toe met de naam `depends_on:`. Dit zorgt ervoor dat de service `db` (de SQL Server-container) voor onze webtoepassing (fabrikamfiber.web) wordt gestart.

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>De webconfiguratie bijwerken

Werk in het project **FabrikamFiber.Web** de verbindingsreeks in het bestand **web.config** bij om te verwijzen naar de SQL Server in de container.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Als u een andere SQL Server wilt gebruiken tijdens het bouwen van een release-build van uw webtoepassing, voeg dan nog een verbindingsreeks toe aan uw web.release.config-bestand.

### <a name="test-your-container"></a>Uw container testen

Druk op **F5** om de toepassing in de container uit te voeren en fouten op te sporen.

Edge opent de gedefinieerde startpagina van uw toepassing met het IP-adres van de container op het interne NAT-netwerk (meestal 172.x.x.x). Zie [dit artikel][link-debug-container] voor meer informatie over foutopsporing in toepassingen in containers met behulp van Visual Studio 2017.

![Voorbeeld van fabrikam in een container][image-web-preview]

De container is nu klaar om te worden gebouwd en verpakt in een Service Fabric-toepassing. Zodra u de installatiekopie van de container hebt gebouwd op uw computer, kunt u deze naar een containerregister pushen en naar elke host ophalen om te worden uitgevoerd.

## <a name="get-the-application-ready-for-the-cloud"></a>De toepassing voorbereiden op de cloud

Om de toepassing voor te bereiden op het uitvoeren in Service Fabric in Azure, moeten we twee stappen voltooien:

1. Maak de poort beschikbaar waarop we onze webtoepassing in de Service Fabric-cluster moeten kunnen bereiken.
2. Geef voor de toepassing een SQL-database die gereed is voor productie.

### <a name="expose-the-port-for-the-app"></a>De poort voor de app beschikbaar maken
Het Service Fabric-cluster dat we hebben geconfigureerd, heeft standaard poort *80* geopend in de Azure Load Balancer, die binnenkomend verkeer naar het cluster balanceert. We kunnen onze container op deze poort beschikbaar maken via ons docker-compose.yml-bestand.

Open in Visual Studio **Solution Explorer**, zoek **docker-compose**, en open het bestand **docker-compose.yml**.

Wijzig het knooppunt `fabrikamfiber.web:`, voeg een onderliggend knooppunt toe met de naam `ports:`.

Voeg een tekenreeksvermelding `- "80:80"` toe. Zo moet uw docker-compose.yml-bestand eruitzien:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Een SQL-productiedatabase gebruiken
Wanneer deze in productie worden uitgevoerd, moeten we onze gegevens behouden in de database. Er is momenteel geen manier om behoud van gegevens in een container te waarborgen, daarom kunt u productiegegevens niet opslaan in SQL Server in een container.

U wordt aangeraden gebruik te maken van een Azure SQL Database. Als u een beheerde SQL Server in Azure wilt instellen en uitvoeren, gaat u naar het artikel [Azure SQL Database-quickstarts][link-azure-sql].

>[!NOTE]
>Denk eraan de verbindingsreeksen te wijzigen naar de SQL-server in het bestand **web.release.config** in het project **FabrikamFiber.Web**.
>
>Deze toepassing mislukt probleemloos als geen SQL database bereikbaar is. U kunt ervoor kiezen om door te gaan en de toepassing te implementeren zonder SQL-server.

## <a name="deploy-with-visual-studio-team-services"></a>Implementeren met Visual Studio Team Services

Als u een implementatie met behulp van Visual Studio Team Services instelt, moet u de [extensie Continuous Delivery Tools voor Visual Studio 2017][link-visualstudio-cd-extension] installeren. Met deze extensie kunt u eenvoudig implementeren in Azure door Visual Studio Team Services te configureren en uw app te implementeren naar uw Service Fabric-cluster.

Om te beginnen, moet uw code worden gehost in broncodebeheer. De rest van deze sectie gaat ervan uit dat **git** wordt gebruikt.

### <a name="set-up-a-vsts-repo"></a>Een VSTS-opslagplaats instellen
Klik op de rechterbenedenhoek van Visual Studio op **Toevoegen aan broncodebeheer** > **Git** (of op de optie die u voorkeur heeft).

![druk op de knop Broncodebeheer][image-source-control]

In het deelvenster _Team Explorer_ drukt u op **Git-opslagplaats publiceren**.

Selecteer de naam van de VSTS-opslagplaats en druk op **Opslagplaats**.

![opslagplaats naar VSTS publiceren][image-publish-repo]

Nu uw code wordt gesynchroniseerd met een VSTS-bronopslagplaats, kunt u continue integratie en continue levering configureren.

### <a name="setup-continuous-delivery"></a>Continue levering instellen

Klik in _Solution Explorer_, met de rechtermuisknop op de **oplossing** > **Continue levering configureren**.

Selecteer het Azure-Abonnement.

Stel **Type Host** in op **Service Fabric Cluster**.

Stel **Doelhost** in op de service fabric-cluster die u in de vorige sectie hebt gemaakt.

Kies een **Containerregister** voor het publiceren van de container.

>[!TIP]
>Gebruik de knop **Bewerken** om een containerregister te maken.

Druk op **OK**.

![Continue integratie voor Service Fabric instellen][image-setup-ci]
   
   Zodra de configuratie is voltooid, wordt de container geïmplementeerd naar Service Fabric. Wanneer u updates naar de opslagplaats pusht, worden een nieuwe build en versie uitgevoerd.
   
   >[!NOTE]
   >Het bouwen van de containerinstallatiekopieën duurt ongeveer 15 minuten.
   >De eerste implementatie in het Service Fabric-cluster zorgt ervoor dat de basis Windows Server Core-container afbeeldingen worden gedownload. Het downloaden duurt 5-10 minuten extra.

Blader naar de Fabrikam Call Center-toepassing met de URL van uw cluster: bijvoorbeeld *http://mycluster.westeurope.cloudapp.azure.com*

Nu u de Fabrikam Call Center-app in een container hebt geïmplementeerd, kunt u de [Azure-portal][link-azure-portal] openen en de toepassing zien die wordt uitgevoerd in Service Fabric. Open om de toepassing uit te proberen een webbrowser en ga naar de URL van uw Service Fabric-cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Docker-project in Visual Studio maken
> * Een bestaande toepassing in een container plaatsen
> * Doorlopende integratie met Visual Studio en VSTS instellen

In het volgende gedeelte van de zelfstudie krijgt u informatie over het instellen van [bewaking voor de container](service-fabric-tutorial-monitoring-wincontainers.md).

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
