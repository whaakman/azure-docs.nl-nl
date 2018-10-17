---
title: Achtergrondtaken uitvoeren met WebJobs in Azure App Service
description: Informatie over het gebruik van WebJobs voor achtergrondtaken uitvoeren in Azure App Service WebApps, API-apps of mobiele apps.
services: app-service
documentationcenter: ''
author: ggailey777
manager: jeconnoc
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: glenga;msangapu;david.ebbo;suwatch;pbatum;naren.soni;
ms.openlocfilehash: 901cf32557e0a437e938ceb50ecd500c69c8c3be
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364027"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Achtergrondtaken uitvoeren met WebJobs in Azure App Service

## <a name="overview"></a>Overzicht
WebJobs is een functie van [Azure App Service](https://docs.microsoft.com/azure/app-service/) waarmee u kunt een programma of script binnen dezelfde context als een web-app, API-app of mobiele app uitvoeren. Er is geen extra kosten gebruik van WebJobs.

Dit artikel wordt beschreven hoe u WebJobs implementeren met behulp van de [Azure-portal](https://portal.azure.com) voor het uploaden van een uitvoerbaar bestand of script. Zie voor meer informatie over het ontwikkelen en WebJobs implementeren met behulp van Visual Studio [WebJobs toepassen met Visual Studio](websites-dotnet-deploy-webjobs.md).

De Azure WebJobs SDK kan worden gebruikt met WebJobs om veel programming taken te vereenvoudigen. Zie voor meer informatie, [wat de WebJobs SDK is](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure Functions biedt een andere manier om uit te voeren programma's en scripts. Zie voor een vergelijking tussen de functies en WebJobs [kiezen tussen Flow, Logic Apps, Functions en WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>WebJob-typen

De volgende tabel beschrijft de verschillen tussen *continue* en *geactiveerd* WebJobs.


|Continu  |Geactiveerd  |
|---------|---------|
| Begint onmiddellijk wanneer de webtaak wordt gemaakt. Het programma of script, doet om te voorkomen dat de taak beëindigen, gewoonlijk zijn werk binnen een oneindige lus. Als de taak eindigt, kunt u deze opnieuw starten. | Wordt alleen gestart als geactiveerd handmatig of volgens een schema. |
| Wordt uitgevoerd op alle exemplaren die de web-app wordt uitgevoerd op. U kunt eventueel de webtaak beperken tot één exemplaar. |Wordt uitgevoerd op een enkele instantie die door Azure wordt geselecteerd voor de taakverdeling.|
| Biedt ondersteuning voor foutopsporing op afstand. | Biedt geen ondersteuning voor foutopsporing op afstand.|

> [!NOTE]
> Een web-app kan een time-out na 20 minuten van inactiviteit. Alleen aanvragen naar de werkelijke web-app opnieuw instellen van de timer. Configuratie van de app bekijken in Azure portal of die aanvragen verzonden naar de site geavanceerde hulpmiddelen (https:// < app_name >. scm.azurewebsites.net) niet herstellen voor de timer. Als uw app wordt uitgevoerd op een continue of geplande webtaken inschakelen **Always On** om ervoor te zorgen dat de webtaken betrouwbaar uit te voeren. Deze functie is alleen beschikbaar in de Basic, Standard en Premium [Prijscategorieën](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="acceptablefiles"></a>Ondersteunde bestandstypen voor scripts of programma 's

De volgende bestandstypen worden ondersteund:

* .cmd, .bat, .exe (met behulp van Windows-cmd)
* .ps1 (met behulp van PowerShell)
* .sh (met Bash)
* .php (met behulp van PHP)
* .PY (met behulp van Python)
* .js (met behulp van Node.js)
* JAR (met behulp van Java)

## <a name="CreateContinuous"></a> Een continue WebJob maken

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![WebJobs selecteren](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** weergeeft, schakelt **toevoegen**.

    ![WebJob-pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myContinuousWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'. |
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het bestand uitvoerbaar bestand of script, evenals alle ondersteunende bestanden die nodig zijn voor het programma of script uitvoeren. De ondersteunde typen voor uitvoerbaar bestand of script bestand worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Continu | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Schalen** | Meerdere exemplaren | Alleen beschikbaar voor doorlopende webtaken. Bepaalt of het programma of script wordt uitgevoerd op alle exemplaren of slechts één exemplaar. De mogelijkheid om te worden uitgevoerd op meerdere exemplaren niet van toepassing op gratis of gedeeld [Prijscategorieën](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). | 

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Als u wilt stoppen of opnieuw starten van een doorlopende webtaak, met de rechtermuisknop op de webtaak in de lijst en klikt u op **stoppen** of **Start**.

    ![Een doorlopende webtaak stoppen](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="CreateOnDemand"></a> Een handmatig geactiveerde-webtaak maken

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![WebJobs selecteren](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** weergeeft, schakelt **toevoegen**.

    ![WebJob-pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myTriggeredWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'.|
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het bestand uitvoerbaar bestand of script, evenals alle ondersteunende bestanden die nodig zijn voor het programma of script uitvoeren. De ondersteunde typen voor uitvoerbaar bestand of script bestand worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Geactiveerd | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Triggers** | Handmatig | |

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. Als u wilt de webtaak uitvoeren, met de rechtermuisknop op de naam in de lijst en klikt u op **uitvoeren**.
   
    ![Webtaak uitvoeren](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateScheduledCRON"></a> Een geplande webtaak maken

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![WebJobs selecteren](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** weergeeft, schakelt **toevoegen**.

   ![WebJob-pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myScheduledWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'. |
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het bestand uitvoerbaar bestand of script, evenals alle ondersteunende bestanden die nodig zijn voor het programma of script uitvoeren. De ondersteunde typen voor uitvoerbaar bestand of script bestand worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Geactiveerd | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Triggers** | Gepland | Voor de planning betrouwbaar werkt, moet u de functie altijd op inschakelen. Altijd beschikbaar is alleen beschikbaar in de Basic, Standard en Premium-Prijscategorieën.|
   | **CRON-expressie** | 0 0/20 * * * * | [CRON-expressies](#cron-expressions) worden beschreven in de volgende sectie. |

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="cron-expressions"></a>CRON-expressies

Kunt u een [CRON-expressie](../azure-functions/functions-bindings-timer.md#cron-expressions) in de portal of een `settings.job` bestand in de hoofdmap van de webtaak *.zip* -bestand, zoals in het volgende voorbeeld:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

> [!NOTE]
> Wanneer u een WebJob vanuit Visual Studio implementeert, Markeer uw `settings.job` eigenschappen als het bestand **kopiëren indien nieuwer**.

## <a name="ViewJobHistory"></a> De geschiedenis van de taak weergeven

1. Selecteer de webtaak die u wilt zien van de geschiedenis voor, en selecteer vervolgens de **logboeken** knop.
   
   ![Knop voor logboeken](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. In de **webtaak Details** pagina, selecteert u een tijd om de details voor een run te bekijken.
   
   ![WebJob-Details](./media/web-sites-create-web-jobs/webjobdetails.png)

3. In de **Details uitvoering van webtaak** weergeeft, schakelt **in-/ uitschakelen uitvoer** de tekst van de inhoud van het logboek wilt zien.
   
    ![Webtaak details van de uitvoering](./media/web-sites-create-web-jobs/webjobrundetails.png)

   De uitvoertekst in een afzonderlijke browservenster Selecteer **downloaden**. Voor het downloaden van de tekst zelf, met de rechtermuisknop op **downloaden** en opties van uw browser gebruiken om op te slaan van de inhoud van het bestand.
   
5. Selecteer de **WebJobs** breadcrumb-koppeling aan de bovenkant van de pagina te gaan naar een lijst van WebJobs.

    ![Webtaak breadcrumb](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![Lijst met WebJobs in de geschiedenis van dashboard](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="NextSteps"></a> Volgende stappen

De Azure WebJobs SDK kan worden gebruikt met WebJobs om veel programming taken te vereenvoudigen. Zie voor meer informatie, [wat de WebJobs SDK is](https://github.com/Azure/azure-webjobs-sdk/wiki).
