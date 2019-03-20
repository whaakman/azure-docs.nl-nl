---
title: Overzicht van Azure Application Gateway-configuratie
description: Dit artikel bevat informatie over het configureren van de verschillende onderdelen in Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 03/04/2019
ms.author: absha
ms.openlocfilehash: 515243cb043bac8e9f28a3c63808e4fbd9b8f525
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57905029"
---
# <a name="application-gateway-configuration-overview"></a>Overzicht van de configuratie van de Application Gateway

Application-gateway bestaat uit verschillende onderdelen die op verschillende manieren voor het uitvoeren van verschillende scenario's kunnen worden geconfigureerd. Dit artikel begeleidt u bij hoe elk onderdeel moet worden geconfigureerd.

![Application-gateway-onderdelen](./media/configuration-overview/configuration-overview1.png)

Bovenstaande afbeelding ziet u de configuratie van een toepassing met 3 listeners. De eerste twee listeners voor meerdere locaties zijn `http://acme.com/*` en `http://fabrikam.com/*`, respectievelijk. Beide luisteren op poort 80. De derde listener is een basislistener met end-to-end SSL-beëindiging. 

## <a name="prerequisites"></a>Vereisten

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure-netwerk en toegewezen subnet

Application gateway is een specifieke implementatie in uw virtuele netwerk. Binnen uw virtuele netwerk is een speciaal subnet vereist voor uw application gateway. U kunt meerdere exemplaren van de implementatie van de gateway van een bepaalde toepassing hebben in dit subnet. U kunt ook andere Toepassingsgateways in het subnet implementeren, maar u kunt een andere resource in het subnet van de toepassingsgateway niet implementeren.  

> [!NOTE]   
> Met een combinatie van Standard_v2 en Standard Application Gateway in hetzelfde subnet wordt niet ondersteund.

#### <a name="size-of-the-subnet"></a>Grootte van het subnet

In het geval van v1-SKU, worden in application Gateway één privé IP-adres per exemplaar, plus een andere privé-IP-adres gebruikt als een privé front-end-IP-configuratie is geconfigureerd. Ook Azure reserveert de eerste vier en laatste IP-adres in elk subnet voor intern gebruik. Bijvoorbeeld, als een application gateway is ingesteld op drie exemplaren en geen privé front-end-IP, klikt u vervolgens een/29 subnet, grootte of hoger is vereist. In dit geval de application gateway maakt gebruik van drie IP-adressen. Als u drie exemplaren en een IP-adres voor de privé front-end-IP-configuratie, klikt u vervolgens een/28 hebt subnet groot of groter is nodig omdat tijdens vier IP-adressen zijn vereist.

#### <a name="network-security-groups-supported-on-the-application-gateway-subnet"></a>Netwerkbeveiligingsgroepen die worden ondersteund op het subnet voor Application Gateway

Netwerkbeveiligingsgroepen (nsg's) worden ondersteund in de Application Gateway-subnet met de volgende beperkingen: 

- Uitzonderingen moeten in worden geplaatst voor binnenkomend verkeer op poort 65503 65534 voor Application Gateway v1-SKU en poorten 65200-65535 voor de v2-SKU. Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten zijn niet externe entiteiten, inclusief de klanten van deze gateways kunnen initiëren wijzigingen op de eindpunten.

- Uitgaande verbinding met internet kan niet worden geblokkeerd.

- Verkeer van de tag AzureLoadBalancer moet worden toegestaan.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Lijst met toegestane adressen Application Gateway toegang tot een paar bron-IP 's

In dit scenario kan worden gedaan met nsg's op het subnet van de gateway. De volgende beperkingen moeten worden geplaatst op het subnet in de vermelde volgorde van prioriteit:

1. Toestaan van binnenkomend verkeer van bron-IP/IP-bereik.
2. Toestaan van binnenkomende aanvragen van alle bronnen op poorten 65503 65534 voor [back-end health communicatie](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.
3. Toestaan van binnenkomende Azure Load Balancer-tests (tag AzureLoadBalancer) en binnenkomend verkeer in virtuele netwerken (VirtualNetwork-tag) op de [NSG](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Alle andere binnenkomende verkeer met een weigeren alle regel blokkeren.
5. Sta uitgaand verkeer naar internet voor alle bestemmingen.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Gebruiker gedefinieerde routes die worden ondersteund op het subnet voor Application Gateway

Gebruiker gedefinieerde routes (udr's) worden ondersteund op het subnet van de gateway, in het geval van v1-SKU, zolang ze de end-to-end verzoek/reactie-communicatie niet wijzigen.

Bijvoorbeeld, u kunt een UDR in het subnet van de gateway instellen om te verwijzen naar een firewallapparaat voor pakketinspecties uitvoeren, maar moet u ervoor zorgen dat het pakket de inspectie van de bestemming van het bericht kan bereiken. Dit niet doet, kan leiden tot onjuiste health test of verkeer omleiden gedrag. Dit omvat geleerde routes of 0.0.0.0/0 standaardroutes doorgegeven door ExpressRoute of VPN-Gateways in het virtuele netwerk.

In het geval van v2 worden-SKU, udr's op het subnet van de gateway niet ondersteund. Zie voor meer informatie, [automatisch schalen en Zone-redundante Application Gateway (openbare Preview)](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#known-issues-and-limitations).

## <a name="frontend-ip"></a>Front-end-IP

U kunt de toepassingsgateway om ofwel een openbaar IP-adres of een privé IP-adres of beide. Openbaar IP-adres is vereist wanneer u een back-end die is vereist om te worden gebruikt door clients via internet via een internetgerichte VIP host. Openbaar IP-adres is niet vereist voor een intern eindpunt dat geen toegang heeft tot het Internet, ook wel bekend als een interne load balancer (ILB)-eindpunt. Het is een goed idee om de gateway te configureren met een ILB als u interne line-of-business-toepassingen gebruikt die geen toegang hebben tot het internet. Ook is dit handig als u services en lagen gebruikt in een toepassing met meerdere lagen die zich binnen een beveiligingsgrens bevinden, en als deze toepassing geen toegang heeft tot het internet, maar er wel round-robinbelastingverdeling, sessiepersistentie of SSL-beëindiging (Secure Sockets Layer) vereist is.

Slechts één openbaar IP-adres of een privé IP-adres wordt ondersteund. Het front-end-IP-adres kiest u tijdens het maken van de toepassingsgateway. 

- U kunt in het geval van een openbaar IP-adres, een nieuw openbaar IP-adres maken of een bestaande openbare IP-adres wilt gebruiken op dezelfde locatie als de toepassingsgateway. Als u een nieuw openbaar IP-adres maakt, kan het type IP-adres geselecteerd (statisch of dynamisch) kan niet later worden gewijzigd. Zie voor meer informatie, [statische versus dynamische openbare IP-adres](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-vs-dynamic-public-ip) 

- In het geval van een privé IP-adres, kunt u kiezen om op te geven van een privé IP-adres van het subnet waarin de Application Gateway is gemaakt. Als u niet expliciet is opgegeven, worden automatisch een willekeurige IP-adres geselecteerd uit het subnet. Zie voor meer informatie, [een toepassingsgateway maken met een interne load balancer (ILB)-eindpunt.](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm)

Een Frontend-IP is gekoppeld aan *listener* die controleert op inkomende aanvragen via de Frontend-IP.

## <a name="listeners"></a>Listeners

Een listener is een logische eenheid waarmee wordt gecontroleerd op de inkomende verbindingsaanvragen met de poort, protocol, host en IP-adres. Daarom bij het configureren van de listener die u wilt opgeven die waarden van de poort, protocol, host en IP-adres die zijn hetzelfde als de bijbehorende waarden in de inkomende aanvraag op de gateway. Wanneer u een toepassingsgateway met behulp van de Azure portal maakt, maken u ook een standaard-listener door het kiezen van het protocol en poort voor de listener. U kunt ook kiezen of u wilt inschakelen van ondersteuning voor HTTP2 op de listener. Nadat de toepassingsgateway is gemaakt, kunt u de instelling van deze standaard-listener (*appGatewayHttpListener*/*appGatewayHttpsListener*) en/of nieuwe listeners maken.

### <a name="listener-type"></a>Listener-type

U kunt kiezen tussen [basic- of multi-site-listener](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners) tijdens het maken van een nieuwe listener. 

- Als u een enkele site achter een Application gateway host, kiest u basislistener. Informatie over [over het maken van een toepassingsgateway met basislistener](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Als u meer dan één webtoepassing of meerdere subdomeinen van hetzelfde bovenliggende domein op hetzelfde exemplaar van application gateway configureert, kiest u listener voor meerdere locaties. Voor de listener voor meerdere locaties moet u bovendien een hostnaam invoeren. Dit is omdat Application Gateway maakt gebruik van HTTP 1.1-hostheaders voor het hosten van meer dan één website op hetzelfde openbare IP-adres en dezelfde poort.![1551057450710](C:/Users/absha/AppData/Roaming/Typora/typora-user-images/1551057450710.png)


> [!NOTE]
> In het geval van v1-SKU's, worden de listeners verwerkt in de volgorde waarin die ze worden weergegeven. Om die reden als een basislistener overeenkomt met een binnenkomende aanvraag verwerkt eerst. Multi-site-listeners moeten daarom worden geconfigureerd voordat u een basislistener om te zorgen dat verkeer wordt gerouteerd naar de juiste back-end.
>
> In het geval van v2-SKU's, zijn multi-site-listeners verwerkt, voordat u eenvoudige listeners.

### <a name="frontend-ip"></a>Front-end-IP

Kies de frontend-IP dat u van plan bent om te koppelen aan deze listener. De listener wordt moet deze luisteren op de inkomende aanvraag op dit IP-adres.

### <a name="frontend-port"></a>Frontend-poort

Kies de frontend-poort. U kunt kiezen uit de bestaande poorten of een nieuwe maken. U kunt een waarde van de [het toegestane bereik van poorten](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Hiermee kunt u niet alleen bekende poorten, zoals 80 en 443 gebruiken, maar is een aangepaste poort geschikt zijn voor uw gebruik toegestaan. Een poort kan worden gebruikt voor openbare gerichte listeners of een persoonlijke gerichte listeners.

### <a name="protocol"></a>Protocol

U moet kiezen tussen HTTP en HTTPS-protocol. 

- Als u ervoor HTTP kiest, het verkeer tussen de gateway van de client en de toepassing worden overgebracht niet-versleuteld.

- Selecteer HTTPS als u geïnteresseerd bent in [Secure Sockets Layer (SSL) beëindiging](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssl-terminationl) of [end to end SSL-versleuteling](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Als u HTTPS kiest, wordt het verkeer tussen de client en de application gateway, worden versleuteld en de SSL-verbinding wordt beëindigd bij de application gateway.  Als u wilt dat end to end SSL-versleuteling, Daarnaast moet u HTTPS-protocol kiezen tijdens het configureren van *back-end-HTTP-instelling*. Dit zorgt ervoor dat het verkeer wordt opnieuw versleuteld wanneer ze uit de Application Gateway naar de back-end.

  Een certificaat is vereist voor het configureren van Secure Sockets Layer (SSL)-beëindiging en end-to-end SSL-versleuteling, moet worden toegevoegd aan de listener om het inschakelen van de toepassingsgateway voor het afleiden van een symmetrische sleutel volgens de specificatie van de SSL-protocol. De symmetrische sleutel wordt vervolgens gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden naar de gateway. Het gatewaycertificaat moet zich in Personal Information Exchange (PFX)-indeling. De bestandsindeling kunt u exporteert de persoonlijke sleutel die is vereist voor de toepassingsgateway om uit te voeren van de versleuteling en ontsleuteling van verkeer. 

#### <a name="supported-certs"></a>Ondersteunde certificaten

Zelfondertekende certificaten, CA-certificaten, jokerteken certificaten en VW certificaten worden ondersteund.

### <a name="additional-protocol-support"></a>Aanvullende protocolondersteuning

#### <a name="http2-support"></a>Ondersteuning voor HTTP2

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients verbinding maken met application gateway alleen listeners. De communicatie met de back-endserverpools is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment codevoorbeeld laat zien hoe u deze kunt inschakelen:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Websocket-ondersteuning

Websocket-ondersteuning is standaard ingeschakeld. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen. U kunt WebSockets gebruiken met HTTP en HTTPS-listeners. 

### <a name="custom-error-page"></a>Aangepaste foutpagina

Aangepaste foutpagina's kunnen worden gedefinieerd op globaal niveau, evenals het niveau van de listener, maar het globale niveau aangepaste foutpagina's maken vanuit Azure portal is momenteel niet ondersteund. U kunt een aangepaste foutpagina weergegeven voor een 403 WAF-fout of een 502 onderhoudspagina configureren op het niveau van de listener. U moet ook een openbaar toegankelijke blob-URL voor de code van de status fout op te geven. Zie voor meer informatie, [maken aangepaste foutpagina](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Foutcodes voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Gebruik voor het configureren van een globale aangepaste foutpagina [Azure PowerShell voor configuratie](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration) 

### <a name="ssl-policy"></a>SSL-beleid

U kunt SSL-Certificaatbeheer centraliseren en versleuteling en ontsleuteling overhead verminderen vanuit een back-end-server-farm. Deze gecentraliseerde SSL verwerken ook kunt u een centrale SSL-beleid dat geschikt voor de beveiligingsvereisten van uw organisatie opgeven.  U kunt kiezen tussen standaard, vooraf gedefinieerde en aangepaste SSL-beleid. 

U kunt SSL-beleid voor het beheren van SSL-protocolversies configureren. U kunt de toepassingsgateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. SSL 2.0 en 3.0 zijn standaard al uitgeschakeld en zijn niet configureerbaar. Zie voor meer informatie, [overzicht van Application Gateway SSL-beleid](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Nadat een listener is gemaakt, koppelt u deze aan de regel voor doorsturen op een aanvraag waarmee wordt bepaald hoe de aanvraag is ontvangen op de listener is naar de back-end worden doorgestuurd.

## <a name="request-routing-rule"></a>Regel voor het doorsturen van aanvragen

Tijdens het maken van de application gateway met behulp van de Azure portal, maakt u een standaardregel (*rule1*), die verbindt de listener met standaard (*appGatewayHttpListener*) met het standaard back-end-adresgroep (*appGatewayBackendPool*) en de standaardinstellingen van de back-end-HTTP (*appGatewayBackendHttpSettings*). Nadat de toepassingsgateway is gemaakt, u kunt de instelling van deze standaardregel bewerken en/of nieuwe regels maken.

### <a name="rule-type"></a>Regeltype

U kunt kiezen tussen [basic of pad gebaseerde regel](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule) tijdens het maken van een nieuwe regel. 

- Als u wilt doorsturen van alle aanvragen op de bijbehorende listener (bijvoorbeeld: blog.contoso.com/*) voor een eenmalige back-endpool, kiest u basislistener. 
- Kies de pad-gebaseerde listener als u wilt dat voor het routeren van aanvragen met specifieke URL-pad naar specifieke back-endpools. De pad-patroon wordt alleen toegepast op het pad van de URL, niet naar de queryparameters.


> [!NOTE]
>
> In het geval van v1-SKU's, wordt die overeenkomt met het patroon van de inkomende aanvraag verwerkt in de volgorde waarin de paden in de URL-pad-kaart van het pad gebaseerde regel worden weergegeven. Om die reden als een aanvraag overeenkomt met het patroon in twee of meer paden in de URL-path-map, klikt u vervolgens het pad op waarin wordt vermeld voor het eerst wordt worden gekoppeld en de aanvraag doorgestuurd naar de back-end die zijn gekoppeld aan het opgegeven pad.
>
> In het geval van v2-SKU's bevat een exacte overeenkomst hogere prioriteit boven de volgorde waarin de paden worden weergegeven in de URL-path-map. Voor die reden als een aanvraag overeenkomt met het patroon in twee of meer paden, en vervolgens de aanvraag doorgestuurd naar de back-end die zijn gekoppeld aan het opgegeven pad die overeenkomt met precies met de aanvraag. Als het pad in de inkomende aanvraag komt niet exact overeen met een pad in de URL-path-map, wordt klikt u vervolgens die overeenkomt met het patroon van de inkomende aanvraag verwerkt in de volgorde waarin de paden worden weergegeven in de URL-pad-kaart van het pad gebaseerde regel.

### <a name="associated-listener"></a>Bijbehorende listener

Moet u een listener voor de regel koppelen zodat de *routeringsregel aanvragen* die zijn gekoppeld aan de *listener* wordt geëvalueerd om vast te stellen de *back-endpool* waaraan de de aanvraag is om te worden gerouteerd.

### <a name="associated-backend-pool"></a>Gekoppelde back-end-Pool

De back-end-doelen die de aanvragen zijn ontvangen door de listener fungeren moeten met back-end-pool koppelen. In het geval van een eenvoudige regel mag slechts één back-endpool omdat alle aanvragen op de bijbehorende listener, worden doorgestuurd naar deze back-endpool. Voeg meerdere back-endpools overeenkomt met elke URL-pad in het geval van een pad gebaseerde regel. De aanvragen die overeenkomen met het URL-pad die u hier opgeeft, zal worden doorgestuurd naar de bijbehorende back-endpool. Daarnaast een standaard back-end-adresgroep toevoegen omdat de aanvragen die niet overeenkomen met een URL-pad opgegeven in deze regel wordt doorgestuurd naar deze.

### <a name="associated-backend-http-setting"></a>Gekoppelde back-end-HTTP-instelling

Een back-end-HTTP-instelling voor elke regel toevoegen. De aanvragen worden doorgestuurd van de Application Gateway naar de back-end-doelen met behulp van het poortnummer, protocol en andere instellingen die zijn opgegeven in deze instelling. In het geval van een eenvoudige regel mag slechts één back-end-HTTP-instelling omdat alle aanvragen op de bijbehorende listener, worden doorgestuurd naar de bijbehorende back-end-doelen met behulp van deze HTTP-instelling. Voeg meerdere back-end-HTTP-instellingen die overeenkomt met elke URL-pad in het geval van een pad gebaseerde regel. De aanvragen die overeenkomen met het URL-pad die u hier opgeeft, zal worden doorgestuurd naar de bijbehorende back-end-doelen met behulp van de HTTP-instellingen die overeenkomt met elke URL-pad. Een standaard-HTTP-instellingen ook toevoegen omdat de aanvragen die niet overeenkomen met een URL-pad opgegeven in deze regel wordt doorgestuurd naar de standaard-back-end-adresgroep met behulp van de standaard-HTTP-instellingen.

### <a name="redirection-setting"></a>Omleidingsinstelling voor

Als de omleiding is geconfigureerd voor een eenvoudige regel, wordt alle aanvragen op de bijbehorende listener worden omgeleid naar het doel van omleiding, waardoor u globale omleiding. Als de omleiding is geconfigureerd voor een pad gebaseerde regel, de aanvragen alleen op het gebied van een specifieke site, bijvoorbeeld een winkelwagen gebied aangeduid met/winkelwagen / *, wordt omgeleid naar het doel van omleiding, waardoor u op pad gebaseerde omleiding. 

Zie voor meer informatie over de mogelijkheden voor omleiding [Mapomleiding-overzicht](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

- #### <a name="redirection-type"></a>Type omleiding

  Kies het type van omleiding van vereist: Permanente, tijdelijke, gevonden of naar andere.

- #### <a name="redirection-target"></a>Doel van omleiding

  U kunt kiezen tussen een ander listener of een externe site als doel van omleiding. 

  - ##### <a name="listener"></a>Listener

    Kiezen listener als het doel van omleiding helpt bij het omleiden van een listener voor een ander listener op de gateway. Deze instelling is vereist als u wilt inschakelen van HTTP naar HTTPS-omleiding, dat wil zeggen, omleidings-verkeer van de bron-listener controleren op de binnenkomende HTTP-aanvragen naar de doel-listener controleren op de inkomende HTTPS-aanvragen. U kunt ook de queryreeks en het pad in de oorspronkelijke aanvraag moeten worden opgenomen in de aanvraag doorgestuurd naar het doel van omleiding.![Application-gateway-onderdelen](./media/configuration-overview/configure-redirection.png)

    Zie voor meer informatie over HTTP naar HTTPS-omleiding, [HTTP naar HTTP-omleiding met behulp van portal](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal), [HTTP naar HTTP-omleiding met behulp van PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell), [HTTP naar HTTP-omleiding met behulp van CLI](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

  - ##### <a name="external-site"></a>Externe site

    Externe site kiezen als u wilt omleiden van verkeer op de listener dus regel zijn gekoppeld worden omgeleid naar een externe site. U kunt de query-tekenreeks in de oorspronkelijke aanvraag moeten worden opgenomen in de aanvraag doorgestuurd naar het doel van omleiding. U kunt het pad in de oorspronkelijke aanvraag naar de externe site niet doorsturen.

    Zie voor meer informatie over Mapomleiding naar externe site [verkeer omleiden naar de externe site met behulp van PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell) en [https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-http-header-setting"></a>Herschrijf de HTTP-header-instelling

Op deze manier kunt u toevoegen, verwijderen of bijwerken van de HTTP-aanvraag- en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en back-end-pools.    U kunt deze functie alleen via PowerShell configureren. Portal en CLI-ondersteuning is nog niet beschikbaar. Zie voor meer informatie, [Herschrijf de HTTP-headers](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) overzicht en [configureren HTTP-header herschrijven](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration).

## <a name="http-settings"></a>HTTP-instellingen

De application gateway routeert verkeer naar de back-endservers met behulp van de configuratie die is opgegeven in dit onderdeel. Zodra u een HTTP-instelling hebt gemaakt, moet u deze koppelen aan een of meer regels voor doorsturen aanvragen.

### <a name="cookie-based-affinity"></a>Cookiegebaseerde affiniteit

Deze functie is handig als u wilt behouden een gebruikerssessie op dezelfde server. Met behulp van door de gatewaybeheerde cookies kan Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde server leiden voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie. Als de toepassing niet kan cookies gebaseerde affiniteit verwerken, klikt u vervolgens kunt u zich niet kunnen deze mogelijkheid gebruiken. Voor het gebruik van cookies gebaseerde sessieaffiniteit, moet u ervoor zorgen dat de clients ondersteuning voor cookies bieden moeten. 

### <a name="connection-draining"></a>Verwerkingsstop voor verbindingen

Verwerkingsstop voor verbindingen helpt u om back-endgroepsleden zonder problemen te verwijderen tijdens geplande service-updates. Deze instelling kan worden toegepast op alle leden van een back-endpool tijdens het maken van regels. Eenmaal is ingeschakeld, application-gateway zorgt ervoor dat alle ongedaan maken registreren exemplaren van een back-endpool niet alle nieuwe aanvragen ontvangen terwijl bestaande aanvragen om te voltooien binnen een geconfigureerde tijdslimiet. Dit geldt voor zowel back-endexemplaren die door een API-aanroep expliciet uit de back-endgroep worden verwijderd als voor back-endexemplaren die door de statuscontroles worden gerapporteerd als beschadigd.

### <a name="protocol"></a>Protocol

Application gateway ondersteunt zowel HTTP als HTTPS-protocol voor Routeringsaanvragen voor de back-endservers. Als de HTTP-protocol is gekozen, is het verkeer stromen niet versleuteld naar de back-endservers. In deze gevallen waarbij niet-versleutelde communicatie met de back-endservers kan niet worden acceptabele gebruikt, moet u het HTTPS-protocol. Deze instelling in combinatie met HTTPS-protocol te kiezen in de listener, kunt u inschakelen [end-to-end SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hierdoor kunt u veilig gevoelige gegevens verzenden naar de back-end die is versleuteld. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

### <a name="port"></a>Poort

Dit is de poort die de back-endservers luisteren op het verkeer dat afkomstig is van de toepassingsgateway. U kunt poorten tussen 1 en 65535 zijn.

### <a name="request-timeout"></a>Time-out van de aanvraag

Het aantal seconden dat de toepassingsgateway moet wachten op reactie ontvangen van de back-endpool alvorens een foutbericht "Time-out verbinding".

### <a name="override-backend-path"></a>Back-endpad overschrijven

Deze instelling kunt u een optionele aangepaste doorsturen-pad moet worden gebruikt wanneer de aanvraag wordt doorgestuurd naar de back-end configureren. Hiermee kopieert u een deel van het binnenkomende pad die overeenkomt met het aangepaste pad opgegeven in de **overschrijven van het back-endpad** veld naar de doorgestuurde pad. Zie de onderstaande tabel voor informatie over de werking van de capaciteit.

- Als de HTTP-instellingen is gekoppeld aan een regel voor het doorsturen van een algemene aanvraag:

  | Oorspronkelijke aanvraag  | Back-endpad overschrijven | Aanvraag doorgestuurd naar de back-end |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home/            | /override/            | / vervangen/home /              |
  | / home/secondhome / | /override/            | / onderdrukking/home/secondhome /   |

- Als de HTTP-instellingen is gekoppeld aan een regel voor het doorsturen van een pad op basis van aanvraag:

  | Oorspronkelijke aanvraag           | Padregel       | Back-endpad overschrijven | Aanvraag doorgestuurd naar de back-end |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | / vervangen/home /              |
  | / pathrule/home/secondhome / | /pathrule*      | /override/            | / onderdrukking/home/secondhome /   |
  | /Home/                     | /pathrule*      | /override/            | / vervangen/home /              |
  | / home/secondhome /          | /pathrule*      | /override/            | / onderdrukking/home/secondhome /   |
  | /pathrule/home /            | / pathrule/home * | /override/            | /override/                   |
  | / pathrule/home/secondhome / | / pathrule/home * | /override/            | / vervangen/secondhome /        |

### <a name="use-for-app-service"></a>Gebruiken voor App Service

Dit is een snelkoppeling naar de gebruikersinterface die u de twee vereiste instellingen voor back-end van App service selecteert-kunnen ophalen van de hostnaam van de back-endadres en wordt een nieuwe aangepaste test gemaakt. De reden waarom voormalige is voltooid, wordt uitgelegd in de sectie voor **kiezen hostnaam van de back-endadres** instelling. Een nieuwe test wordt gemaakt wanneer de test-header ook wordt opgehaald van-adres van de back-end-lid.

### <a name="use-custom-probe"></a>Aangepaste test gebruiken

Deze instelling wordt gebruikt om te koppelen een [aangepaste test](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) met deze HTTP-instelling. U kunt slechts één aangepaste test koppelen met een HTTP-instelling. Als u niet expliciet een aangepaste test vervolgens koppelen [standaard test](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) wordt gebruikt voor het bewaken van de status van de back-end. Het verdient aanbeveling dat u een aangepaste test als u gedetailleerdere controle over de statuscontrole van uw back-ends wilt maken.

> [!NOTE]   
> Aangepaste test begint niet met de status van de back-endpool bewaken, tenzij de bijbehorende HTTP-instelling expliciet gekoppeld aan een listener is.

### <a name="pick-host-name-from-backend-address"></a>Kies een hostnaam uit de back-endadressen

Deze mogelijkheid wordt dynamisch de *host* header in de aanvraag voor de hostnaam van de back-endpool met behulp van een IP-adres of de volledig gekwalificeerde domeinnaam (FQDN). Dit is handig in de scenario's waar de domeinnaam van de back-end verschilt van de DNS-naam van de toepassingsgateway en de back-end maakt gebruik van een specifieke host-header of SNI-extensie op te lossen naar het juiste eindpunt, zoals in het geval van multitenant-services als de back-end. Omdat appservice een service met meerdere tenants met behulp van een gedeelde ruimte met één IP-adres is, kan een appservice zijn alleen toegankelijk voor de hostnamen die zijn geconfigureerd in de instellingen van het aangepaste domein. Naam van het aangepaste domein is standaard *example.azurewebsites.net*. Dus als u toegang tot uw appservice met behulp van application gateway met een van beide een hostnaam niet expliciet worden geregistreerd in appservice of met de FQDN van de toepassingsgateway wilt, hebt u de hostnaam in de oorspronkelijke aanvraag aan de hostnaam van de appservice, negeren door inschakelen van **kiezen hostnaam van de back-endadres** instelling.

Als u een aangepast domein en de bestaande aangepaste DNS-naam naar de App-service hebt toegewezen, hoeft niet u deze instelling wilt inschakelen.

> [!NOTE]   
> Deze instelling is niet vereist voor App Service Environment (ASE) sinds de as-omgeving is een specifieke implementatie. 

### <a name="host-name-override"></a>Host naam negeren

Deze functie vervangt de *host* -header in de inkomende aanvraag op de toepassingsgateway de naam van de host die u hier opgeeft. Bijvoorbeeld, als www\.contoso.com is opgegeven als de **hostnaam** instellen, de oorspronkelijke aanvraag https://appgw.eastus.cloudapp.net/path1 wordt gewijzigd in https://www.contoso.com/path1 wanneer de aanvraag wordt doorgestuurd naar de back-endserver. 

## <a name="backend-pool"></a>Back-endpool

Een back-endpool kan worden waarnaar wordt verwezen naar vier typen van de leden van de back-end: een specifieke virtuele machine, virtuele-machineschaalset, een IP-adres/de FQDN of een appservice. Elke back-endpool kan verwijzen naar meerdere leden van hetzelfde type. Leden van verschillende typen in dezelfde back endadresgroep aan te wijzen, wordt niet ondersteund. 

Nadat u een back endpool maakt, moet u deze koppelen aan een of meer aanvraag-routeringsregels. U moet ook statuscontroles voor elke back-endpool configureren op uw application gateway. Als een aanvraag voor routering regelvoorwaarde wordt voldaan, verzendt de toepassingsgateway het verkeer naar de in orde servers (zoals wordt bepaald door de statuscontroles) in de bijbehorende back-endpool.

## <a name="health-probes"></a>Statuscontroles

Hoewel application-gateway controleert de status van alle resources in de back-end standaard, is het raadzaam maken van een aangepaste test voor elke back-end-HTTP-instelling zodat u deze hebt een meer nauwkeurige controle over de statuscontrole. Zie voor informatie over het configureren van aangepaste statustest, [aangepaste health test instellingen](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]   
> Als u een aangepaste statustest maakt, moet u deze te koppelen aan een back-end-HTTP-instelling. Aangepaste test begint niet met de status van de back-endpool bewaken, tenzij de bijbehorende HTTP-instelling expliciet gekoppeld aan een listener is.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over Application Gateway-onderdelen, kunt u het volgende doen:

- [Een toepassingsgateway maken in Azure portal](quick-create-portal.md)
- [Maak een toepassingsgateway met behulp van PowerShell](quick-create-powershell.md)
- [Maak een toepassingsgateway met behulp van Azure CLI](quick-create-cli.md)