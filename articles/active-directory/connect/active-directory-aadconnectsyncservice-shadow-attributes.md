---
title: Azure AD Connect sync-service de kenmerken van | Microsoft Docs
description: Hierin wordt beschreven hoe de kenmerken van werken in Azure AD Connect sync-service.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 0b6a7f22d744480a40a878c979986cdd7667109c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect sync-service de kenmerken
De meeste kenmerken worden dezelfde manier weergegeven in Azure AD omdat ze in uw lokale Active Directory. Maar bepaalde kenmerken sommige speciale verwerking hebben en de waarde van het kenmerk in Azure AD is mogelijk anders dan wat Azure AD Connect synchroniseert.

## <a name="introducing-shadow-attributes"></a>Introductie van de kenmerken
Enkele kenmerken hebben twee manieren in Azure AD. Zowel de lokale waarde en een berekende waarde worden opgeslagen. Deze extra kenmerken worden de kenmerken van genoemd. De twee meest voorkomende kenmerken waarin u zien hoe dit gedrag zijn **userPrincipalName** en **proxyAddress**. De wijziging in de kenmerkwaarden gebeurt wanneer er waarden zijn in deze kenmerken die niet-geverifieerd domeinen. Maar de synchronisatie-engine in Connect leest de waarde in het kenmerk shadow dus vanuit het perspectief, het kenmerk is bevestigd door Azure AD.

U kunt de kenmerken van de schaduw met behulp van de Azure portal of met PowerShell niet zien. Maar het concept kennis helpt u bij het oplossen van bepaalde scenario's waarin het kenmerk verschillende waarden voor de on-premises een en in de cloud.

Bekijk voor meer informatie over het gedrag, in dit voorbeeld van Fabrikam:  
![Domeinen](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
Ze hebben meerdere UPN-achtervoegsels in hun lokale Active Directory, maar ze deze alleen hebt gecontroleerd.

### <a name="userprincipalname"></a>UserPrincipalName
Een gebruiker heeft de volgende kenmerkwaarden in een niet-geverifieerd domein:

| Kenmerk | Waarde |
| --- | --- |
| lokale userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD-shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD-userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

Het kenmerk userPrincipalName is de waarde die u ziet wanneer met behulp van PowerShell.

Omdat de waarde van het kenmerk echte lokaal wordt opgeslagen in Azure AD, wanneer u het fabrikam.com-domein verifiëren, Azure AD-het kenmerk userPrincipalName updates met de waarde van de shadowUserPrincipalName. U hebt geen Synchroniseer eventuele wijzigingen van Azure AD Connect voor deze waarden moeten worden bijgewerkt.

### <a name="proxyaddresses"></a>proxyAddresses
Hetzelfde proces voor het opnemen van alleen geverifieerde domeinen vindt ook plaats voor proxyAddresses, maar met een aantal extra logica. De controle op geverifieerde domeinen alleen voor gebruikers postvak wordt uitgevoerd. Een e-mailfunctionaliteit gebruiker of neem contact op met vertegenwoordigen een gebruiker in een andere Exchange-organisatie en u kunt alle waarden in proxyAddresses toevoegen aan deze objecten.

Voor een postvakgebruiker, on-premises of in Exchange Online, wordt alleen de waarden voor geverifieerde domeinen weergegeven. Het kan er als volgt uit:

| Kenmerk | Waarde |
| --- | --- |
| lokale proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

In dit geval  **smtp:abbie.spencer@fabrikam.com**  is verwijderd, omdat dat domein is niet geverifieerd. Maar ook toegevoegd Exchange  **SIP:abbie.spencer@fabrikamonline.com** . Fabrikam nog niet is gebruikt Lync/Skype lokale, maar Azure AD en Exchange Online worden voorbereid voor.

Deze logica voor proxyAddresses wordt aangeduid als **ProxyCalc**. ProxyCalc wordt aangeroepen met elke wijziging van een gebruiker wanneer:

- De gebruiker is een service-abonnement dat Exchange Online omvat, zelfs als de gebruiker is geen licentie voor Exchange toegewezen. Bijvoorbeeld als de gebruiker de Office E3-SKU is toegewezen, maar is alleen SharePoint Online toegewezen. Dit geldt zelfs als uw postvak nog steeds lokaal is.
- Het kenmerk msExchRecipientTypeDetails heeft een waarde.
- U aanbrengt een wijziging in proxyAddresses of userPrincipalName.

ProxyCalc kan even duren voor het verwerken van een gebruiker wordt gewijzigd en is niet synchroon met de Azure AD Connect-exportproces.

> [!NOTE]
> De logica ProxyCalc heeft een aantal extra gedrag voor geavanceerde scenario's niet in dit onderwerp wordt beschreven. In dit onderwerp is beschikbaar voor u meer inzicht in het gedrag en niet alle interne logische-document.

### <a name="quarantined-attribute-values"></a>In quarantaine geplaatste kenmerkwaarden
De kenmerken worden ook gebruikt als er dubbele kenmerkwaarden. Zie voor meer informatie [dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
