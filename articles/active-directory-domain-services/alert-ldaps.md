---
title: 'Azure Active Directory Domain Services: Oplossen van problemen met Secure LDAP-configuratie | Microsoft Docs'
description: Secure LDAP voor probleemoplossing voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 453018f486ca3fda91d8447208fe3d936722522e
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473957"
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Azure AD Domain Services - configuratie voor het oplossen van Secure LDAP

Dit artikel bevat oplossingen voor algemene problemen bij [configureren van secure LDAP](configure-ldaps.md) voor Azure AD Domain Services.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Configuratie van Secure LDAP Network Security Group

**Waarschuwing:**

*Secure LDAP via internet is ingeschakeld voor het beheerde domein. Toegang tot poort 636 is echter niet vergrendeld met behulp van een netwerkbeveiligingsgroep. Dit kan een beveiligingsrisico voor gebruikersaccounts in het beheerde domein voor wachtwoord brute-force-aanvallen.*

### <a name="secure-ldap-port"></a>Beveiligde LDAP-poort

Wanneer secure LDAP is ingeschakeld, wordt u aangeraden het maken van aanvullende regels voor binnenkomende LDAPS toegang alleen van bepaalde IP-adressen. Uw domein beschermen deze regels tegen gewelddadige aanvallen die kunnen een beveiligingsrisico vormen. Poort 636 biedt toegang tot uw beheerde domein. Dit is het bijwerken van uw NSG voor toegang van Secure LDAP:

1. Navigeer naar de [Netwerkbeveiligingsgroepen tabblad](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) in Azure portal
2. Kies de NSG die is gekoppeld aan uw domein uit de tabel.
3. Klik op **inkomende beveiligingsregels**
4. De poort 636-regel maken
   1. Klik op **toevoegen** op de bovenste navigatiebalk.
   2. Kies **IP-adressen** voor de bron.
   3. Geef de poortbereiken van bron voor deze regel.
   4. Invoer '636' voor poortbereiken van doel.
   5. -Protocol is **TCP**.
   6. Geef de regel een passende naam, beschrijving en prioriteit. Prioriteit van deze regel moet hoger zijn dan uw "Deny alle" Regelprioriteit, hebt u een.
   7. Klik op **OK**.
5. Controleer of de regel is gemaakt.
6. Controleer de status van uw domein in twee uur om ervoor te zorgen dat u de stappen correct hebt voltooid.

> [!TIP]
> Poort 636 is niet de enige regel die nodig zijn voor Azure AD Domain Services om te functioneren. Voor meer informatie gaat u naar de [richtlijnen voor netwerken](network-considerations.md) of [oplossen NSG-configuratie](alert-nsg.md) artikelen.
>

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: Secure LDAP-certificaat verloopt

**Waarschuwing:**

*Het certificaat voor secure LDAP voor het beheerde domein verloopt op [datum]].*

**Oplossing:**

Een nieuw certificaat voor secure LDAP maken met de volgende stappen wordt beschreven in de [secure LDAP configureren](configure-ldaps.md) artikel.

## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](contact-us.md).
