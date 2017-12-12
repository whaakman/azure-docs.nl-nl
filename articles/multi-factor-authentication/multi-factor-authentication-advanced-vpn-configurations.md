---
title: Geavanceerde scenario's met Azure MFA en VPN's van derden
description: Stapsgewijze configuratie handleidingen voor Azure MFA integreren met Cisco, Citrix en Juniper.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: 62caebb6dec5b3603bf7618fdaf183e9a98d992e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Geavanceerde scenario's met Azure multi-factor Authentication en VPN-oplossingen van derden
Azure multi-factor Authentication kan worden gebruikt voor het naadloos verbinding maken met verschillende VPN-oplossingen van derden. Dit artikel is gericht op Cisco® ASA-VPN-apparaat, Citrix NetScaler SSL VPN-apparaat en de Juniper netwerken Secure Access/Pulse Secure verbinding Secure SSL VPN-apparaat. Configuratie-handleidingen om deze drie algemene apparaten op te lossen is gemaakt. Multi-factor Authentication-Server kan ook worden geïntegreerd met de meest andere systemen die gebruikmaken van RADIUS-, LDAP, IIS of verificatie op basis van claims in AD FS. U vindt meer informatie in [MFA-Server-configuraties](multi-factor-authentication-get-started-server.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN-apparaat en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Cisco® ASA VPN-apparaat voor aanvullende beveiliging voor Cisco AnyConnect® VPN-aanmeldingen en toegang tot portal.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende opties voor het downloaden van de handleidingen gedetailleerde stapsgewijze configuratie.

| Handleiding voor de configuratie | Beschrijving |
| --- | --- |
| [Cisco ASA met Anyconnect VPN- en Azure MFA-configuratie voor LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Uw Cisco ASA-VPN-apparaat integreren met Azure MFA met LDAP |
| [Cisco ASA met Anyconnect VPN- en Azure MFA-configuratie voor RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Uw Cisco ASA-VPN-apparaat integreren met Azure MFA met behulp van RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN- en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Citrix NetScaler SSL VPN-apparaat voor aanvullende beveiliging voor Citrix NetScaler SSL VPN-aanmeldingen en toegang tot portal.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende opties voor het downloaden van de handleidingen gedetailleerde stapsgewijze configuratie.

| Handleiding voor de configuratie | Beschrijving |
| --- | --- |
| [Citrix NetScaler SSL VPN- en Azure MFA-configuratie voor LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Uw Citrix NetScaler SSL VPN integreren met Azure MFA-apparaat met behulp van LDAP |
| [Citrix NetScaler SSL VPN- en Azure MFA-configuratie voor RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Uw toestel Citrix NetScaler SSL VPN-integreren met Azure MFA met behulp van RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN-apparaat en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Juniper/Pulse Secure SSL VPN-apparaat voor aanvullende beveiliging voor Juniper/Pulse Secure SSL VPN-aanmeldingen en toegang tot portal.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende opties voor het downloaden van de handleidingen gedetailleerde stapsgewijze configuratie.

| Handleiding voor de configuratie | Beschrijving |
| --- | --- |
| [Juniper Pulse/veilige SSL VPN- en Azure MFA-configuratie voor LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Uw Juniper/Pulse Secure SSL VPN integreren met Azure MFA-apparaat met behulp van LDAP |
| [Juniper Pulse/veilige SSL VPN- en Azure MFA-configuratie voor RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Uw toestel Juniper/Pulse Secure SSL VPN-integreren met Azure MFA met behulp van RADIUS |

## <a name="next-steps"></a>Volgende stappen

- [Uitbreiden van uw bestaande infrastructuur voor verificatie met de NPS-extensie voor Azure multi-factor Authentication](multi-factor-authentication-nps-extension.md)

- [Instellingen configureren voor Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)