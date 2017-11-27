---
title: Achtergrondtaken uitvoeren met WebJobs in Azure App Service
description: Informatie over hoe u webtaken achtergrondtaken uitvoeren in Azure App Service WebApps, API-apps of mobiele apps.
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 91839d8f547340d55f6badb3350a393a48a13c7d
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Achtergrondtaken uitvoeren met WebJobs in Azure App Service

## <a name="overview"></a>Overzicht
WebJobs is een functie van [Azure App Service](https://docs.microsoft.com/azure/app-service/) waarmee u een programma of script uitvoeren in de context als een web-app, API-app of mobiele app. Er is geen extra kosten WebJobs gebruiken.

Dit artikel laat zien hoe u webtaken implementeren met behulp van de [Azure-portal](https://portal.azure.com) voor het uploaden van een uitvoerbaar bestand of script. Zie voor meer informatie over het ontwikkelen en implementeren van WebJobs met behulp van Visual Studio [implementeren WebJobs met Visual Studio](websites-dotnet-deploy-webjobs.md).

De Azure WebJobs SDK kan worden gebruikt met de WebJobs veel programmeringstaken te vereenvoudigen. Zie voor meer informatie [wat de WebJobs SDK is](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure Functions bevat een andere manier om uit te voeren programma's en scripts. Zie voor een vergelijking tussen de functies en WebJobs [Kies tussen stroom, Logic Apps, functies en WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>Webtaak typen

De volgende tabel beschrijft de verschillen tussen *continue* en *geactiveerd* WebJobs.


|Doorlopend  |Geactiveerd  |
|---------|---------|
| Start zodra de webtaak wordt gemaakt. Het programma of script, doet om te voorkomen dat de taak beëindigen, gewoonlijk de taken binnen een oneindige lus. Als de taak eindigt, kunt u het opnieuw opstarten. | Wordt alleen gestart als geactiveerd handmatig of volgens een schema. |
| Wordt uitgevoerd op alle exemplaren die op de web-app wordt uitgevoerd. U kunt eventueel de webtaak beperken tot één exemplaar. |Wordt uitgevoerd op één instantie die door Azure wordt geselecteerd voor taakverdeling.|
| Biedt ondersteuning voor foutopsporing op afstand. | Biedt geen ondersteuning voor foutopsporing op afstand.|

> [!NOTE]
> Een web-app kan een time-out na 20 minuten van inactiviteit. Alleen aanvragen naar de site scm (implementatie) of naar de web-app-pagina's in de portal opnieuw instellen van de timer. Aanvragen voor de werkelijke site opnieuw niet de timer voor het. Als uw app wordt uitgevoerd op een continue of geplande webtaken inschakelen **altijd op** om ervoor te zorgen dat de WebJobs betrouwbaar worden uitgevoerd. Deze functie is alleen beschikbaar in de Basic, Standard en Premium [Prijscategorieën](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="acceptablefiles"></a>Ondersteunde bestandstypen voor scripts of programma 's

De volgende bestandstypen worden ondersteund:

* .cmd, .bat, .exe (met behulp van Windows cmd)
* .ps1 (met PowerShell)
* .sh (met behulp van Bash)
* .php (met behulp van PHP)
* .PY (met behulp van Python)
* .js (met behulp van Node.js)
* JAR (met Java)

## <a name="CreateContinuous"></a>Maken van een doorlopende webtaak

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![Selecteer WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** pagina **toevoegen**.

    ![Webtaak pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myContinuousWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'. |
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het uitvoerbare bestand of script bestand, evenals alle ondersteunende bestanden die nodig zijn voor het uitvoeren van het programma of script. De ondersteunde bestandstypen uitvoerbaar bestand of script worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Doorlopend | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Schalen** | Meerdere exemplaren | Alleen beschikbaar voor doorlopende webtaken. Hiermee wordt bepaald of het programma of script wordt uitgevoerd op alle exemplaren of slechts één exemplaar. De optie uit te voeren op meerdere exemplaren niet van toepassing op de Free of Shared [Prijscategorieën](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). | 

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Als u wilt stoppen of opnieuw starten van een doorlopende webtaak, met de rechtermuisknop op de webtaak in de lijst en klikt u op **stoppen** of **Start**.

    ![Stoppen van een doorlopende webtaak](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="CreateOnDemand"></a>Maken van een webtaak handmatig triggered

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![Selecteer WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** pagina **toevoegen**.

    ![Webtaak pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myTriggeredWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'.|
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het uitvoerbare bestand of script bestand, evenals alle ondersteunende bestanden die nodig zijn voor het uitvoeren van het programma of script. De ondersteunde bestandstypen uitvoerbaar bestand of script worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Geactiveerd | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Triggers** | Handmatig | |

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. De webtaak wordt uitgevoerd, met de rechtermuisknop op de naam ervan in de lijst en klikt u op **uitvoeren**.
   
    ![Webtaak uitvoeren](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateScheduledCRON"></a>Een geplande webtaak maken

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **App Service** pagina van uw App Service-web-app, API-app of mobiele app.

2. Selecteer **WebJobs**.

   ![Selecteer WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. In de **WebJobs** pagina **toevoegen**.

   ![Webtaak pagina](./media/web-sites-create-web-jobs/wjblade.png)

3. Gebruik de **webtaak toevoegen** instellingen zoals opgegeven in de tabel.

   ![Webtaak pagina toevoegen](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Instelling      | Voorbeeldwaarde   | Beschrijving  |
   | ------------ | ----------------- | ------------ |
   | **Naam** | myScheduledWebJob | Een naam die uniek is binnen een App Service-app. Moet beginnen met een letter of cijfer en mag geen andere waarde dan speciale tekens bevatten '-' en '_'. |
   | **Bestand uploaden** | ConsoleApp.zip | Een *.zip* -bestand met het uitvoerbare bestand of script bestand, evenals alle ondersteunende bestanden die nodig zijn voor het uitvoeren van het programma of script. De ondersteunde bestandstypen uitvoerbaar bestand of script worden vermeld in de [ondersteunde bestandstypen](#acceptablefiles) sectie. |
   | **Type** | Geactiveerd | De [webtaak typen](#webjob-types) eerder in dit artikel worden beschreven. |
   | **Triggers** | Gepland | Schakel de functie altijd op voor de planning op betrouwbare wijze werken. AlwaysOn is alleen beschikbaar in de Basic, Standard en Premium-prijscategorie.|
   | **CRON-expressie** | 0 0/20 * * * * | [CRON-expressies](#cron-expressions) in de volgende sectie worden beschreven. |

4. Klik op **OK**.

   De nieuwe webtaak wordt weergegeven op de **WebJobs** pagina.

   ![Lijst met WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="cron-expressions"></a>CRON-expressies

Een [CRON expressie](https://en.wikipedia.org/wiki/Cron) bestaat uit zes velden: `{second} {minute} {hour} {day} {month} {day of the week}`.  Hier volgen enkele voorbeelden:

* Om de 15 minuten:`0 */15 * * * *`
* Elk uur (dat wil zeggen, wanneer het aantal minuten 0 is):`0 0 * * * *` 
* Elk uur uit 9: 00 uur tot 5 PM:`0 0 9-17 * * *` 
* Om 9:30 AM dagelijks:`0 30 9 * * *`
* Om 9:30 AM elke weekdag:`0 30 9 * * 1-5`

U kunt de CRON-expressie invoeren in de portal of omvatten een `settings.job` bestand in de hoofdmap van uw webtaak *.zip* -bestand, zoals in het volgende voorbeeld:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

> [!NOTE]
> Wanneer u een webtaak vanuit Visual Studio implementeert, markeer de `settings.job` bestandseigenschappen als **kopiëren indien nieuwer**.

## <a name="ViewJobHistory"></a>De Taakgeschiedenis weergeven

1. Selecteer de webtaak die u wilt zien van de geschiedenis voor en selecteer vervolgens de **logboeken** knop.
   
   ![Knop voor logboeken](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. In de **webtaak Details** pagina, selecteert u een tijd om de details voor een run te bekijken.
   
   ![Webtaak Details](./media/web-sites-create-web-jobs/webjobdetails.png)

3. In de **Details uitvoering van webtaak** pagina **wisselknop uitvoer** om te zien van de tekst van de inhoud van het logboek.
   
    ![Web-job details uitvoering](./media/web-sites-create-web-jobs/webjobrundetails.png)

   Overzicht van de uitvoertekst in een apart browservenster Selecteer **downloaden**. Voor het downloaden van de tekst zelf, met de rechtermuisknop op **downloaden** en opties van uw browser met inhoud van het bestand opslaan.
   
5. Selecteer de **WebJobs** breadcrumb koppeling aan de bovenkant van de pagina te gaan naar een lijst met WebJobs.

    ![Webtaak breadcrumb](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![Lijst met WebJobs in geschiedenis dashboard](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="NextSteps"></a> Volgende stappen

De Azure WebJobs SDK kan worden gebruikt met de WebJobs veel programmeringstaken te vereenvoudigen. Zie voor meer informatie [wat de WebJobs SDK is](https://github.com/Azure/azure-webjobs-sdk/wiki).
