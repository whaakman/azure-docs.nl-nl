---
title: Ontwikkelen en implementeren met Visual Studio - Azure WebJobs
description: Informatie over het ontwikkelen en implementeren van Azure WebJobs in Azure App Service met behulp van Visual Studio.
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 8793485f2f1967e8c14ef335f14f81c9b946f974
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Ontwikkelen en implementeren met Visual Studio - Azure App Service API

## <a name="overview"></a>Overzicht

Dit onderwerp wordt uitgelegd hoe u met Visual Studio een consoletoepassing-project implementeert in een WebApp in [App Service](app-service-web-overview.md) als een [Azure webtaak](http://go.microsoft.com/fwlink/?LinkId=390226). Voor informatie over het implementeren van WebJobs met behulp van de [Azure-portal](https://portal.azure.com), Zie [achtergrondtaken uitvoeren met de WebJobs](web-sites-create-web-jobs.md).

Visual Studio een consoletoepassing webtaken ingeschakeld-project implementeert, worden twee taken uitgevoerd:

* Runtime-bestanden worden gekopieerd naar de juiste map in de web-app (*App_Data/taken/continue* voor doorlopende webtaken *App_Data/taken/geactiveerd* voor geplande en on-demand WebJobs).
* Stelt [Azure Scheduler-taken](#scheduler) voor WebJobs die zijn gepland op bepaalde tijden. (Dit is niet nodig voor doorlopende webtaken.)

Een project WebJobs ingeschakeld heeft de volgende items toegevoegd:

* De [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-pakket.
* Een [webtaak-publiceren settings.json](#publishsettings) -bestand met de implementatie en de scheduler-instellingen. 

![Diagram van wat wordt toegevoegd aan een Console-App implementatie als een webtaak moet worden ingeschakeld](./media/websites-dotnet-deploy-webjobs/convert.png)

U kunt deze items toevoegen aan een bestaande Console-toepassing-project of gebruik een sjabloon te maken van een nieuw project voor de consoletoepassing WebJobs ingeschakeld. 

U kunt een project implementeert als een webtaak zelfstandig of koppelen aan een webproject dat automatisch wordt geïmplementeerd wanneer u het webproject implementeren. Als u wilt koppelen projecten, Visual Studio bevat de naam van het project WebJobs ingeschakeld in een [webjobs list.json](#webjobslist) bestand in het webproject.

![Diagram van webtaak project koppelen aan webproject](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2015 gebruikt, installeert u de [Azure SDK voor .NET (Visual Studio 2015)](https://azure.microsoft.com/downloads/).

Als u Visual Studio 2017 gebruikt, installeert u de [ontwikkelen van Azure werkbelasting](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

## <a id="convert"></a>WebJobs-implementatie voor een bestaande consoletoepassingsproject inschakelen

U hebt hiervoor twee opties:

* [Schakel automatische implementatie met een webproject](#convertlink).

  Een bestaand project in de Console-toepassing zodanig configureren dat deze automatisch wordt geïmplementeerd als een webtaak wanneer u een webproject implementeert. Gebruik deze optie als u wilt uw webtaak uitvoeren in dezelfde web-app waarin u de gerelateerde webtoepassing uitvoeren.

* [Implementatie zonder een webproject inschakelen](#convertnolink).

  Een bestaand project-consoletoepassing te implementeren als een webtaak zelfstandig gebruikt, met geen koppeling naar een webproject configureren. Gebruik deze optie als u wilt uitvoeren in een web-app een webtaak zelfstandig gebruikt, met geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources webtaak onafhankelijk van uw webtoepassingsbronnen.

### <a id="convertlink"></a>Automatische WebJobs-implementatie met een webproject inschakelen

1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **bestaand Project als Azure webtaak**.
   
    ![Bestaand Project als Azure webtaak](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    De [toevoegen Azure webtaak](#configure) dialoogvenster wordt weergegeven.
2. In de **projectnaam** vervolgkeuzelijst, selecteer de consoletoepassing project wilt toevoegen als een webtaak.
   
    ![Project selecteren in het dialoogvenster Azure webtaak toevoegen](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Voltooi de [toevoegen Azure webtaak](#configure) dialoogvenster en klik vervolgens op **OK**. 

### <a id="convertnolink"></a>WebJobs-implementatie zonder een webproject inschakelen
1. Met de rechtermuisknop op het project-consoletoepassing in **Solution Explorer**, en klik vervolgens op **publiceren als Azure webtaak...** . 
   
    ![Als Azure webtaak publiceren](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    De [toevoegen Azure webtaak](#configure) dialoogvenster wordt weergegeven, met het project is geselecteerd de **projectnaam** vak.
2. Voltooi de [toevoegen Azure webtaak](#configure) in het dialoogvenster en klik vervolgens op **OK**.
   
   De **webpublicatie** wizard wordt weergegeven.  Als u niet onmiddellijk publiceren wilt, moet u de wizard sluiten. De instellingen die u hebt ingevoerd voor worden opgeslagen wanneer u wilt [het project implementeren](#deploy).

## <a id="create"></a>Maak een nieuw project voor WebJobs ingeschakeld
Voor het maken van een nieuw project voor webtaken is ingeschakeld, kunt u de projectsjabloon consoletoepassing gebruiken en WebJobs implementatie inschakelen zoals toegelicht in [de vorige sectie](#convert). Als alternatief kunt kunt u het WebJobs-nieuw project-sjabloon gebruiken:

* [Gebruik de sjabloon WebJobs-nieuw project voor een onafhankelijke webtaak](#createnolink)
  
    Een project maken en deze te implementeren door zichzelf als een webtaak, met geen koppeling naar een webproject configureren. Gebruik deze optie als u wilt uitvoeren in een web-app een webtaak zelfstandig gebruikt, met geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources webtaak onafhankelijk van uw webtoepassingsbronnen.
* [Gebruik de sjabloon WebJobs-nieuw project voor een webtaak wordt gekoppeld aan een webproject](#createlink)
  
    Maak een project die is geconfigureerd voor automatisch als een webtaak implementeren wanneer een webproject in de oplossing is geïmplementeerd. Gebruik deze optie als u wilt uw webtaak uitvoeren in dezelfde web-app waarin u de gerelateerde webtoepassing uitvoeren.

> [!NOTE]
> De sjabloon voor de API-nieuw project automatisch NuGet-pakketten worden geïnstalleerd en bevat een code in *Program.cs* voor de [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Als u niet wilt dat de WebJobs SDK wilt gebruiken, verwijder of wijzig de `host.RunAndBlock` -instructie in *Program.cs*.
> 
> 

### <a id="createnolink"></a>Gebruik de sjabloon WebJobs-nieuw project voor een onafhankelijke webtaak
1. Klik op **bestand** > **nieuw Project**, en klik vervolgens in de **nieuw Project** dialoogvenster vak Klik **Cloud**  >   **Azure webtaak (.NET Framework)**.
   
    ![Het dialoogvenster New Project webtaak sjabloon weergeven](./media/websites-dotnet-deploy-webjobs/np.png)
2. Volg de instructies die eerder naar weergegeven [de consoletoepassing project een onafhankelijk WebJobs-project maken](#convertnolink).

### <a id="createlink"></a>Gebruik de sjabloon WebJobs-nieuw project voor een webtaak wordt gekoppeld aan een webproject
1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **nieuwe Azure webtaak Project**.
   
    ![Nieuwe Azure webtaak Project menu-item](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    De [toevoegen Azure webtaak](#configure) dialoogvenster wordt weergegeven.
2. Voltooi de [toevoegen Azure webtaak](#configure) in het dialoogvenster en klik vervolgens op **OK**.

## <a id="configure"></a>Het dialoogvenster Azure webtaak toevoegen
De **toevoegen Azure webtaak** dialoogvenster kunt u de naam van een webtaak en voert u de instelling voor uw webtaak uit. 

![Dialoogvenster Azure webtaak toevoegen](./media/websites-dotnet-deploy-webjobs/aaw2.png)

De velden in dit dialoogvenster overeenkomen met de velden op de **webtaak toevoegen** dialoogvenster van de Azure portal. Zie voor meer informatie [achtergrondtaken uitvoeren met de WebJobs](web-sites-create-web-jobs.md).

> [!NOTE]
> * Zie voor meer informatie over de implementatie van het opdrachtregelprogramma [inschakelen vanaf de opdrachtregel of continue levering van Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Als u een webtaak implementeert en vervolgens besluit u wilt wijzigen van het type webtaak en opnieuw distribueren, moet u verwijdert de *webjobs-publiceren settings.json* bestand. Dit maakt Visual Studio weergeven de publicatieopties, zodat u het type webtaak kunt wijzigen.
> * Als u een webtaak implementeert en de uitvoeringsmodus later van continue naar niet-doorlopende of omgekeerd wijzigen, maakt Visual Studio een nieuw webtaak in Azure, wanneer u opnieuw implementeert. Als u andere schema-instellingen wijzigen, maar laat uitvoermodus dezelfde of schakelen tussen geplande en op verzoek, Visual Studio updates van de bestaande taak plaats van een nieuwe maken.
> 
> 

## <a id="publishsettings"></a>webtaak-publiceren settings.json
Wanneer u een consoletoepassing voor WebJobs-implementatie configureert, Visual Studio installeert de [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-pakket en winkels planningsgegevens in een *webtaak-publiceren settings.json*  bestand in het project *eigenschappen* map van de API-project. Hier volgt een voorbeeld van het bestand:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

U kunt dit bestand rechtstreeks bewerken en Visual Studio biedt IntelliSense. Het schema van het bestand wordt opgeslagen op [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) en er kan worden weergegeven.  

## <a id="webjobslist"></a>webjobs list.json
Wanneer u een project WebJobs ingeschakeld aan een webproject koppelen, Visual Studio slaat de naam van de API-project in een *webjobs list.json* bestand in het webproject *eigenschappen* map. De lijst kan meerdere WebJobs projecten bevatten, zoals wordt weergegeven in het volgende voorbeeld:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

U kunt dit bestand rechtstreeks bewerken en Visual Studio biedt IntelliSense. Het schema van het bestand wordt opgeslagen op [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) en er kan worden weergegeven.

## <a id="deploy"></a>Een API-project implementeert
Een API-project dat u hebt gekoppeld aan een webproject implementeert automatisch met het webproject. Zie voor meer informatie over de implementatie van web project **instructies begeleidt** > **implementeren app** in de linkernavigatiebalk.

Als u wilt een API-project implementeert zelfstandig, met de rechtermuisknop op het project in **Solution Explorer** en klik op **publiceren als Azure webtaak...** . 

![Als Azure webtaak publiceren](./media/websites-dotnet-deploy-webjobs/paw.png)

Voor een onafhankelijke webtaak dezelfde **webpublicatie** wizard die wordt gebruikt voor webprojecten wordt weergegeven, maar met minder instellingen beschikbaar om te wijzigen.
