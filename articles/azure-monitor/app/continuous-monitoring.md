---
title: Doorlopende bewaking van uw DevOps-release-pijplijn met Azure DevOps en Azure Application Insights | Microsoft Docs
description: Hier vindt u instructies voor het snel instellen van doorlopende bewaking met Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 41999defb01e024773b6364f169a1ce3b1377237
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056930"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Doorlopende bewaking toevoegen aan uw release-pijplijn

Azure DevOps-Services kan worden geïntegreerd met Azure Application Insights om toe te staan de voortdurende bewaking van uw DevOps-release-pijplijn gedurende de ontwikkelfase software. 

Azure DevOps-Services biedt nu ondersteuning voor doorlopende bewaking waarbij release-pijplijnen bewakingsgegevens van Application Insights en andere Azure-resources kunnen opnemen. Wanneer een Application Insights-waarschuwing wordt gedetecteerd, wordt de implementatie beperkt kan blijven of worden teruggedraaid terug tot de waarschuwing is opgelost. Als alle controles zijn voltooid, kunnen implementaties doorgaan automatisch van test naar productie zonder de noodzaak van handmatige tussenkomst. 

## <a name="configure-continuous-monitoring"></a>Doorlopende bewaking configureren

1. Selecteer een bestaande Azure DevOps-Services-Project.

2. Beweeg de muisaanwijzer over **Build and Release** > Selecteer **Releases** > Klik op de **plusteken** > **release-definitie maken** > Zoeken naar **bewaking** > **Azure App Service-implementatie met de voortdurende bewaking.**

   ![Nieuwe Azure DevOps Services Release-pijplijn](media/continuous-monitoring/001.png)

3. Klik op **toepassen.**

4. Naast het rode uitroepteken selecteert u de tekst in het blauw naar **omgevingstaken weergeven.**

   ![Taken van de omgeving weergeven](media/continuous-monitoring/002.png)

   Configuratievenster, gebruik de volgende tabel om de invoervelden in te vullen.

    | Parameter        | Waarde |
   | ------------- |:-----|
   | **Omgevingsnaam**      | De naam van die de release-pijplijn-omgeving beschrijft |
   | **Azure-abonnement** | Vervolgkeuzelijst gevuld met alle Azure-abonnementen gekoppeld aan de organisatie Azure DevOps-Services|
   | **Naam van App Service** | Handmatige invoer van een nieuwe waarde is mogelijk vereist voor dit veld, afhankelijk van andere instellingen |
   | **Resourcegroep**    | Vervolgkeuzelijst vult met beschikbare resourcegroepen |
   | **Application Insights-resourcenaam** | Er wordt een vervolgkeuzelijst met alle Application Insights-resources die met de eerder geselecteerde resourcegroep overeenkomen.

5. Selecteer **Configure Application Insights-waarschuwingen**

6. Selecteer voor waarschuwingsregels standaard, **opslaan** > Voer een korte beschrijving > klikt u op **OK**

## <a name="modify-alert-rules"></a>Regels voor waarschuwingen wijzigen

1. Als u wilt de vooraf gedefinieerde instellingen voor waarschuwingen wijzigen, klikt u op het vak met de **weglatingstekens...**  aan de rechterkant van **waarschuwingsregels.**

   (Out-of-box vier waarschuwingsregels aanwezig zijn: Beschikbaarheid, mislukte aanvragen, serverreactietijd, serveruitzonderingen.)

2. Klik op het symbool-omlaag naast **beschikbaarheid.**

3. Wijzigen van de beschikbaarheid **drempelwaarde** om te voldoen aan uw vereisten voor het niveau van service.

   ![Waarschuwing voor wijzigen](media/continuous-monitoring/003.png)

4. Selecteer **OK** > **opslaan** > Voer een korte beschrijving > klikt u op **OK.**

## <a name="add-deployment-conditions"></a>Voorwaarden toevoegen

1. Klikt u op **pijplijn** > Selecteer de **vooraf** of **na de implementatie voorwaarden** symbool, afhankelijk van de fase waarvoor een doorlopende monitoring-gate.

   ![Vóór de implementatie-voorwaarden](media/continuous-monitoring/004.png)

2. Stel **Gates** naar **ingeschakeld** > **goedkeuring gates**> klikt u op **toevoegen.**

3. Selecteer **Azure Monitor** (deze optie geeft u de mogelijkheid tot Toegangswaarschuwingen zowel van Azure Monitor en Application Insights)

    ![Azure Monitor](media/continuous-monitoring/005.png)

4. Voer een **Gates time-out** waarde.

5. Voer een **Interval van steekproeven.**

## <a name="deployment-gate-status-logs"></a>Implementatielogboeken gate-status

Als u implementatie gates toevoegt, wordt in een waarschuwing in Application Insights die groter is dan de eerder ingestelde drempel is gekomen, uw implementatie van ongewenste release promotie beschermt. Als de waarschuwing opgelost is, kan de implementatie automatisch voortgezet.

U ziet dat dit gedrag, selecteer **Releases** > Release met de rechtermuisknop op de naam **open** > **Logboeken.**

![Logboeken](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Azure-pijplijnen probeer deze [snelstartgidsen.](https://docs.microsoft.com/azure/devops/pipelines)
