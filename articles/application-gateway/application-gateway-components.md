---
title: Azure Application Gateway-onderdelen
description: In dit artikel bevat informatie over de verschillende onderdelen in Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: a7b4dd14cd6270838fde33b0b62ec5adeceb3434
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247015"
---
# <a name="application-gateway-components"></a>Application gateway-onderdelen

 Een application gateway fungeert als de één contactpunt voor clients. Het verdeeld binnenkomende verkeer van de toepassing over meerdere back endpools, zoals virtuele Azure-machines, virtuele-machineschaalsets, App Services of on-premises/externe servers. U doet dit door deze verschillende onderdelen die worden beschreven in dit artikel gebruikt.

![Application-gateway-onderdelen](.\media\application-gateway-components\application-gateway-components.png)

## <a name="frontend-ip-address"></a>IP-adres voor front-end

Een front-end-IP-adres is het IP-adres dat is gekoppeld aan de toepassingsgateway. U kunt de toepassingsgateway om ofwel een openbaar IP-adres of een privé IP-adres of beide. Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway. Uw virtuele netwerk en openbare IP-adres moeten zich op dezelfde locatie bevinden als uw application gateway.

### <a name="static-vs-dynamic-public-ip-address"></a>Statische versus dynamische openbare IP-adres

De v1-SKU biedt ondersteuning voor statische interne IP-adressen, maar biedt geen ondersteuning voor statische openbare IP-adressen. Het VIP kunt wijzigen als de toepassingsgateway is gestopt en gestart. De DNS-naam die is gekoppeld aan de application gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

De v2-SKU van Application Gateway biedt ondersteuning voor statische openbare IP-adressen, evenals de statische interne IP-adressen. Slechts één openbaar IP-adres of een privé IP-adres wordt ondersteund.

Een Frontend-IP-adres is gekoppeld aan een *listener* nadat deze is gemaakt. Een listener controleert op inkomende aanvragen via het Frontend-IP-adres.

## <a name="listeners"></a>Listeners

Voordat u met behulp van uw application gateway begint, moet u een of meer listeners toevoegen. Een listener is een logische eenheid die wordt gecontroleerd op binnenkomende verbindingsaanvragen en de aanvragen worden geaccepteerd als het protocol, de poort, de host en de IP-adres met de listenerconfiguratie overeenkomt. Er kunnen zich meerdere listeners die is gekoppeld aan een application gateway en ze kunnen worden gebruikt voor hetzelfde protocol. Nadat de listener binnenkomende aanvragen van clients detecteert, stuurt de Application Gateway deze aanvragen naar de back-endservers in de back-endadresgroep met behulp van de aanvraag routeringsregels toe die u definieert voor de listener die de inkomende aanvraag ontvangen.

Listeners ondersteunen de volgende poorten en protocollen:

### <a name="ports"></a>Poorten

Dit is de poort waarop de listener voor de clientaanvraag luistert. U kunt poorten tussen 1 en 65502 voor V1-SKU's en 1-65199 voor V2 SKU configureren.

### <a name="protocols"></a>Protocollen

Application gateway ondersteunt de volgende vier protocollen: HTTP, HTTPS, HTTP/2 en WebSocket

Wanneer u de listener configureert, moet u kiezen tussen de protocollen HTTP en HTTPS. Application gateway biedt systeemeigen ondersteuning voor de WebSockets en HTTP/2-protocollen. U kunt WebSockets gebruiken met HTTP en HTTPS-listeners.

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment codevoorbeeld laat zien hoe u deze kunt inschakelen:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

Websocket-ondersteuning is standaard ingeschakeld. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen.

U kunt een HTTPS-listener voor SSL-beëindiging. Een HTTPS-listener verplaatst de versleuteling en ontsleuteling werken aan uw toepassingsgateway zodat uw webservers worden niet als enige belast door die overhead. Uw toepassingen zijn vervolgens kunt richten op hun zakelijke logica.

U moet ten minste één SSL-servercertificaat implementeren voor een HTTPS-listener. Zie voor meer informatie, [het configureren van SSL-beëindiging](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

### <a name="custom-error-pages"></a>Aangepaste foutpagina's

Application gateway kunt u aangepaste foutpagina's in plaats van standaard foutpagina's maken. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken. Application-gateway kan een aangepaste foutpagina weergegeven wanneer een aanvraag tijdelijk niet bereikbaar voor de back-end. Zie voor meer informatie, [aangepaste foutpagina's voor uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Typen listeners

Er zijn twee soorten listeners:

- **Basic**: Dit type listener luistert naar een enkel domein-site waarop het heeft een DNS-toewijzing van één IP-adres van de toepassingsgateway. Deze listenerconfiguratie is vereist wanneer u een enkele site achter een application gateway host.
- **Meerdere sites**: Deze listenerconfiguratie is vereist wanneer u meer dan één webtoepassing op de dezelfde toepassingsgateway-exemplaar configureren. Hiermee kunt u een efficiëntere topologie voor uw implementaties configureren door maximaal 100 websites toe te voegen aan één toepassingsgateway. Elke website kan worden omgeleid naar een eigen back-endpool. Bijvoorbeeld:  Voor drie subdomeinen: abc.contoso.com, xyz.contoso.com en pqr.contoso.com die verwijst naar het IP-adres van de toepassingsgateway. Maak drie listeners van het type *multi-site* en elke listener configureren voor de desbetreffende poort en protocol instellen.

Nadat u een listener maakt, koppelt u deze aan een aanvraag routeringsregel waarmee wordt bepaald hoe de ontvangen op de listener-aanvragen worden doorgestuurd naar de back-end.

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst. Multi-site-listeners moeten worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.

## <a name="request-routing-rule"></a>Regel voor het doorsturen van aanvragen

Dit is het belangrijkste onderdeel van de toepassingsgateway en bepaalt hoe het verkeer op de listener die zijn gekoppeld aan deze regel wordt gerouteerd. De regel verbindt de listener, de back-endserverpool en de back-end-HTTP-instellingen. Hiermee definieert u welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Een listener kan worden gekoppeld aan slechts één regel.

Er zijn twee typen van regels voor het doorsturen van aanvraag:

- **Basic:** Alle aanvragen op de bijbehorende listener (bijvoorbeeld: blog.contoso.com/*) worden doorgestuurd naar de gekoppelde back-endadresgroep met behulp van de bijbehorende HTTP-instelling.
- **Op basis van pad:** Dit regeltype kunt u de aanvragen versturen op de bijbehorende listener aan een specifieke back endadresgroep op basis van de URL in de aanvraag. Als het pad van de URL in een aanvraag overeenkomt met het padpatroon in een pad gebaseerde regel, wordt de aanvraag doorgestuurd met behulp van die regel. De pad-patroon wordt alleen toegepast op het pad van de URL, niet naar de queryparameters.

Als het pad van de URL van een aanvraag voor een listener komt niet overeen met een van de regels op basis van het pad, wordt de aanvraag wordt doorgestuurd naar de *standaard* back-endpool en de *standaard* HTTP-instellingen.

Regels worden verwerkt in de volgorde waarin die ze zijn geconfigureerd. Het verdient aanbeveling dat multi-site-regels zijn geconfigureerd voordat basic regels om te verminderen de kans dat verkeer wordt doorgestuurd naar de onjuiste back-end als de basisregel wordt gezocht naar verkeer op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

### <a name="redirection-support"></a>Ondersteuning voor omleiding

De regel voor het doorsturen van aanvraag kunt u omleiden van verkeer op de application gateway. Dit is een algemeen omleidingsmechanisme, zodat u op basis van regels kunt omleiden van en naar elke poort die u gebruikt. Dit kan helpen u bij het inschakelen van automatische HTTP naar HTTPS-omleiding om te controleren of dat alle communicatie tussen een toepassing en uw gebruikers vindt plaats via een gecodeerde pad. U kunt de doel-listener of een externe site voor de omleiding die u wilt opgeven. De configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en de query-tekenreeks toe te voegen aan de omgeleide URL. U kunt ook kiezen of omleiding een tijdelijke (HTTP-statuscode 302) of een permanente omleiding (HTTP-statuscode 301 is). Wanneer u een basisregel, wordt de configuratie van de omleidings-is gekoppeld aan de listener van een bron en is een algemene omleiding. Als u een pad gebaseerde regel gebruikt, wordt de configuratie van de omleidings-is gedefinieerd in de URL-path-map. Zodat deze alleen van toepassing op het padgebied van het specifieke van een site. Zie voor meer informatie, [omleiden van verkeer op uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="http-headers"></a>HTTP-headers

Toepassingsgateway voegt x doorgestuurd voor x-doorgestuurd-protocol en x-doorgestuurd-poort kopteksten in de aanvraag doorgestuurd naar de back-end. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn HTTP of HTTPS. X-doorgestuurd-poort geeft de poort waarop de aanvraag bij de application gateway is bereikt. Toepassingsgateway voegt ook X-oorspronkelijke-Host-header met de oorspronkelijke host-header waarmee de aanvraag is ontvangen. Deze header is handig in scenario's zoals de integratie van Azure-Website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end.

#### <a name="rewrite-http-headers"></a>HTTP-headers opnieuw genereren

Met behulp van de aanvraag routeringsregels kunt u toevoegen, verwijderen of bijwerken van HTTP (S) aanvragen en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en back-end-pools, via de application gateway. De headers kunnen niet alleen worden ingesteld op statische waarden, maar ook voor andere headers en servervariabelen belangrijk. Hierdoor kunt u uitvoeren van verschillende belangrijke use-cases, zoals het ophalen van IP-adres van de clients, verwijderen van gevoelige informatie over de back-end, toevoegen van extra beveiligingsmaatregelen, enzovoort. Zie voor meer informatie, [Herschrijf de HTTP-headers in uw application gateway](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP-instellingen

De application gateway routeert verkeer naar de back-endservers met het poortnummer, protocol en andere instellingen die zijn opgegeven in deze resource.

De back endadresgroepen ondersteunen de volgende poorten en protocollen:

### <a name="ports"></a>Poorten

Dit is de poort die de back-endservers luisteren op het verkeer dat afkomstig is van de toepassingsgateway. U kunt poorten tussen 1 en 65535 zijn.

### <a name="protocols"></a>Protocollen

Application gateway ondersteunt zowel HTTP als HTTPS-protocol voor Routeringsaanvragen voor de back-endservers.

Als de HTTP-protocol is gekozen, is het verkeer stromen niet versleuteld naar de back-endservers.

In deze gevallen waarbij niet-versleutelde communicatie met de back-endservers kan niet worden acceptabele gebruikt, moet u het HTTPS-protocol. Deze instelling in combinatie met HTTPS-protocol te kiezen in de listener, kunt u inschakelen [end-to-end SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hierdoor kunt u veilig gevoelige gegevens verzenden naar de back-end die is versleuteld. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

Hier kunt u verschillende mogelijkheden voor HTTP-instelling.

### <a name="cookie-based-session-affinity"></a>Cookies gebaseerde sessieaffiniteit

Deze functie is handig als u wilt behouden een gebruikerssessie op dezelfde server. Met behulp van door de gatewaybeheerde cookies kan Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde server leiden voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie.

Als de toepassing niet kan cookies gebaseerde affiniteit verwerken, klikt pas u weer deze mogelijkheid gebruikmaken

### <a name="connection-draining"></a>Verwerkingsstop voor verbindingen

Verwerkingsstop voor verbindingen helpt u om back-endgroepsleden zonder problemen te verwijderen tijdens geplande service-updates. Deze instelling kan worden toegepast op alle leden van een back-endpool tijdens het maken van regels. Eenmaal is ingeschakeld, application-gateway zorgt ervoor dat alle ongedaan maken registreren exemplaren van een back-endpool niet alle nieuwe aanvragen ontvangen terwijl bestaande aanvragen om te voltooien binnen een geconfigureerde tijdslimiet. Dit geldt voor zowel back-endexemplaren die door een API-aanroep expliciet uit de back-endgroep worden verwijderd als voor back-endexemplaren die door de statuscontroles worden gerapporteerd als beschadigd.

### <a name="override-backend-path"></a>Back-endpad overschrijven

Deze functie kunt u het pad in de URL overschrijven zodat de aanvragen voor een specifiek pad kunnen worden doorgestuurd naar een ander pad. Bijvoorbeeld, als u van plan bent voor het routeren van aanvragen voor contoso.com/images naar standaard, voert u '/' in dit tekstvak en vervolgens deze HTTP-instelling toevoegen aan de regel die is gekoppeld aan contoso.com/images.

### <a name="pick-host-name-from-a-backend-address"></a>Naam van de host vanaf een back endadres kiezen

Hiermee stelt u deze mogelijkheid de *host* header in de aanvraag voor de hostnaam van de back-endpool met behulp van een IP-adres of FQDN-naam - FQDN-naam. Dit is handig in de scenario's waar de domeinnaam van de back-end verschilt van de DNS-naam van de toepassing, zoals in een scenario waarbij Azure App Service wordt gebruikt als de back-end. Aangezien Azure App Service een omgeving met meerdere tenants met behulp van een gedeelde ruimte met één IP-adres is, kunnen een App Service zijn alleen toegankelijk voor de hostnamen die zijn geconfigureerd in de instellingen van het aangepaste domein. Dit is standaard *example.azurewebsites.net*. Als u toegang tot uw App Service met behulp van application gateway met een hostnaam is niet geregistreerd in App Service of met de FQDN van de toepassingsgateway wilt, hebt u voor de onderdrukking van de hostnaam in de oorspronkelijke aanvraag aan de hostnaam van de App Service.

### <a name="host-override"></a>**Host-overschrijven**

Deze functie vervangt de *host* -header in de inkomende aanvraag op de toepassingsgateway de naam van de host die u hier opgeeft.

Zodra u een HTTP-instelling hebt gemaakt, moet u deze koppelen aan een of meer regels voor doorsturen aanvragen.

## <a name="backend-pool"></a>Back-endpool

De back-endpool wordt gebruikt voor het routeren van aanvragen naar de back-endservers, die de aanvraag dienen. Back-endpools kunnen bestaan uit NIC's, de virtuele machine schalen sets, openbare IP-adressen, interne IP-adressen, FQDN-naam en multitenant back-ends, zoals Azure App Service. Back-endpoolleden voor Application gateway zijn niet gekoppeld aan een beschikbaarheidsset. Leden van de back-endpools kunnen zich in meerdere computerclusters, datacenters, of buiten Azure, zolang ze IP-connectiviteit hebben. U kunt verschillende back-endpools voor verschillende soorten aanvragen kunt maken. Maak bijvoorbeeld een back-endpool voor algemene aanvragen en andere back-endpool voor aanvragen voor de microservices voor uw toepassing.

Nadat u een back endpool maakt, moet u deze koppelen aan een of meer aanvraag-routeringsregels. U moet ook statuscontroles voor elke back-endpool configureren op uw application gateway. Als een aanvraag voor routering regelvoorwaarde wordt voldaan, verzendt de toepassingsgateway het verkeer naar de in orde servers (zoals wordt bepaald door de statuscontroles) in de bijbehorende back-endpool.

## <a name="health-probes"></a>Statuscontroles

Application gateway standaard controleert de status van alle resources in de back-endpool en worden een resource als slecht beschouwd uit de groep automatisch verwijderd. Application gateway voor het bewaken van de exemplaren die niet in orde blijft en ze terug naar de goede back-endpool toegevoegd zodra ze beschikbaar komen en op statuscontroles reageren. Application gateway verzendt dat de statuscontroles met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen.

Naast het gebruik van standaard health test controle, kunt u ook de statustest aan de behoeften van de vereisten van uw toepassing aanpassen. Aangepaste tests kunnen u een meer nauwkeurige controle over de statuscontrole. Wanneer u aangepaste tests, kunt u de testinterval, de URL en pad om te testen en het aantal mislukte reacties op accepteren voordat het back-end-pool-exemplaar is gemarkeerd als niet in orde. Het raadzaam dat u aangepaste tests configureert om de status van elke back-end-toepassingen worden gecontroleerd. Zie voor meer informatie, [controleren van de status van uw toepassingsgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over Application Gateway-onderdelen, kunt u het volgende doen:
* [Een toepassingsgateway maken in Azure portal](quick-create-portal.md)
* [Maak een toepassingsgateway met behulp van PowerShell](quick-create-powershell.md)
* [Maak een toepassingsgateway met behulp van Azure CLI](quick-create-cli.md).