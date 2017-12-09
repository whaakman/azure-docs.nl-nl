---
title: Doorlopende bewaking van uw DevOps-release-pijplijn met VSTS en Azure Application Insights | Microsoft Docs
description: Bevat instructies voor het instellen van snel doorlopende bewaking met Application Insights
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: article
manager: carmonm
ms.openlocfilehash: 5bfbdd0033f966422a84071a694845627827f016
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Continue bewaking toevoegen aan uw pijplijn release

Visual Studio Team Services (VSTS) kan worden geïntegreerd met Azure Application Insights om toe te staan doorlopende bewaking van uw DevOps release pijplijn gedurende de ontwikkelingscyclus software. 

VSTS biedt nu ondersteuning voor doorlopende bewaking waarbij release pijplijnen bewakingsgegevens van Application Insights en andere Azure-resources kunnen opnemen. Wanneer een waarschuwing Application Insights wordt gedetecteerd, wordt de implementatie kan blijven gated of teruggedraaid terug tot de waarschuwing is opgelost. Als alle controles doorgeven, kunnen implementaties doorgaan automatisch test helemaal naar productie zonder handmatige interventie nodig. 

## <a name="configure-continuous-monitoring"></a>Continue bewaking configureren

1. Selecteer een bestaand VSTS-Project.

2. Beweeg de muisaanwijzer over **bouwen en de vrijgave van** > Selecteer **Releases** > Klik op de **plusteken** > **maken release definitie** > Zoeken naar **bewaking** > **-Azure App Service-implementatie met doorlopende bewaking.**

   ![Nieuwe VSTS Release-definitie](.\media\app-insights-continuous-monitoring\001.png)

3. Klik op **toepassen.**

4. Naast de rood uitroepteken selecteert u de tekst in blauw weergegeven en **omgevingstaken weergeven.**

   ![Omgeving-taken weergeven](.\media\app-insights-continuous-monitoring\002.png)

   Er verschijnt een configuratie-venster, gebruik de volgende tabel om de invoervelden in te vullen.

    | Parameter        | Waarde |
   | ------------- |:-----|
   | **De omgevingsnaam van de**      | Naam voor de definitie vrijgeven omgeving |
   | **Azure-abonnement** | Vervolgkeuzelijst gevuld met de Azure-abonnementen gekoppeld aan het account VSTS|
   | **Naam van App Service** | Handmatige invoer van een nieuwe waarde mogelijk zijn vereist voor dit veld afhankelijk van andere selecties |
   | **Resourcegroep**    | Vervolgkeuzelijst gevuld met beschikbare resourcegroepen |
   | **Resourcenaam van Application Insights** | Vervolgkeuzelijst wordt gevuld met alle Application Insights-resources die met de eerder geselecteerde resourcegroep overeenkomen.

5. Selecteer **Configure Application Insights-waarschuwingen**

6. Standaardregels voor waarschuwing, selecteert u **opslaan** > Voer een korte beschrijving > klikt u op **OK**

## <a name="modify-alert-rules"></a>Regels voor waarschuwingen wijzigen

1. Voor het wijzigen van de vooraf gedefinieerde instellingen voor waarschuwingen, klik in het vak met **weglatingstekens...**  rechts van **waarschuwing regels.**

   (Out of box vier waarschuwingsregels aanwezig zijn: beschikbaarheid, mislukte verzoeken, serverreactietijd, Server-uitzonderingen.)

2. Klik op het symbool-omlaag naast **beschikbaarheid.**

3. Wijzigen van de beschikbaarheid **drempelwaarde** om te voldoen aan uw vereisten voor het niveau van service.

   ![Waarschuwing voor wijzigen](.\media\app-insights-continuous-monitoring\003.png)

4. Selecteer **OK** > **opslaan** > Voer een korte beschrijving > klikt u op **OK.**

## <a name="add-deployment-conditions"></a>Implementatie van voorwaarden toevoegen

1. Klik op **pijplijn** > Selecteer de **Pre** of **na de implementatie voorwaarden** symbool, afhankelijk van de fase waarvoor een continue bewaking gate.

   ![Voorafgaand aan de implementatie-voorwaarden](.\media\app-insights-continuous-monitoring\004.png)

2. Stel **poorten** naar **ingeschakeld** > **goedkeuring poorten**> klikt u op **toevoegen.**

3. Selecteer **Azure Monitor** (deze optie kunt u de toegang tot waarschuwingen zowel van Azure Monitor en Application Insights)

    ![Azure Monitor](.\media\app-insights-continuous-monitoring\005.png)

4. Voer een **poorten time-out** waarde.

5. Voer een **steekproef nemen Interval.**

## <a name="deployment-gate-status-logs"></a>Status van de implementatielogboeken-gate

Als u de implementatie poorten toevoegt, wordt in een waarschuwing in Application Insights die groter is dan de eerder gedefinieerde drempelwaarde uw implementatie van ongewenste release promotie beschermt. Als de waarschuwing opgelost is, kan de implementatie automatisch voortgezet.

Als u wilt dit gedrag observeren, selecteer **Releases** > Release met de rechtermuisknop op de naam **openen** > **Logboeken.**

![Logboeken](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over VSTS bouwen en Release probeer deze [snelstartgidsen.](https://docs.microsoft.com/vsts/build-release/)
