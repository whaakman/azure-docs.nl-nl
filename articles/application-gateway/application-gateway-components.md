---
title: Azure Application Gateway-onderdelen
description: In dit artikel bevat informatie over de verschillende onderdelen in Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 14f400a1b85e213496ac98996d6c2378cf559026
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56675792"
---
# <a name="application-gateway-components"></a>Application Gateway-onderdelen

 Een Application Gateway fungeert als de één contactpunt voor clients. Het verdeeld binnenkomende verkeer van de toepassing over meerdere back endpools, zoals virtuele Azure-machines, VMSS, App Services of on-premises/externe servers. Om dit te doen, maakt het gebruik van verschillende onderdelen die hieronder worden beschreven:

![Application-gateway-onderdelen](.\media\application-gateway-components\application-gateway-components.png)

## <a name="frontend-ip"></a>Front-end-IP

Fronted IP is het IP (Internet Protocol)-adres dat is gekoppeld aan de toepassingsgateway. U kunt de toepassingsgateway om ofwel een openbaar IP-adres of een privé-IP of beide. Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway. Uw virtuele netwerk en openbare IP-adres moeten zich op dezelfde locatie bevinden als uw Application Gateway.

### <a name="static-vs-dynamic-public-ip"></a>Statische versus dynamische openbare IP-adres

De v1-SKU biedt ondersteuning voor statische interne IP-adressen, maar biedt geen ondersteuning voor statische openbare IP-adressen. Het VIP kunt wijzigen als de toepassingsgateway is gestopt en gestart. De DNS-naam die is gekoppeld aan de application gateway verandert niet gedurende de levenscyclus van de gateway. Om deze reden is het aanbevolen om een CNAME-alias gebruiken en wijs het DNS-adres van de toepassingsgateway.

De v2-SKU van Application Gateway biedt ondersteuning voor statische openbare IP-adressen en ook als interne IP-adressen. Slechts één openbaar IP-adres wordt ondersteund in een toepassingsgateway.

Na het maken van een Frontend-IP, deze is gekoppeld aan *listeners* die inkomende aanvragen via de Frontend-IP controleren.

## <a name="listeners"></a>Listeners

Voordat u met behulp van uw Application Gateway begint, moet u een of meer listeners toevoegen. Een listener is een proces met behulp van waarmee de Application Gateway wordt gecontroleerd op de inkomende verbindingsaanvragen met het protocol en poort die u configureert. Nadat de listener binnenkomende aanvragen van clients detecteert, stuurt de Application Gateway deze aanvragen naar de back-endservers in de back-endadresgroep met behulp van de aanvraag routeringsregels toe die u definieert voor de listener die de inkomende aanvraag ontvangen.

Listeners ondersteunen de volgende poorten en protocollen:

### <a name="ports"></a>Poorten

Dit is de poort waarop de listener naar de clientaanvraag luistert. Het volgende bereik is toegestaan voor de configuratie van de poort: 1-65535

### <a name="protocols"></a>Protocollen

Application Gateway ondersteunt de volgende protocollen voor 4: HTTP, HTTPS en HTTP/2, Websocket

Tijdens het configureren van de listener, moet u kiezen tussen HTTP en HTTPS-protocol. Application Gateway biedt systeemeigen ondersteuning voor WebSockets en HTTP/2-protocollen. U kunt WebSockets gebruiken met HTTP en HTTPS-listeners. 

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment codevoorbeeld laat zien hoe u deze kunt inschakelen:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

Websocket-ondersteuning is standaard ingeschakeld. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen.

U kunt een HTTPS-listener voor SSL-beëindiging die wordt het werk van versleuteling en ontsleuteling naar uw Application Gateway-offload zodat uw webservers kunnen voorkomt u prijzige kostbare overhead voor versleuteling en ontsleuteling en uw toepassingen zich kunnen richten op hun bedrijfslogica. U moet ten minste één SSL-servercertificaat implementeren voor een HTTPS-listener. Zie voor meer informatie, [het configureren van SSL-beëindiging](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

### <a name="custom-error-pages"></a>Aangepaste foutpagina's

Met Application Gateway kunt u aangepaste foutpagina's maken in plaats van standaardfoutpagina's weer te geven. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken. Application-gateway kan een aangepaste foutpagina weergegeven wanneer een aanvraag tijdelijk niet bereikbaar voor de back-end. Zie voor meer informatie, [aangepaste foutpagina's voor uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/custom-error)

### <a name="types-of-listeners"></a>Typen Listeners

Er zijn twee soorten listeners:

- **Basic**: Dit type listener luistert naar een enkel domein-site waarop het heeft een DNS-toewijzing van één IP-adres van de toepassingsgateway. Deze listenerconfiguratie is vereist wanneer u een enkele site achter een Application gateway hosten
- **Meerdere sites**: Deze listenerconfiguratie is vereist wanneer u meer dan één webtoepassing op hetzelfde exemplaar van application gateway configureert. Hiermee kunt u een efficiëntere topologie voor uw implementaties configureren door maximaal 100 websites toe te voegen aan één toepassingsgateway. Elke website kan worden omgeleid naar een eigen back-endpool. Bijvoorbeeld:  Voor drie subdomeinen: abc.alpha.com, xyz.alpha.com en pqr.alpha.com die verwijst naar het IP-adres van de toepassingsgateway. 3-listeners van het type 'multi-site' maken en configureren van elke listener voor de respectieve poort en protocolinstelling. 

Nadat een listener is gemaakt, koppelt u deze aan de regel voor doorsturen op een aanvraag waarmee wordt bepaald hoe de aanvraag is ontvangen op de listener is naar de back-end worden doorgestuurd.

Listeners worden verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst. Multi-site-listeners moeten worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.

## <a name="request-routing-rule"></a>Regel voor het doorsturen van aanvragen

Dit is het belangrijkste onderdeel van de toepassingsgateway en bepaalt hoe het verkeer op de listener die zijn gekoppeld aan deze regel wordt gerouteerd. De regel verbindt de listener, de back-endserverpool en de back-end-HTTP-instellingen en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Een listener kan worden gekoppeld aan slechts één regel.

Er zijn twee typen van regels voor het doorsturen van aanvraag:

- **Basic:** Alle aanvragen op de bijbehorende listener (bijvoorbeeld: blog.contoso.com/*) worden doorgestuurd naar de gekoppelde back-endadresgroep met behulp van de bijbehorende HTTP-instelling.
- **Op basis van pad:** Dit type regel biedt u de mogelijkheid voor het routeren van de aanvragen op de bijbehorende listener aan een specifieke back endadresgroep op basis van de URL in de aanvraag. Als het pad van de URL in een aanvraag overeenkomt met het padpatroon in een pad gebaseerde regel, wordt de aanvraag doorgestuurd met behulp van die regel. De pad-patroon wordt alleen toegepast op het pad van de URL, niet naar de queryparameters. 

Als het pad van de URL van een aanvraag voor een listener komt niet overeen met een van de regels op basis van het pad, wordt de aanvraag wordt doorgestuurd naar de *standaard* back-endpool en de *standaard* HTTP-instellingen.

Regels worden verwerkt in de volgorde waarin die ze zijn geconfigureerd. Het verdient aanbeveling dat multi-site-regels zijn geconfigureerd voordat basic regels om te verminderen de kans dat verkeer wordt doorgestuurd naar de onjuiste back-end als de basisregel wordt gezocht naar verkeer op basis van de poort voordat de multi-site-regel wordt geëvalueerd.

### <a name="redirection-support"></a>Ondersteuning van omleiding

De regel voor het doorsturen van aanvraag kunt u omleiden van verkeer op de Application Gateway. Dit is een algemeen omleidingsmechanisme, zodat u op basis van regels kunt omleiden van en naar elke poort die u gebruikt. Dit kan helpen u bij het inschakelen van automatische HTTP naar HTTPS-omleiding om te controleren of dat alle communicatie tussen de toepassing en uw gebruikers vindt plaats via een gecodeerde pad. U kunt de doel-listener of een externe site, waarvoor omleiding is vereist. De configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en de query-tekenreeks toe te voegen aan de omgeleide URL. U kunt ook kiezen of omleiding een tijdelijke (HTTP-statuscode 302) of een permanente omleiding (HTTP-statuscode 301 is). Wanneer u een basisregel, wordt de configuratie van de omleidings-is gekoppeld aan de listener van een bron en is een algemene omleiding. Als u een pad gebaseerde regel gebruikt, wordt de configuratie van de omleidings-is gedefinieerd in de URL-path-map. Zodat deze alleen van toepassing op het padgebied van het specifieke van een site. Zie voor meer informatie, [omleiden van verkeer op uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="http-headers"></a>HTTP-Headers

Application Gateway voegt x doorgestuurd voor x-doorgestuurd-protocol en x-doorgestuurd-poort kopteksten in de aanvraag doorgestuurd naar de back-end. De indeling voor x-doorgestuurd-voor-header is een door komma's gescheiden lijst met IP: poort. De geldige waarden voor de x-doorgestuurd-protocol zijn http of https. X-doorgestuurd-poort geeft de poort waarop de aanvraag bij de application gateway is bereikt. Application Gateway voegt ook X-oorspronkelijke-Host-header met de oorspronkelijke Host-header waarmee de aanvraag is ontvangen. Deze header is handig in scenario's zoals de integratie van Azure-Website, waar de binnenkomende host-header is gewijzigd voordat het verkeer wordt doorgestuurd naar de back-end.

#### <a name="rewrite-http-headers"></a>HTTP-headers opnieuw genereren

Met behulp van de aanvraag routeringsregels toe die u kunt toevoegen, verwijderen of bijwerken van de aanvraag- en reactieheaders HTTP (S), terwijl de aanvraag en respons pakketten tussen de client en de back-end-pools, via de Application Gateway verplaatsen. De headers kunnen niet alleen worden ingesteld op statische waarden, maar ook voor andere headers en servervariabelen belangrijk. Hierdoor kunt u uitvoeren van verschillende belangrijke use-cases, zoals het ophalen van IP-adres van de clients, verwijderen van gevoelige informatie over de back-end, toevoegen van extra beveiligingsmaatregelen, enzovoort. Zie voor meer informatie,[Herschrijf de HTTP-headers in uw Application Gateway](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)

## <a name="http-settings"></a>HTTP-instellingen

De Application Gateway routeert verkeer naar de back-endservers met behulp van het poortnummer, protocol en andere instellingen die zijn opgegeven in deze resource. 

De back endadresgroepen ondersteunen de volgende poorten en protocollen:

### <a name="ports"></a>Poorten

Dit is de poort waarop de back-endservers luisteren op het verkeer dat afkomstig is van de toepassingsgateway. Het volgende bereik is toegestaan voor de configuratie van de poort: 1-65535

### <a name="protocols"></a>Protocollen

Application Gateway ondersteunt zowel HTTP als HTTPS-protocol voor Routeringsaanvragen voor de back-endservers.

Als HTTP-protocol is gekozen, is het verkeer stromen niet versleuteld naar de back-endservers. 

In deze gevallen waarbij niet-versleutelde communicatie met de back-endservers kan niet worden acceptabele gebruikt, moet u HTTPS-protocol. Deze instelling in combinatie met HTTPS-protocol te kiezen in de listener, kunt u inschakelen [end-to-end SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Dat kunt u veilig gevoelige gegevens verzenden naar de back-end die is versleuteld. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

Met behulp van de HTTP-instelling kunt u gebruikmaken van verschillende mogelijkheden:

### <a name="cookie-based-session-affinity"></a>Cookies gebaseerde sessieaffiniteit

Deze functie is handig als u wilt behouden een gebruikerssessie op dezelfde server. Met behulp van de gatewaybeheerde cookies kan de Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde server leiden voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie.

Als de toepassing niet kan cookies gebaseerde affiniteit verwerken, klikt pas u weer deze mogelijkheid gebruikmaken

### <a name="connection-draining"></a>Verwerkingsstop voor verbindingen

Verwerkingsstop voor verbindingen helpt u om back-endgroepsleden zonder problemen te verwijderen tijdens geplande service-updates. Deze instelling kan worden toegepast op alle leden van een back-endpool tijdens het maken van regels. Wanneer de instelling is ingeschakeld, zorgt Application Gateway ervoor dat alle exemplaren die worden uitgeschreven bij een back-endgroep geen nieuwe aanvraag ontvangen en dat bestaande aanvragen binnen een geconfigureerde tijdslimiet worden voltooid. Dit geldt voor zowel back-endexemplaren die door een API-aanroep expliciet uit de back-endgroep worden verwijderd als voor back-endexemplaren die door de statuscontroles worden gerapporteerd als beschadigd.

### <a name="override-backend-path"></a>Back-endpad overschrijven

Deze functie kunt u het pad in de URL overschrijven zodat de aanvragen voor een specifiek pad kunnen worden doorgestuurd naar een ander pad. Bijvoorbeeld, als u van plan bent voor het routeren van aanvragen voor contoso.com/images naar standaard, voert u '/' in dit tekstvak en vervolgens deze HTTP-instelling toevoegen aan de regel die is gekoppeld aan contoso.com/images.

### <a name="pick-host-name-from-a-backend-address"></a>Naam van de host vanaf een back endadres kiezen

Hiermee stelt u deze mogelijkheid de *host* header in de aanvraag voor de hostnaam van de back-endpool (IP of FQDN-naam). Dit is handig in de scenario's waar de domeinnaam van de back-end verschilt van de DNS-naam van de toepassing, zoals in een scenario waarbij Azure App Service wordt gebruikt als back-end. Dit komt doordat omdat Azure App Service een omgeving met meerdere tenants met behulp van een gedeelde ruimte met één IP-adres is, een App Service alleen via de hostnamen die zijn geconfigureerd in de instellingen van aangepast domein kunnen worden geopend. Standaard is het "example.azurewebsites.net" en als u toegang tot uw App Service met behulp van Application Gateway met een hostnaam is niet geregistreerd in App Service of met de FQDN van de toepassingsgateway wilt, hebt u voor de onderdrukking van de hostnaam in de oorspronkelijke aanvraag voor de App. De hostnaam van de service.

### <a name="host-override"></a>**Host-overschrijven**

Deze functie vervangt de *host* -header in de inkomende aanvraag op de toepassingsgateway de naam van de host die u hier opgeeft. 

Zodra u een HTTP-instelling hebt gemaakt, moet u deze koppelen aan een of meer regels voor doorsturen aanvragen.

## <a name="backend-pool"></a>Back-Endpool

De back-endpool wordt gebruikt voor het routeren van aanvragen naar de back-endservers die behoeve van de aanvraag. Back-endpools kunnen bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Appservice. Back-endpoolleden voor Application Gateway zijn niet gekoppeld aan een beschikbaarheidsset. Leden van de back-endpools kunnen zich in meerdere computerclusters, datacenters, of buiten Azure, zolang ze IP-connectiviteit hebben. U kunt verschillende back-endpools voor verschillende soorten aanvragen kunt maken. Maak bijvoorbeeld een back-endpool voor algemene aanvragen en andere back-endpool voor aanvragen voor de microservices voor uw toepassing.

Na het maken van een back-endgroep die u wilt koppelen aan een of meer aanvraag-routeringsregels. U moet ook statuscontroles voor elke back-endpool configureren op uw Application Gateway. Als een aanvraag voor routering regelvoorwaarde wordt voldaan, verzendt de toepassingsgateway het verkeer naar de in orde servers (zoals wordt bepaald door de statuscontroles) in de bijbehorende back-endpool.

## <a name="health-probes"></a>Statuscontroles

Azure Application Gateway standaard controleert de status van alle resources in de back-end-pool en worden een resource als slecht beschouwd uit de groep automatisch verwijderd. Application Gateway blijft voor het bewaken van de exemplaren en terug naar de goede back-end-adrespool toegevoegd zodra ze beschikbaar komen en op statuscontroles reageren. Application gateway verzendt dat de statuscontroles met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen.

Naast het gebruik van standaard health test controle, kunt u ook de statustest aan de behoeften van de vereisten van uw toepassing aanpassen. Aangepaste tests kunnen u een meer nauwkeurige controle over de statuscontrole. Wanneer u aangepaste tests, kunt u de testinterval, de URL en pad om te testen en hoeveel mislukte reacties op accepteren voordat u markeert het exemplaar van de back-end-pool is beschadigd. Het is raadzaam dat u aangepaste tests configureert om de status van elke back-end-toepassingen worden gecontroleerd. Zie voor meer informatie, [bewaken van de status van uw Application gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview)

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over Application Gateway-onderdelen, u [een Application Gateway kunt maken in Azure portal](quick-create-portal.md) of een [maken van een toepassingsgateway met behulp van PowerShell](quick-create-powershell.md) of [maken een Application Gateway met behulp van Azure CLI](quick-create-cli.md).