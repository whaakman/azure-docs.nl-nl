---
title: Oplossen van uw Azure Application Insights-beschikbaarheidstests | Microsoft Docs
description: Problemen oplossen-webtests die in Azure Application Insights. Ontvang een waarschuwing wanneer een website niet meer beschikbaar is of traag reageert.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 87bc87d7d105d581f0143e87044fb0337c0fd7f6
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305226"
---
# <a name="troubleshooting"></a>Problemen oplossen

In dit artikel helpt bij het oplossen van veelvoorkomende problemen die zich voordoen kunnen bij het gebruik van de beschikbaarheidsbewaking van webtoepassingen.

## <a name="ssltls-errors"></a>SSL/TLS-fouten

|Symptoom/foutbericht| Mogelijke oorzaken|
|--------|------|
|Kan geen beveiligde SSL/TLS-kanaal maken  | SSL-versie. Alleen TLS 1.0, 1.1 en 1.2 worden ondersteund. **SSLv3 wordt niet ondersteund.**
|TLSv1.2 Record Layer: Waarschuwing (niveau: Onherstelbare fout beschrijving: Bad Record MAC)| Zie StackExchange-thread voor [informatie](https://security.stackexchange.com/questions/39844/getting-ssl-alert-write-fatal-bad-record-mac-during-openssl-handshake).
|URL die is niet mogelijk is het een CDN (Content Delivery Network) | Dit kan worden veroorzaakt door een onjuiste configuratie van uw CDN |  

### <a name="possible-workaround"></a>Mogelijke tijdelijke oplossingen

* Als de URL's die het probleem zich voordoet altijd afhankelijke resources zijn, is het raadzaam om uit te schakelen **afhankelijke aanvragen parseren** voor de Webtest.

## <a name="test-fails-only-from-certain-locations"></a>Test mislukt alleen van bepaalde locaties

|Symptoom/foutbericht| Mogelijke oorzaken|
|----|---------|
|Een verbindingspoging is mislukt, omdat de verbonden partij niet juist heeft gereageerd na een bepaalde periode  | Testagents op bepaalde locaties worden geblokkeerd door een firewall.|
|    |Omleiding van bepaalde IP-adressen plaatsvindt via (Load Balancers, Geo traffic managers, Azure Express Route). 
|    |Als u Azure ExpressRoute, er zijn scenario's waarbij pakketten kunnen worden verwijderd in gevallen waar [asymmetrische routering optreedt](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing).|

## <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Onregelmatige testfout met een protocolfout

|Symptoom/foutbericht| Mogelijke oorzaken|
|----|---------|
Protocol schending CR moet worden gevolgd door LF | Dit gebeurt wanneer onjuist gevormde headers zijn gedetecteerd. Speciaal, zijn enkele headers mogelijk geen gebruik CRLF om aan te geven van einde van regel, die in strijd is met de HTTP-specificatie en wordt daarom validatie op het niveau van de .NET-WebRequest mislukt.
 || Dit kan ook worden veroorzaakt door netwerktaakverdelers of CDN's.

> [!NOTE]
> De URL mislukt mogelijk niet in browsers die een soepelere validatie van HTTP-headers. Zie dit blogbericht voor een gedetailleerde uitleg van dit probleem: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

## <a name="common-troubleshooting-questions"></a>Veelgestelde vragen voor probleemoplossing

### <a name="site-looks-okay-but-i-see-test-failures-why-is-application-insights-alerting-me"></a>Site er goed uitziet, maar ik Zie storingen testen? Waarom is Application Insights waarschuwingen mij?

   * Heeft uw test **afhankelijke aanvragen parseren** ingeschakeld? Die resulteert in een strikte controle van resources, scripts, zoals afbeeldingen, enzovoort. Dit soort fouten is mogelijk niet zichtbaar zijn in een browser. Controleer alle afbeeldingen, scripts, stijlmodellen en andere bestanden geladen door de pagina. Als een van deze mislukt, wordt de test gerapporteerd als mislukt, zelfs als de belangrijkste HTML-pagina wordt geladen zonder problemen. Als u wilt ongevoelig de test voor dergelijke storingen, schakelt u de afhankelijke aanvragen parseren in de Testconfiguratie.

   * Als u wilt verminderen de kans op ruis van tijdelijke netwerkproblemen enzovoort, zorg ervoor dat nieuwe pogingen inschakelen voor mislukte configuratie is ingeschakeld. U kunt ook testen vanaf meer locaties en de waarschuwingsregeldrempel dienovereenkomstig beheren om te voorkomen dat de locatiespecifieke problemen onnodige waarschuwingen veroorzaken.

   * Klik op een van de rode punten uit de ervaring van de beschikbaarheid of een storing van de beschikbaarheid van de Search explorer om de details van waarom we gerapporteerd dat de fout te bekijken. Het testresultaat, samen met de gecorreleerde telemetrie serverzijde (indien ingeschakeld) kan helpen te begrijpen waarom de test is mislukt. Veelvoorkomende oorzaken van problemen van voorbijgaande aard zijn problemen met de netwerkverbinding of de verbinding.

   * Heeft de time-out voor de test? We tests na 2 minuten worden afgebroken. Als uw ping of een test met meerdere stappen langer dan twee minuten duurt, zullen we die rapporteren als mislukt. Houd rekening met de test verdelen in meerdere degene die kunnen worden voltooid in korter duurt.

   * Alle locaties melden dat mislukt, of slechts enkele van deze? Als er slechts enkele fouten gemeld, mogelijk vanwege problemen met de netwerk-/ CDN. Nogmaals, te klikken op de rode punten moet te begrijpen waarom de locatie fouten gerapporteerd.

### <a name="i-did-not-get-an-email-when-the-alert-triggered-or-resolved-or-both"></a>Ik krijg geen een e-mailbericht wanneer de waarschuwing geactiveerd of opgelost of beide?

Controleer de configuratie van de klassieke waarschuwingen om te bevestigen dat uw e-mailadres is rechtstreeks worden weergegeven, of een distributielijst met u is geconfigureerd voor het ontvangen van meldingen. Als dit het geval is, controleert u de configuratie van de lijst met distributiepunten om te bevestigen dat kan de externe e-mailberichten ontvangen. Controleer ook als de mailbeheerder van uw e-mogelijk beleid geconfigureerd dat dit probleem kunnen veroorzaken.

### <a name="i-did-not-receive-the-webhook-notification"></a>De webhook-melding is niet weergegeven?

Controleer of de toepassing die de webhook-meldingen ontvangen is beschikbaar en is de webhook-aanvragen worden verwerkt. Zie [dit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) voor meer informatie.

### <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Onregelmatige testfout met een protocolfout?

De fout 'Schending van protocol... CR moet worden gevolgd door LF' geeft een probleem met de server (of afhankelijkheden) aan. Dit gebeurt wanneer onjuist gevormde headers zijn ingesteld in het antwoord. Dit kan worden veroorzaakt door load balancers of CDN's. Specifiek, enkele headers mogelijk geen gebruik CRLF aan het einde van regel, die in strijd is met de HTTP-specificatie geven en daarom validatie op het niveau van .NET-WebRequest mislukt. Inspecteer de reactie op spot-headers die mogelijk.

> [!NOTE]
> De URL mislukt mogelijk niet in browsers die een soepelere validatie van HTTP-headers. Zie dit blogbericht voor een gedetailleerde uitleg van dit probleem: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

### <a name="i-dont-see-any-related-server-side-telemetry-to-diagnose-test-failures"></a>Ik zie niet alle verwante telemetrie serverzijde om testfouten? *

Als u Application Insights hebt ingesteld voor uw app aan serverzijde, kan dit komen doordat er [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd. Selecteer een andere beschikbaarheidsset-resultaat.

### <a name="can-i-call-code-from-my-web-test"></a>Kan ik code aanroepen via mijn webtest?

Nee. De stappen van de test moeten zich in het bestand .webtest bevinden. U kunt geen andere webtests aanroepen of lussen gebruiken. Maar er zijn verschillende invoegtoepassingen die nuttig kunnen zijn.


### <a name="is-there-a-difference-between-web-tests-and-availability-tests"></a>Is er een verschil tussen webtests en beschikbaarheidstests?

De twee voorwaarden kunnen door elkaar worden gebruikt. 'Beschikbaarheidstest' is een algemenere term waar niet alleen webtests met meerdere stappen, maar ook tests met enkele URL-ping onder vallen.

### <a name="id-like-to-use-availability-tests-on-our-internal-server-that-runs-behind-a-firewall"></a>Ik wil graag beschikbaarheidstests gebruiken voor onze interne server die achter een firewall wordt uitgevoerd.

   Er zijn twee mogelijke oplossingen:

   * Configureer uw firewall om binnenkomende aanvragen van de [IP-adressen van onze webtestagents](../../azure-monitor/app/ip-addresses.md) toe te staan.
   * Schrijf uw eigen code om uw interne server periodiek te testen. Voer de code uit als achtergrondproces op een testserver achter de firewall. De resultaten van het testproces kunnen worden verzonden naar Application Insights door de API [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) te gebruiken in het SDK-kernpakket. Hiervoor moet uw testserver uitgaande toegang hebben tot het opname-eindpunt van Application Insights, maar dit is een veel kleiner beveiligingsrisico dan wanneer u binnenkomende aanvragen toestaat. De resultaten worden niet weergegeven in de blades voor de beschikbaarheidswebtests, maar worden weergegeven als beschikbaarheidsresultaten in Analytics, Search en Metric Explorer.

### <a name="uploading-a-multi-step-web-test-fails"></a>Het uploaden van een webtest met meerdere stappen mislukt

Enkele redenen dat dit kan gebeuren:
   * Er is een limiet van 300 K.
   * Lussen worden niet ondersteund.
   * Verwijzingen naar andere webtests worden niet ondersteund.
   * Gegevensbronnen worden niet ondersteund.

### <a name="my-multi-step-test-doesnt-complete"></a>Mijn test met meerdere stappen wordt niet voltooid

Er is een limiet van 100 aanvragen per test. De test wordt ook gestopt als deze wordt meer dan twee minuten uitgevoerd.

### <a name="how-can-i-run-a-test-with-client-certificates"></a>Hoe voer ik een test uit met clientcertificaten?

Dit wordt momenteel niet ondersteund.

## <a name="who-receives-the-classic-alert-notifications"></a>Wie de (klassiek) waarschuwingsmeldingen ontvangen?

In deze sectie is alleen van toepassing op klassieke waarschuwingen en helpt u optimaliseren van uw meldingen van waarschuwingen om ervoor te zorgen dat alleen de gewenste geadresseerden meldingen ontvangen. Meer informatie geven over het verschil tussen [klassieke waarschuwingen](../platform/alerts-classic.overview.md)en de nieuwe ervaring voor waarschuwingen verwijzen naar de [waarschuwingen overzichtsartikel](../platform/alerts-overview.md). Voor het beheren van de waarschuwing zich melding in de nieuwe waarschuwingen gebruik [actiegroepen](../platform/action-groups.md).

* We raden het gebruik van specifieke ontvangers voor klassieke waarschuwingen.

* Voor waarschuwingen over fouten van X van Y-locaties, de **bulksgewijs/groep** selectievakje, indien ingeschakeld, verzendt naar gebruikers met de rol van beheerder/co-beheerder.  In wezen _alle_ beheerders van de _abonnement_ meldingen ontvangt.

* Voor waarschuwingen over metrische gegevens over beschikbaarheid de **bulksgewijs/groep** selectievakje-als ingeschakeld, verzendt naar gebruikers met de rol van eigenaar, bijdrager of lezer in het abonnement. In feite _alle_ gebruikers met toegang tot het abonnement de Application Insights-resource in het bereik en meldingen ontvangt. 

> [!NOTE]
> Als u momenteel gebruikmaakt van de **bulksgewijs/groep** selectievakje, en uitschakelen, kunt u zich niet meer herstellen van de wijziging.

Gebruik de nieuwe waarschuwing ervaring/bijna realtime waarschuwingen als u meldingen naar gebruikers op basis van hun rol nodig hebt. Met [actiegroepen](../platform/action-groups.md), kunt u e-mailmeldingen voor gebruikers configureren met een van de rollen Inzender of eigenaar/lezer is (niet gecombineerd samen als één optie).

## <a name="next-steps"></a>Volgende stappen

* [WebTest met meerdere stappen testen](availability-multistep.md)
* [URL-ping-tests](monitor-web-app-availability.md)
