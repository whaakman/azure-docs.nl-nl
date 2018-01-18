---
title: 'Azure Active Directory Domain Services: Probleemoplossing beveiligde LDAP-configuratie | Microsoft Docs'
description: Beveiligde LDAP voor probleemoplossing voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: ad98f3fb1ddb753976be627764d34864e5bf3d50
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Azure AD Domain Services - probleemoplossing Secure LDAP-configuratie

Dit artikel vindt u oplossingen voor algemene problemen wanneer [configureren van beveiligde LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md) voor Azure AD Domain Services.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Configuratie van de Netwerkbeveiligingsgroep voor beveiligde LDAP

**Waarschuwing:**

*Beveiligde LDAP via internet is ingeschakeld voor het beheerde domein. Echter, toegang tot poort 636 is niet vergrendeld met behulp van een netwerkbeveiligingsgroep. Dit kan een beveiligingsrisico voor gebruikersaccounts op het beheerde domein aanvallen met brute kracht wachtwoord.*

### <a name="secure-ldap-port"></a>Beveiligde LDAP-poort

Als beveiligde LDAP is ingeschakeld, wordt u aangeraden extra regels voor binnenkomende LDAPS toegang alleen van bepaalde IP-adressen maken. Uw domein beschermen deze regels tegen gewelddadige aanvallen die een beveiligingsrisico inhouden. Poort 636 staat toegang tot uw beheerde domein. Dit is het bijwerken van uw NSG om toegang te verlenen voor beveiligde LDAP:

1. Navigeer naar de [Netwerkbeveiligingsgroepen tabblad](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) in de Azure portal
2. Kies het NSG die is gekoppeld aan uw domein uit de tabel.
3. Klik op **beveiligingsregels voor binnenkomende verbindingen**
4. De poort 636 regel maken
   1. Klik op **toevoegen** op de bovenste navigatiebalk.
   2. Kies **IP-adressen** voor de bron.
   3. Geef de bron-poortbereiken voor deze regel.
   4. Invoer '636' voor poortbereiken bestemming.
   5. -Protocol is **TCP**.
   6. Geef de regel een geschikte naam, beschrijving en prioriteit. Prioriteit van deze regel moet hoger zijn dan de prioriteit van de regel "Alle Deny", als er een.
   7. Klik op **OK**.
5. Controleer of de regel is gemaakt.
6. Controleer de status van uw domein in twee uur om ervoor te zorgen dat u de stappen correct hebt voltooid.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
