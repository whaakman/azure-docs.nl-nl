---
title: Ontwikkelen en implementeren met Visual Studio - Azure WebJobs
description: Informatie over het ontwikkelen en implementeren van Azure WebJobs in Azure App Service met behulp van Visual Studio.
services: app-service
documentationcenter: ''
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 09/12/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 300ccf5da3528be63ae618338b8953d05b0e799d
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337458"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Ontwikkelen en implementeren met Visual Studio - Azure App Service WebJobs

## <a name="overview"></a>Overzicht

In dit onderwerp wordt uitgelegd hoe u Visual Studio gebruiken voor het implementeren van een consoletoepassingsproject in een WebApp in [App Service](app-service-web-overview.md) als een [Azure-webtaak](https://go.microsoft.com/fwlink/?LinkId=390226). Voor informatie over hoe u WebJobs implementeren met behulp van de [Azure-portal](https://portal.azure.com), Zie [achtergrondtaken uitvoeren met WebJobs](web-sites-create-web-jobs.md).

Wanneer Visual Studio een consoletoepassingsproject WebJobs is ingeschakeld implementeert, worden twee taken uitgevoerd:

* Runtime-bestanden worden gekopieerd naar de juiste map in de web-app (*App_Data/jobs/continuous* voor doorlopende webtaken *App_Data/jobs/triggered* voor geplande en on-demand WebJobs).
* Stelt u [Azure Scheduler](https://docs.microsoft.com/azure/scheduler/) taken voor webtaken die zijn gepland om te worden uitgevoerd op bepaalde tijdstippen. (Dit is niet nodig voor doorlopende webtaken.)

Een project WebJobs-functionaliteit heeft de volgende items toegevoegd aan het:

* De [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-pakket.
* Een [webjob-publiceren settings.json](#publishsettings) -bestand met de implementatie en de scheduler-instellingen. 

![Diagram van wat wordt toegevoegd aan een Console-App implementeren als een WebJob inschakelen](./media/websites-dotnet-deploy-webjobs/convert.png)

U kunt deze items toevoegen aan een bestaande Console Application-project of een sjabloon gebruiken om een nieuwe consoletoepassing WebJobs ingeschakeld-project te maken. 

U kunt een project implementeren als een WebJob zelfstandig of koppelen aan een webproject, zodat deze automatisch wordt geïmplementeerd wanneer u het webproject implementeren. Om een koppeling projecten, Visual Studio bevat de naam van het project WebJobs ingeschakeld in een [webjobs list.json](#webjobslist) bestand in de webproject.

![Diagram van de webtaak project koppelen aan de web-project](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2015 gebruikt, installeert u de [Azure SDK voor .NET (Visual Studio 2015)](https://azure.microsoft.com/downloads/).

Als u Visual Studio 2017 gebruikt, installeert u de [Azure-ontwikkelworkload](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

## <a id="convert"></a> WebJobs-implementatie voor een bestaande consoletoepassingsproject inschakelen

U hebt hiervoor twee opties:

* [Inschakelen van automatische implementatie met een webproject](#convertlink).

  Een bestaande consoletoepassingsproject zodanig configureren dat deze automatisch wordt geïmplementeerd als een WebJob wanneer u een webproject implementeert. Gebruik deze optie als u wilt uitvoeren van de webtaak in dezelfde web-app waarin u de gerelateerde web-App uitvoeren.

* [Implementatie zonder een webproject inschakelen](#convertnolink).

  Configureer een bestaande consoletoepassingsproject te implementeren als een webtaak met zichzelf, met geen koppeling naar een web-project. Gebruik deze optie als u wilt een WebJob in een web-app uitvoeren met zichzelf, terwijl geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources voor Webjobs onafhankelijk van uw web application-resources.

### <a id="convertlink"></a> Inschakelen van automatische WebJobs-implementatie met een webproject

1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **bestaand Project als Azure WebJob**.
   
    ![Bestaand Project als Azure WebJob](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven.
2. In de **projectnaam** vervolgkeuzelijst, selecteer de consoletoepassing project om toe te voegen als een WebJob.
   
    ![Project selecteren in het dialoogvenster Azure-webtaak toevoegen](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Voltooi de [Azure-webtaak toevoegen](#configure) dialoogvenster en klik vervolgens op **OK**. 

### <a id="convertnolink"></a> WebJobs-implementatie zonder een webproject inschakelen
1. Met de rechtermuisknop op het consoletoepassingsproject in **Solution Explorer**, en klik vervolgens op **publiceren als Azure WebJob...** . 
   
    ![Als Azure WebJob publiceren](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven, met het project is geselecteerd de **projectnaam** vak.
2. Voltooi de [Azure-webtaak toevoegen](#configure) in het dialoogvenster en klik vervolgens op **OK**.
   
   De **webpublicatie** wizard wordt weergegeven.  Als u niet onmiddellijk publiceren wilt, moet u de wizard sluiten. De instellingen die u hebt ingevoerd voor worden opgeslagen wanneer u wilt [het project implementeren](#deploy).

## <a id="create"></a>Maak een nieuw project voor het WebJobs-ingeschakeld
Voor het maken van een nieuw project voor WebJobs is ingeschakeld, kunt u gebruik van de Console Application projectsjabloon, maken en inschakelen van WebJobs-implementatie, zoals wordt beschreven [de vorige sectie](#convert). Als alternatief kunt kunt u de WebJobs-nieuw project-sjabloon gebruiken:

* [Gebruik de WebJobs-nieuw project-sjabloon voor een onafhankelijke WebJob](#createnolink)
  
    Maak een project en configureer deze om te implementeren op zichzelf als WebJob, met geen koppeling naar een web-project. Gebruik deze optie als u wilt een WebJob in een web-app uitvoeren met zichzelf, terwijl geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources voor Webjobs onafhankelijk van uw web application-resources.
* [Gebruik de WebJobs-nieuw project-sjabloon voor een webtaak die zijn gekoppeld aan een webproject](#createlink)
  
    Maak een project dat is geconfigureerd voor het automatisch als een WebJob implementeren als een webproject in dezelfde oplossing wordt geïmplementeerd. Gebruik deze optie als u wilt uitvoeren van de webtaak in dezelfde web-app waarin u de gerelateerde web-App uitvoeren.

> [!NOTE]
> De WebJobs nieuwe-projectsjabloon, maken automatisch NuGet-pakketten installeert en bevat de code in *Program.cs* voor de [WebJobs SDK](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Als u niet wilt dat met de WebJobs-SDK, verwijderen of wijzigen de `host.RunAndBlock` -instructie in *Program.cs*.
> 
> 

### <a id="createnolink"></a> Gebruik de WebJobs-nieuw project-sjabloon voor een onafhankelijke WebJob
1. Klik op **bestand** > **nieuw Project**, en klik vervolgens in de **nieuw Project** vak klikt u op dialoogvenster **Cloud**  >   **Azure-webtaak (.NET Framework)**.
   
    ![Dialoogvenster voor nieuw Project webtaak sjabloon weergeven](./media/websites-dotnet-deploy-webjobs/np.png)
2. Volg de aanwijzingen die eerder aan weergegeven [de consoletoepassing project een onafhankelijke WebJobs-project maken](#convertnolink).

### <a id="createlink"></a> Gebruik de WebJobs-nieuw project-sjabloon voor een webtaak die zijn gekoppeld aan een webproject
1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **nieuw Project voor Azure-webtaak**.
   
    ![Nieuwe Azure-webtaak Project menu-item](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven.
2. Voltooi de [Azure-webtaak toevoegen](#configure) in het dialoogvenster en klik vervolgens op **OK**.

## <a id="configure"></a>Het dialoogvenster Azure-webtaak toevoegen
De **Azure-webtaak toevoegen** dialoogvenster kunt u de naam van een webtaak en voert u de instelling voor de webtaak uit. 

![Azure-webtaak dialoogvenster toevoegen](./media/websites-dotnet-deploy-webjobs/aaw2.png)

De velden in dit dialoogvenster overeenkomen met de velden op de **webtaak toevoegen** dialoogvenster van de Azure-portal. Zie voor meer informatie, [achtergrondtaken uitvoeren met WebJobs](web-sites-create-web-jobs.md).

> [!NOTE]
> * Zie voor meer informatie over de implementatie van de opdrachtregel [opdrachtregel inschakelen of continue levering van Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Als u een WebJob implementeert en vervolgens besluit dat u wilt wijzigen van het type van de webtaak en opnieuw implementeren, moet u verwijdert de *webjobs-publiceren settings.json* bestand. Dit maakt Visual Studio de publicatieopties opnieuw, weergeven, zodat u het type van de webtaak kunt wijzigen.
> * Als u een WebJob implementeren en de uitvoeringsmodus later van continue naar niet-doorlopende of vice versa wijzigen, maakt Visual Studio een nieuwe WebJob in Azure wanneer u opnieuw implementeert. Als u andere schema-instellingen wijzigen, maar laat de eigenschap uitvoermodus hetzelfde of schakelen tussen de geplande en op aanvraag, Visual Studio de bestaande taak voor het bijwerken in plaats van een nieuwe maken.
> 
> 

## <a id="publishsettings"></a>webjob-publish-settings.json
Wanneer u een consoletoepassing voor WebJobs-implementatie configureert, Visual Studio installeert de [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-pakket en winkels planningsgegevens in een *webjob-publiceren settings.json*  bestand in het project *eigenschappen* map van het WebJobs-project. Hier volgt een voorbeeld van het bestand:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

U kunt dit bestand rechtstreeks bewerken en Visual Studio biedt IntelliSense. Het bestandsschema wordt opgeslagen op [ https://schemastore.org ](https://schemastore.org/schemas/json/webjob-publish-settings.json) en er kunnen worden weergegeven.  

## <a id="webjobslist"></a>webjobs-list.json
Als u een project WebJobs ingeschakeld aan een web-project koppelt, Visual Studio slaat de naam van het WebJobs-project in een *webjobs list.json* bestand in het webproject *eigenschappen* map. De lijst kan meerdere WebJobs projecten bevatten, zoals wordt weergegeven in het volgende voorbeeld:

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

U kunt dit bestand rechtstreeks bewerken en Visual Studio biedt IntelliSense. Het bestandsschema wordt opgeslagen op [ https://schemastore.org ](https://schemastore.org/schemas/json/webjobs-list.json) en er kunnen worden weergegeven.

## <a id="deploy"></a>Een project WebJobs implementeren
Een WebJobs-project dat u hebt gekoppeld aan een webproject implementeert automatisch met het webproject. Zie voor meer informatie over de implementatie van web project **instructies begeleidt** > **implementeren app** in het linkernavigatievenster.

Voor het implementeren van een WebJobs-project zelf, met de rechtermuisknop op het project in **Solution Explorer** en klikt u op **publiceren als Azure WebJob...** . 

![Als Azure WebJob publiceren](./media/websites-dotnet-deploy-webjobs/paw.png)

Voor een onafhankelijke WebJob hetzelfde **webpublicatie** wizard die wordt gebruikt voor webprojecten wordt weergegeven, maar met minder instellingen die beschikbaar zijn om te wijzigen.
