---
title: End-to-end SSL on Azure Application Gateway inschakelen
description: In dit artikel wordt een overzicht van de Application Gateway end-to-end SSL ondersteunen.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2018
ms.author: amsriva
ms.openlocfilehash: e7020ef5c1f7411c7226e7a2db489112ee6bf0a4
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945498"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Overzicht van end-to-end SSL met Application Gateway

Application Gateway ondersteunt SSL-beëindiging op de gateway, na welk verkeer normaal gesproken onversleuteld naar de back-endservers. Met deze functie voorkomt u prijzige overhead voor het versleutelen en ontsleutelen voor uw webservers. Voor sommige gebruikers is onversleutelde communicatie met de back-endserververs echter onacceptabel. Deze onversleutelde communicatie kan het gevolg zijn van de beveiligings- of nalevingsvereisten of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor zulke toepassingen ondersteunt Application Gateway end-to-end SSL-versleuteling.

End-to-end SSL kunt u veilig gevoelige gegevens verzenden naar de back-end versleuteld terwijl nog steeds profiteren van de voordelen van de Layer 7 load balancing-functies die application gateway biedt. Voorbeelden van deze functies zijn: sessieaffiniteit op basis van cookies, routering op basis van een URL, ondersteuning voor routering op basis van sites en de mogelijkheid om X-Forwarded-\*-headers in te voegen.

Wanneer Application Gateway is geconfigureerd met de communicatiemodus voor end-to-end SSL, worden door Application Gateway de SSL-sessies op de gateway beëindigd en wordt het gebruikersverkeer ontsleuteld. Vervolgens worden de geconfigureerde regels toegepast voor het selecteren van het juiste exemplaar van de back-endgroep waarnaar het verkeer moet worden doorgeleid. Application Gateway initieert vervolgens een nieuwe SSL-verbinding met de back-endserver en versleutelt de gegevens opnieuw met het openbare-sleutelcertificaat van de back-endserver, voordat de aanvraag naar de back-endserver wordt verstuurd. End-to-end SSL wordt ingeschakeld door de protocolinstelling **BackendHTTPSetting** naar HTTPS, die wordt vervolgens toegepast op een back-endpool. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

![end-to-end SSL-scenario][1]

In dit voorbeeld worden-aanvragen via TLS1.2 doorgestuurd naar de back-endservers in Pool1 met behulp van end-to-end SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-end SSL en het opnemen van certificaten in een lijst van toegestane certificaten

Application Gateway communiceert alleen met bekende back-endexemplaren waarvan het certificaat is opgenomen in een lijst van toegestane certificaten voor de Application Gateway. Als u het opnemen van certificaten in een lijst van toegestane certificaten wilt inschakelen, moet u de openbare sleutel van de back-endservercertificaten uploaden naar de Application Gateway (niet het basiscertificaat). Alleen verbindingen met bekende en goedgekeurde back-ends zijn vervolgens toegestaan. De resterende back-ends hebben een gatewayfout als resultaat. Zelfondertekende certificaten zijn uitsluitend bedoeld voor testdoeleinden en het wordt afgeraden om deze in een productieomgeving te gebruiken. Dergelijke certificaten moeten zijn opgenomen in de whitelist met de application gateway zoals beschreven in de voorgaande stappen voordat ze kunnen worden gebruikt.

> [!NOTE]
> Certificaatinstallatie verificatie is niet vereist voor betrouwbare Azure-services zoals Azure Web Apps.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>End-to-end SSL met de v2-SKU

Certificaten voor serververificatie zijn afgeschaft en vervangen door vertrouwde basiscertificaten is opgenomen in de Application Gateway v2-SKU. Ze werken op dezelfde manier voor verificatie van certificaten met enkele belangrijke verschillen:

- Certificaten die zijn ondertekend door bekende CA instanties waarvan CN komt overeen met de hostnaam van de in de back-end-HTTP-instellingen hoeven niet een extra stap voor end-to-end SSL om te werken. 

   Bijvoorbeeld, als de back-end-certificaten zijn uitgegeven door een bekende Certificeringsinstantie en heeft een algemene naam van contoso.com en de back-end-http-instelling host veld ook is ingesteld op contoso.com, zijn klikt u vervolgens geen extra stappen vereist. U kunt instellen dat de back-end-http protocol stellen op HTTPS en beide de health-test- en de pad SSL is ingeschakeld. Als u van Azure Web Apps of andere Azure-web-services als uw back-end gebruikmaakt, dit zijn ook impliciet worden vertrouwd en er is geen verdere stappen nodig zijn voor end-to-end SSL.
- Als het certificaat zelfondertekend is of ondertekend door onbekende tussenpersonen, moeten klikt u vervolgens om in te schakelen van end-to-end SSL in v2 SKU voor een vertrouwd basiscertificaat worden gedefinieerd. Application Gateway communiceert alleen met back-ends voor het basiscertificaat waarvan servercertificaat komt overeen met een van de lijst met vertrouwde basiscertificaten in de back-end-http-instelling die is gekoppeld aan de groep.
- Naast de basis-certificaat overeenkomen valideert Application Gateway ook als de Host die is opgegeven in de back-end-http-instelling komt overeen met die van de algemene naam (CN) aangeboden door de back-endserver SSL-certificaat. Bij een poging tot stand brengen van een SSL-verbinding met de back-end, wordt de Server Name Indication (SNI)-extensie in Application Gateway ingesteld op de Host die is opgegeven in de back-end-http-instelling.
- Als **kiezen hostnaam van de back-endadres** in plaats van de Host-veld in de back-end-http-instelling is gekozen, en vervolgens de SNI-header is altijd ingesteld op de back-endpool FQDN-naam en de algemene naam van de back-endserver SSL certificaat moet overeenkomen met de FQDN-naam. Back-endpoolleden met IP-adressen worden niet ondersteund in dit scenario.
- Het basiscertificaat is een met base64 gecodeerde basiscertificaat van de back-end-servercertificaten.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over end-to-end SSL, gaat u naar [end-to-end SSL configureren met behulp van Application Gateway met PowerShell](application-gateway-end-to-end-ssl-powershell.md) te maken van een toepassingsgateway met behulp van end-to-end SSL.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
