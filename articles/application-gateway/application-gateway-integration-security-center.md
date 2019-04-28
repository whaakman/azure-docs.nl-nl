---
title: Application Gateway-integratie met Azure Security Center | Microsoft Docs
description: Deze pagina bevat informatie over hoe Application Gateway is geïntegreerd in Azure Security Center.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: victorh
ms.openlocfilehash: 10f115b64f0bd3f7e557da2bedbf3327d0ef483d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122290"
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Overzicht van de integratie van Application Gateway en Azure Security Center

Meer informatie over hoe Application Gateway en Security Center helpen beschermen van uw web application-resources. Web application firewall (WAF) voor Application gateway kan worden geïntegreerd met [Security Center](../security-center/security-center-intro.md) voor een naadloze weergave om te voorkomen, detecteren en reageren op bedreigingen voor niet-beveiligde webtoepassingen in uw omgeving.

## <a name="overview"></a>Overzicht

Application Gateway WAF is een aanbeveling in Security Center voor het beveiligen van webtoepassingen tegen aanvallen en beveiligingsproblemen. Ingeschakeld-webresources die niet zijn beveiligd met WAF weergeven in het security center als aanbevelingen met hoge urgentie. Aanbevelingen voor web application firewalls worden weergegeven op de **overzicht** pagina onder **toepassingen**.

![integratie met security center][1]

Geen aanbevelingen met betrekking tot de web application firewall opent een nieuwe pagina worden de details van de aanbeveling klikken.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Een web application firewall toevoegen aan een bestaande resource

Navigeer naar **alle services** > **beveiliging en identiteit** > **Security Center** en klik op **Security Center - overzicht**, klikt u op **toepassingen**. Op **Security Center - toepassingen**, de tabel bevat een lijst met toepassingen die Security Center gedetecteerd in uw abonnement.

![webtoepassingen][3]

Door te klikken op een webtoepassing met een belangrijk probleem, u krijgt de **beveiligingsstatus van de toepassing** pagina. In de onderstaande afbeelding, de web-App die niet wordt beveiligd door een firewall voor webtoepassingen. 

![webresources niet beveiligd][2]

Klik op **toevoegen van een web application firewall** onder **aanbevelingen** openen de **een Web Application Firewall toevoegen** pagina.

Als u niet een bestaande toepassingsgateway hebt, of wilt maken van een nieuwe, klikt u op **nieuw** en klik op **maken van een nieuwe Web Application Firewall**, en klikt u op **Microsoft - Application-Gateway** . Hiermee gaat u door de stappen om een toepassingsgateway te maken. Uw web-App wordt op dit moment worden toegevoegd als een beveiligde bron, nu Security Center worden bijgehouden dat deze bron wordt beveiligd door een firewall voor webtoepassingen. Dit komt deze niet toevoegen als lid van een back-end-pool.

Als u een bestaande toepassingsgateway hebt, kunt u deze onder **bestaande oplossing gebruiken**

![pagina voor de Web application firewall toevoegen][4]

Een webtoepassing naar een application gateway via Security Center toe te voegen, wordt de resource als lid van een back-end-adresgroep niet toegevoegd. Dit moet worden uitgevoerd op de toepassingsgatewayresource rechtstreeks.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Een resource toevoegen aan een bestaande web application firewall

Navigeer naar **alle services** > **beveiliging en identiteit** > **Security Center** en klik op **Security Center - overzicht**, klikt u op **partneroplossingen**. Bestaande Security Center op de hoogte Toepassingsgateways weergeven in de **oplossingen van partners** pagina.

![Oplossingen van partners][7]

Klik op **app koppelen** openen **toepassingen koppelen**, krijgt u hier de opties voor het selecteren van bestaande toepassingen. Kies de toepassingen te beschermen en klik op **OK**. Hiermee wordt de webtoepassing naar de back-endpool van de toepassingsgateway niet toegevoegd. Hiermee wordt de resources ingesteld als een beveiligde bron zodat Security Center kunt bijhouden. Als u wilt toevoegen de resource als lid van een back-end-pool, dit moet worden uitgevoerd op de toepassingsgateway van de huidige pagina, klikt u op **oplossingenconsole** om te worden genomen om de toepassingsgatewayresource waar u de web-App aan kunt toevoegen de back-endpool.

![oplossingen partnertoepassingen][6]

## <a name="finalize-configuration"></a>Configuratie voltooien

Security Center houdt bij of toepassingen toegevoegd aan een application gateway als een beveiligde bron.  Het controleert de status van deze resource en zorgt ervoor dat deze wordt beveiligd door een application gateway. De volgende stap is het toevoegen van de privé IP-adres, het openbare IP-adres of de NIC van de virtuele machine naar de back-endadresgroep van de toepassingsgateway. Totdat dit is een extra aanbeveling gedaan **Toepassingsbeveiliging** wordt weergegeven totdat de resource is toegevoegd.

![pagina voor de Web application firewall toevoegen][5]

## <a name="security-alerts"></a>Beveiligingswaarschuwingen

In Security Center navigeren naar **detectie** > **beveiligingswaarschuwingen**.  Hier vindt u WAF waarschuwingen voor uw application gateways. Waarschuwingen worden opgedeeld per WAF-regel.

![Beveiligingswaarschuwingen][8]

Te klikken op een regel om een lijst met waarschuwingen voor die specifieke regel van WAF te bieden. Elke waarschuwing bevat aanvullende informatie op het zoeken. De details bevatten een koppeling naar de application gateway.
 
![Waarschuwingsdetails][9]

## <a name="next-steps"></a>Volgende stappen

Voor informatie over het inschakelen van de web application firewall in een bestaande toepassingsgateway, gaat u naar [maken of bijwerken van een Azure-toepassingsgateway met web application firewall](application-gateway-web-application-firewall-portal.md).

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png