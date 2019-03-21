---
title: End-to-end SSL on Azure Application Gateway inschakelen
description: In dit artikel wordt een overzicht van de Application Gateway end-to-end SSL ondersteunen.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: 92799019d13de71d911767d8e400598513587667
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258392"
---
# <a name="overview-of-ssl-termination-and-end-to-end-ssl-with-application-gateway"></a>Overzicht van SSL-beëindiging en end-to-end SSL met Application Gateway

Secure Sockets Layer (SSL) is de standaard-beveiliging-technologie voor het tot stand brengen van een gecodeerde verbinding tussen een webserver en een browser. Deze koppeling zorgt ervoor dat alle gegevens tussen de webserver en browsers doorgegeven blijven persoonlijke en versleuteld. Application gateway ondersteunt zowel SSL-beëindiging aan de gateway, evenals de end-to-end SSL-versleuteling.

## <a name="ssl-termination"></a>SSL-beëindiging

Application Gateway ondersteunt SSL-beëindiging op de gateway, na welk verkeer normaal gesproken onversleuteld naar de back-endservers. Er zijn een aantal voordelen van het SSL-beëindiging aan de application gateway doet:

- **Verbeterde prestaties** – de grootste prestaties tegenkomen wanneer u SSL-ontsleuteling doen is de eerste handshake. Voor betere prestaties, de server doen ontsleutelen SSL-sessie-id's in de cache opslaat en TLS sessietickets voor negering beheert. Als dit wordt gedaan bij de application gateway, kunnen alle aanvragen van dezelfde client waarden in het cachegeheugen te gebruiken. Als dit voltooid op de back-endservers, heeft de telkens wanneer die van de client-aanvragen gaat u naar een andere server de client naar re‑authenticate. Het gebruik van TLS-tickets kan helpen bij het oplossen van dit probleem, maar ze worden niet ondersteund door alle clients en kunnen lastig zijn om te configureren en beheren.
- **Beter gebruik van de back-endservers** – SSL/TLS-verwerking is zeer intensieve CPU en wordt steeds meer intensieve sleutelgrootten vergroten. Dit werk verwijderen uit de back-endservers kan ze zich richten op wat ze efficiënt zijn leveren van inhoud op.
- **Intelligente routering** – door het ontsleutelen van het verkeer, de application gateway toegang heeft tot de inhoud van de aanvraag, zoals kopteksten, URI, enzovoort, en kunt deze gegevens om aanvragen te sturen.
- **Certificaatbeheer** – certificaten hoeft alleen te worden gekocht en geïnstalleerd op de toepassingsgateway en niet alle back-endservers. Dit bespaart tijd en geld.

Een SSL-certificaat is vereist voor het configureren van SSL-beëindiging, moet worden toegevoegd aan de listener om in te schakelen van de toepassingsgateway voor het afleiden van een symmetrische sleutel volgens de specificatie van de SSL-protocol. De symmetrische sleutel wordt vervolgens gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden naar de gateway. Het SSL-certificaat moet zich in Personal Information Exchange (PFX)-indeling. De bestandsindeling kunt u exporteert de persoonlijke sleutel die is vereist voor de toepassingsgateway om uit te voeren van de versleuteling en ontsleuteling van verkeer.

> [!NOTE] 
>
> Application gateway biedt geen een mogelijkheid om te maken van een nieuw certificaat of een certificaataanvraag verzenden naar een certificeringsinstantie (CA).

Voor de SSL-verbinding om te werken, moet u ervoor zorgen dat het SSL-certificaat voldoet aan de volgende voorwaarden:

- Dat de huidige datum en tijd is binnen de "Geldig vanaf" en "Geldig tot" datumbereik op het certificaat.
- Deze naam van het certificaat' algemene' (CN) komt overeen met de host-header in de aanvraag. Bijvoorbeeld, als de client een aanvraag voor `https://www.contoso.com/`, moet de algemene naam `www.contoso.com`.

### <a name="certificates-supported-for-ssl-termination"></a>Certificaten die worden ondersteund voor SSL-beëindiging

Application gateway ondersteunt de volgende typen certificaten:

- Certificaat van de CA (certificeringsinstantie): Een CA-certificaat is een digitaal certificaat dat is uitgegeven door een certificeringsinstantie (CA)
- VW (uitgebreide validatie)-certificaat: Een certificaat VW is een standaardcertificaat branche richtlijnen. Hiermee schakelt u de browser locator balk groen en bedrijfsnaam ook publiceren.
- Certificaat met jokertekens: Dit certificaat biedt ondersteuning voor een willekeurig aantal subdomeinen op basis van *. site.com, waar uw subdomein vervangen de *. Het site.com, echter niet het geval, ondersteuning, zodat als de gebruikers toegang hebben tot uw website zonder dat de voorloopspaties 'www' typen, het certificaat met jokertekens wordt niet behandeld die.
- Zelfondertekende certificaten: Clientbrowsers deze certificaten niet vertrouwt, en de gebruiker wordt gewaarschuwd dat de virtuele servicecertificaat niet deel van een vertrouwensketen uitmaakt. Zelfondertekende certificaten zijn goed voor test- of omgevingen waar beheerders bepalen van de clients en beveiligingswaarschuwingen van de browser veilig kunnen negeren. Werkbelastingen voor productie mag nooit gebruik van zelfondertekende certificaten.

Zie voor meer informatie, [configureren van SSL-beëindiging met application gateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

## <a name="end-to-end-ssl-encryption"></a>End-to-end SSL-versleuteling

Sommige klanten kunnen geen niet-versleutelde communicatie met de back-endservers vereisen. Dit kan komen door de beveiligings- of nalevingsvereisten, of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor zulke toepassingen ondersteunt Application Gateway end-to-end SSL-versleuteling.

End-to-end SSL kunt u veilig gevoelige gegevens verzenden naar de back-end versleuteld terwijl nog steeds profiteren van de voordelen van de Layer 7 load balancing-functies die application gateway biedt. Voorbeelden van deze functies zijn: sessieaffiniteit op basis van cookies, routering op basis van een URL, ondersteuning voor routering op basis van sites en de mogelijkheid om X-Forwarded-\*-headers in te voegen.

Wanneer Application Gateway is geconfigureerd met de communicatiemodus voor end-to-end SSL, worden door Application Gateway de SSL-sessies op de gateway beëindigd en wordt het gebruikersverkeer ontsleuteld. Vervolgens worden de geconfigureerde regels toegepast voor het selecteren van het juiste exemplaar van de back-endgroep waarnaar het verkeer moet worden doorgeleid. Application Gateway initieert vervolgens een nieuwe SSL-verbinding met de back-endserver en versleutelt de gegevens opnieuw met het openbare-sleutelcertificaat van de back-endserver, voordat de aanvraag naar de back-endserver wordt verstuurd. Reacties van de webserver ondergaan hetzelfde proces terug naar de eindgebruiker. End-to-end SSL wordt ingeschakeld door de protocolinstelling [back-end-HTTP-instelling](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) naar HTTPS, die wordt vervolgens toegepast op een back-endpool.

De SSL-beleid is van toepassing op zowel de front-end en back-end-verkeer. Op de front-end, Application Gateway fungeert als de server en het beleid wordt afgedwongen. Op de back-end, Application Gateway fungeert als de client en de informatie protocol/codering verzendt als de voorkeur tijdens de SSL-handshake.

Application gateway communiceert alleen met deze back-end-exemplaren een van beide endexemplaren waarvan het certificaat met de application gateway of waarvan de certificaten zijn ondertekend door bekende CA instanties waar CN van het certificaat komt overeen met de naam van de host in de http- instellingen voor back-end. Het gaat hierbij om het vertrouwde Azure-services zoals Azure App service-web-apps en Azure API Management.

Als de certificaten van de leden in de back-endpool zijn niet ondertekend door bekende CA-instanties, moet elk exemplaar in de back-endgroep waarvoor end-to-end SSL ingeschakeld worden geconfigureerd met een certificaat zodat beveiligde communicatie. Het certificaat toe te voegen, zorgt u ervoor dat de application gateway communiceert alleen met bekende back-end-exemplaren. Dit verder beveiligt de end-to-end communicatie.

> [!NOTE] 
>
> Certificaatinstallatie verificatie is niet vereist voor betrouwbare Azure-services zoals Azure App service-web-apps en Azure API Management.

> [!NOTE] 
>
> Het certificaat dat is toegevoegd aan **back-end-HTTP-instelling** om te verifiëren van de back-end servers kunnen niet dezelfde zijn als het certificaat dat is toegevoegd aan de **listener** voor SSL-beëindiging bij application gateway of een ander voor Verbeterde beveiliging.

![end-to-end SSL-scenario][1]

In dit voorbeeld worden-aanvragen via TLS1.2 doorgestuurd naar de back-endservers in Pool1 met behulp van end-to-end SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-end SSL en het opnemen van certificaten in een lijst van toegestane certificaten

Application Gateway communiceert alleen met bekende back-endexemplaren waarvan het certificaat is opgenomen in een lijst van toegestane certificaten voor de Application Gateway. Als u het opnemen van certificaten in een lijst van toegestane certificaten wilt inschakelen, moet u de openbare sleutel van de back-endservercertificaten uploaden naar de Application Gateway (niet het basiscertificaat). Alleen verbindingen met bekende en goedgekeurde back-ends zijn vervolgens toegestaan. De resterende back-ends hebben een gatewayfout als resultaat. Zelfondertekende certificaten zijn uitsluitend bedoeld voor testdoeleinden en het wordt afgeraden om deze in een productieomgeving te gebruiken. Dergelijke certificaten moeten zijn opgenomen in de whitelist met de application gateway zoals beschreven in de voorgaande stappen voordat ze kunnen worden gebruikt.

> [!NOTE]
> Certificaatinstallatie verificatie is niet vereist voor betrouwbare Azure-services zoals Azure App Service.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>End-to-end SSL met de v2-SKU

Certificaten voor serververificatie zijn afgeschaft en vervangen door vertrouwde basiscertificaten is opgenomen in de Application Gateway v2-SKU. Ze werken op dezelfde manier voor verificatie van certificaten met enkele belangrijke verschillen:

- Certificaten die zijn ondertekend door bekende CA instanties waarvan CN komt overeen met de hostnaam van de in de back-end-HTTP-instellingen hoeven niet een extra stap voor end-to-end SSL om te werken. 

   Bijvoorbeeld, als de back-end-certificaten zijn uitgegeven door een bekende Certificeringsinstantie en heeft een algemene naam van contoso.com en de back-end-http-instelling host veld ook is ingesteld op contoso.com, zijn klikt u vervolgens geen extra stappen vereist. U kunt instellen dat de back-end-http protocol stellen op HTTPS en beide de health-test- en de pad SSL is ingeschakeld. Als u Azure App Service of andere Azure-web-services als uw back-end, dit zijn ook impliciet worden vertrouwd en er is geen verdere stappen nodig zijn voor end-to-end SSL.
- Als het certificaat zelfondertekend is of ondertekend door onbekende tussenpersonen, moeten klikt u vervolgens om in te schakelen van end-to-end SSL in v2 SKU voor een vertrouwd basiscertificaat worden gedefinieerd. Application Gateway communiceert alleen met back-ends voor het basiscertificaat waarvan servercertificaat komt overeen met een van de lijst met vertrouwde basiscertificaten in de back-end-http-instelling die is gekoppeld aan de groep.
- Naast de basis-certificaat overeenkomen valideert Application Gateway ook als de Host die is opgegeven in de back-end-http-instelling komt overeen met die van de algemene naam (CN) aangeboden door de back-endserver SSL-certificaat. Bij een poging tot stand brengen van een SSL-verbinding met de back-end, wordt de Server Name Indication (SNI)-extensie in Application Gateway ingesteld op de Host die is opgegeven in de back-end-http-instelling.
- Als **kiezen hostnaam van de back-endadres** in plaats van de Host-veld in de back-end-http-instelling is gekozen, en vervolgens de SNI-header is altijd ingesteld op de back-endpool FQDN-naam en de algemene naam van de back-endserver SSL certificaat moet overeenkomen met de FQDN-naam. Back-endpoolleden met IP-adressen worden niet ondersteund in dit scenario.
- Het basiscertificaat is een met base64 gecodeerde basiscertificaat van de back-end-servercertificaten.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over end-to-end SSL, gaat u naar [end-to-end SSL configureren met behulp van Application Gateway met PowerShell](application-gateway-end-to-end-ssl-powershell.md) te maken van een toepassingsgateway met behulp van end-to-end SSL.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
