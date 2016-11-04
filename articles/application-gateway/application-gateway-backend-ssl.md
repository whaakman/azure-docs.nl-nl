---
title: SSL-beleid en end-to-end SSL inschakelen in Application Gateway | Microsoft Docs
description: Op deze pagina wordt de ondersteuning voor end-to-end SSL in Application Gateway beschreven.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: amsriva

---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>SSL-beleid en end-to-end SSL inschakelen in Application Gateway
## <a name="overview"></a>Overzicht
Application Gateway ondersteunt SSL-beëindiging op de gateway, waarna het verkeer normaal gesproken onversleuteld naar de back-endservers wordt doorgeleid. Dit ontlast de webservers van de kostbare overhead voor de versleuteling/ontsleuteling. Voor sommige gebruikers is onversleutelde communicatie met de back-endserververs echter onacceptabel. Dit kan zijn vanwege de beveiligings- of nalevingsvereisten, of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor zulke toepassingen ondersteunt Application Gateway nu end-to-end SSL-versleuteling.

Met end-to-end SSL kunt u veilig gevoelige gegevens versleuteld verzenden naar de back-endserver, terwijl u gebruikmaakt van de voordelen van de Layer 7-taakverdelingsfuncties die Application Gateway te bieden heeft, zoals affiniteit op basis van cookies, routering op basis van een URL, ondersteuning voor routering op basis van sites of de mogelijkheid om X-Forwarded-*-headers in te voegen.

Wanneer Application Gateway is geconfigureerd met de communicatiemodus voor end-to-end SSL, worden door Application Gateway de SSL-sessies van de gebruiker op de gateway beëindigd en het gebruikersverkeer ontsleuteld. Vervolgens worden de geconfigureerde regels toegepast voor het selecteren van het juiste exemplaar van de back-endgroep waarnaar het verkeer moet worden doorgeleid. Application Gateway initieert vervolgens een nieuwe SSL-verbinding met de back-endserver en versleutelt de gegevens opnieuw met het openbare-sleutelcertificaat van de back-endserver voordat de aanvraag naar de back-endserver wordt verstuurd. End-to-end SSL wordt ingeschakeld door de protocolinstelling in BackendHTTPSetting in te stellen op Https. Dit wordt vervolgens toegepast op een back-endgroep. Elke back-endserver in de back-endgroep waarvoor end-to-end SSL is ingeschakeld, moet worden geconfigureerd met een certificaat zodat beveiligde communicatie mogelijk is.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

In dit voorbeeld kunnen aanvragen voor https://contoso.com via HTTP worden doorgestuurd naar ContosoServerPool, en wordt https://fabrikam.com via HTTPS doorgestuurd naar FabrikamServerPool met behulp van end-to-end SSL.

## <a name="end-to-end-ssl-and-white-listing-of-certificates"></a>End-to-end SSL en het opnemen van certificaten in een lijst van toegestane certificaten
Application Gateway communiceert alleen met bekende back-endexemplaren waarvan het certificaat is opgenomen in een lijst van toegestane certificaten voor de toepassingsgateway. Als u het opnemen van certificaten in een lijst van toegestane certificaten wilt inschakelen, moet u de openbare sleutel van de back-endservercertificaten uploaden naar de toepassingsgateway. Alleen verbindingen met bekende back-endservers die zijn opgenomen in een lijst van toegestane back-endservers worden dan toegestaan. Voor andere verbindingen wordt een gatewayfout weergegeven. Zelfondertekende certificaten zijn uitsluitend bedoeld voor testdoeleinden en het wordt afgeraden om deze in een productieomgeving te gebruiken. Dergelijke certificaten moeten ook zijn opgenomen in een lijst van toegestane certificaten voor de toepassingsgateway zoals hierboven wordt beschreven. Wanneer dit het geval is, kunnen ze worden gebruikt.

## <a name="application-gateway-ssl-policy"></a>SSL-beleid in Application Gateway
Application Gateway ondersteunt ook SSL-onderhandelingsbeleidsregels die door de gebruiker kunnen worden geconfigureerd. Op die manier kunnen klanten het beheer van SSL-verbindingen op de toepassingsgateway beter afstemmen.

1. SSL 2.0 en 3.0 worden geforceerd uitgeschakeld voor alle toepassingsgateways. Ze kunnen op geen enkele wijze worden geconfigureerd.
2. Bij het definiëren van het SSL-beleid kunt u een van de volgende drie protocollen uitschakelen: TLSv1_0, TLSv1_1, TLSv1_2.
3. Als er geen SSL-beleid is gedefinieerd, worden alle drie protocollen (TLSv1_0, TLSv1_1, TLSv1_2) ingeschakeld.

## <a name="next-steps"></a>Volgende stappen
Nadat u meer te weten bent gekomen over end-to-end SSL en SSL-beleid, gaat u naar [Enable end to end SSL on application gateway](application-gateway-end-to-end-ssl-powershell.md) (End-to-end SSL inschakelen in Application Gateway) om een toepassingsgateway te maken waarbij het verkeer versleuteld naar de back-endserver kan worden verzonden.

<!--HONumber=Oct16_HO3-->


