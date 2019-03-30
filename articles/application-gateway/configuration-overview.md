---
title: Overzicht van Azure Application Gateway-configuratie
description: In dit artikel wordt beschreven hoe u de onderdelen van Azure Application Gateway configureren
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: absha
ms.openlocfilehash: 371d15f59c091f7ac38d36bfe3de5f4b31e4482c
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629635"
---
# <a name="application-gateway-configuration-overview"></a>Overzicht van de configuratie van de Application Gateway

Azure Application Gateway bestaat uit verschillende onderdelen die u op verschillende manieren voor verschillende scenario's configureren kunt. Dit artikel leest u hoe het configureren van elk onderdeel.

![Stroomdiagram voor Application Gateway-onderdelen](./media/configuration-overview/configuration-overview1.png)

Deze afbeelding ziet u een toepassing met drie listeners. De eerste twee multi-site-listeners voor zijn `http://acme.com/*` en `http://fabrikam.com/*`, respectievelijk. Beide luisteren op poort 80. De derde is een basislistener waarvoor end-to-end Secure Sockets Layer (SSL) beëindigd.

## <a name="prerequisites"></a>Vereisten

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure-netwerk en toegewezen subnet

Een application gateway is een specifieke implementatie in uw virtuele netwerk. Binnen uw virtuele netwerk is een speciaal subnet vereist voor de toepassingsgateway. U kunt meerdere exemplaren van de implementatie van de gateway van een bepaalde toepassing hebben in een subnet. U kunt ook andere Toepassingsgateways in het subnet implementeren. Maar u kunt een andere resource in het subnet van de toepassingsgateway niet implementeren.

> [!NOTE]
> Standard_v2 en standaard Azure Application Gateway kunnen niet worden gecombineerd in hetzelfde subnet.

#### <a name="size-of-the-subnet"></a>Grootte van het subnet

Application Gateway verbruikt 1 privé IP-adres per exemplaar, plus een andere privé-IP-adres als een privé front-end-IP-adres is geconfigureerd.

Azure reserveert ook 5 IP-adressen in elk subnet voor intern gebruik: de eerste 4 en de laatste IP-adressen. Denk bijvoorbeeld 15 application gateway-instanties met geen privé front-end-IP-adres. U moet ten minste 20 IP-adressen voor dit subnet: 5 voor intern gebruik en 15 voor de application gateway-instanties. U moet dus een/27 subnet groot of groter zijn.

Houd rekening met een subnet met 27 application gateway-instanties en een IP-adres voor een privé front-end-IP-adres. In dit geval moet u 33 IP-adressen: 27 voor de application gateway-instanties, 1 voor de privé-front-end en 5 voor intern gebruik. U moet dus een /26 subnet groot of groter zijn.

U wordt aangeraden dat u een subnetgrootte van ten minste/28. Deze grootte biedt u 11 bruikbare IP-adressen. Als de belasting van uw toepassing meer dan 10 IP-adressen vereist, kunt u overwegen een/27 of/26 grootte van het gatewaysubnet.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Netwerkbeveiligingsgroepen op de Application Gateway-subnet

Netwerkbeveiligingsgroepen (nsg's) worden ondersteund in Application Gateway. Maar er zijn enkele beperkingen:

- U moet uitzonderingen voor verkeer dat binnenkomt op poorten 65503 65534 voor de SKU van Application Gateway v1 en poorten 65200-65535 voor de v2-SKU opnemen. Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Deze poorten worden beveiligd (vergrendeld) met Azure-certificaten. Externe entiteiten, inclusief de klanten van deze gateways, kunnen de wijzigingen op de eindpunten zonder de juiste certificaten in plaats niet starten.

- Uitgaande verbinding met internet kan niet worden geblokkeerd. Standaard regels voor uitgaand verkeer in de NSG toestaan verbinding met internet. We raden aan dat u:

  - De standaardregels voor uitgaand niet worden verwijderd.
  - Maak geen andere regels voor uitgaand verkeer die uitgaande verbinding met internet weigeren.

- Verkeer van de **AzureLoadBalancer** tag moet worden toegestaan.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Lijst met toegestane adressen Application Gateway toegang tot een paar bron-IP 's

Voor dit scenario gebruikt u nsg's op de Application Gateway-subnet. Plaats de volgende beperkingen op het subnet in deze volgorde van prioriteit:

1. Toestaan van binnenkomend verkeer van een bron-IP/IP-bereik.
2. Toestaan van binnenkomende aanvragen van alle bronnen op poorten 65503 65534 voor [back-endstatus communicatie](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Deze poorten worden beveiligd (vergrendeld) met Azure-certificaten. Externe entiteiten kunnen wijzigingen op de eindpunten niet starten zonder de juiste certificaten op locatie.
3. Toestaan dat binnenkomende Azure Load Balancer controleert (*AzureLoadBalancer* tag) en binnenkomend verkeer van virtueel netwerk (*VirtualNetwork* tag) op de [netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Al het andere binnenkomende verkeer blokkeren met behulp van een regel voor weigeren alle.
5. Sta uitgaand verkeer naar internet voor alle bestemmingen.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Gebruiker gedefinieerde routes die worden ondersteund op het subnet voor Application Gateway

Gebruiker gedefinieerde routes (udr's) worden ondersteund op de Application Gateway-subnet voor de v1-SKU, zolang ze end-to-end verzoek/reactie-communicatie niet wijzigen. U kunt bijvoorbeeld een UDR in het subnet voor Application Gateway instellen om te verwijzen naar een firewallapparaat voor inspecties van pakketten. Maar u moet ervoor zorgen dat het pakket de beoogde bestemming na controle kan bereiken. Dit niet doet, kan leiden tot onjuiste statustest of gedrag routering van verkeer. Dit omvat geleerde routes of 0.0.0.0/0 standaardroutes die zijn doorgegeven door de Azure ExpressRoute of VPN-gateways in het virtuele netwerk.

Voor de v2-SKU, worden niet udr's ondersteund op de Application Gateway-subnet. Zie voor meer informatie, [automatisch schalen en zoneredundantie voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#known-issues-and-limitations).

> [!NOTE]
> Met behulp van udr's op het subnet voor Application Gateway zorgt ervoor dat de status in de [back-endstatus weergave](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) wordt weergegeven als 'Onbekend'. Het is ook ervoor zorgt dat het genereren van Application Gateway-logboeken en metrische gegevens mislukken. U wordt aangeraden dat u udr's niet in de Application Gateway-subnet gebruikt zodat u de back-endstatus, logboeken en metrische gegevens kunt weergeven.

## <a name="front-end-ip"></a>Front-end-IP

U kunt de toepassingsgateway met een openbaar IP-adres, een privé IP-adres of beide. Een openbaar IP-adres is vereist wanneer u een back-end die clients moeten toegang hebben tot via het internet via een internetgerichte VIP (virtual IP) host. 

Een openbaar IP-adres is niet vereist voor een intern eindpunt dat niet wordt blootgesteld aan internet. Deze staat bekend als een *interne load balancer* (ILB)-eindpunt. Een application gateway ILB is handig voor interne line-of-business-toepassingen die niet worden blootgesteld aan internet. Het is ook handig voor services en lagen in een toepassing met meerdere lagen binnen een beveiligingsgrens die niet worden blootgesteld aan internet, maar waarvoor round robin-distributie, sessiepersistentie of SSL-beëindiging te laden.

Slechts 1 openbaar IP-adres of 1 privé-IP-adres wordt ondersteund. U kunt het front-end-IP-adres kiezen wanneer u de toepassingsgateway maakt.

- U kunt voor een openbaar IP-adres, maak een nieuwe openbare IP-adres of een bestaande openbare IP-adres gebruiken op dezelfde locatie als de toepassingsgateway. Als u een nieuwe openbare IP-adres, het type van de IP-adres dat u maakt (statische of dynamische) kunnen niet later worden gewijzigd. Zie voor meer informatie, [statische versus dynamische openbare IP-adres](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-components#static-vs-dynamic-public-ip-address).

- Voor een privé IP-adres, kunt u een privé IP-adres van het subnet waarin de application gateway wordt gemaakt. Als u er geen opgeeft, wordt automatisch een willekeurige IP-adres geselecteerd uit het subnet. Zie voor meer informatie, [een toepassingsgateway maken met een interne load balancer](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Een front-end-IP-adres is gekoppeld aan een *listener*, die controleert op binnenkomende aanvragen op de front-end-IP-adres.

## <a name="listeners"></a>Listeners

Een listener is een logische eenheid die controleert op binnenkomende verbindingsaanvragen met behulp van de poort, protocol, host en IP-adres. Wanneer u de listener configureert, moet u waarden opgeven voor deze die overeenkomen met de bijbehorende waarden in de inkomende aanvraag op de gateway.

Wanneer u een toepassingsgateway met behulp van Azure portal maakt, maken u ook een standaard-listener door het kiezen van het protocol en poort voor de listener. U kunt kiezen of u wilt inschakelen van ondersteuning voor HTTP2 op de listener. Nadat u de application gateway maakt, kunt u de instellingen van die standaard-listener (*appGatewayHttpListener*/*appGatewayHttpsListener*) of maak nieuwe listeners.

### <a name="listener-type"></a>Listener-type

Wanneer u een nieuwe listener maakt, kiest u tussen [ *basic* en *multi-site*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners).

- Als u een enkele site achter een application gateway host, kies basic. Informatie over [over het maken van een toepassingsgateway met een basislistener](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Als u meer dan één webtoepassing of meerdere subdomeinen van hetzelfde bovenliggende domein op hetzelfde exemplaar van application gateway configureert, kiest u de listener voor meerdere locaties. Voor een listener voor meerdere locaties, moet u ook een hostnaam invoeren. Dit is omdat Application Gateway maakt gebruik van HTTP 1.1-hostheaders voor het hosten van meer dan één website op hetzelfde openbare IP-adres en dezelfde poort.

#### <a name="order-of-processing-listeners"></a>Volgorde van de verwerking van listeners

Voor de SKU v1 worden listeners verwerkt in de volgorde waarin ze worden weergegeven. Als een basislistener overeenkomt met een inkomende aanvraag, verwerkt de listener die aanvraag eerst. Stel daarom multi-site-listeners voordat basic listeners om ervoor te zorgen dat verkeer wordt doorgestuurd naar de juiste back-end.

Multi-site-listeners zijn voor de SKU v2 verwerkt, voordat u eenvoudige listeners.

### <a name="front-end-ip"></a>Front-end-IP

Kies de front-end-IP-adres dat u van plan bent om te koppelen aan deze listener. De listener luistert naar binnenkomende aanvragen op dit IP-adres.

### <a name="front-end-port"></a>Front-endpoort

Kies de front-endpoort. Selecteer een bestaande poort of een nieuwe maken. Kies een waarde van de [het toegestane bereik van poorten](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). U kunt niet alleen bekende poorten, zoals 80 en 443, maar toegestane aangepaste poort die geschikt is. Een poort kan worden gebruikt voor listeners voor openbare of particuliere gerichte listeners.

### <a name="protocol"></a>Protocol

Kies HTTP of HTTPS:

- Als u HTTP kiest, wordt het verkeer tussen de client en de application gateway is niet versleuteld.

- Kies HTTPS als u wilt dat [SSL-beëindiging](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssl-terminationl) of [end-to-end SSL-versleuteling](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Het verkeer tussen de client en de application gateway is versleuteld. En de SSL-verbinding wordt beëindigd bij de application gateway. Als u end-to-end SSL-versleuteling wilt gebruiken, moet u HTTPS kiezen en configureert de **back-end-HTTP** instelling. Dit zorgt ervoor dat verkeer wordt opnieuw versleuteld wanneer ze uit de application gateway naar de back-end.

Voor het configureren van SSL-beëindiging en end-to-end SSL-versleuteling, moet u een certificaat toevoegen aan de listener om in te schakelen van de toepassingsgateway voor het afleiden van een symmetrische sleutel. Dit wordt bepaald door de specificatie van het SSL-protocol. De symmetrische sleutel wordt gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden naar de gateway. Het gatewaycertificaat moet zich in Personal Information Exchange (PFX)-indeling. Deze indeling kunt u exporteert de persoonlijke sleutel die de gateway gebruikt voor het versleutelen en ontsleutelen van verkeer.

#### <a name="supported-certificates"></a>Ondersteunde certificaten

Zie [certificaten die worden ondersteund voor SSL-beëindiging](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Aanvullende protocolondersteuning

#### <a name="http2-support"></a>Ondersteuning voor HTTP2

Ondersteuning voor HTTP/2-protocol is beschikbaar voor clients die verbinding met application gateway alleen listeners maken. De communicatie met de back-endserver van toepassingen is via HTTP/1.1. Ondersteuning voor HTTP/2 is standaard uitgeschakeld. De volgende Azure PowerShell-codefragment laat zien hoe deze:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Ondersteuning voor WebSocket

WebSocket-ondersteuning is standaard ingeschakeld. Er is geen gebruiker configureerbare instelling inschakelen of uitschakelen. U kunt WebSockets gebruiken met HTTP en HTTPS-listeners.

### <a name="custom-error-pages"></a>Aangepaste foutpagina's

U kunt aangepaste fout bij het globale niveau of de listener definiëren. Maar het globale niveau aangepaste foutpagina's maken vanuit Azure portal is momenteel niet ondersteund. U kunt een aangepaste foutpagina weergegeven voor een fout 403 web application firewall of een 502 onderhoudspagina configureren op het niveau van de listener. U moet ook een openbaar toegankelijke blob-URL voor de opgegeven foutstatuscode opgeven. Zie voor meer informatie [Aangepaste foutpagina's maken voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Foutcodes voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Zie configureren van een globale aangepaste foutpagina [Azure PowerShell-configuratie](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="ssl-policy"></a>SSL-beleid

U kunt centraliseer het beheer van SSL-certificaat en verminder ontsleutelen overhead voor een back-end-server-farm. Gecentraliseerde SSL-verwerking kunt u een centrale SSL-beleid dat geschikt voor de beveiligingsvereisten van uw opgeven. U kunt ervoor kiezen *standaard*, *vooraf gedefinieerde*, of *aangepaste* SSL-beleid.

U configureren SSL-beleid voor het besturingselement SSL-protocolversies. U kunt een toepassingsgateway voor het weigeren van TLS1.0 TLS1.1 en TLS1.2 configureren. Standaard worden SSL 2.0 en 3.0 zijn uitgeschakeld en niet kunnen worden geconfigureerd. Zie voor meer informatie, [overzicht van Application Gateway SSL-beleid](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Nadat u een listener maakt, koppelt u deze met een regel routering van toepassingsaanvragen. Deze regel bepaalt hoe de aanvragen die worden ontvangen op de listener worden doorgestuurd naar de back-end.

## <a name="request-routing-rules"></a>Aanvragen van regels voor doorsturen

Wanneer u een toepassingsgateway met behulp van Azure portal maakt, maakt u een standaardregel (*rule1*). Deze regel verbindt de listener met standaard (*appGatewayHttpListener*) met het standaard back-end-adresgroep (*appGatewayBackendPool*) en de standaardinstellingen van de back-end-HTTP ( *appGatewayBackendHttpSettings*). Nadat u de gateway maakt, kunt u de instellingen van de standaardregel bewerken of nieuwe regels maken.

### <a name="rule-type"></a>Regeltype

Wanneer u een regel maakt, kiest u tussen [ *basic* en *op pad gebaseerde*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule).

- Kies basic als u wilt doorsturen van alle aanvragen op de bijbehorende listener (bijvoorbeeld *blog<i></i>.contoso.com/\*)* naar één back-end-groep.
- Kies op pad gebaseerde als u wilt routeren van aanvragen van specifieke URL-paden naar specifieke back-end-pools. De pad-patroon wordt alleen toegepast op het pad van de URL, niet naar de queryparameters.

#### <a name="order-of-processing-rules"></a>Volgorde van de regels worden verwerkt

Voor de SKU v1 worden patroonvergelijking van inkomende aanvragen verwerkt in de volgorde waarin de paden worden weergegeven in de URL-pad-kaart van het pad gebaseerde regel. Als een aanvraag overeenkomt met het patroon in twee of meer paden in de pad-kaart, wordt het pad dat wordt vermeld eerst gekoppeld. En de aanvraag wordt doorgestuurd naar de back-end die is gekoppeld aan het opgegeven pad.

Voor de SKU v2 is een exacte overeenkomst hogere prioriteit dan de volgorde van pad in de URL-path-map. Als een aanvraag overeenkomt met het patroon van twee of meer paden, wordt de aanvraag doorgestuurd naar de back-end die is gekoppeld aan het pad dat precies overeenkomt met de aanvraag. Als het pad in de inkomende aanvraag komt niet exact overeen met een pad op de kaart, wordt patroonvergelijking van de aanvraag verwerkt in de lijst pad kaart volgorde voor het pad gebaseerde regel.

### <a name="associated-listener"></a>Bijbehorende listener

Koppelen van een listener voor de regel zodat de *routering van toepassingsaanvragen regel* die gekoppeld aan de listener wordt geëvalueerd voor de back-end-adrespool voor het routeren van de aanvraag om te bepalen.

### <a name="associated-back-end-pool"></a>Gekoppelde back-end-pool

Koppelen aan de regel ontvangt van de back-end-adrespool met de back-end-doelen die aanvragen die verwerken de listener.

 - Voor een eenvoudige regel is slechts één back-end-pool toegestaan. Alle aanvragen op de bijbehorende listener worden doorgestuurd naar die groep back-end.

 - Voor een pad gebaseerde regel toevoegen meerdere back-end-adresgroepen die overeenkomen met voor elk URL-pad. De aanvragen die voldoen aan het URL-pad dat opgegeven, worden doorgestuurd naar de bijbehorende back-end-pool. Voeg ook een standaard back-end-adresgroep. Aanvragen die niet overeenkomen met een URL-pad in de regel worden doorgestuurd naar die groep.

### <a name="associated-back-end-http-setting"></a>Gekoppelde back-end-HTTP-instelling

Een back-end-HTTP-instelling voor elke regel toevoegen. Aanvragen worden doorgestuurd van de application gateway naar de back-end-doelen met behulp van het poortnummer, protocol en andere informatie die opgegeven in deze instelling.

Voor een eenvoudige regel mag slechts één back-end-HTTP-instelling. Alle aanvragen op de bijbehorende listener worden doorgestuurd naar de bijbehorende back-end-doelen met behulp van deze HTTP-instelling.

Een back-end-HTTP-instelling voor elke regel toevoegen. Aanvragen worden doorgestuurd van de application gateway naar de back-end-doelen met behulp van het poortnummer, het protocol en andere informatie die opgegeven in deze instelling.

Voor een eenvoudige regel mag slechts één back-end-HTTP-instelling. Alle aanvragen op de bijbehorende listener worden doorgestuurd naar de bijbehorende back-end-doelen met behulp van deze HTTP-instelling.

Voor een pad gebaseerde regel toevoegen meerdere back-end HTTP-instellingen die overeenkomen met voor elk URL-pad. Aanvragen die voldoen aan het URL-pad in deze instelling worden doorgestuurd naar de bijbehorende back-end-doelen met behulp van de HTTP-instellingen die met elke URL-pad overeenkomen. Een standaard-HTTP-instelling ook toe te voegen. Aanvragen die niet overeenkomen met een URL-pad in deze regel worden doorgestuurd naar het standaard back-end-adrespool met behulp van de standaardinstelling voor HTTP.

### <a name="redirection-setting"></a>Omleidingsinstelling voor

Als omleiding voor een eenvoudige regel is geconfigureerd, worden alle aanvragen op de bijbehorende listener omgeleid naar het doel. Dit is *globale* omleiding. Als omleiding is geconfigureerd voor een pad gebaseerde regel, worden alleen-aanvragen in een specifieke site gebied omgeleid. Een voorbeeld is een winkelwagen winkelwagen gebied dat wordt aangeduid met */cart/\**. Dit is *op pad gebaseerde* omleiding.

Zie voor meer informatie over omleidingen [overzicht van Application Gateway redirect](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="redirection-type"></a>Type omleiding

Kies het type van omleiding vereist: *Permanent(301)*, *Temporary(307)*, *Found(302)*, of *Zie other(303)*.

#### <a name="redirection-target"></a>Doel van omleiding

Kies een ander listener of een externe site als doel voor de omleiding.

##### <a name="listener"></a>Listener

Kies listener als het doel van omleiding omleiden van verkeer van één listener naar een andere op de gateway. Deze instelling is vereist als u wilt dat HTTP-naar-HTTPS-omleiding. Het verkeer van de bron-listener die controleert op binnenkomende HTTP-aanvragen naar de doel-listener die controleert op inkomende HTTPS-aanvragen worden omgeleid. U kunt ook de queryreeks en het pad van de oorspronkelijke aanvraag opnemen in de aanvraag die wordt doorgestuurd naar het doel van omleiding.

![Dialoogvenster voor Application Gateway-onderdelen](./media/configuration-overview/configure-redirection.png)

Zie voor meer informatie over HTTP-naar-HTTPS-omleiding:
- [HTTP-naar-HTTP-omleiding met behulp van Azure portal](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal)
- [HTTP-naar-HTTP-omleiding met behulp van PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell)
- [HTTP-HTTP-omleiding met behulp van de Azure CLI](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

##### <a name="external-site"></a>Externe site

Externe site kiezen als u wilt omleiden van verkeer op de listener die gepaard gaat met deze regel naar een externe site. U kunt de query-tekenreeks van de oorspronkelijke aanvraag opnemen in de aanvraag die wordt doorgestuurd naar het doel van omleiding. U kunt het pad naar de externe site die in de oorspronkelijke aanvraag is niet doorsturen.

Zie voor meer informatie over omleiding:
- [Verkeer omleiden naar een externe site met behulp van PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell)
- [Verkeer omleiden naar een externe site met behulp van CLI](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-the-http-header-setting"></a>De HTTP-header-instelling schrijven

Deze instelling wordt toegevoegd, verwijderd of updates van HTTP-aanvraag- en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en de back-end-pools. U kunt alleen deze mogelijkheid via PowerShell configureren. Azure portal en CLI-ondersteuning zijn nog niet beschikbaar. Zie voor meer informatie:

 - [Herschrijf de HTTP-kopteksten, overzicht](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)
 - [HTTP-header herschrijven configureren](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration)

## <a name="http-settings"></a>HTTP-instellingen

De application gateway routeert verkeer naar de back-endservers met behulp van de configuratie die u hier. Nadat u een HTTP-instelling hebt gemaakt, moet u deze koppelen aan een of meer regels voor routering van toepassingsaanvragen.

### <a name="cookie-based-affinity"></a>Affiniteit op basis van cookies

Deze functie is handig als u wilt behouden een gebruikerssessie op dezelfde server. Cookies gateway beheerde kunnen de application gateway direct daarop volgende verkeer van een gebruikerssessie op dezelfde server voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie. Als de toepassing niet kan cookies gebaseerde affiniteit verwerken, kunt u deze functie niet gebruiken. Als u wilt gebruiken, zorg ervoor dat de clients ondersteuning bieden voor cookies.

### <a name="connection-draining"></a>Verwerkingsstop voor verbindingen

Verwerkingsstop voor verbindingen, kunt u probleemloos back-end-pool leden te verwijderen tijdens de geplande service-updates. U kunt deze instelling toepassen op alle leden van een back-end-adrespool tijdens het maken van de regel. Het zorgt ervoor dat alle ongedaan maken registreren exemplaren van een back-end-groep nieuwe aanvragen niet ontvangen. In de tussentijd zorgen, bestaande aanvragen zijn toegestaan binnen een geconfigureerde tijdslimiet wordt voltooid. Verwerkingsstop voor verbindingen geldt voor back-end-exemplaren die expliciet zijn verwijderd uit de back-end-adrespool met een API-aanroep. Dit geldt ook voor back-end-exemplaren die worden gerapporteerd als *niet in orde* tests door de status.

### <a name="protocol"></a>Protocol

Application Gateway ondersteunt zowel HTTP als HTTPS voor Routeringsaanvragen voor de back-endservers. Als u HTTP kiest, wordt verkeer naar de back-endservers is niet versleuteld. Als niet-versleutelde communicatie niet acceptabel is, kiest u HTTPS.

Deze instelling in combinatie met HTTPS in de listener ondersteunt [end-to-end SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hiermee kunt u veilig gevoelige gegevens versleuteld naar de back-end verzenden. Elke back-end-server in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie.

### <a name="port"></a>Poort

Deze instelling geeft de poort waarop de back-endservers verkeer van de toepassingsgateway luisteren. U kunt poorten tussen 1 en 65535 zijn.

### <a name="request-timeout"></a>Time-out van de aanvraag

Deze instelling is het aantal seconden dat de toepassingsgateway wacht op een reactie ontvangen van de back-end-adrespool voordat deze wordt geretourneerd met een foutbericht "Tijdsoverschrijding bij verbinding".

### <a name="override-back-end-path"></a>Pad van de back-end overschrijven

Deze instelling kunt u een optionele aangepaste doorsturen-pad moet worden gebruikt wanneer de aanvraag wordt doorgestuurd naar de back-end configureren. Een deel van het binnenkomende pad die overeenkomt met het aangepaste pad in de **overschrijven van het back-endpad** veld wordt gekopieerd naar de doorgestuurde pad. De volgende tabel laat zien hoe deze functie werkt:

- Als de HTTP-instelling is gekoppeld aan een basisregel routering van toepassingsaanvragen:

  | Oorspronkelijke aanvraag  | Pad van de back-end overschrijven | Aanvraag voor een back-end worden doorgestuurd |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home/            | /override/            | / vervangen/home /              |
  | / home/secondhome / | /override/            | / onderdrukking/home/secondhome /   |

- Als de HTTP-instelling is gekoppeld aan een pad gebaseerde routering van toepassingsaanvragen regel:

  | Oorspronkelijke aanvraag           | Padregel       | Pad van de back-end overschrijven | Aanvraag voor een back-end worden doorgestuurd |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | / vervangen/home /              |
  | / pathrule/home/secondhome / | /pathrule*      | /override/            | / onderdrukking/home/secondhome /   |
  | /Home/                     | /pathrule*      | /override/            | / vervangen/home /              |
  | / home/secondhome /          | /pathrule*      | /override/            | / onderdrukking/home/secondhome /   |
  | /pathrule/home /            | / pathrule/home * | /override/            | /override/                   |
  | / pathrule/home/secondhome / | / pathrule/home * | /override/            | / vervangen/secondhome /        |

### <a name="use-for-app-service"></a>Gebruik voor appservice

Dit is een snelkoppeling naar de gebruikersinterface die u de twee vereiste instellingen voor de back-end van Azure App Service selecteert. Hiermee kunnen **hostnaam van de back-end-adres kiezen**, maar er wordt een nieuwe aangepaste test gemaakt. (Zie voor meer informatie de [Kies hostnaam van de back-end-adres](#pick) sectie van dit artikel instellen.) Een nieuwe test is gemaakt en de test-header van het lid van de back-end-adres wordt opgehaald.

### <a name="use-custom-probe"></a>Aangepaste test gebruiken

Deze instelling wordt gekoppeld aan een [aangepaste test](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) met een HTTP-instelling. U kunt slechts één aangepaste test koppelen met een HTTP-instelling. Als u niet expliciet een aangepaste test, koppelt de [standaard test](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) wordt gebruikt voor het bewaken van de status van de back-end. U wordt aangeraden dat u voor meer controle over de statuscontrole van uw back-ends van een aangepaste test maken.

> [!NOTE]
> De aangepaste test niet de status van de back-endpool wordt bewaakt, tenzij de bijbehorende HTTP-instelling expliciet gekoppeld aan een listener is.

### <a id="pick"/></a>Hostnaam van de back-end-adres kiezen

Deze mogelijkheid wordt dynamisch de *host* header in de aanvraag voor de hostnaam van de back-endpool. Het maakt gebruik van een IP-adres of FQDN-naam.

Deze functie is handig wanneer de domeinnaam van de back-end af van de DNS-naam van de toepassingsgateway wijkt en de back-end is gebaseerd op een specifieke host-header of Server Name Indication (SNI)-extensie op te lossen naar het juiste eindpunt.

Een voorbeeld van de aanvraag is een multitenant-services als de back-end. Een appservice is een multitenant-service die gebruikmaakt van een gedeelde ruimte met één IP-adres. Een appservice kan dus alleen worden geopend via de hostnamen die zijn geconfigureerd in de instellingen van aangepast domein.

Naam van het aangepaste domein is standaard *example.azurewebsites.<i> </i>net*. Voor toegang tot uw appservice met behulp van een toepassingsgateway met behulp van een hostnaam die niet expliciet geregistreerd in de appservice of via de application gateway FQDN-naam, moet u de hostnaam in de oorspronkelijke aanvraag aan de hostnaam van de appservice onderdrukken. U doet dit door inschakelen de **kiezen hostnaam van de back-endadres** instelling.

Voor een aangepast domein met bestaande aangepaste DNS-naam is toegewezen aan de appservice, hebt u geen deze instelling wilt inschakelen.

> [!NOTE]
> Deze instelling is niet vereist voor App Service Environment voor PowerApps, dit is een specifieke implementatie.

### <a name="host-name-override"></a>Host naam negeren

Deze functie vervangt de *host* -header in de inkomende aanvraag in de application gateway met de naam van de host die u opgeeft.

Bijvoorbeeld, als *www.contoso<i></i>.com* is opgegeven in de **hostnaam** instellen, de oorspronkelijke aanvraag *https:/<i></i>/appgw.eastus.cloudapp.net/path1* wordt gewijzigd naar *https:/<i></i>/www.contoso.com/path1* wanneer de aanvraag wordt doorgestuurd naar de back-endserver.

## <a name="back-end-pool"></a>Back-end-pool

U kunt een back-end-adrespool verwijzen naar vier typen back-end-onderdelen: een specifieke virtuele machine, een virtuele-machineschaalset, een IP-adres/de FQDN of een appservice. Elke back-end-groep kan verwijzen naar meerdere leden van hetzelfde type. Die verwijst naar de leden van verschillende typen in de dezelfde back-end-pool wordt niet ondersteund.

Nadat u een back-end-pool maakt, moet u deze koppelen aan een of meer regels voor routering van toepassingsaanvragen. U moet ook statuscontroles voor elke back-end-pool configureren op uw application gateway. Wanneer een voor Aanvraagroutering regelvoorwaarde wordt voldaan, verzendt de toepassingsgateway het verkeer aan de orde servers (zoals wordt bepaald door de statuscontroles) in de bijbehorende back-end-pool.

## <a name="health-probes"></a>Statuscontroles

Een application gateway bewaakt de status van alle resources in de back-end standaard. Maar we raden aan een aangepaste test voor elke back-end-HTTP-instelling om op te halen meer controle over de statuscontrole te maken. Zie voor informatie over het configureren van een aangepaste test, [aangepaste health test instellingen](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]
> Nadat u een aangepaste statustest maakt, moet u deze te koppelen aan een back-end-HTTP-instelling. Een aangepaste test wordt niet de status van de back-end-pool controleren, tenzij de bijbehorende HTTP-instelling expliciet gekoppeld aan een listener is.

## <a name="next-steps"></a>Volgende stappen

Nu dat u over Application Gateway-onderdelen weet, kunt u het volgende doen:

- [Een toepassingsgateway maken in Azure portal](quick-create-portal.md)
- [Een toepassingsgateway maken met behulp van PowerShell](quick-create-powershell.md)
- [Een toepassingsgateway maken met behulp van de Azure CLI](quick-create-cli.md)
