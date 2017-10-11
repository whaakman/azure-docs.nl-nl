---
title: Schalen aantal exemplaren handmatig of automatisch schalen met Azure Portal | Microsoft Docs
description: Informatie over het schalen van uw Azure-services.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: d171538ea57839eccddcc74ca099a39aee34ea10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Aantal exemplaren handmatig of automatisch schalen
In de [Azure Portal](https://portal.azure.com/), kunt u het aantal exemplaren van de service handmatig instellen of, kunt u parameters instellen zodat deze automatisch schalen op basis van vraag. Dit wordt meestal aangeduid als *uitschalen* of *schalen*.

Voordat u schalen op basis van exemplaren, moet u overwegen schalen wordt beïnvloed door **prijscategorie** naast het aantal exemplaren. Andere Prijscategorieën kan hebben verschillende aantallen kernen en het geheugen en dus betere prestaties voor hetzelfde aantal exemplaren hebben (dit is *opschalen* of *omlaag schalen*). Dit artikel behandelt specifiek *schalen* en *uit*.

In de portal kunnen worden geschaald en u kunt ook de [REST-API](https://msdn.microsoft.com/library/azure/dn931953.aspx) of [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) schaal aanpassen handmatig of automatisch.

> [!NOTE]
> Dit artikel wordt beschreven hoe u een instelling voor automatisch schalen maakt in de portal op [http://portal.azure.com](http://portal.azure.com). Instellingen voor automatisch schalen gemaakt in deze portal kunnen niet worden bewerkt het de klassieke portal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).
> 
> 

## <a name="scaling-manually"></a>Handmatig schalen
1. In de [Azure Portal](https://portal.azure.com/), klikt u op **Bladeren**, navigeer naar de resource die u schalen wilt, zoals een **App Service-abonnement**.
2. Klik op **instellingen > Scale-out (App Service-abonnement).**
3. Aan de bovenkant van de **Scale** blade ziet u een geschiedenis van acties van de service automatisch schalen.
   
    ![Scale-blade](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > Alleen de acties die worden uitgevoerd door automatisch schalen wordt in deze grafiek weergegeven. Als u het aantal exemplaren handmatig aanpast, wordt de wijziging niet weerspiegeld in deze grafiek.
   > 
   > 
4. U kunt handmatig pas het aantal **exemplaren** met schuifregelaar.
5. Klik op de **opslaan** opdracht en u zult worden geschaald naar dat aantal exemplaren bijna onmiddellijk.

## <a name="scaling-based-on-a-pre-set-metric"></a>Schalen op basis van een vooraf ingestelde waarde
Als u wilt dat het aantal exemplaren automatisch wordt aangepast op basis van een metriek, selecteert u de metrische gegevens die u wilt in de **schalen door** vervolgkeuzelijst. Bijvoorbeeld: voor een **App Service-abonnement** kunnen worden geschaald door **CPU-Percentage**.

1. Wanneer u een metriek selecteert krijgt u een schuifregelaar en/of, tekstvakken in te voeren van het aantal exemplaren dat u schalen wilt tussen:
   
    ![Scale-blade met CPU-Percentage](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Automatisch schalen duurt nooit uw service of meer dan de grenzen die u hebt ingesteld, ongeacht de belasting.
2. Ten tweede kunt u het doelbereik voor de metriek kiezen. Bijvoorbeeld, als u hebt gekozen **CPU-percentage**, u kunt een doel voor de gemiddelde CPU voor alle exemplaren van instellen in uw service. Een scale-out er gebeurt wanneer de gemiddelde CPU-capaciteit overschrijdt het maximum dat u definieert, een schaal in gebeurt ook wanneer het gemiddelde CPU zakt onder het minimum.
3. Klik op de **opslaan** opdracht. Automatisch schalen, om de paar minuten om ervoor te zorgen dat u zich in de exemplaar-bereik en het doel voor de metriek controleren. Wanneer uw service extra verkeer ontvangt, krijgt u meer exemplaren zonder dat u een actie uitvoert.

## <a name="scale-based-on-other-metrics"></a>Schalen op basis van andere metrische gegevens
U kunt schalen op basis van metrische gegevens dan de standaardinstellingen die worden weergegeven in de **schalen door** vervolgkeuzelijst en kan zelfs een complexe reeks scale-out hebben en schalen in regels.

### <a name="adding-or-changing-a-rule"></a>Het toevoegen of wijzigen van een regel
1. Kies de **planning en prestaties regels** in de **schalen door** dropdown: ![Prestatieregels](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Als u had eerder automatisch schalen, ziet op u een weergave van de exacte regels die u had.
3. Om te schalen op basis van een andere metrische klikt u op de **regel toevoegen** rij. U kunt ook klikken op een van de bestaande rijen te wijzigen van de metrische gegevens die u eerder in de metriek die u schalen wilt door had.
   ![Regel toevoegen](./media/insights-how-to-scale/Insights_AddRule.png)
4. Nu moet u selecteren welke metrische gegevens die u wilt schalen door. Bij het kiezen van een metriek er een aantal zaken zijn te overwegen:
   
   * De *resource* de metriek is afkomstig uit. Dit wordt normaal gesproken zijn hetzelfde als de bron die u hebben. Als u wilt schalen door de diepte van een opslagwachtrij, is de bron de wachtrij die u schalen wilt door.
   * De *metrische naam* zelf.
   * De *tijd aggregatie* van de metrische gegevens. Dit is hoe de gegevens combineren ligt boven de *duur*.
5. Nadat uw metriek kiezen kiest u de drempelwaarde voor de metrische gegevens en de operator. U kunt bijvoorbeeld zeggen **groter is dan** **80%**.
6. Kies vervolgens de actie die u wilt maken. Er zijn een aantal verschillende soorten acties:
   
   * Vergroten of verkleinen door - Hiermee kunt toevoegen of verwijderen de **waarde** aantal exemplaren dat u definieert
   * Vergroten of verkleinen percentage - Hiermee wijzigt u het aantal exemplaren met een percentage. U kan bijvoorbeeld 25 geplaatst de **waarde** veld, en als u momenteel 8 exemplaren had, 2, zou worden toegevoegd.
   * Vergroten of verkleinen naar: hierdoor het aantal exemplaren wordt ingesteld op de **waarde** u definieert.
7. Ten slotte kunt u coolbar omlaag - hoe lang deze regel na de vorige actie scale wachten moet schalen opnieuw.
8. Klik na het configureren van de regel **OK**.
9. Wanneer u alle regels die u hebt geconfigureerd, moet u bereikt de **opslaan** opdracht.

### <a name="scaling-with-multiple-steps"></a>Schaling mogelijk met meerdere stappen
De bovenstaande voorbeelden zijn vrij eenvoudig. Als u wilt worden agressievere over het schalen van (of omlaag), kunt u echter ook meerdere schaalregels voor de dezelfde metriek toevoegen. U kunt bijvoorbeeld twee scale regels definiëren op CPU-percentage:

1. Uitbreiden door 1 exemplaar als CPU-percentage hoger dan 60 is %
2. Uitbreiden door 3 exemplaren als CPU-percentage hoger dan 85 is %

![Meerdere schaalregels](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Met deze extra regel als uw load groter is dan 85% voordat een schaalactie krijgt u twee extra exemplaren in plaats van een.

## <a name="scale-based-on-a-schedule"></a>Schalen op basis van een planning
Standaard bij het maken van een scale-regel wordt altijd toegepast. U kunt zien dat wanneer u op de koptekst van profiel:

![Profiel](./media/insights-how-to-scale/Insights_Profile.png)

U kunt echter hebben agressievere schalen gedurende de dag of de week, dan op het weekend. U kan zelfs uw service volledig buiten kantooruren afgesloten.

1. U doet dit door op het profiel dat u hebt, selecteert u **terugkeerpatroon** in plaats van **altijd,** en kies de tijdstippen die u wilt dat het profiel toe te passen.
2. Als u bijvoorbeeld een profiel dat van toepassing is tijdens de week, in de **dagen** dropdown schakelt **zaterdag** en **zondag**.
3. Als u een profiel dat van toepassing de overdag is, stel de **begintijd** aan de tijd van de dag dat u starten wilt op.
   
    ![Standaardterugkeerpatroon](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Klik op **OK**.
5. Vervolgens moet u het profiel dat u wilt toepassen op andere tijden toevoegen. Klik op de **profiel toevoegen** rij.
    ![Werk](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Naam van uw nieuwe, tweede, profiel, bijvoorbeeld u het kunt aanroepen **uitgeschakeld werk**.
7. Selecteer vervolgens **terugkeerpatroon** opnieuw, en kies het gewenste gedurende deze tijd exemplaar count-bereik.
8. Met het standaardprofiel kiezen de **dagen** u wilt dat dit profiel toe te passen, en de **begintijd** gedurende de dag.
   
   > [!NOTE]
   > Automatisch schalen die de regels voor zomer-en gebruikt voor afhankelijk van wat **tijdzone** u selecteert. Echter tijdens wintertijd de UTC-offset de base tijdzoneverschil, niet de zomer-en besparingen UTC-offset wordt weergegeven.
   > 
   > 
9. Klik op **OK**.
10. U moet nu, ongeacht regels die u wilt toepassen tijdens uw tweede profiel toevoegen. Klik op **regel toevoegen**, en u kunt vervolgens dezelfde regel die u tijdens het standaardprofiel hebt maken.
    
    ![Regel voor het werk uit toevoegen](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Zorg ervoor dat zowel een regel maken voor scale-out en schaal in hetzij tijdens het profiel voor het aantal exemplaren wordt alleen vergroten (of verlagen).
12. Tot slot op **opslaan**.

## <a name="next-steps"></a>Volgende stappen
* [Service metrische gegevens controleren](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
* [Inschakelen bewaking en diagnostische gegevens](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens op uw service.
* [Ontvang waarschuwingsmeldingen](insights-receive-alert-notifications.md) wanneer er operationele gebeurtenissen plaatsvinden of metrische gegevens een drempelwaarde overschrijden.
* [Bewaken van toepassingsprestaties](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoert in de cloud.
* [Weergeven van gebeurtenissen en het activiteitenlogboek](insights-debugging-with-events.md) voor meer informatie over alles wat u in uw service heeft plaatsgevonden.
* [Beschikbaarheid en reactiesnelheid van een webpagina bewaken](../application-insights/app-insights-monitor-web-app-availability.md) met Application Insights zodat u ontdekken kunt als uw pagina niet beschikbaar is.

