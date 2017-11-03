---
title: Gateway-integratie van toepassingen met Azure Security Center | Microsoft Docs
description: "Deze pagina bevat informatie over hoe Application Gateway is geïntegreerd in Azure Security Center."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: davidmu
ms.openlocfilehash: abed4095bcacac4ca3995acfaafae2f16d5f1532
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Overzicht van de integratie tussen Application Gateway en Azure Security Center

Meer informatie over hoe Application Gateway en Security Center beter beveiligen van uw webtoepassingsbronnen. Application gateway web application firewall (WAF) kan worden geïntegreerd met [Security Center](../security-center/security-center-intro.md) voor een naadloze weergeven om te voorkomen, detecteren en reageren op bedreigingen voor de niet-beveiligde webtoepassingen in uw omgeving.

## <a name="overview"></a>Overzicht

Application Gateway WAF wordt aanbevolen in Security Center voor het beveiligen van webtoepassingen van aanvallen en beveiligingsproblemen. Ingeschakeld webbronnen die niet zijn beveiligd door WAF weergegeven in het Beveiligingscentrum als hoog dreigingsniveau aanbevelingen. Aanbevelingen voor web application firewalls worden weergegeven op de **overzicht** pagina onder **toepassingen**.

![integratie met security center][1]

Als u geen aanbevelingen over web application firewall Hiermee opent u een nieuwe blade worden de details van de aanbeveling op.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Web application firewall toevoegen aan een bestaande resource

Navigeer naar **meer Services** > **beveiliging en identiteit** > **Security Center** en klik op de **Security Center - overzicht** blade, klikt u op **toepassingen**. Op de **Security Center - toepassingen** blade in de tabel bevat een lijst met toepassingen die Security Center gedetecteerd in uw abonnement.

![webtoepassingen][3]

Door te klikken op een webtoepassing met een kritiek probleem, u krijgt de **beveiligingsstatus van de toepassing** blade. In de onderstaande afbeelding, de webtoepassing die niet wordt beveiligd door een web application firewall. 

![webbronnen niet beveiligd][2]

Klik op **web application firewall toevoegen** onder **aanbevelingen** openen de **Web Application Firewall toevoegen** blade.

Als u geen hebben van een bestaande toepassingsgateway of wilt maken van een nieuwe, klikt u op **nieuw** en klik op de **maken van een nieuwe Web Application Firewall** blade en klik op **Microsoft - Application Gateway**. Hiermee gaat u door de stappen om een toepassingsgateway te maken. Uw webtoepassing is op dit punt wordt toegevoegd als een beveiligde bron, nu Security Center, houdt dat deze bron wordt beveiligd door een web application firewall. Hiermee wordt het niet als een lid van de groep back-end toegevoegd.

Als u een bestaande toepassingsgateway hebt, kunt u deze onder **bestaande oplossing gebruiken**

![blade Web application firewall toevoegen][4]

Toevoegen van dat een webtoepassing om een toepassingsgateway door Security Center te heeft de resource niet toevoegen als een lid van de groep back-end, moet u dit doen op de toepassingsgatewayresource rechtstreeks.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Een resource toevoegen aan een bestaande web application firewall

Navigeer naar **meer Services** > **beveiliging en identiteit** > **Security Center** en klik op de **Security Center - overzicht** blade, klikt u op **partneroplossingen**. Bestaande Security Center op de hoogte Toepassingsgateways weergeven in de **partneroplossingen** blade.

![partneroplossingen][7]

Klik op **app koppelen** openen de **toepassingen koppelen** blade Hier krijgt u de opties voor het selecteren van bestaande toepassingen. Kies de toepassingen te beschermen en klik op **OK**. Hiermee wordt de webtoepassing naar de back-endpool van de toepassingsgateway niet toegevoegd. Hiermee stelt u de resources als een beveiligde bron zodat Security Center kunnen worden bijgehouden. Als u wilt de bron toevoegen als een lid van de groep back-end, moet u dit doen in de toepassingsgateway, uit de huidige blade kunt u **oplossingenconsole** om te worden uitgevoerd om de toepassingsgatewayresource waarin u de webtoepassing aan de endadresgroep back-kunt toevoegen.

![toepassingen van partners oplossingen][6]

## <a name="finalize-configuration"></a>Configuratie voltooien

Security Center houdt toepassingen toegevoegd aan een application gateway als een beveiligde bron.  Het controleert de status van deze resource en zorgt ervoor dat deze wordt beveiligd door een toepassingsgateway. De volgende stap is het toevoegen van de privé-IP, openbare IP-adres of NIC van uw virtuele machine naar de back-endpool van de toepassingsgateway. Totdat een extra aanbeveling van dit is **Toepassingsbeveiliging voltooien** wordt weergegeven totdat de bron is toegevoegd.

![blade Web application firewall toevoegen][5]

## <a name="security-alerts"></a>Beveiligingswaarschuwingen

In Security Center navigeren naar **detectie** > **beveiligingswaarschuwingen**.  Hier vindt u WAF waarschuwingen voor uw Toepassingsgateways. Waarschuwingen worden opgesplitst op WAF regel.

![beveiligingswaarschuwingen][8]

Te klikken op een regel om een lijst met waarschuwingen voor die specifieke WAF regel te bieden. Elke waarschuwing worden aanvullende details over het zoeken. De gegevens bevatten een koppeling naar de toepassingsgateway.
 
![Waarschuwingsdetails][9]

## <a name="next-steps"></a>Volgende stappen

Als u wilt weten hoe web application firewall inschakelen in een bestaande toepassingsgateway, gaat u naar [maken of bijwerken van een Azure-toepassingsgateway met web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png