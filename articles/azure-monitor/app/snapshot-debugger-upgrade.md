---
title: Azure Application Insights Snapshot Debugger upgrade voor .NET-apps | Microsoft Docs
description: Snapshot Debugger bijwerken naar de nieuwste versie in Azure App Services, of via Nuget-pakketten
services: application-insights
author: MarioHewardt
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: Mario.Hewardt
ms.reviewer: mbullwin
ms.openlocfilehash: 54b79897ee378cda52106fe704e25c50a4325f38
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632162"
---
# <a name="upgrading-the-snapshot-debugger"></a>Een upgrade van de Snapshot Debugger

Om te voorzien in de best mogelijke beveiliging voor uw gegevens, is Microsoft van TLS 1.0 en 1.1 TLS zijn, waarvan is aangetoond kwetsbaar voor vastberaden aanvallers. Als u een oudere versie van de site-extensie gebruikt, moet deze een upgrade naar blijven werken. Dit document bevat een overzicht van de stappen die nodig zijn voor uw Snapshot debugger upgraden naar de nieuwste versie. Er zijn twee primaire upgradepaden afhankelijk van als u de Snapshot Debugger met behulp van site-uitbreiding hebt ingeschakeld of als u een SDK/Nuget toegevoegd aan uw toepassing gebruikt. Beide methoden voor upgrades worden hieronder besproken. 

## <a name="upgrading-the-site-extension"></a>Een upgrade van de site-extensie

Als u de Snapshot debugger met behulp van de site-extensie hebt ingeschakeld, kunt u eenvoudig upgraden met behulp van de volgende procedure:

1. Meld u aan bij Azure Portal.
2. Navigeer naar de resource met Application Insights en Snapshot debugger ingeschakeld. Bijvoorbeeld, voor een Web-App, gaat u naar de resource App Service:

   ![Schermafbeelding van de afzonderlijke App Service-resource met de naam DiagService01](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Als u bent gegaan met de bron, klikt u op de Application Insights in de blade overzicht:

   ![Schermopname van de drie knoppen. Middelste knop met de naam van Application Insights is geselecteerd](./media/snapshot-debugger-upgrade/application-insights-button.png)

4. Er wordt een nieuwe blade geopend met de huidige instellingen. Als u wilt een goed idee om uw instellingen te wijzigen, kunt u ze zijn. De **toepassen** knop aan de onderkant van de blade is niet standaard ingeschakeld en moet u een van de instellingen op de knop activeren in-of uitschakelen. U hoeft niet te wijzigen van de werkelijke instellingen, in plaats daarvan kunt u de instelling wijzigen en vervolgens direct weer wijzigen. We raden aan bij het omschakelen van de Profiler instellen en vervolgens de optie **toepassen**.

   ![Schermopname van App serviceconfiguratie van Application Insights-pagina met de knop toepassen rood gemarkeerd](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

5. Nadat u op **toepassen**, u wordt gevraagd om de wijzigingen te bevestigen.

    > [!NOTE]
    > De site wordt opnieuw opgestart als onderdeel van het upgradeproces.

   ![Schermopname van App Service de toepassing bewaking prompt. In het tekstvak wordt een bericht weergegeven: "We nu wijzigingen in uw appinstellingen toegepast en onze hulpprogramma's voor het koppelen van uw Application Insights-resource om de web-app te installeren. Hiermee wordt de site opnieuw opgestart. Wilt u doorgaan?'](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

6. Klik op **Ja** de wijzigingen moeten worden toegepast. Tijdens het proces verschijnt een melding weergegeven dat wijzigingen worden toegepast:

   ![Schermafbeelding van de wijzigingen toepassen - extensies-bericht dat wordt weergegeven in de rechterbovenhoek bijwerken](./media/snapshot-debugger-upgrade/updating-extensions.png)

Nadat deze is voltooid, een **'Wijzigingen worden toegepast'** melding wordt weergegeven.

   ![Schermopname van het bericht wijzigingen worden toegepast](./media/snapshot-debugger-upgrade/changes-are-applied.png)

De site is nu bijgewerkt en kan worden gebruikt.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Snapshot Debugger met behulp van SDK/Nuget upgraden

Als de toepassing een versie van gebruikt `Microsoft.ApplicationInsights.SnapshotCollector` lagere versie dan versie 1.3.1, moet deze worden bijgewerkt naar een [nieuwere versie](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) verder wilt werken.
