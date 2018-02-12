---
title: Beveiligde Azure VPN-gateway RADIUS-verificatie met NPS-server voor meervoudige verificatie | Microsoft-Docs
description: Beschrijft het integreren van Azure-gateway RADIUS-verificatie met NPS-server voor multi-factor Authentication.
services: vpn-gateway
documentationcenter: na
author: genlin
manager: willchen
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: genli
ms.openlocfilehash: 0754c6cab9de2f93e9a57e202227a81c51bedf4c
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Integratie van Azure VPN-gateway RADIUS-verificatie met NPS-server voor multi-factor Authentication 

Het artikel wordt beschreven hoe Network Policy Server (NPS) server integreren met Azure VPN-gateway RADIUS-verificatie voor het leveren van multi-factor Authentication (MFA) voor punt-naar-site VPN-verbindingen. 

## <a name="prerequisite"></a>Vereiste

Schakel MFA in door de gebruikers moeten zich in Azure Active Directory (gesynchroniseerde van on-premises of cloud alleen) en de gebruiker moet al het bewijs van proces voltooien voor MFA.  Gebruikers kunnen het bewijs van proces uitvoeren met behulp van https://myapps.microsoft.com.

## <a name="detailed-steps"></a>Gedetailleerde stappen

### <a name="step-1-create-virtual-network-gateway"></a>Stap 1 maken virtuele netwerkgateway

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het virtuele netwerk die als host voor de virtuele netwerkgateway fungeert, **subnetten**, en selecteer vervolgens **gatewaysubnet** om een subnet te maken. 

    ![De afbeelding over het toevoegen van gatewaysubnet](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. De gateway van een virtueel netwerk maken met de volgende instellingen:

    - **Gatewaytype**: Selecteer **VPN**.
    - **VPN-type**: Selecteer **op Route gebaseerde**.
    - **SKU**: Selecteer een SKU-type op basis van uw vereisten.
    - **Virtueel netwerk**: Selecteer het virtuele netwerk dat u het gatewaysubnet gemaakt.

        ![De afbeelding over gateway-instellingen voor virtueel netwerk](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Stap 2 de NPS configureren voor Azure MFA

1. Op de NPS-server [installeren van de NPS-extensie voor Azure MFA](../multi-factor-authentication/multi-factor-authentication-nps-extension.md#install-the-nps-extension).
2. Open de console van NSP, met de rechtermuisknop op **RADUIS Clients**, selecteer **nieuw**. Een client RADUIS maken met de volgende instellingen:

    - **Beschrijvende naam**: Typ een naam.
    - **Adres (IP of DNS)**: Typ het gatewaysubnet dat u in de stap 1 hebt gemaakt.
    - **Gedeeld geheim**: Typ een geheim en onthoudt. We gebruiken deze later.

    ![De afbeelding over RADUIS clientinstellingen](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  In de **Geavanceerd** tabblad, stelt u de naam van de leverancier op **RADIUS-standaard** en zorg ervoor dat de **extra opties** niet zijn geselecteerd.

    ![De afbeelding over geavanceerde clientinstellingen RADUIS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Ga naar **beleid** > **netwerkbeleid**, dubbelklikt u op **verbindingen met Microsoft Routing and Remote Access server** beleid, selecteer  **Toegang verlenen**, en klik vervolgens op **OK**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Stap 3-Configureer de virtuele netwerkgateway

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open de virtuele netwerkgateway die u hebt gemaakt, zorg ervoor dat het Gatewaytype **VPN** en het VPN-type is **op route gebaseerde**.
3. Klik op **wijs siteconfiguratie** > **nu configureren**, en voeg vervolgens de volgende instellingen:

    - **-Adresgroep**: Typ het gatewaysubnet dat u in de stap 1 hebt gemaakt.
    - **Verificatietype**: Selecteer **RADIUS-verificatie**.
    - **Het IP-adres**: het IP-adres van de NPS-server.

    ![De afbeelding over dat site-instellingen](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
- [Uw bestaande NPS-infrastructuur integreren met Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)
