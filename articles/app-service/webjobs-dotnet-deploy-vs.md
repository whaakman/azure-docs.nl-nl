---
title: Ontwikkelen en implementeren met Visual Studio - Azure WebJobs
description: Informatie over het ontwikkelen en implementeren van Azure WebJobs in Azure App Service met behulp van Visual Studio.
services: app-service
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 02/18/2019
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: ede7e2fe3a2ab4c0dfd4efaea5ec789924968194
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750154"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Ontwikkelen en implementeren met Visual Studio - Azure App Service WebJobs

In dit artikel wordt uitgelegd hoe u Visual Studio gebruiken voor het implementeren van een consoletoepassingsproject in een WebApp in [App Service](overview.md) als een [Azure-webtaak](https://go.microsoft.com/fwlink/?LinkId=390226). Voor informatie over hoe u WebJobs implementeren met behulp van de [Azure-portal](https://portal.azure.com), Zie [achtergrondtaken uitvoeren met WebJobs](webjobs-create.md).

U kunt meerdere WebJobs publiceren naar één web-app. Zorg ervoor dat elke WebJob in een web-app een unieke naam heeft.

Versie 3.x van de [Azure WebJobs SDK](webjobs-sdk-how-to.md) kunt ontwikkelen van webtaken die worden uitgevoerd als .NET Core-apps, terwijl u versie 2.x ondersteunt alleen het .NET Framework. De manier waarop u een project WebJobs implementeren is ten opzichte van .NET Framework die verschillende .NET Core-projecten.

## <a name="webjobs-as-net-core-console-apps"></a>WebJobs als .NET Core-console-apps

Bij het gebruik van versie 3.x van de WebJobs, kunt u maken en publiceren van WebJobs als .NET Core-console-apps. Zie voor stapsgewijze instructies voor het maken en publiceren van een .NET Core-consoletoepassing naar Azure als een WebJob [aan de slag met de Azure WebJobs-SDK voor verwerking op de achtergrond gebeurtenisgestuurde](webjobs-sdk-get-started.md).

> [!NOTE]
> .NET core WebJobs kan niet worden gekoppeld met webprojecten. Als u de webtaak met een web-app implementeren wilt, moet u [webtaak als een .NET Framework-consoletoepassing maken](#webjobs-as-net-framework-console-apps).  

### <a name="deploy-to-azure-app-service"></a>Implementeren in Azure App Service

Een .NET Core-webtaak in App Service publiceren vanuit Visual Studio maakt gebruik van de dezelfde hulpmiddelen als het publiceren van een ASP.NET Core-app.

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)] 

### <a name="webjob-types"></a>WebJob-typen

Standaard wordt een webtaak gepubliceerd vanuit een .NET Core console-project wordt alleen uitgevoerd als geactiveerd of op aanvraag. U kunt ook het project bijwerken [uitgevoerd volgens een schema](#scheduled-execution) continu worden uitgevoerd.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

#### <a name="scheduled-execution"></a>Geplande uitvoering

Wanneer u een .NET Core-consoletoepassing naar Azure publiceert een nieuwe *settings.job* bestand wordt toegevoegd aan het project. Dit bestand gebruiken om een planning worden uitgevoerd voor de webtaak. Zie voor meer informatie, [plannen van een geactiveerde WebJob](#scheduling-a-triggered-webjob).

#### <a name="continuous-execution"></a>Continue uitvoering

U kunt Visual Studio gebruiken om te wijzigen van de webtaak om uit te voeren continu wanneer AlwaysOn is ingeschakeld in Azure.

1. Als u dit nog niet hebt gedaan, [publiceert u het project naar Azure](#deploy-to-azure-app-service).

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**.

1. In de **publiceren** tabblad **instellingen**. 

1. In de **profielinstellingen** dialoogvenster kiezen **doorlopend** voor **webtaak Type**, en kies **opslaan**.

    ![Dialoogvenster Instellingen voor een WebJob publiceren](./media/webjobs-dotnet-deploy-vs/publish-settings.png)

1. Selecteer **publiceren** om opnieuw te publiceren van de webtaak met de bijgewerkte instellingen.

## <a name="webjobs-as-net-framework-console-apps"></a>WebJobs als .NET Framework-consoletoepassingen  

Wanneer Visual Studio een project .NET Framework-consoletoepassing WebJobs-functionaliteit is geïmplementeerd, worden twee taken uitgevoerd:

* Runtime-bestanden worden gekopieerd naar de juiste map in de web-app (*App_Data/jobs/continuous* voor doorlopende webtaken en *App_Data/jobs/triggered* voor geplande of on-demand WebJobs).
* Stelt u [Azure Scheduler](https://docs.microsoft.com/azure/scheduler/) taken voor webtaken die zijn gepland om te worden uitgevoerd op bepaalde tijdstippen. (Dit is niet nodig voor doorlopende webtaken.)

Een project WebJobs-functionaliteit heeft de volgende items toegevoegd aan het:

* De [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-pakket.
* Een [webjob-publiceren settings.json](#publishsettings) -bestand met de implementatie en de scheduler-instellingen. 

![Diagram van wat wordt toegevoegd aan een Console-App implementeren als een WebJob inschakelen](./media/webjobs-dotnet-deploy-vs/convert.png)

U kunt deze items toevoegen aan een bestaande Console Application-project of een sjabloon gebruiken om een nieuwe consoletoepassing WebJobs ingeschakeld-project te maken. 

U kunt een project implementeren als een WebJob zelfstandig of koppelen aan een webproject, zodat deze automatisch wordt geïmplementeerd wanneer u het webproject implementeren. Om een koppeling projecten, Visual Studio bevat de naam van het project WebJobs ingeschakeld in een [webjobs list.json](#webjobslist) bestand in de webproject.

![Diagram van de webtaak project koppelen aan de web-project](./media/webjobs-dotnet-deploy-vs/link.png)

### <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2015 gebruikt, installeert u de [Azure SDK voor .NET (Visual Studio 2015)](https://azure.microsoft.com/downloads/).

Als u Visual Studio 2017 gebruikt, installeert u de [Azure-ontwikkelworkload](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

### <a id="convert"></a> WebJobs-implementatie voor een bestaande consoletoepassingsproject inschakelen

U hebt hiervoor twee opties:

* [Inschakelen van automatische implementatie met een webproject](#convertlink).

  Een bestaande consoletoepassingsproject zodanig configureren dat deze automatisch wordt geïmplementeerd als een WebJob wanneer u een webproject implementeert. Gebruik deze optie als u wilt uitvoeren van de webtaak in dezelfde web-app waarin u de gerelateerde web-App uitvoeren.

* [Implementatie zonder een webproject inschakelen](#convertnolink).

  Configureer een bestaande consoletoepassingsproject te implementeren als een webtaak met zichzelf, met geen koppeling naar een web-project. Gebruik deze optie als u wilt een WebJob in een web-app uitvoeren met zichzelf, terwijl geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources voor Webjobs onafhankelijk van uw web application-resources.

#### <a id="convertlink"></a> Inschakelen van automatische WebJobs-implementatie met een webproject

1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **bestaand Project als Azure WebJob**.
   
    ![Bestaand Project als Azure WebJob](./media/webjobs-dotnet-deploy-vs/eawj.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven.
2. In de **projectnaam** vervolgkeuzelijst, selecteer de consoletoepassing project om toe te voegen als een WebJob.
   
    ![Project selecteren in het dialoogvenster Azure-webtaak toevoegen](./media/webjobs-dotnet-deploy-vs/aaw1.png)
3. Voltooi de [Azure-webtaak toevoegen](#configure) dialoogvenster en klik vervolgens op **OK**. 

#### <a id="convertnolink"></a> WebJobs-implementatie zonder een webproject inschakelen
1. Met de rechtermuisknop op het consoletoepassingsproject in **Solution Explorer**, en klik vervolgens op **publiceren als Azure WebJob...** . 
   
    ![Als Azure WebJob publiceren](./media/webjobs-dotnet-deploy-vs/paw.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven, met het project is geselecteerd de **projectnaam** vak.
2. Voltooi de [Azure-webtaak toevoegen](#configure) in het dialoogvenster en klik vervolgens op **OK**.
   
   De **webpublicatie** wizard wordt weergegeven.  Als u niet onmiddellijk publiceren wilt, moet u de wizard sluiten. De instellingen die u hebt ingevoerd voor worden opgeslagen wanneer u wilt [het project implementeren](#deploy).

### <a id="create"></a>Maak een nieuw project voor het WebJobs-ingeschakeld
Voor het maken van een nieuw project voor WebJobs is ingeschakeld, kunt u gebruik van de Console Application projectsjabloon, maken en inschakelen van WebJobs-implementatie, zoals wordt beschreven [de vorige sectie](#convert). Als alternatief kunt kunt u de WebJobs-nieuw project-sjabloon gebruiken:

* [Gebruik de WebJobs-nieuw project-sjabloon voor een onafhankelijke WebJob](#createnolink)
  
    Maak een project en configureer deze om te implementeren op zichzelf als WebJob, met geen koppeling naar een web-project. Gebruik deze optie als u wilt een WebJob in een web-app uitvoeren met zichzelf, terwijl geen webtoepassing die wordt uitgevoerd in de web-app. Het is raadzaam om dit te doen om te kunnen schalen van uw resources voor Webjobs onafhankelijk van uw web application-resources.
* [Gebruik de WebJobs-nieuw project-sjabloon voor een webtaak die zijn gekoppeld aan een webproject](#createlink)
  
    Maak een project dat is geconfigureerd voor het automatisch als een WebJob implementeren als een webproject in dezelfde oplossing wordt geïmplementeerd. Gebruik deze optie als u wilt uitvoeren van de webtaak in dezelfde web-app waarin u de gerelateerde web-App uitvoeren.

> [!NOTE]
> De WebJobs nieuwe-projectsjabloon, maken automatisch NuGet-pakketten installeert en bevat de code in *Program.cs* voor de [WebJobs SDK](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Als u niet wilt dat met de WebJobs-SDK, verwijderen of wijzigen de `host.RunAndBlock` -instructie in *Program.cs*.
> 
> 

#### <a id="createnolink"></a> Gebruik de WebJobs-nieuw project-sjabloon voor een onafhankelijke WebJob
1. Klik op **bestand** > **nieuw Project**, en klik vervolgens in de **nieuw Project** vak klikt u op dialoogvenster **Cloud**  >   **Azure-webtaak (.NET Framework)**.
   
    ![Dialoogvenster voor nieuw Project webtaak sjabloon weergeven](./media/webjobs-dotnet-deploy-vs/np.png)
2. Volg de aanwijzingen die eerder aan weergegeven [de consoletoepassing project een onafhankelijke WebJobs-project maken](#convertnolink).

#### <a id="createlink"></a> Gebruik de WebJobs-nieuw project-sjabloon voor een webtaak die zijn gekoppeld aan een webproject
1. Met de rechtermuisknop op het webproject in **Solution Explorer**, en klik vervolgens op **toevoegen** > **nieuw Project voor Azure-webtaak**.
   
    ![Nieuwe Azure-webtaak Project menu-item](./media/webjobs-dotnet-deploy-vs/nawj.png)
   
    De [Azure-webtaak toevoegen](#configure) in het dialoogvenster wordt weergegeven.
2. Voltooi de [Azure-webtaak toevoegen](#configure) in het dialoogvenster en klik vervolgens op **OK**.

### <a id="configure"></a>Het dialoogvenster Azure-webtaak toevoegen
De **Azure-webtaak toevoegen** dialoogvenster kunt u de naam van een webtaak en voert u de instelling voor de webtaak uit. 

![Azure-webtaak dialoogvenster toevoegen](./media/webjobs-dotnet-deploy-vs/aaw2.png)

De velden in dit dialoogvenster overeenkomen met de velden op de **webtaak toevoegen** dialoogvenster van de Azure-portal. Zie voor meer informatie, [achtergrondtaken uitvoeren met WebJobs](webjobs-create.md).

> [!NOTE]
> * Zie voor meer informatie over de implementatie van de opdrachtregel [opdrachtregel inschakelen of continue levering van Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Als u een WebJob implementeert en vervolgens besluit dat u wilt wijzigen van het type van de webtaak en opnieuw implementeren, moet u verwijdert de *webjobs-publiceren settings.json* bestand. Dit maakt Visual Studio de publicatieopties opnieuw, weergeven, zodat u het type van de webtaak kunt wijzigen.
> * Als u een WebJob implementeren en de uitvoeringsmodus later van continue naar niet-doorlopende of vice versa wijzigen, maakt Visual Studio een nieuwe WebJob in Azure wanneer u opnieuw implementeert. Als u andere schema-instellingen wijzigen, maar laat de eigenschap uitvoermodus hetzelfde of schakelen tussen de geplande en op aanvraag, Visual Studio de bestaande taak voor het bijwerken in plaats van een nieuwe maken.
> 
> 

### <a id="publishsettings"></a>webjob-publish-settings.json
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

### <a id="webjobslist"></a>webjobs-list.json
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

### <a id="deploy"></a>Een project WebJobs implementeren
Een WebJobs-project dat u hebt gekoppeld aan een webproject implementeert automatisch met het webproject. Zie voor meer informatie over de implementatie van web project **instructies begeleidt** > **implementeren app** in het linkernavigatievenster.

Voor het implementeren van een WebJobs-project zelf, met de rechtermuisknop op het project in **Solution Explorer** en klikt u op **publiceren als Azure WebJob...** . 

![Als Azure WebJob publiceren](./media/webjobs-dotnet-deploy-vs/paw.png)

Voor een onafhankelijke WebJob hetzelfde **webpublicatie** wizard die wordt gebruikt voor webprojecten wordt weergegeven, maar met minder instellingen die beschikbaar zijn om te wijzigen.

## <a name="scheduling-a-triggered-webjob"></a>Een geactiveerde WebJob plannen

Maakt gebruik van WebJobs een *settings.job* bestand om te bepalen wanneer een WebJob wordt uitgevoerd. Dit bestand gebruiken om een planning worden uitgevoerd voor de webtaak. Het volgende voorbeeld wordt elk uur uitgevoerd vanaf 9: 00 tot 17: 00 uur:

```json
{
    "schedule": "0 0 9-17 * * *"
}
```

Dit bestand moet worden gevonden in de hoofdmap van de WebJobs-map, langs zijde van de webtaak script, zoals `wwwroot\app_data\jobs\triggered\{job name}` of `wwwroot\app_data\jobs\continuous\{job name}`. Wanneer u een WebJob vanuit Visual Studio implementeert, Markeer uw `settings.job` eigenschappen als het bestand **kopiëren indien nieuwer**. 

Wanneer u [een webtaak maken vanuit Azure portal](webjobs-create.md), wordt de settings.job-bestand voor u gemaakt.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

### <a name="cron-expressions"></a>CRON-expressies

WebJobs maakt gebruik van de dezelfde CRON-expressies voor het plannen van de timertrigger in Azure Functions. Zie voor meer informatie over CRON-ondersteuning, de [naslagartikel voor timer trigger](../azure-functions/functions-bindings-timer.md#cron-expressions).

### <a name="settingjob-reference"></a>Setting.job verwijzing

De volgende instellingen worden ondersteund door WebJobs:

| **Instelling** | **Type**  | **Beschrijving** |
| ----------- | --------- | --------------- |
| `is_in_place` | Alle | Kan de taak om uit te voeren in plaats zonder het eerst wordt gekopieerd naar een tijdelijke map. Zie voor meer informatie, [WebJobs werkmap](https://github.com/projectkudu/kudu/wiki/WebJobs#webjob-working-directory). |
| `is_singleton` | Doorlopend | Alleen de webtaken worden uitgevoerd op een enkele instantie als uitgeschaald. Zie voor meer informatie, [een continue taak instellen als singleton](https://github.com/projectkudu/kudu/wiki/WebJobs-API#set-a-continuous-job-as-singleton). |
| `schedule` | Geactiveerd | De webtaak uitvoeren volgens een schema op basis van een CRON. Zie voor meer informatie, de [naslagartikel voor timer trigger](../azure-functions/functions-bindings-timer.md#cron-expressions). |
| `stopping_wait_time`| Alle | Biedt controle over het afsluitgedrag. Zie voor meer informatie, [correct afsluiten](https://github.com/projectkudu/kudu/wiki/WebJobs#graceful-shutdown). |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de WebJobs SDK](webjobs-sdk-how-to.md)
