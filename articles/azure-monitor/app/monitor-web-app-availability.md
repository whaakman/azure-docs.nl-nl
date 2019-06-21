---
title: De beschikbaarheid en reactiesnelheid van een website bewaken | Microsoft Docs
description: Stel webtests in Application Insights in. Ontvang een waarschuwing wanneer een website niet meer beschikbaar is of traag reageert.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: abe55007aa8a8719d6b6f1659e00a089a2e28771
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303735"
---
# <a name="monitor-the-availability-of-any-website"></a>De beschikbaarheid van een website bewaken

Nadat u uw web-app/website hebt geïmplementeerd, kunt u terugkerende tests instellen voor het bewaken van beschikbaarheid en reactiesnelheid. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) verzendt regelmatig webaanvragen naar uw toepassing vanaf verschillende punten over de hele wereld. Deze waarschuwt u als uw toepassing reageert niet of langzaam reageert.

U kunt beschikbaarheidstests instellen voor alle HTTP- en HTTPS-eindpunten die toegankelijk zijn op het openbare internet. U hebt geen wijzigingen aanbrengen in de website die u test. In feite hoeft deze zelfs te zijn van een site die u eigenaar bent. U kunt de beschikbaarheid van een REST-API die afhankelijk van uw service testen.

### <a name="types-of-availability-tests"></a>Soorten beschikbaarheidstests:

Er zijn drie soorten beschikbaarheidstests:

* [URL-pingtest](#create-a-url-ping-test): een eenvoudige test die u in Azure Portal kunt instellen.
* [WebTest met meerdere stappen](availability-multistep.md): Een registratie van een reeks webaanvragen, die kan worden afgespeeld om te testen complexere scenario's. Webtests met meerdere stappen zijn gemaakt in Visual Studio Enterprise en geüpload naar de portal voor uitvoering.
* [Aangepaste bijhouden Beschikbaarheidstests](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability?view=azure-dotnet): De `TrackAvailability()` methode kan worden gebruikt om uw eigen aangepaste beschikbaarheidstests maken.

**U kunt maximaal 100 beschikbaarheidstests per Application Insights-resource maken.**

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken

Als u wilt een beschikbaarheidstest maken, moet u eerst een Application Insights-resource maken. Als u al een resource hebt gemaakt, gaat u verder naar de volgende sectie voor [maken van een URL-Pingtest](#create-a-url-ping-test).

Selecteer in de Azure-portal **een resource maken** > **hulpprogramma's voor ontwikkelaars** > **Application Insights** en [maken een Application Insights-resource](create-new-resource.md).

## <a name="create-a-url-ping-test"></a>Een URL-pingtest aanmaken

De naam 'URL-Pingtest' is een deel van een misnomer. Als u wilt wissen, is deze test niet beschikbaar voor gebruik van ICMP (Internet Control Message Protocol) om te controleren op beschikbaarheid van uw site. In plaats daarvan gebruikt meer geavanceerde functionaliteit van de HTTP-aanvraag om te valideren of een eindpunt reageert. Ook de prestaties die zijn gekoppeld aan de respons worden gemeten en voegt de mogelijkheid om aangepaste succescriteria die zijn gekoppeld aan meer geavanceerde functies, zoals afhankelijke aanvragen parseren en toestaan voor nieuwe pogingen.

Uw eerste als beschikbaarheidsaanvraag wilt maken, opent u het deelvenster van de beschikbaarheid en selecteer **Test maken**.

![Vul in elk geval de URL van uw website in](./media/monitor-web-app-availability/availability-create-test-001.png)

### <a name="create-a-test"></a>Een test maken

|Instelling| Uitleg
|----|----|----|
|**URL** |  De URL mag een webpagina die u wilt testen, maar deze moet zichtbaar zijn vanaf het openbare internet. De URL kan een queryreeks bevatten. Zo kunt u bijvoorbeeld oefenen met uw database. Als de URL naar een omleiding is opgelost, kunnen we deze tot maximaal 10 omleidingen opvolgen.|
|**Afhankelijke aanvragen parseren**| Aanvragen voor afbeeldingen, scripts, Stijlbestanden en andere bestanden die deel van de webpagina wordt weergegeven onder belasting uitmaken testen. De opgenomen reactietijd is inclusief de tijd die nodig is om deze bestanden op te halen. De test mislukt als een van deze resources kunnen niet worden gedownload binnen de time-out voor de hele test. Als de optie niet is ingeschakeld, vraagt de test alleen het bestand op van de URL die u hebt opgegeven. Deze optie resulteert in een strengere controle inschakelen. De test kan mislukken voor gevallen waarin u mogelijk geen merkbare bij het handmatig bladeren door de site.
|**Nieuwe pogingen inschakelen**|Wanneer de test mislukt, wordt deze na een korte periode opnieuw uitgevoerd. Fouten worden pas gerapporteerd als er drie opeenvolgende pogingen mislukken. Daaropvolgende tests worden vervolgens met de gebruikelijke testfrequentie uitgevoerd. Volgende pogingen worden tijdelijk uitgesteld tot er weer een test slaagt. Deze regel wordt onafhankelijk toegepast op elke testlocatie. **Deze optie wordt aangeraden**. Gemiddeld verdwijnt ongeveer 80% van de fouten na het opnieuw proberen.|
|**Testfrequentie**| Hiermee stelt u op hoe vaak de test wordt uitgevoerd vanaf elke testlocatie. Met een standaardfrequentie van vijf minuten en vijf testlocaties wordt uw site gemiddeld per minuut getest.|
|**Testlocaties**| Zijn de plaatsen van waar onze servers webaanvragen naar uw URL verzenden. **Onze minimum aantal aanbevolen testlocaties is vijf** om te zorgen dat u problemen in uw website van netwerkproblemen onderscheiden kunt. U kunt maximaal 16 locaties selecteren.

**Als uw URL niet zichtbaar zijn vanaf het openbare internet is, kunt u selectief opent u uw firewall om toe te staan alleen de testtransacties via**. Raadpleeg voor meer informatie over de firewall-uitzonderingen voor onze testagents beschikbaarheid, de [IP-adres handleiding](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses#availability-tests).

> [!NOTE]
> Het wordt aangeraden testen vanaf meerdere locaties met **een minimum van vijf locaties**. Zo wordt voorkomen dat vals alarm dat leiden tijdelijke problemen met een specifieke locatie tot kan. Bovendien we hebben ontdekt dat de optimale configuratie is dat de **aantal testlocaties gelijk zijn aan de waarschuwingslocatie + 2**.

### <a name="success-criteria"></a>Criteria voor succes

|Instelling| Uitleg
|----|----|----|
| **Time-out voor testen** |Verlaag deze waarde om te worden gewaarschuwd over trage reacties. De test wordt als mislukt beschouwd als er binnen deze periode geen reactie van uw site is ontvangen. Als u **Parse onafhankelijke aanvragen** hebt geselecteerd, moeten alle afbeeldingen, stijlbestanden, scripts en andere afhankelijke resources binnen deze periode worden ontvangen.|
| **HTTP-antwoord** | De geretourneerde statuscode die voor een geslaagde test staat. 200 is de code die aangeeft dat er een normale webpagina is geretourneerd.|
| **Inhoudsovereenkomst** | Een tekenreeks, zoals 'Welkom!' Er wordt getest of er in elke respons een exacte (hoofdlettergevoelige) overeenkomst wordt gevonden. Het moet een eenvoudige tekenreeks zijn, zonder jokertekens. Als uw pagina-inhoud wordt gewijzigd, moet u deze tekenreeks mogelijk ook bijwerken. **Alleen Engelse tekens worden ondersteund met inhoudsovereenkomsten** |

### <a name="alerts"></a>Waarschuwingen

|Instelling| Uitleg
|----|----|----|
|**Near-realtime (Preview)** | Het is raadzaam om met behulp van de bijna realtime waarschuwingen. Configureren van dit type waarschuwing wordt uitgevoerd nadat de beschikbaarheidstest is gemaakt.  |
|**Klassiek** | We niet meer wordt aanbevolen met klassieke waarschuwingen voor nieuwe beschikbaarheidstests.|
|**Voor waarschuwingslocatie**|U wordt aangeraden een minimum van 3/5 locaties. De optimale relatie tussen waarschuwingslocatie en het aantal testlocaties **waarschuwingslocatie** = **aantal testlocaties - 2, met een minimum van vijf testlocaties.**|

## <a name="see-your-availability-test-results"></a>De resultaten van de beschikbaarheidstest bekijken

Beschikbaarheid van de resultaten kunnen worden gevisualiseerd met lijn- en spreidingsdiagrammen plot weergaven.

Na een paar minuten, klikt u op **vernieuwen** om te zien van de testresultaten.

![Regel weergeven](./media/monitor-web-app-availability/availability-refresh-002.png)

De weergave teststappen bevat voorbeelden van de testresultaten met details van de diagnostische test-stap in deze. De testengine slaat diagnostische gegevens op voor tests met fouten. Bij geslaagde tests wordt diagnostische informatie voor een subset van de uitvoeringen opgeslagen. Beweeg de muisaanwijzer over een van de groene/rode punten om te bekijken van de test, test de naam en locatie.

![Regel weergeven](./media/monitor-web-app-availability/availability-scatter-plot-003.png)

Selecteer een bepaalde test of locatie, of verklein de periode om meer resultaten te zien uit de periode die voor u van belang is. Gebruik Search Explorer om resultaten van alle uitvoeringen weer te geven, of gebruik Analytics-query's om aangepaste rapporten uit te voeren op deze gegevens.

## <a name="inspect-and-edit-tests"></a>Tests bekijken en bewerken

Als u wilt bewerken, tijdelijk uitschakelen of verwijderen een test klikt u op het beletselteken naast de testnaam van een. Het duurt maximaal 20 minuten voor wijzigingen in de configuratie doorgegeven aan alle testagents na een wijziging wordt aangebracht.

![Details van de test weergeven. Bewerken en een WebTest uitschakelen](./media/monitor-web-app-availability/edit.png)

Het is verstandig beschikbaarheidstests of de regels voor waarschuwingen die eraan zijn gekoppeld uit te schakelen wanneer u onderhoud uitvoert op uw service.

## <a name="if-you-see-failures"></a>Als u mislukte tests ziet

Klik op een rode punt.

![Op een rode punt klikken](./media/monitor-web-app-availability/open-instance-3.png)

In een resultaat van beschikbaarheidstest ziet u de details van transacties voor alle onderdelen. U kunt hier het volgende doen:

* De reactie inspecteren die is ontvangen van uw server.
* Fout vaststellen met gecorreleerde serverzijde telemetriegegevens die zijn verzameld tijdens het verwerken van de beschikbaarheidstest is mislukt.
* Meld een probleem of werkitem in Git of Azure-kaarten voor het bijhouden van het probleem. De bug bevat een koppeling naar deze gebeurtenis.
* Het webtestresultaat openen in Visual Studio.

Meer informatie over de diagnostische gegevens over de end-to-transactie-ervaring [hier](../../azure-monitor/app/transaction-diagnostics.md).

Klik op de uitzonderingsrij om de details van de server-side '-uitzondering waardoor de van synthetische beschikbaarheidstest mislukt te bekijken. U krijgt ook de [momentopname voor foutopsporing](../../azure-monitor/app/snapshot-debugger.md) voor uitgebreidere code niveau diagnostische gegevens.

![Diagnostische gegevens van server-side](./media/monitor-web-app-availability/open-instance-4.png)

Naast de onbewerkte resultaten kunt u ook twee belangrijke metrische gegevens over beschikbaarheid in weergeven [Metrics Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started):

1. Beschikbaarheid: Percentage van de tests zijn geslaagd, bekeken over alle testuitvoeringen.
2. Testduur: Gemiddelde testduur over alle testuitvoeringen.

## <a name="automation"></a>Automation

* Gebruik [PowerShell-scripts om automatisch een beschikbaarheidstest in te stellen](../../azure-monitor/app/powershell.md#add-an-availability-test).
* Stel een [webhook](../../azure-monitor/platform/alerts-webhooks.md) in die wordt aangeroepen wanneer er een waarschuwing wordt gegenereerd.

## <a name="troubleshooting"></a>Problemen oplossen

Toegewezen [probleemoplossingsartikel](troubleshoot-availability.md).

## <a name="next-steps"></a>Volgende stappen

* [Beschikbaarheid van waarschuwingen](availability-alerts.md)
* [Webtests met meerdere stappen](availability-multistep.md)


