---
title: De beschikbaarheid en reactiesnelheid van een website bewaken | Microsoft Docs
description: Stel webtests in Application Insights in. Ontvang een waarschuwing wanneer een website niet meer beschikbaar is of traag reageert.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: a5177293b24ec400714d8f87be4198a76d59214a
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878717"
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>De beschikbaarheid en reactiesnelheid van een website bewaken
Nadat u uw webtoepassing of website hebt geïmplementeerd op een server, kunt u tests instellen om de beschikbaarheid en responsiviteit te bewaken. [Azure Application Insights](app-insights-overview.md) verzendt regelmatig webaanvragen naar uw toepassing vanaf verschillende punten over de hele wereld. U wordt gewaarschuwd als uw toepassing niet of langzaam reageert.

U kunt beschikbaarheidstests instellen voor alle HTTP- en HTTPS-eindpunten die toegankelijk zijn op het openbare internet. U hoeft niks toe te voegen aan de website die u test. Het hoeft niet eens uw site te zijn: u kunt ook een REST API-service testen waarvan u afhankelijk bent.

Er zijn twee soorten beschikbaarheidstests:

* [URL-pingtest](#create): een eenvoudige test die u in Azure Portal kunt instellen.
* [Webtest met meerdere stappen](#multi-step-web-tests): deze test kunt u in Visual Studio Enterprise maken en uploaden naar de portal.

Per toepassingsresource kunt u maximaal 100 beschikbaarheidstests maken.


## <a name="create"></a>Een resource openen voor uw beschikbaarheidstestrapporten

**Als u Application Insights al hebt geconfigureerd** voor uw web-app, opent u de Application Insights-bron in [Azure Portal](https://portal.azure.com).

**Of, als u uw rapporten in een nieuwe resource wilt zien:** ga naar [Azure Portal](https://portal.azure.com) en maak een Application Insights-resource.

![Nieuw > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Klik op **Alle resources** om de blade Overzicht van de nieuwe resource te openen.

## <a name="setup"></a>Een URL-pingtest maken
Open de blade Beschikbaarheid en voeg een test toe.

![Vul in elk geval de URL van uw website in](./media/app-insights-monitor-web-app-availability/001-create-test.png)

* **De URL** kan iedere webpagina zijn die u wilt testen, maar deze moet zichtbaar zijn vanaf het openbare internet. De URL kan een queryreeks bevatten. Zo kunt u bijvoorbeeld oefenen met uw database. Als de URL naar een omleiding is opgelost, kunnen we deze tot maximaal 10 omleidingen opvolgen.
* **Parseren van afhankelijke aanvragen**: als deze optie is ingeschakeld, vraagt de test om afbeeldingen, scripts, stijlbestanden en andere bestanden die deel van de geteste webpagina uitmaken. De opgenomen reactietijd is inclusief de tijd die nodig is om deze bestanden op te halen. De test mislukt als al deze resources niet succesvol kunnen worden gedownload binnen de timeout voor de hele test. Als de optie niet is ingeschakeld, vraagt de test alleen het bestand op van de URL die u hebt opgegeven.

* **Nieuwe pogingen inschakelen**: als deze optie is ingeschakeld, wordt de test, als de test is mislukt, na een korte periode opnieuw uitgevoerd. Fouten worden pas gerapporteerd als er drie opeenvolgende pogingen mislukken. Daaropvolgende tests worden vervolgens met de gebruikelijke testfrequentie uitgevoerd. Volgende pogingen worden tijdelijk uitgesteld tot er weer een test slaagt. Deze regel wordt onafhankelijk toegepast op elke testlocatie. Deze optie wordt aangeraden. Gemiddeld verdwijnt ongeveer 80% van de fouten na het opnieuw proberen.

* **Testfrequentie**: stel in hoe vaak de test wordt uitgevoerd vanaf elke testlocatie. Met een standaardfrequentie van vijf minuten en vijf testlocaties wordt uw site gemiddeld per minuut getest.

* **Testlocaties** zijn de plaatsen van waaraf onze servers webaanvragen verzenden naar uw URL. Onze minimum aantal aanbevolen testlocaties is vijf om te zorgen dat u problemen in uw website van netwerkproblemen onderscheiden kunt. U kunt maximaal 16 locaties selecteren.

> [!NOTE]
> * Het is raadzaam testen vanaf meerdere locaties met een minimum van vijf locaties. Zo wordt voorkomen dat vals alarm dat leiden tijdelijke problemen met een specifieke locatie tot kan. We hebben bovendien gevonden dat de optimale configuratie is dat het aantal testlocaties gelijk zijn aan de waarschuwingslocatie + 2. 
> * De optie 'Afhankelijke aanvragen parseren' leidt tot een strengere controle inschakelen. De test kan mislukken voor aanvragen die mogelijk geen merkbare bij het handmatig bladeren door de site.

* **Criteria voor succes**:

    **Timeout van de test**: verlaag deze waarde om te worden gewaarschuwd over trage reacties. De test wordt als mislukt beschouwd als er binnen deze periode geen reactie van uw site is ontvangen. Als u **Parse onafhankelijke aanvragen** hebt geselecteerd, moeten alle afbeeldingen, stijlbestanden, scripts en andere afhankelijke resources binnen deze periode worden ontvangen.

    **HTTP-antwoord**: de geretourneerde statuscode die staat voor een geslaagde test. 200 is de code die aangeeft dat er een normale webpagina is geretourneerd.

    **Inhoudsovereenkomst**: een tekenreeks, zoals 'Welkom!' Er wordt getest of er in elke respons een exacte (hoofdlettergevoelige) overeenkomst wordt gevonden. Het moet een eenvoudige tekenreeks zijn, zonder jokertekens. Als uw pagina-inhoud wordt gewijzigd, moet u deze tekenreeks mogelijk ook bijwerken.

* **Voor waarschuwingslocatie**: We raden aan een minimum van 3/5 locaties. De optimale relatie tussen waarschuwingslocatie en het aantal testlocaties **waarschuwingslocatie** = **aantal testlocaties** - 2, met een minimum van vijf testen locaties.

## <a name="multi-step-web-tests"></a>Webtests met meerdere stappen
U kunt een scenario bewaken dat bestaat uit een reeks URL's. Als u bijvoorbeeld een verkoopwebsite bewaakt, kunt u testen of het toevoegen van items aan de winkelwagen goed werkt.

> [!NOTE]
> Er worden kosten in rekening gebracht voor webtests met meerdere stappen. [Prijsoverzicht](https://azure.microsoft.com/pricing/details/application-insights/).
> 

Als u een test met meerdere stappen wilt maken, neemt u het scenario op met Visual Studio Enterprise en uploadt u vervolgens de opname naar Application Insights. Application Insights speelt het scenario opnieuw met intervallen en controleert de reacties.

> [!NOTE]
> * U kunt in uw tests geen gecodeerde functies of lussen gebruiken. De test moet volledig zijn opgenomen in het .webtest-script. U kunt echter wel standaard-invoegtoepassingen gebruiken.
> * Alleen Engelse tekens worden ondersteund in de webtest met meerdere stappen. Als u Visual Studio in andere talen gebruikt, werkt u het definitiebestand van de webtest bij om niet-Engelse tekens te vertalen of uit te sluiten.
>

#### <a name="1-record-a-scenario"></a>1. Een scenario opnemen
Gebruik Visual Studio Enterprise om een websessie op te nemen.

1. Maak een project om de webprestaties te testen.

    ![Maak in de Visual Studio Enterprise-versieneen project vanaf de sjabloon Web Performance en Load Test.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Ziet u de sjabloon Web Performance en Load Test niet?* Sluit Visual Studio Enterprise. Open **Visual Studio Installer** om de Visual Studio Enterprise-installatie te wijzigen. Selecteer onder **Afzonderlijke onderdelen** de optie **Hulpprogramma's voor webprestaties en belastingstests**.

2. Open het bestand .webtest en begin met opnemen.

    ![Open het bestand .webtest en klik op Opnemen.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Voer de gebruikersacties uit die u in uw test wilt simuleren: open de website, plaats een product in de winkelwagen, enzovoort. Stop vervolgens de test.

    ![De webtestrecorder wordt uitgevoerd in Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Zorg ervoor dat het scenario niet te lang duurt. Er geldt een limiet van 100 stappen en 2 minuten.
4. Bewerk de test als volgt:

   * Voeg validaties toe om de ontvangen tekst en reactiecodes te controleren.
   * Verwijder alle overbodige interacties. U kunt ook afhankelijke aanvragen voor afbeeldingen of naar advertentie- of trackingsites verwijderen.

     U kunt alleen het testscript bewerken. U kunt geen aangepaste code toevoegen of andere webtests aanroepen. Voeg geen lussen toe aan de test. U kunt standaardinvoegtoepassingen voor webtest gebruiken.
5. Voer de test uit in Visual Studio om er zeker van te zijn dat deze werkt.

    De webtestrunner opent een webbrowser en herhaalt de acties die u hebt opgenomen. Controleer of de test werkt zoals verwacht.

    ![Open in Visual Studio het bestand .webtest en klik op Uitvoeren.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. De webtest uploaden naar Application Insights
1. Maak in de Application Insights-portal een webtest.

    ![Kies op de blade Webtests de optie Toevoegen.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Selecteer de test met meerdere stappen en upload het .webtest-bestand.

    ![Selecteer de webtest met meerdere stappen.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Stel de testlocaties, frequentie en waarschuwingsparameters op dezelfde manier in als voor pingtests.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Tijd en willekeurige cijfers invoegen in uw test met meerdere stappen
Stel dat u een hulpprogramma test dat tijdsafhankelijke gegevens ontvangt van een externe feed (bijvoorbeeld een feed met aandelenkoersen). Wanneer u uw webtest opneemt, moet u specifieke tijden gebruiken, maar u stelt deze in als testparameters: StartTime en EndTime.

![Een webtest met parameters](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Wanneer u de test uitvoert, moet EndTime altijd de huidige tijd zijn. StartTime moet een kwartier in het verleden liggen.

Web Test invoegtoepassingen bieden de manier om parameters voor tijden toe te voegen.

1. Voeg een webtestinvoegtoepassing toe voor elke gewenste variabele parameterwaarde. Kies in de werkbalk van de webtest de optie **Webtestinvoegtoepassing toevoegen**.

    ![Kies Webtestinvoegtoepassing toevoegen en selecteer een type.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    In dit voorbeeld gebruiken we twee exemplaren van de invoegtoepassing Date Time. Een exemplaar is voor "15 minuten geleden" en een ander voor “nu”.
2. Open de eigenschappen van elke invoegtoepassing. Geef de invoegtoepassing een naam en stel deze zodanig in dat de huidige tijd wordt gebruikt. Stel voor een van de toepassingen Minuten toevoegen in op -15.

    ![Naam instellen > Huidige tijd gebruiken > Minuten toevoegen.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. Gebruik in de webtestparameters {{plug-in name}} om te verwijzen naar de naam van de invoegtoepassing.

    ![Gebruik in de testparameter {{plug-in name}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Upload uw test nu naar de portal. Het gebruikt de dynamische waarden bij elke uitvoering van de test.


## <a name="monitor"></a>De resultaten van uw beschikbaarheidstest bekijken

Klik na een paar minuten op **Vernieuwen** om de testresultaten weer te geven. 

![Samenvatting van de resultaten op de Startblade](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Het spreidingsdiagram laat u voorbeelden van de testresultaten met details over de diagnostische teststappen zien. De testengine slaat diagnostische gegevens op voor tests met fouten. Bij geslaagde tests wordt diagnostische informatie voor een subset van de uitvoeringen opgeslagen. Beweeg de muisaanwijzer over een van de groene/rode punten om de tijdstempel, duur, locatie en naam van de test te bekijken. Klik op een punt in het spreidingsdiagram om de details van een testresultaat te bekijken.  

Selecteer een bepaalde test of locatie, of verklein de periode om meer resultaten te zien uit de periode die voor u van belang is. Gebruik Search Explorer om resultaten van alle uitvoeringen weer te geven, of gebruik Analytics-query's om aangepaste rapporten uit te voeren op deze gegevens.

Naast de onbewerkte resultaten kunt u twee metrische beschikbaarheidsgegevens gebruiken in Metrics Explorer: 

1. Beschikbaarheid: percentage van de tests die zijn geslaagd, bekeken over alle testuitvoeringen. 
2. Testduur: gemiddelde testduur van alle testuitvoeringen.

U kunt filters toepassen op de testnaam of -locatie om trends van een bepaalde test en/of locatie te analyseren.

## <a name="edit"></a> Tests bekijken en bewerken

Selecteer op de pagina Overzicht een specifieke test. Hier kunt u de specifieke resultaten bekijken en de test bewerken of tijdelijk uitschakelen.

![Een webtest bewerken of uitschakelen](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Het is verstandig beschikbaarheidstests of de regels voor waarschuwingen die eraan zijn gekoppeld uit te schakelen wanneer u onderhoud uitvoert op uw service. 

## <a name="failures"></a>Als u mislukte tests ziet
Klik op een rode punt.

![Op een rode punt klikken](./media/app-insights-monitor-web-app-availability/open-instance-3.png)

In een resultaat van beschikbaarheidstest ziet u de details van transacties voor alle onderdelen. U kunt hier het volgende doen:

* De reactie inspecteren die is ontvangen van uw server.
* Fout vaststellen met gecorreleerde telemetrie aan de serverzijde die zijn verzameld tijdens het verwerken van de beschikbaarheidstest is mislukt.
* Meld een probleem of werkitem in Git of Azure-kaarten voor het bijhouden van het probleem. De bug bevat een koppeling naar deze gebeurtenis.
* Het webtestresultaat openen in Visual Studio.

Meer informatie over de diagnostische gegevens over de end-to-transactie-ervaring [hier](app-insights-transaction-diagnostics.md).

Klik op de uitzonderingsrij om de details van de server-side-uitzondering waardoor de van synthetische beschikbaarheidstest mislukt te bekijken. U krijgt ook de [momentopname voor foutopsporing](app-insights-snapshot-debugger.md) voor uitgebreidere code niveau diagnostische gegevens.

![Server side diagnostische gegevens](./media/app-insights-monitor-web-app-availability/open-instance-4.png)

## <a name="alerts"></a> Beschikbaarheid van waarschuwingen
U kunt de volgende typen regels voor waarschuwingen op de beschikbaarheid van gegevens met behulp van de ervaring voor klassieke waarschuwingen hebben:
1. X van Y-locaties rapportage van fouten in een bepaalde periode
2. Beschikbaarheid van het totale percentage val onder een drempelwaarde
3. Gemiddelde duur toeneemt buiten een drempelwaarde

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Waarschuwing bij X van Y-locaties die fouten rapporteren
De X van Y-locaties waarschuwingsregel is standaard ingeschakeld in de [nieuwe waarschuwingen van geïntegreerde ervaring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), wanneer u een nieuwe beschikbaarheidstest maken. U kunt zich afmelden op de optie 'klassieke' te selecteren of uitschakelen van de waarschuwingsregel te kiezen.

![Ontwikkel-ervaring](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

**Belangrijke**: met de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), de voorkeuren waarschuwingsregel ernst en meldingen met [actiegroepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **moet** geconfigureerd in de waarschuwingen optreden. Zonder de volgende stappen maakt u alleen in de portal meldingen ontvangt. 

1. Na het opslaan van de beschikbaarheidstest, klik op de nieuwe testnaam om naar de details ervan te gaan. Klik op 'waarschuwing bewerken' ![bewerken na het opslaan](./media/app-insights-monitor-web-app-availability/editaftersave.png)

2. De gewenste ernst, de beschrijving van regel en bovenal - de actiegroep waarvoor de voorkeuren voor meldingen die u wilt gebruiken voor deze waarschuwingsregel instellen.
![Bewerken na het opslaan](./media/app-insights-monitor-web-app-availability/setactiongroup.png)


> [!NOTE]
> * Configureer de actiegroepen voor het ontvangen van meldingen wanneer de waarschuwing wordt geactiveerd door de bovenstaande stappen te volgen. Zonder deze stap wordt u alleen in de portal-meldingen ontvangen wanneer de regel wordt geactiveerd.
>

### <a name="alert-on-availability-metrics"></a>Waarschuwing op basis van metrische gegevens over beschikbaarheid
Met behulp van de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), kunt u waarschuwingen over de beschikbaarheid van gesegmenteerde statistische en duur van de metrische gegevens ook testen:

1. Selecteer een Application Insights-resource in de ervaring voor metrische gegevens en een beschikbaarheid van metrische waarde selecteren: ![selectie van de metrische gegevens over beschikbaarheid](./media/app-insights-monitor-web-app-availability/selectmetric.png)

2. Optie in het menu, u naar de nieuwe ervaring waarin u kunt selecteren specifieke tests of locaties gaat voor het instellen van de waarschuwingsregel op waarschuwingen configureren. U kunt ook de actiegroepen voor deze waarschuwingsregel hier configureren.
    ![Configuratie van de beschikbaarheid van waarschuwingen](./media/app-insights-monitor-web-app-availability/availabilitymetricalert.png)

### <a name="alert-on-custom-analytics-queries"></a>Waarschuwing bij aangepaste analytics-query 's
Met behulp van de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), u kunt waarschuwingen op [aangepaste logboeken-query's](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). Met aangepaste query's, kunt u erop wijzen op een willekeurige voorwaarde die helpt u bij het ophalen van de meest betrouwbare signaal van problemen met de beschikbaarheid. Dit is ook vooral van toepassing, als u aangepaste beschikbaarheidsresultaten met behulp van de SDK TrackAvailability verzendt. 

> [!Tip]
> * De metrische gegevens over de van beschikbaarheidsgegevens bevatten een aangepaste beschikbaarheidsresultaten die u door het aanroepen van onze SDK TrackAvailability kan indienen. U kunt de waarschuwingen voor de ondersteuning voor metrische gegevens op de waarschuwing op de van de aangepaste beschikbaarheidsresultaten.
>

## <a name="dealing-with-sign-in"></a>Omgaan met aanmelden
Als uw gebruikers zich aanmelden bij uw app, hebt u verschillende functies om de aanmelding te simuleren, zodat u pagina’s na het aanmelden kunt testen. Welke aanpak u gebruikt, hangt af van het type beveiliging van de app.

In alle gevallne moet u een account maken in uw toepassing voor testdoeleinden. Beperk indien mogelijk de machtigingen voor dit testaccount, zodat webtests echte gebruikers niet beïnvloeden.

### <a name="simple-username-and-password"></a>Eenvoudige gebruikersnaam en wachtwoord
Een webtest op de gebruikelijke manier registreren. Verwijder eerst de cookies.

### <a name="saml-authentication"></a>SAML-verificatie
Gebruik de beschikbare SAML-invoegtoepassing voor webtests.

### <a name="client-secret"></a>Clientgeheim
Als uw app een aanmeldroute heeft die een klantgeheim omvat, gebruik dan deze route. Azure Active Directory (AAD) is een voorbeeld van een service die aanmelden met een clientgeheim bevat. In AAD is het klantgeheim de App Key.

Hier is een voorbeeldwebtest van een Azure web-app met een App Key:

![Voorbeeld clientgeheim](./media/app-insights-monitor-web-app-availability/110.png)

1. Token van AAD krijgen met klantgeheim (AppKey).
2. Bearer token uit antwoord halen.
3. API oproepen met bearer token in de autorisatie-header.

Zorg ervoor dat de webtest een eigenlijke client is - dit wil zeggen: een eigen app in AAD heeft - en zijn clientId + App Key gebruikt. Uw service onder de test heeft ook een eigen app in AAD: de appID URI van deze app wordt weergegeven in het “resource”-veld tijdens de webtest.

### <a name="open-authentication"></a>Open verificatie
Een voorbeeld van open verificatie is het aanmelden met uw Microsoft- of Google-account. Veel apps die OAuth gebruiken, bieden een alternatief met clientgeheim, zodat uw eerste tactiek moet zijn deze mogelijkheid te onderzoeken.

Als uw test moet aanmelden met OAuth, is de algemene benadering:

* Gebruik een hulpprogramma zoals Fiddler om het verkeer tussen de webbrowser, de verificatiesite en uw app te onderzoeken.
* Voer twee of meer aanmeldingen uit via verschillende apparaten en browsers, of met lange periodes ertussen (zodat de tokens verlopen).
* Vergelijk de verschillende sessies om te bepalen welk token is doorgegeven door de verificatiesite en daarna, na het aanmelden, wordt doorgegeven aan uw appserver.
* Neem een webtest op met Visual Studio.
* Maak parameters van de tokens. Stel de parameter in wanneer er een token wordt geretourneerd van de verificator en gebruik deze in de query voor de site.
  (Visual Studio probeert de testparameters toe te voegen, maar voegt de parameters voor de tokens niet correct toe.)

## <a name="performance-tests"></a>Prestatietests
U kunt een belastingtest op uw website uitvoeren. Zoals de beschikbaarheidstest kunt enkel- of meervoudige aanvragen sturen vanuit onze punten over de hele wereld verspreid. In tegenstelling tot een beschikbaarheidstest worden vele verzoeken verzonden, waarmee meerdere gelijktijdige gebruikers worden gesimuleerd.

Open op de blade Overzicht **Instellingen**, **Prestatietests**. Wanneer u een test maakt, wordt u uitgenodigd om verbinding maken met of een Azure DevOps-account maken.

Wanneer de test voltooid is, worden de responstijden en succespercentages weergegeven.


![Prestatietest](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> Gebruik [Live Stream](app-insights-live-stream.md) en [Profiler](app-insights-profiler.md) om de effecten van een prestatietest te volgen.
>

## <a name="automation"></a>Automation
* Gebruik [PowerShell-scripts om automatisch een beschikbaarheidstest in te stellen](app-insights-powershell.md#add-an-availability-test).
* Stel een [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) in die wordt aangeroepen wanneer er een waarschuwing wordt gegenereerd.

## <a name="qna"></a> FAQ

* *Site er goed uitziet, maar ik Zie storingen testen? Waarom is Application Insights waarschuwingen mij?*

    * Heeft uw test 'Parseren van afhankelijke aanvragen' ingeschakeld? Die resulteert in een strikte controle van resources, scripts, zoals afbeeldingen, enzovoort. Dit soort fouten is mogelijk niet zichtbaar zijn in een browser. Controleer alle afbeeldingen, scripts, stijlmodellen en andere bestanden geladen door de pagina. Als één van deze mislukt, wordt de test gerapporteerd als mislukt, zelfs als de html-pagina correct laadt. Als u de test ongevoelig wilt maken voor dergelijke storingen, schakelt u de optie 'Afhankelijke aanvragen parseren' in de testconfiguratie uit. 

    * Als u kans op ruis van tijdelijke netwerkproblemen enzovoort wilt verminderen, schakelt u de optie 'Nieuwe pogingen inschakelen voor mislukte webtesten' in. U kunt ook testen vanaf meer locaties en de waarschuwingsregeldrempel dienovereenkomstig beheren om te voorkomen dat de locatiespecifieke problemen onnodige waarschuwingen veroorzaken.

    * Klik op een van de rode punten uit de ervaring van de beschikbaarheid of een storing van de beschikbaarheid van de Search explorer om de details van waarom we gerapporteerd dat de fout te bekijken. Het testresultaat, samen met de gecorreleerde telemetrie aan de serverzijde (indien ingeschakeld) kan helpen te begrijpen waarom de test is mislukt. Veelvoorkomende oorzaken van problemen van voorbijgaande aard zijn problemen met de netwerkverbinding of de verbinding. 

    * Heeft de time-out voor de test? We tests na 2 minuten worden afgebroken. Als uw ping of een test met meerdere stappen langer dan twee minuten duurt, zullen we die rapporteren als mislukt. Houd rekening met de test verdelen in meerdere degene die kunnen worden voltooid in korter duurt.

    * Alle locaties melden dat mislukt, of slechts enkele van deze? Als er slechts enkele fouten gemeld, mogelijk vanwege problemen met de netwerk-/ CDN. Nogmaals, te klikken op de rode punten moet te begrijpen waarom de locatie fouten gerapporteerd.

* *Ik krijg geen een e-mailbericht wanneer de waarschuwing geactiveerd of opgelost of beide?*

    Controleer de configuratie van de klassieke waarschuwingen om te bevestigen dat uw e-mailadres is rechtstreeks worden weergegeven, of een distributielijst met u is geconfigureerd voor het ontvangen van meldingen. Als dit het geval is, controleert u de configuratie van de lijst met distributiepunten om te bevestigen dat kan de externe e-mailberichten ontvangen. Controleer ook als de mailbeheerder van uw e-mogelijk beleid geconfigureerd dat dit probleem kunnen veroorzaken.

* *De webhook-melding is niet weergegeven?*

    Controleer of de toepassing die de webhook-meldingen ontvangen is beschikbaar en is de webhook-aanvragen worden verwerkt. Zie [dit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) voor meer informatie.

* *Onregelmatige testfout met een protocolfout?*

    De fout 'Schending van protocol... CR moet worden gevolgd door LF' geeft een probleem met de server (of afhankelijkheden) aan. Dit gebeurt wanneer onjuist gevormde headers zijn ingesteld in het antwoord. Dit kan worden veroorzaakt door load balancers of CDN's. Meer in het bijzonder maken enkele headers mogelijk geen gebruik CRLF om het einde van de regel aan te geven, wat in strijd is met de HTTP-specificatie en waardoor validatie op het niveau van .NET-WebRequest mislukt. Controleer het antwoord om headers te signaleren die mogelijk fouten bevatten.
    
    Opmerking: de URL mislukt mogelijk niet in browsers die een beperkte validatie van HTTP-headers hebben. Zie dit blogbericht voor een gedetailleerde uitleg van dit probleem: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  
    
* *Ik zie geen gerelateerde telemetriegegevens van de server om testfouten vast te stellen*
    
    Als u Application Insights hebt ingesteld voor uw app aan serverzijde, kan dit komen doordat er [steekproeven](app-insights-sampling.md) worden uitgevoerd. Selecteer een andere beschikbaarheidsset-resultaat.

* *Kan ik code aanroepen via mijn webtest?*

    Nee. De stappen van de test moeten zich in het bestand .webtest bevinden. U kunt geen andere webtests aanroepen of lussen gebruiken. Maar er zijn verschillende invoegtoepassingen die nuttig kunnen zijn.

* *Wordt HTTPS ondersteund?*

    ondersteunen TLS 1.1 en TLS 1.2.
* *Is er een verschil tussen webtests en beschikbaarheidstests?*

    De twee voorwaarden kunnen door elkaar worden gebruikt. 'Beschikbaarheidstest' is een algemenere term waar niet alleen webtests met meerdere stappen, maar ook tests met enkele URL-ping onder vallen.
    
* *Ik wil graag beschikbaarheidstests gebruiken op onze interne server die achter een firewall wordt uitgevoerd.*

    Er zijn twee mogelijke oplossingen:
    
    * Configureer uw firewall om binnenkomende aanvragen van de [IP-adressen van onze webtestagents](app-insights-ip-addresses.md) toe te staan.
    * Schrijf uw eigen code om uw interne server periodiek te testen. Voer de code uit als achtergrondproces op een testserver achter de firewall. De resultaten van het testproces kunnen worden verzonden naar Application Insights door de API [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) te gebruiken in het SDK-kernpakket. Hiervoor moet uw testserver uitgaande toegang hebben tot het opname-eindpunt van Application Insights, maar dit is een veel kleiner beveiligingsrisico dan wanneer u binnenkomende aanvragen toestaat. De resultaten worden niet weergegeven in de blades voor de beschikbaarheidswebtests, maar worden weergegeven als beschikbaarheidsresultaten in Analytics, Search en Metric Explorer.

* *Het uploaden van een webtest met meerdere stappen mislukt*

    Enkele redenen dat dit kan gebeuren:
    * Er is een limiet van 300 K.
    * Lussen worden niet ondersteund.
    * Verwijzingen naar andere webtests worden niet ondersteund.
    * Gegevensbronnen worden niet ondersteund.

* *Mijn test met meerdere stappen wordt niet voltooid*

    Er is een limiet van 100 aanvragen per test. De test wordt ook gestopt als deze wordt meer dan twee minuten uitgevoerd.

* *Hoe voer ik een test uit met clientcertificaten?*

    Dat wordt niet ondersteund.


## <a name="next"></a>Volgende stappen
[Diagnostische logboeken doorzoeken][diagnostic]

[Problemen oplossen][qna]

[IP-adressen van webtest-agents](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
