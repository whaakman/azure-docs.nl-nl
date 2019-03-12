---
title: Eindpuntbewaking van Azure Traffic Manager | Microsoft Docs
description: In dit artikel krijgt u inzicht in hoe Traffic Manager eindpuntbewaking en failover automatisch eindpunt gebruikt om u te helpen Azure-klanten toepassingen met hoge beschikbaarheid implementeren
services: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2018
ms.author: kumud
ms.openlocfilehash: 50ed230993f1df07b463297605a144830476803d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57540246"
---
# <a name="traffic-manager-endpoint-monitoring"></a>Eindpuntcontrole van Traffic Manager

Met Azure Traffic Manager omvat ingebouwde eindpuntbewaking en failover van de automatische-eindpunt. Deze functie helpt u toepassingen met hoge beschikbaarheid die tolerant omgaan met het eindpunt is mislukt, met inbegrip van Azure-regio fouten te leveren.

## <a name="configure-endpoint-monitoring"></a>Eindpuntbewaking configureren

Als u wilt controleren-eindpunt configureren, moet u de volgende instellingen op uw Traffic Manager-profiel:

* **Protocol**. Kies HTTP, HTTPS of TCP als protocol gebruikt bij het scannen van het eindpunt van Traffic Manager, om te controleren of de status. Bewaking van HTTPS wordt niet gecontroleerd dat of uw SSL-certificaat geldig is--er alleen wordt gecontroleerd of het certificaat aanwezig is.
* **Poort**. Kies de poort die wordt gebruikt voor de aanvraag.
* **Path**. Deze configuratie-instelling is alleen geldig voor de protocollen HTTP en HTTPS, voor welke opgeven van het pad naar de instelling vereist is. Het leveren van deze instelling voor de TCP-protocol resulteert in een fout bewaking. Geef het relatieve pad en de naam van de webpagina of het bestand dat gebruikmaakt van de bewaking voor HTTP en HTTPS-protocol. Een slash (/) is een geldige vermelding voor het relatieve pad. Deze waarde geeft aan dat het bestand in de hoofdmap (standaard is).
* **Instellingen voor aangepaste header** deze configuratie-instelling kunt u specifieke HTTP-headers voor de status controleert Traffic Manager verzendt naar eindpunten onder een profiel. De aangepaste kopteksten kunnen worden opgegeven op het niveau van een profiel van toepassing zijn voor alle eindpunten in dit profiel en/of op het niveau van een eindpunt alleen van toepassing op dit eindpunt. U kunt aangepaste headers gebruiken voor het met statuscontroles naar eindpunten in een omgeving met meerdere tenants correct worden doorgestuurd naar de bestemming door een host-header op te geven. U kunt deze instelling ook gebruiken door toe te voegen unieke kopteksten die kunnen worden gebruikt voor het identificeren van Traffic Manager afkomstig is van HTTP (S)-aanvragen en ze anders verwerkt. U kunt maximaal acht-header: waarde-paren seprated opgeven door een komma. Bijvoorbeeld, "header1:value1, header2:value2'. 
* **Verwachte status code bereiken** deze instelling kunt u meerdere succes code bereiken in de indeling 200 299, 301 301 opgeven. Als deze statuscodes zijn ontvangen als antwoord van een eindpunt als een controle van gatewayservicestatus is gestart, worden deze eindpunten als in orde gemarkeerd in Traffic Manager. U kunt maximaal 8 statusbereik code opgeven. Deze instelling geldt alleen voor HTTP en HTTPS-protocol en voor alle eindpunten. Deze instelling is op het niveau van het Traffic Manager-profiel en wordt standaard de waarde 200 is gedefinieerd als de code van de status geslaagd.
* **Testinterval**. Deze waarde wordt bepaald hoe vaak een eindpunt is ingeschakeld voor de status van een testinterval Traffic Manager-agent. U kunt hier de twee waarden opgeven: 30 seconden (normaal zoeken) en 10 seconden (snelle probing). Als er geen waarden zijn opgegeven, wordt het profiel wordt ingesteld op een standaardwaarde van 30 seconden. Ga naar de [prijzen van Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) pagina voor meer informatie over de prijzen voor snelle testinterval.
* **Aantal mislukte aanmeldingen getolereerd**. Deze waarde wordt bepaald hoeveel storingen die een testinterval Traffic Manager-agent maximaal voordat u dit eindpunt als niet in orde markeert wordt toegestaan. De waarde kan liggen tussen 0 en 9. Een waarde van 0 wordt aangegeven dat een controle storing kan leiden tot dit eindpunt moet worden gemarkeerd als niet in orde. Als er geen waarde is opgegeven, wordt de standaardwaarde van 3.
* **Time-out voor testen**. Deze eigenschap geeft u de hoeveelheid tijd die het testinterval Traffic Manager-agent moet worden gewacht voordat u overweegt een fout te controleren wanneer een statustest voor de controle wordt verzonden naar het eindpunt. Als het scannen van Interval is ingesteld op 30 seconden, kunt u de time-outwaarde tussen 5 en 10 seconden instellen. Als er geen waarde is opgegeven, wordt een standaardwaarde van 10 seconden. Als het scannen van Interval is ingesteld op 10 seconden, kunt u de time-outwaarde tussen 5 en 9 seconden instellen. Als er geen time-outwaarde is opgegeven, wordt een standaardwaarde van 9 seconden.

    ![Eindpuntcontrole van Traffic Manager](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

    **Afbeelding:  Eindpuntcontrole van Traffic Manager**

## <a name="how-endpoint-monitoring-works"></a>Eindpuntbewaking werking

Als het controle-protocol is ingesteld als HTTP of HTTPS, maakt de testinterval Traffic Manager-agent een GET-aanvraag naar het eindpunt met het protocol, poort en relatief pad opgegeven. Als deze wordt in een reactie 200 OK, of een van de antwoorden geconfigureerd de ** verwachte statuscode * bereiken ** en vervolgens dit eindpunt is in orde beschouwd. Als het antwoord is een andere waarde, of als er geen reactie wordt ontvangen binnen de time-outperiode die is opgegeven, klikt u vervolgens de Traffic Manager probing agent opnieuw probeert overeenkomstig de instelling van het aantal verdragen van fouten (opnieuw probeert worden uitgevoerd als deze instelling 0 is). Als het aantal achtereenvolgende mislukte pogingen hoger dan de instelling aantal mislukte aanmeldingen getolereerd is, is dit eindpunt als niet in orde gemarkeerd. 

Als het controle-protocol TCP, initieert de testinterval Traffic Manager-agent een TCP-verbindingsaanvraag met behulp van de opgegeven poort. Als het eindpunt op de aanvraag met een antwoord om de verbinding te maken reageert, die controle van gatewayservicestatus is gemarkeerd als een succes en de testinterval Traffic Manager-agent de TCP-verbinding wordt hersteld. Als het antwoord is een andere waarde, of als er geen reactie wordt ontvangen binnen de time-outperiode opgegeven, de Traffic Manager probing agent opnieuw probeert overeenkomstig de instelling aantal mislukte aanmeldingen getolereerd is (opnieuw probeert zijn gemaakt als deze instelling 0 is). Als het aantal achtereenvolgende mislukte pogingen hoger dan de instelling aantal mislukte aanmeldingen getolereerd is, is dit eindpunt niet in orde gemarkeerd.

In alle gevallen Traffic Manager tests vanaf meerdere locaties en de bepaling opeenvolgende mislukte gebeurt binnen elke regio. Dit betekent ook dat eindpunten statuscontroles van Traffic Manager met een hogere frequentie dan de instelling die wordt gebruikt voor het scannen van Interval ontvangt.

>[!NOTE]
>Voor HTTP of HTTPS-protocol bewaking, is een gebruikelijk om aan de kant van het eindpunt voor het implementeren van een aangepaste pagina in uw toepassing - bijvoorbeeld /health.aspx. Met dit pad voor het bewaken, kunt u toepassingsspecifieke controles, zoals prestatiemeteritems te controleren of de controle van de beschikbaarheid van de database uitvoeren. Op basis van deze aangepaste controles, retourneert de pagina een juiste HTTP-statuscode.

Alle eindpunten in een Traffic Manager-profiel delen controle-instellingen. Als u nodig hebt met verschillende instellingen voor controle voor verschillende eindpunten, kunt u [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Status van eindpunt en het profiel

U kunt in- en uitschakelen van Traffic Manager-profielen en eindpunten. Echter, een wijziging in de Eindpuntstatus ook optreden als gevolg van Traffic Manager instellingen en processen geautomatiseerde.

### <a name="endpoint-status"></a>Eindpuntstatus

U kunt inschakelen of uitschakelen van een bepaald eindpunt. De onderliggende service, die mogelijk nog steeds in orde is, wordt niet beïnvloed. Wijzigen van de Eindpuntstatus bepaalt de beschikbaarheid van het eindpunt in Traffic Manager-profiel. Wanneer de Eindpuntstatus van een is uitgeschakeld, Traffic Manager controleert niet de status en het eindpunt is niet opgenomen in een DNS-antwoord.

### <a name="profile-status"></a>Status van het profiel

Met behulp van de instelling van de status van profiel, kunt u inschakelen of uitschakelen van een specifiek profiel. Terwijl de status van endpoint invloed is op één eindpunt, status van het profiel van invloed op het volledige profiel, inclusief alle eindpunten. Wanneer u een profiel uitschakelen, de eindpunten zijn niet ingeschakeld voor de gezondheid en geen eindpunten zijn opgenomen in een DNS-antwoord. Een [NXDOMAIN](https://tools.ietf.org/html/rfc2308) antwoordcode wordt geretourneerd voor de DNS-query.

### <a name="endpoint-monitor-status"></a>Status van eindpunt bewaken

Status monitor-eindpunt is een Traffic Manager gegenereerde waarde met de status van het eindpunt. U kunt geen deze instelling handmatig wijzigen. De status van de endpoint-monitor is een combinatie van de resultaten van eindpuntbewaking en de status van de geconfigureerde endpoint. De mogelijke waarden van status monitor-eindpunt worden weergegeven in de volgende tabel:

| Status van het profiel | Eindpuntstatus | Status van eindpunt bewaken | Opmerkingen |
| --- | --- | --- | --- |
| Uitgeschakeld |Ingeschakeld |Inactief |Het profiel is uitgeschakeld. Hoewel de Eindpuntstatus is ingeschakeld, wordt de status van het profiel (uitgeschakeld) voorrang. Eindpunten in uitgeschakelde profielen worden niet bewaakt. Een antwoord NXDOMAIN-antwoordcode wordt geretourneerd voor de DNS-query. |
| &lt;Alle&gt; |Uitgeschakeld |Uitgeschakeld |Het eindpunt is uitgeschakeld. Uitgeschakelde eindpunten worden niet bewaakt. Het eindpunt is niet opgenomen in de DNS-antwoorden, daarom is er geen verkeer ontvangen. |
| Ingeschakeld |Ingeschakeld |Online |Het eindpunt wordt gecontroleerd en in orde is. Het is opgenomen in de DNS-antwoorden en kan verkeer ontvangen. |
| Ingeschakeld |Ingeschakeld |Verminderd |Statuscontroles voor controle-eindpunt mislukken. Het eindpunt is niet opgenomen in de DNS-antwoorden en geen verkeer ontvangt. <br>Een uitzondering hierop is als alle eindpunten zijn gedegradeerd, in dat geval ze allemaal beschouwd als reactie op de query worden geretourneerd).</br>|
| Ingeschakeld |Ingeschakeld |CheckingEndpoint |Het eindpunt wordt bewaakt, maar de resultaten van de eerste test zijn nog niet ontvangen. CheckingEndpoint is een tijdelijke situatie die treedt meestal op onmiddellijk na het toevoegen of inschakelen van een eindpunt in het profiel. Een eindpunt in deze status is opgenomen in de DNS-antwoorden en kan verkeer ontvangen. |
| Ingeschakeld |Ingeschakeld |Gestopt |De cloud of -web-app die het eindpunt naar verwijst wordt niet uitgevoerd. Controleer de instellingen voor cloud-service of web-app. Dit kan ook gebeuren als het eindpunt van het type genest eindpunt en het onderliggende profiel is uitgeschakeld of niet actief is. <br>Een eindpunt met de status gestopt wordt niet gecontroleerd. Het is niet opgenomen in de DNS-antwoorden en geen verkeer ontvangt. Een uitzondering hierop is als alle eindpunten zijn gedegradeerd, in welk geval ze allemaal zal worden beschouwd als reactie op de query worden geretourneerd.</br>|

Zie voor meer informatie over hoe status monitor-eindpunt wordt berekend voor geneste eindpunten [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).

>[!NOTE]
> De status van een eindpunt gestopt bewaken vindt plaats op App Service als uw webtoepassing wordt niet uitgevoerd in de laag standaard of hoger. Zie voor meer informatie, [Traffic Manager-integratie met App Service](/azure/app-service/web-sites-traffic-manager).

### <a name="profile-monitor-status"></a>Status van het profiel

De monitor status van het profiel is een combinatie van de status van het geconfigureerde profiel en de waarden voor status monitor eindpunt voor alle eindpunten. De mogelijke waarden worden in de volgende tabel beschreven:

| Status van het profiel (zoals geconfigureerd) | Status van eindpunt bewaken | Status van het profiel | Opmerkingen |
| --- | --- | --- | --- |
| Uitgeschakeld |&lt;alle&gt; of een profiel met geen eindpunten zijn gedefinieerd. |Uitgeschakeld |Het profiel is uitgeschakeld. |
| Ingeschakeld |De status van ten minste één eindpunt is gedegradeerd. |Verminderd |Bekijk de waarden voor de status van afzonderlijke eindpunt om te bepalen welke eindpunten meer aandacht vereisen. |
| Ingeschakeld |De status van ten minste één eindpunt is Online. Er zijn geen eindpunten hebben een status van gedegradeerd. |Online |De service wordt verkeer accepteert. Er is geen verdere actie vereist. |
| Ingeschakeld |De status van ten minste één eindpunt is CheckingEndpoint. Er zijn geen eindpunten in de status Online of gedegradeerd. |CheckingEndpoints |De status van deze overgang treedt op wanneer een profiel als gemaakt of ingeschakeld. De status van het eindpunt wordt voor de eerste keer gecontroleerd. |
| Ingeschakeld |De status van alle eindpunten in het profiel zijn uitgeschakeld of gestopt, of het profiel bevat geen eindpunten zijn gedefinieerd. |Inactief |Er zijn geen eindpunten zijn actief, maar het profiel is nog steeds ingeschakeld. |

## <a name="endpoint-failover-and-recovery"></a>Eindpunt-failover en herstel

Traffic Manager controleert periodiek de status van elk eindpunt, inclusief niet in orde eindpunten. Traffic Manager detecteert wanneer een eindpunt in orde is en terug in rotatie gehaald.

Een eindpunt is niet in orde als een van de volgende gebeurtenissen optreedt:
- Als de controle-protocol HTTP of HTTPS is:
    - Een niet-200-respons, of een antwoord dat niet de status dat is opgegeven in de **code statusbereik verwacht** instelt, wordt ontvangen (met inbegrip van een andere 2xx-code of een 301/302-omleiding).
- Als de controle-protocol TCP is: 
    - Een antwoord dan ACK of SYN-ACK wordt ontvangen in reactie op de synchronisatie-aanvraag verzonden door Traffic Manager om een verbinding tot stand brengen.
- Een time-out opgetreden. 
- Alle andere verbindingsprobleem leidt tot het eindpunt wordt niet bereikbaar is.

Zie voor meer informatie over het oplossen van problemen met mislukte controles [probleemoplossing gedegradeerd op Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md). 

De tijdlijn van de volgende afbeelding is een gedetailleerde beschrijving van het bewakingsproces van Traffic Manager-eindpunt met de volgende instellingen: HTTP protocol bewaking is, het testinterval-interval is 30 seconden, het aantal verdragen fouten is 3, time-outwaarde is 10 seconden en de DNS TTL is 30 seconden.

![Traffic Manager-eindpunt failover en failback-reeks](./media/traffic-manager-monitoring/timeline.png)

**Afbeelding:  Traffic manager-eindpunt failovers en herstel-reeks**

1. **OPHALEN**. Voor elk eindpunt voert de Traffic Manager-controle van systeem een GET-aanvraag op het pad dat is opgegeven in de instellingen voor controle.
2. **200 OK of aangepaste code bereik opgegeven controle-instellingen voor Traffic Manager-profiel** . Het bewakingssysteem wordt verwacht dat een HTTP 200 OK of de of aangepaste code bereik opgegeven Traffic Manager-profiel bewaking instellingen dat wordt geretourneerd binnen 10 seconden. Wanneer deze dit antwoord ontvangt, wordt gedetecteerd dat de service beschikbaar is.
3. **30 seconden tussen controles**. De statuscontrole van het eindpunt wordt herhaald elke 30 seconden.
4. **Service is niet beschikbaar**. De service niet beschikbaar is. Traffic Manager weet niet tot de volgende statuscontrole.
5. **Probeert te krijgen tot de controlepad**. Het bewakingssysteem een GET-aanvraag wordt uitgevoerd, maar geen antwoord ontvangen binnen de time-outperiode van 10 seconden (u kunt ook een niet-200-respons kan worden ontvangen). Vervolgens probeert deze drie keer meer intervallen van 30 seconden. Als een van de pogingen geslaagd is, klikt u vervolgens het aantal pogingen wordt opnieuw ingesteld.
6. **De status ingesteld op gedegradeerd**. Na een storing op een vierde opeenvolgende markeert het systeem van toezicht de Eindpuntstatus niet beschikbaar als gedegradeerd.
7. **Verkeer wordt gewijzigd naar andere eindpunten**. De naamservers van Traffic Manager DNS worden bijgewerkt en Traffic Manager retourneert het eindpunt niet meer in reactie op DNS-query's. Nieuwe verbindingen worden doorgestuurd naar de andere beschikbare eindpunten. Vorige DNS-antwoorden die dit eindpunt kunnen echter nog steeds worden opgeslagen in de recursieve DNS-servers en DNS-clients. Clients echter ook doorgaan met het eindpunt totdat de DNS-cache verloopt. Als de DNS-cache is verlopen, kunnen clients nieuwe DNS-query's en worden doorgestuurd naar verschillende eindpunten. De Cacheduur wordt bepaald door de TTL-instelling in het Traffic Manager-profiel, bijvoorbeeld: 30 seconden.
8. **Statuscontroles blijven**. Traffic Manager blijft om te controleren of de status van het eindpunt heeft de status gedegradeerd. Traffic Manager vaststelt wanneer het eindpunt van de status weer.
9. **Service weer online komt**. De service beschikbaar. Het eindpunt wordt de status gedegradeerd in Traffic Manager bewaard totdat het controlesysteem de statuscontrole van de volgende.
10. **Verkeer naar de service wordt hervat**. Traffic Manager een GET-aanvraag verzendt en ontvangt een reactie van de status van 200 OK. De service heeft een goede status geretourneerd. De naamservers van Traffic Manager worden bijgewerkt, en ze beginnen aan de hand van de service DNS-naam in DNS-antwoorden. Verkeer retourneert naar het eindpunt als DNS-reacties uit het cachegeheugen die rendement andere eindpunten vervalt, en als de bestaande verbindingen met andere eindpunten worden beëindigd.

    > [!NOTE]
    > Traffic Manager werkt op DNS-niveau en kan niet deze invloed hebben op de bestaande verbindingen met een willekeurig eindpunt. Wanneer het verkeer tussen de eindpunten (hetzij door gewijzigde profielinstellingen, hetzij tijdens failover en failback) stuurt, stuurt Traffic Manager nieuwe verbindingen met beschikbare eindpunten. Andere eindpunten kunnen echter nog steeds verkeer via bestaande verbindingen ontvangen totdat deze sessies worden beëindigd. Toepassingen moeten de sessieduur van de gebruikt in combinatie met elk eindpunt beperken zodat verkeer naar het leegmaken van bestaande verbindingen.

## <a name="traffic-routing-methods"></a>Routeringsmethoden voor verkeer

Wanneer een eindpunt de status gedegradeerd heeft, is het niet meer in reactie op DNS-query's geretourneerd. In plaats daarvan is een alternatieve eindpunt gekozen en geretourneerd. De verkeersrouteringsmethode geconfigureerd in het profiel bepaalt hoe het andere eindpunt is gekozen.

* **Prioriteit**. Eindpunten vormen een gerangschikte lijst geopend. De eerste beschikbare eindpunt in de lijst wordt altijd geretourneerd. Als de Eindpuntstatus van een is verminderd, wordt het eindpunt van de volgende beschikbare geretourneerd.
* **Weighted**. Elk willekeurig eindpunt dat beschikbaar wordt gekozen in willekeurige volgorde op basis van hun toegewezen gewicht en het gewicht van de andere beschikbare eindpunten.
* **Performance**. Het eindpunt die het dichtst bij de eindgebruiker wordt geretourneerd. Als dit eindpunt niet beschikbaar is, wordt verkeer in Traffic Manager verplaatst naar de eindpunten in de volgende dichtstbijzijnde Azure-regio. U kunt alternatieve failover plannen voor verkeer routeren configureren met behulp van [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Geografische**. Het eindpunt dat is toegewezen aan het leveren van de geografische locatie op basis van de queryaanvraag van IP wordt geretourneerd. Als dit eindpunt niet beschikbaar is, een ander eindpunt kan niet worden geselecteerd voor failover, omdat een geografische locatie kan worden toegewezen aan één eindpunt in een profiel alleen (meer informatie vindt u in de [Veelgestelde vragen over](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Als een best practice bij het gebruik van de geografische routering, raden we onze klanten geneste Traffic Manager-profielen met meer dan één eindpunt gebruiken als de eindpunten van het profiel.
* **Meerdere waarden** meerdere eindpunten die zijn toegewezen aan IPv4/IPv6-adressen worden geretourneerd. Wanneer een query wordt ontvangen voor dit profiel, gezonde eindpunten worden geretourneerd op basis van de **Maximum record aantal werkers** waarde die u hebt opgegeven. Het aantal antwoorden is twee eindpunten.
* **Subnet** het eindpunt dat is toegewezen aan een reeks IP-adresbereiken wordt geretourneerd. Wanneer een aanvraag wordt ontvangen van IP-adres, het eindpunt heeft geretourneerd die voor dat IP-adres is toegewezen. 

Zie voor meer informatie, [methoden voor Traffic Manager traffic routing](traffic-manager-routing-methods.md).

> [!NOTE]
> Een uitzondering op het normale gedrag van de routering van verkeer treedt op wanneer alle in aanmerking komende eindpunten een gedegradeerde status bevindt hebben. Traffic Manager maakt een "aanbevolen inspanning" proberen en *reageert als de eindpunten voor de status gedegradeerd daadwerkelijk in een online status zijn*. Dit gedrag is beter de alternatieve zijn zou om terug te keren niet elk willekeurig eindpunt dat in het DNS-antwoord. Uitgeschakeld of gestopt eindpunten worden niet bewaakt, dus ze worden niet beschouwd als in aanmerking komen voor verkeer.
>
> Dit probleem wordt meestal veroorzaakt door onjuiste configuratie van de service, zoals:
>
> * Een toegangsbeheerlijst [] blokkeren van de statuscontrole van Traffic Manager.
> * Een onjuiste configuratie van de controle-poort of -protocol in de Traffic manager-profiel.
>
> Het gevolg is van dit gedrag is dat als statuscontroles van Traffic Manager niet correct zijn geconfigureerd, wordt deze mogelijk weergegeven van het verkeer routeren alsof Traffic Manager *is* goed werkt. Echter, in dit geval eindpunt-failover kan niet worden uitgevoerd die van invloed is op algemene beschikbaarheid. Het is belangrijk om te controleren dat het profiel ziet u een Online status, niet de status gedegradeerd. Een Online status geeft aan dat de statuscontroles van Traffic Manager werkt zoals verwacht.

Voor meer informatie over het oplossen van problemen mislukt statuscontroles, Zie [probleemoplossing gedegradeerd op Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Volgende stappen

Informatie over [hoe Traffic Manager werkt](traffic-manager-how-it-works.md)

Meer informatie over de [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md) ondersteund door Traffic Manager

Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-manage-profiles.md)

[Problemen met de status gedegradeerd oplossen](traffic-manager-troubleshooting-degraded.md) op een Traffic Manager-eindpunt
