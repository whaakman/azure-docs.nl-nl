---
title: Beveiligde Azure VPN-gateway RADIUS-verificatie met NPS-server voor multi-factor Authentication | Microsoft-Docs
description: Beschrijving van Azure-gateway RADIUS-verificatie integreren met NPS-server voor multi-factor Authentication.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: willchen
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 8e10151cd117a3400893f94559b2c9892de9f3c7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67666219"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Azure VPN-gateway RADIUS-verificatie integreren met NPS-server voor multi-factor Authentication 

Het artikel wordt beschreven hoe u Network Policy Server (NPS) integreren met Azure VPN-gateway RADIUS-verificatie om te leveren van multi-factor Authentication (MFA) voor punt-naar-site VPN-verbindingen. 

## <a name="prerequisite"></a>Vereiste

U MFA inschakelt, moet de gebruikers in Azure Active Directory (Azure AD), die moeten worden gesynchroniseerd vanuit de on-premises of in de cloud van de omgeving. Ook moet de gebruiker al hebt voltooid het proces van automatische inschrijving voor MFA.  Zie voor meer informatie, [Mijn account voor verificatie in twee stappen instellen](../active-directory/user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Gedetailleerde stappen

### <a name="step-1-create-a-virtual-network-gateway"></a>Stap 1: De gateway van een virtueel netwerk maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het virtuele netwerk die als voor de virtuele netwerkgateway host, **subnetten**, en selecteer vervolgens **gatewaysubnet** om een subnet te maken. 

    ![De afbeelding over het gatewaysubnet toevoegen](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Een virtuele netwerkgateway maken door de volgende instellingen op te geven:

    - **Gatewaytype**: Selecteer **VPN**.
    - **VPN-type**: Selecteer **Op route gebaseerd**.
    - **SKU**: Selecteer een SKU-type op basis van uw vereisten.
    - **Virtueel netwerk**: Selecteer het virtuele netwerk waarin u het gatewaysubnet gemaakt.

        ![De afbeelding over instellingen voor virtuele netwerkgateway](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Stap 2 de NPS voor Azure MFA configureren

1. Op de NPS-server [installeren van de NPS-extensie voor Azure MFA](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension).
2. Open de NPS-console, met de rechtermuisknop op **RADIUS-Clients**, en selecteer vervolgens **nieuw**. De RADIUS-client maken door de volgende instellingen op te geven:

    - **Beschrijvende naam**: Typ een naam.
    - **Adres (IP- of DNS)** : Typ het gatewaysubnet dat u in stap 1 hebt gemaakt.
    - **Gedeeld geheim**: Typ een geheime sleutel en onthouden voor later gebruik.

      ![De afbeelding over instellingen voor RADIUS-client](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  Op de **Geavanceerd** tabblad, stelt u de naam van de leverancier op **RADIUS-standaard** en zorg ervoor dat de **extra opties** selectievakje niet is ingeschakeld.

    ![De afbeelding over geavanceerde instellingen voor RADIUS-client](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Ga naar **beleid** > **netwerkbeleid**, dubbelklikt u op **verbindingen met Microsoft Routing and Remote Access server** beleid, selecteer  **Toegang verlenen**, en klik vervolgens op **OK**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Stap 3-Configureer de virtuele netwerkgateway

1. Meld u aan bij [Azure-portal](https://portal.azure.com).
2. Open de virtuele netwerkgateway die u hebt gemaakt. Zorg ervoor dat de gateway van het type is ingesteld op **VPN** en of het VPN-type **op route gebaseerde**.
3. Klik op **verwijzen naar de configuratie van site** > **nu configureren**, en geef vervolgens de volgende instellingen:

    - **Adresgroep**: Typ het gatewaysubnet dat u in de stap 1 hebt gemaakt.
    - **Verificatietype**: Selecteer **RADIUS-verificatie**.
    - **IP-adres van**: Typ het IP-adres van de NPS-server.

      ![Afbeelding van site-instellingen, wijs de](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)
- [Uw bestaande NPS-infrastructuur integreren met Azure Multi-Factor Authentication](../active-directory/authentication/howto-mfa-nps-extension.md)
