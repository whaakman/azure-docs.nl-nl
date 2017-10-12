---
title: End-to-end SSL inschakelen in Azure Application Gateway | Microsoft Docs
description: Op deze pagina wordt de ondersteuning voor end-to-end SSL in Application Gateway beschreven.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: 689ee54dc1db2ea371b08270718278fd98c65bb5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Overzicht van end-to-end SSL met Application Gateway

Application Gateway ondersteunt SSL-beëindiging op de gateway, waarna het verkeer normaal gesproken onversleuteld naar de back-endservers wordt doorgeleid. Met deze functie voorkomt u prijzige overhead voor het versleutelen en ontsleutelen voor uw webservers. Voor sommige gebruikers is onversleutelde communicatie met de back-endserververs echter onacceptabel. Deze onversleutelde communicatie kan het gevolg zijn van de beveiligings- of nalevingsvereisten of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor zulke toepassingen ondersteunt Application Gateway end-to-end SSL-versleuteling.

## <a name="overview"></a>Overzicht

Met end-to-end SSL kunt u veilig gevoelige gegevens versleuteld verzenden naar de back-endserver, terwijl u gebruik blijft maken van de voordelen van de Layer 7-taakverdelingsfuncties die Application Gateway te bieden heeft. Voorbeelden van deze functies zijn: sessieaffiniteit op basis van cookies, routering op basis van een URL, ondersteuning voor routering op basis van sites en de mogelijkheid om X-Forwarded-*-headers in te voegen.

Wanneer Application Gateway is geconfigureerd met de communicatiemodus voor end-to-end SSL, worden door Application Gateway de SSL-sessies op de gateway beëindigd en wordt het gebruikersverkeer ontsleuteld. Vervolgens worden de geconfigureerde regels toegepast voor het selecteren van het juiste exemplaar van de back-endgroep waarnaar het verkeer moet worden doorgeleid. Application Gateway initieert vervolgens een nieuwe SSL-verbinding met de back-endserver en versleutelt de gegevens opnieuw met het openbare-sleutelcertificaat van de back-endserver, voordat de aanvraag naar de back-endserver wordt verstuurd. End-to-end SSL wordt ingeschakeld door de protocolinstelling in BackendHTTPSetting in te stellen op HTTPS. Dit wordt vervolgens toegepast op een back-endgroep. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

![end-to-end SSL-scenario][1]

In dit voorbeeld worden-aanvragen via TLS1.2 doorgestuurd naar de back-endservers in Pool1 met behulp van end-to-end SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>End-to-end SSL en het opnemen van certificaten in een lijst van toegestane certificaten

Application Gateway communiceert alleen met bekende back-endexemplaren waarvan het certificaat is opgenomen in een lijst van toegestane certificaten voor de Application Gateway. Als u het opnemen van certificaten in een lijst van toegestane certificaten wilt inschakelen, moet u de openbare sleutel van de back-endservercertificaten uploaden naar de Application Gateway (niet het basiscertificaat). Alleen verbindingen met bekende en goedgekeurde back-ends zijn vervolgens toegestaan. De resterende back-ends hebben een gatewayfout als resultaat. Zelfondertekende certificaten zijn uitsluitend bedoeld voor testdoeleinden en het wordt afgeraden om deze in een productieomgeving te gebruiken. Dergelijke certificaten moeten ook worden opgenomen in een lijst van toegestane certificaten voor de toepassingsgateway, zoals in de bovenstaande stappen wordt beschreven. Wanneer dit het geval is, kunnen ze worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

Wanneer u meer te weten bent gekomen over end-to-end SSL, gaat u naar [Enable end to end SSL on application gateway](application-gateway-end-to-end-ssl-powershell.md) (End-to-end SSL inschakelen in Application Gateway) om een toepassingsgateway te maken met end-to-end SSL.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
