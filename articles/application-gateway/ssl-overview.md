---
title: End-to-end SSL on Azure Application Gateway inschakelen
description: In dit artikel wordt een overzicht van de Application Gateway end-to-end SSL ondersteunen.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 8/6/2018
ms.author: amsriva
ms.openlocfilehash: 4575bed18697a5661d58dc350c24a9497f7c46ff
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578810"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Overzicht van end-to-end SSL met Application Gateway

Application Gateway ondersteunt SSL-beëindiging op de gateway, na welk verkeer normaal gesproken onversleuteld naar de back-endservers. Met deze functie voorkomt u prijzige overhead voor het versleutelen en ontsleutelen voor uw webservers. Voor sommige gebruikers is onversleutelde communicatie met de back-endserververs echter onacceptabel. Deze onversleutelde communicatie kan het gevolg zijn van de beveiligings- of nalevingsvereisten of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor zulke toepassingen ondersteunt Application Gateway end-to-end SSL-versleuteling.

Met end-to-end SSL kunt u veilig gevoelige gegevens versleuteld verzenden naar de back-endserver, terwijl u gebruik blijft maken van de voordelen van de Layer 7-taakverdelingsfuncties die Application Gateway te bieden heeft. Voorbeelden van deze functies zijn: sessieaffiniteit op basis van cookies, routering op basis van een URL, ondersteuning voor routering op basis van sites en de mogelijkheid om X-Forwarded-\*-headers in te voegen.

Wanneer Application Gateway is geconfigureerd met de communicatiemodus voor end-to-end SSL, worden door Application Gateway de SSL-sessies op de gateway beëindigd en wordt het gebruikersverkeer ontsleuteld. Vervolgens worden de geconfigureerde regels toegepast voor het selecteren van het juiste exemplaar van de back-endgroep waarnaar het verkeer moet worden doorgeleid. Application Gateway initieert vervolgens een nieuwe SSL-verbinding met de back-endserver en versleutelt de gegevens opnieuw met het openbare-sleutelcertificaat van de back-endserver, voordat de aanvraag naar de back-endserver wordt verstuurd. End-to-end SSL wordt ingeschakeld door de protocolinstelling **BackendHTTPSetting** naar HTTPS, die wordt vervolgens toegepast op een back-endpool. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

![end-to-end SSL-scenario][1]

In dit voorbeeld worden-aanvragen via TLS1.2 doorgestuurd naar de back-endservers in Pool1 met behulp van end-to-end SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-end SSL en het opnemen van certificaten in een lijst van toegestane certificaten

Application Gateway communiceert alleen met bekende back-endexemplaren waarvan het certificaat is opgenomen in een lijst van toegestane certificaten voor de Application Gateway. Als u het opnemen van certificaten in een lijst van toegestane certificaten wilt inschakelen, moet u de openbare sleutel van de back-endservercertificaten uploaden naar de Application Gateway (niet het basiscertificaat). Alleen verbindingen met bekende en goedgekeurde back-ends zijn vervolgens toegestaan. De resterende back-ends hebben een gatewayfout als resultaat. Zelfondertekende certificaten zijn uitsluitend bedoeld voor testdoeleinden en het wordt afgeraden om deze in een productieomgeving te gebruiken. Dergelijke certificaten moeten zijn opgenomen in de whitelist met de application gateway zoals beschreven in de voorgaande stappen voordat ze kunnen worden gebruikt.

> [!NOTE]
> Certificaatinstallatie verificatie is niet vereist voor betrouwbare Azure-services zoals Azure Web Apps.

## <a name="next-steps"></a>Volgende stappen

Wanneer u meer te weten bent gekomen over end-to-end SSL, gaat u naar [Een toepassingsgateway configureren met behulp van Azure Portal](create-ssl-portal.md) om een toepassingsgateway te maken met end-to-end SSL.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
