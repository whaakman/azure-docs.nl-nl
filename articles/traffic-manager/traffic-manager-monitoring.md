---
title: Azure Traffic Manager eindpuntcontrole | Microsoft Docs
description: In dit artikel vindt u informatie over hoe Traffic Manager eindpuntcontrole en automatische eindpunt failover gebruikt ter ondersteuning van Azure gebruikers hoge beschikbaarheid toepassingen implementeren
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: 3b30aa04854b779c25582abafc0f9ebba65b71ba
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>Eindpuntcontrole van Traffic Manager

Azure Traffic Manager omvat ingebouwde eindpuntcontrole en automatische eindpunt failover. Deze functie kunt u hoge beschikbaarheid toepassingen leveren die flexibel omgaan met eindpunt mislukt, waaronder Azure-regio fouten.

## <a name="configure-endpoint-monitoring"></a>Eindpunt bewaking configureren

Voor het eindpunt bewaking configureert, moet u de volgende instellingen op uw Traffic Manager-profiel opgeven:

* **Protocol**. Kies HTTP, HTTPS of TCP als protocol gebruikt bij het scannen van uw eindpunt van Traffic Manager, om te controleren van de status. HTTPS-bewaking niet gecontroleerd dat of uw SSL-certificaat geldig is--wordt alleen gecontroleerd of het certificaat aanwezig is.
* **Poort**. Kies de poort die wordt gebruikt voor de aanvraag.
* **Pad**. Deze configuratieinstelling is alleen geldig voor de protocollen HTTP en HTTPS, voor welke opgeven van het pad instelling vereist is. Deze instelling voor de TCP-protocol resulteert in een fout bewaking bieden. Geeft het relatieve pad en de naam van de webpagina die of het bestand dat gebruikmaakt van de bewaking voor TCP-protocol. Een slash (/) is een geldige invoer voor het relatieve pad. Deze waarde geeft aan dat het bestand in de hoofdmap (standaard is).
* **Probing Interval**. Deze waarde geeft aan hoe vaak een eindpunt voor de status van een zoek Traffic Manager-agent wordt gecontroleerd. U kunt hier twee waarden opgeven: 30 seconden (normaal zoeken) en 10 seconden (snel zoeken). Als geen waarden zijn opgegeven, wordt het profiel wordt ingesteld op een standaardperiode van 30 seconden. Ga naar de [Traffic Manager prijzen](https://azure.microsoft.com/pricing/details/traffic-manager) pagina voor meer informatie over snelle Zoek prijzen.
* **Aantal mislukte verdragen**. Deze waarde bepaalt hoeveel mislukte maximaal een zoek Traffic Manager-agent wordt toegestaan voordat dat eindpunt als beschadigd gemarkeerd. De waarde ervan kan variëren tussen 0 en 9. Een waarde 0 betekent dat één bewaking fout kan leiden tot dat eindpunt zijn gemarkeerd als beschadigd. Als geen waarde opgeeft, wordt de standaardwaarde van 3.
* **Time-out voor bewaking**. Deze eigenschap geeft u de hoeveelheid tijd die de zoek Traffic Manager-agent moet worden gewacht voordat het overwegen van een storing controleren wanneer een selectievakje health test wordt verzonden naar het eindpunt. Als het scannen van Interval is ingesteld op 30 seconden, kunt u de time-outwaarde tussen 5 en 10 seconden instellen. Als geen waarde opgeeft, wordt een standaardwaarde van 10 seconden. Als het scannen van Interval is ingesteld op 10 seconden, kunt u de time-outwaarde tussen 5 en 9 seconden instellen. Als er geen time-outwaarde is opgegeven, wordt een standaardwaarde van 9 seconden.

![Eindpuntcontrole van Traffic Manager](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**Afbeelding 1: Traffic Manager-eindpuntcontrole**

## <a name="how-endpoint-monitoring-works"></a>Hoe eindpuntcontrole werkt

Als het controle-protocol is ingesteld als HTTP of HTTPS, doet de zoek Traffic Manager-agent een GET-aanvraag naar het eindpunt met het protocol, de poort en het opgegeven relatieve pad. Als er weer een antwoord 200 OK, is dat eindpunt in orde beschouwd. Als het antwoord is een andere waarde, of als er geen reactie wordt ontvangen binnen de time-outperiode die is opgegeven, wordt de Traffic Manager probing agent probeert opnieuw overeenkomstig de instelling aantal fouten verdragen is (geen probeert opnieuw worden uitgevoerd als deze instelling 0 is). Als het aantal achtereenvolgende mislukte pogingen hoger dan de instelling aantal fouten toegestaan is, is dat eindpunt als beschadigd gemarkeerd. 

Als het controle-protocol TCP, initieert de zoek Traffic Manager-agent een TCP-verbindingsaanvraag met behulp van de opgegeven poort. Als het eindpunt op de aanvraag met een antwoord reageert voor het maken van de verbinding, die de statuscontrole is gemarkeerd als een is voltooid en de zoek Traffic Manager-agent de TCP-verbinding wordt hersteld. Als het antwoord is een andere waarde, of als er geen reactie wordt ontvangen binnen de time-outperiode opgegeven, de Traffic Manager probing agent probeert opnieuw overeenkomstig de instelling aantal fouten verdragen is (geen probeert opnieuw worden gemaakt als deze instelling 0 is). Als het aantal achtereenvolgende mislukte pogingen hoger dan de instelling aantal fouten toegestaan is, is dat eindpunt niet in orde gemarkeerd.

In alle gevallen Traffic Manager-tests vanaf meerdere locaties en de bepaling opeenvolgende mislukte gebeurt er in elke regio. Dit betekent ook dat eindpunten statuscontroles van Traffic Manager met een hogere frequentie dan de instelling die wordt gebruikt voor het scannen van Interval ontvangen.

>[!NOTE]
>Voor HTTP of HTTPS-protocol bewaking, is een gebruikelijk om aan de kant van het eindpunt voor het implementeren van een aangepaste pagina in uw toepassing - bijvoorbeeld /health.aspx. Dit pad gebruiken voor bewaking, kunt u toepassingsspecifieke controles, zoals prestatiemeteritems te controleren of de controle van de beschikbaarheid van de database uitvoeren. Op basis van deze aangepaste controles, retourneert de pagina voor een juiste HTTP-statuscode.

Alle eindpunten in een Traffic Manager-profiel delen controle-instellingen. Als u gebruiken van andere controle-instellingen voor verschillende eindpunten wilt, kunt u [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Status endpoint en profiel

U kunt inschakelen en uitschakelen van Traffic Manager-profielen en eindpunten. Echter, een wijziging in de status van endpoint ook optreden als gevolg van Traffic Manager instellingen en processen geautomatiseerde.

### <a name="endpoint-status"></a>Status endpoint

U kunt in- of uitschakelen van een specifieke eindpunt. De onderliggende service, die nog steeds mogelijk in orde, wordt niet beïnvloed. Wijzigen van de Eindpuntstatus bepaalt de beschikbaarheid van het eindpunt in Traffic Manager-profiel. Wanneer de Eindpuntstatus van een is uitgeschakeld, Traffic Manager controleert de status niet en het eindpunt is niet opgenomen in een DNS-antwoord.

### <a name="profile-status"></a>Status van het profiel

U gebruikt de Profielinstelling van de status, kunt u in- of uitschakelen van een bepaald profiel. Terwijl de status van endpoint invloed is op één eindpunt, status van het profiel van invloed op het volledige profiel, inclusief alle eindpunten. Wanneer u een profiel uitschakelt, wordt de eindpunten zijn niet ingeschakeld voor de gezondheid en er zijn geen eindpunten zijn opgenomen in een DNS-antwoord. Een [NXDOMAIN](https://tools.ietf.org/html/rfc2308) antwoordcode wordt geretourneerd voor de DNS-query.

### <a name="endpoint-monitor-status"></a>Status van endpoint-monitor

Status van endpoint-monitor is een Traffic Manager gegenereerde waarde geeft de status van het eindpunt. U kunt geen deze instelling handmatig wijzigen. De status van de endpoint-monitor is een combinatie van de resultaten van eindpuntcontrole en de status van de geconfigureerde endpoint. De mogelijke waarden van de status van endpoint-monitor worden weergegeven in de volgende tabel:

| Status van het profiel | Status endpoint | Status van endpoint-monitor | Opmerkingen |
| --- | --- | --- | --- |
| Uitgeschakeld |Ingeschakeld |Inactieve |Het profiel is uitgeschakeld. Hoewel de Eindpuntstatus is ingeschakeld, wordt de status van het profiel (uitgeschakeld) voorrang. Eindpunten in uitgeschakelde profielen niet worden bewaakt. Een antwoord NXDOMAIN code wordt geretourneerd voor de DNS-query. |
| &lt;alle&gt; |Uitgeschakeld |Uitgeschakeld |Het eindpunt is uitgeschakeld. Uitgeschakelde eindpunten niet worden bewaakt. Het eindpunt is niet opgenomen in de DNS-antwoorden, daarom wordt er geen verkeer ontvangen. |
| Ingeschakeld |Ingeschakeld |Online |Het eindpunt wordt bewaakt en is in orde. Het is opgenomen in de DNS-antwoorden en verkeer kan ontvangen. |
| Ingeschakeld |Ingeschakeld |Gedegradeerd |Bewaking statuscontroles eindpunt mislukken. Het eindpunt is niet opgenomen in de DNS-antwoorden en geen verkeer ontvangt. <br>Een uitzondering hierop is als alle eindpunten zijn gedegradeerd, in welk geval ze allemaal beschouwd als worden geretourneerd in antwoord op de query).</br>|
| Ingeschakeld |Ingeschakeld |CheckingEndpoint |Het eindpunt wordt bewaakt, maar de resultaten van de eerste test hebben nog niet ontvangen. CheckingEndpoint is een tijdelijke status dat meestal onmiddellijk na het toevoegen of een eindpunt in het profiel inschakelen. Een eindpunt in deze status is opgenomen in de DNS-antwoorden en verkeer kan ontvangen. |
| Ingeschakeld |Ingeschakeld |Stopped |De cloud service of web-app die het eindpunt naar het verwijst wordt niet uitgevoerd. Controleer de instellingen voor cloud service of web-app. Dit kan ook gebeuren als het eindpunt van genest type eindpunt en het onderliggende profiel is uitgeschakeld of niet actief is. <br>Een eindpunt met de status gestopt wordt niet gecontroleerd. Het is niet opgenomen in de DNS-antwoorden en geen verkeer ontvangt. Een uitzondering hierop is als alle eindpunten zijn gedegradeerd, in welk geval ze allemaal beschouwd moeten worden geretourneerd in antwoord op de query.</br>|

Zie voor meer informatie over hoe de status van endpoint-monitor wordt berekend voor geneste eindpunten [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Status van het profiel

De monitor status van het profiel is een combinatie van de status geconfigureerd profiel en de waarden eindpunt monitor status voor alle eindpunten. De mogelijke waarden worden in de volgende tabel beschreven:

| Status van het profiel (zoals geconfigureerd) | Status van endpoint-monitor | Status van het profiel | Opmerkingen |
| --- | --- | --- | --- |
| Uitgeschakeld |&lt;eventuele&gt; of een profiel waarvoor geen eindpunten. |Uitgeschakeld |Het profiel is uitgeschakeld. |
| Ingeschakeld |De status van ten minste één eindpunt is gedegradeerd. |Gedegradeerd |Bekijk de waarden voor de status van afzonderlijke eindpunt om te bepalen welke eindpunten meer aandacht vereisen. |
| Ingeschakeld |De status van ten minste één eindpunt is Online. Er zijn geen eindpunten hebben gedegradeerd status. |Online |De service accepteert verkeer. Er is geen verdere actie vereist. |
| Ingeschakeld |De status van ten minste één eindpunt is CheckingEndpoint. Er zijn geen eindpunten status Online of gedegradeerd. |CheckingEndpoints |De status van deze overgang treedt op wanneer een profiel als gemaakt of ingeschakeld. De status van het eindpunt wordt voor het eerst gecontroleerd. |
| Ingeschakeld |De status van alle eindpunten in het profiel zijn uitgeschakeld of gestopt, of het profiel bevat geen eindpunten zijn gedefinieerd. |Inactieve |Er zijn geen eindpunten actief, maar nog steeds het profiel is ingeschakeld. |

## <a name="endpoint-failover-and-recovery"></a>Eindpunt failovers en herstel

Traffic Manager controleert periodiek de status van elk eindpunt, inclusief slecht eindpunten. Traffic Manager detecteert wanneer een eindpunt in orde wordt en deze terug naar worden gedraaid worden.

Een eindpunt is niet in orde als een van de volgende gebeurtenissen optreedt:
- Als het controle-protocol HTTP of HTTPS is:
    - Er is een niet-200-antwoord ontvangen (met inbegrip van een andere 2xx-code of een 301/302-omleiding).
- Als het controle-protocol TCP is: 
    - Een antwoord dan ACK of SYN-ACK is ontvangen als antwoord op de synchronisatie-aanvraag verzonden door Traffic Manager om een verbinding tot stand brengen.
- Time-out. 
- Alle andere verbindingsprobleem die voortvloeien uit het eindpunt wordt niet bereikbaar.

Zie voor meer informatie over het oplossen van problemen mislukte controles [probleemoplossing gedegradeerd status op Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md). 

De volgende tijdlijn in afbeelding 2 is een gedetailleerde beschrijving van het bewakingsproces van Traffic Manager-eindpunt met de volgende instellingen: HTTP protocol bewaking is, zoek-interval is 30 seconden, aantal verdragen fouten is 3, time-outwaarde is 10 seconden en DNS TTL is 30 seconden.

![Traffic Manager-eindpunt failover en failback-reeks](./media/traffic-manager-monitoring/timeline.png)

**Afbeelding 2: Traffic manager-eindpunt failovers en herstel reeks**

1. **OPHALEN VAN**. Voor elk eindpunt voert de Traffic Manager bewakingssysteem een GET-aanvraag op het pad dat is opgegeven in de controle-instellingen.
2. **200 OK**. Het bewakingssysteem verwacht een HTTP 200 OK-bericht moet worden geretourneerd binnen 10 seconden. Wanneer deze dit antwoord ontvangt, herkent het dat de service beschikbaar is.
3. **30 seconden tussen controles**. De statuscontrole van het eindpunt wordt herhaald elke 30 seconden.
4. **Service is niet beschikbaar**. De service niet beschikbaar. Traffic Manager weet niet tot de volgende statuscontrole.
5. **Probeert te krijgen tot het controlepad**. Het bewakingssysteem voert een GET-aanvraag, maar geen antwoord ontvangen binnen de time-outperiode van 10 seconden (u kunt ook een niet-200-respons kan worden ontvangen). Vervolgens wordt er meer driemaal geprobeerd met een interval van 30 seconden. Als een van de pogingen geslaagd is, klikt u vervolgens het aantal pogingen wordt opnieuw ingesteld.
6. **De status ingesteld op gedegradeerd**. Na een vierde opeenvolgende mislukte markeert het bewakingssysteem de Eindpuntstatus niet beschikbaar als gedegradeerd.
7. **Verkeer wordt verder met de andere eindpunten**. Het Traffic Manager-DNS-naamservers worden bijgewerkt en Traffic Manager retourneert het eindpunt niet langer in reactie op DNS-query's. Nieuwe verbindingen worden omgeleid naar de andere, beschikbare eindpunten. Vorige DNS-antwoorden met dit eindpunt is echter nog steeds van cache recursieve DNS-servers en DNS-clients. Clients worden nog steeds het eindpunt te gebruiken totdat de DNS-cache verloopt. Als de DNS-cache is verlopen, kunnen clients nieuwe DNS-query's maken en worden doorgestuurd naar verschillende eindpunten. De Cacheduur van de wordt beheerd door de TTL-instelling in het Traffic Manager-profiel, bijvoorbeeld: 30 seconden.
8. **Status controleert gaan**. Traffic Manager blijft de status van het eindpunt controleren terwijl het de status gedegradeerd heeft. Traffic Manager detecteert wanneer het eindpunt health weer.
9. **Service weer online wordt gezet**. De service beschikbaar. Het eindpunt wordt de status gedegradeerd in Traffic Manager bewaard totdat het bewakingssysteem de volgende statuscontrole voert.
10. **Het verkeer naar de service wordt hervat**. Traffic Manager een GET-aanvraag verzendt en ontvangt van een statusantwoord 200 OK. De service weer in een foutloze status gebracht. Het Traffic Manager-naamservers worden bijgewerkt en deze begint met de hand van de service DNS-naam in DNS-antwoorden. Verkeer retourneert met het eindpunt als DNS-antwoorden in de cache dat rendement andere eindpunten vervalt, en als de bestaande verbindingen met andere eindpunten zijn beëindigd.

    > [!NOTE]
    > Traffic Manager werkt op het niveau van de DNS-en kan niet het bestaande verbindingen met een willekeurig eindpunt beïnvloeden. Wanneer het verkeer tussen de eindpunten (ofwel door gewijzigde profielinstellingen, of tijdens een failover en failback) wordt verwezen, is het Traffic Manager zorgt ervoor dat nieuwe verbindingen met beschikbare eindpunten. Andere eindpunten mogelijk blijven echter verkeer via bestaande verbindingen ontvangen totdat deze sessies worden beëindigd. Toepassingen moeten de sessieduur gebruikt met elk eindpunt beperken zodat verkeer naar het leegmaken van bestaande verbindingen.

## <a name="traffic-routing-methods"></a>Voor verkeersroutering methoden

Wanneer een eindpunt de status gedegradeerd heeft, wordt het niet langer geretourneerd in antwoord op de DNS-query's. In plaats daarvan is een alternatieve eindpunt gekozen en geretourneerd. De methode verkeer routering is geconfigureerd in het profiel bepaalt hoe het andere eindpunt is gekozen.

* **Prioriteit**. Eindpunten vormen een geprioriteerde lijst. Het eerste beschikbare eindpunt in de lijst wordt altijd geretourneerd. Als de Eindpuntstatus van een is gedegradeerd, wordt het volgende beschikbare eindpunt geretourneerd.
* **Gewogen**. Alle beschikbare eindpunt wordt gekozen in willekeurige volgorde op basis van hun toegewezen gewichten en het gewicht van de andere beschikbare eindpunten.
* **Prestaties**. Het eindpunt die het dichtst bij de eindgebruiker wordt geretourneerd. Als dat eindpunt niet beschikbaar is, wordt het Traffic Manager verkeer verplaatst naar de eindpunten in de volgende dichtstbijzijnde Azure-regio. U kunt plannen voor het verkeer routeren alternatieve failover configureren met behulp van [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Geografische**. Het eindpunt voor de geografische locatie op basis van de aanvraag toegewezen IP's wordt geretourneerd. Als dat eindpunt niet beschikbaar is, een ander eindpunt niet worden geselecteerd failover, omdat een geografische locatie kan alleen worden toegewezen aan één eindpunt in een profiel (meer informatie vindt u in de [Veelgestelde vragen over](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Als een best practice bij gebruik van geografische routering, wordt aangeraden klanten geneste Traffic Manager-profielen met meer dan één eindpunt gebruiken als de eindpunten van het profiel.

Zie voor meer informatie [Traffic Manager-verkeersroutering methoden](traffic-manager-routing-methods.md).

> [!NOTE]
> Een uitzondering op normaal gedrag traffic routing treedt op wanneer alle in aanmerking komende eindpunten een gedegradeerde status hebben. Traffic Manager wordt geprobeerd een 'best effort' en *reageert alsof de eindpunten voor de status gedegradeerd daadwerkelijk zich in een online status*. Dit gedrag is beter, die zijn naar een willekeurig eindpunt niet in het DNS-antwoord retourneren. Niet uitgeschakeld of gestopt eindpunten worden bewaakt, dus deze worden niet beschouwd als in aanmerking komen voor verkeer.
>
> Dit probleem wordt meestal veroorzaakt door onjuiste configuratie van de service, zoals:
>
> * Een toegangsbeheerlijst [] blokkeert de Traffic Manager-status wordt gecontroleerd.
> * Een onjuiste configuratie van de controle-poort of -protocol in Traffic manager-profiel.
>
> Het gevolg is van dit probleem is dat als statuscontroles Traffic Manager niet correct zijn geconfigureerd, lijkt het alsof van het verkeer routering alsof Traffic Manager *is* goed werkt. In dit geval kan niet eindpunt failover gebeurt echter algemene beschikbaarheid betrekking heeft. Het is belangrijk om te controleren dat het profiel ziet u een Online status, niet de status gedegradeerd. Een Online status geeft aan dat de Traffic Manager-statuscontroles werken zoals verwacht.

Voor meer informatie over het oplossen van problemen mislukt statuscontroles, Zie [probleemoplossing gedegradeerd status op Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de werking van Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager

Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-manage-profiles.md)

[Problemen met de status gedegradeerd](traffic-manager-troubleshooting-degraded.md) op een Traffic Manager-eindpunt
