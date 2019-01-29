---
title: Azure AD Connect sync-service de kenmerken van | Microsoft Docs
description: Beschrijving van de werking van de kenmerken in Azure AD Connect sync-service.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 7be6570d61bc3697bec143478404d32123b4f8d2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184220"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect sync-service de kenmerken van
De meeste kenmerken worden dezelfde manier weergegeven in Azure AD zoals in uw on-premises Active Directory. Maar bepaalde kenmerken enkele speciale verwerking hebben en de waarde van het kenmerk in Azure AD is mogelijk anders dan wat Azure AD Connect synchroniseert.

## <a name="introducing-shadow-attributes"></a>Maak kennis met de kenmerken van
Sommige kenmerken zijn twee manieren in Azure AD. Zowel de waarde van de on-premises en een berekende waarde worden opgeslagen. Deze extra kenmerken worden de kenmerken van genoemd. Zijn de twee meest voorkomende kenmerken waar u dit gedrag zien **userPrincipalName** en **proxyAddress**. De wijziging in de kenmerkwaarden treedt op wanneer er waarden zijn in deze kenmerken die niet-gecontroleerde domeinen. Maar de synchronisatie-engine in Connect leest de waarde in het kenmerk shadow, vanuit het perspectief, het kenmerk is bevestigd door Azure AD.

U kunt de kenmerken van met behulp van Azure portal of met PowerShell niet zien. Maar wat het concept helpt u bij het oplossen van bepaalde scenario's waar het kenmerk verschillende waarden voor de on-premises heeft en in de cloud.

Bekijk voor meer informatie over het gedrag, in dit voorbeeld van Fabrikam:  
![Domeinen](./media/how-to-connect-syncservice-shadow-attributes/domains.png)  
Ze hebben meerdere UPN-achtervoegsels in hun on-premises Active Directory, maar ze deze alleen hebt gecontroleerd.

### <a name="userprincipalname"></a>userPrincipalName
Een gebruiker heeft de volgende kenmerkwaarden in een niet-gecontroleerd domein:

| Kenmerk | Value |
| --- | --- |
| on-premises userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

Het kenmerk userPrincipalName is de waarde die u ziet wanneer u met behulp van PowerShell.

Omdat de waarde van het kenmerk echte on-premises zijn opgeslagen in Azure AD, wanneer u het domein fabrikam.com verifiëren, Azure AD het kenmerk userPrincipalName bijgewerkt met de waarde van de shadowUserPrincipalName. U hoeft niet te synchroniseren van wijzigingen uit Azure AD Connect voor deze waarden worden bijgewerkt.

### <a name="proxyaddresses"></a>proxyAddresses
Hetzelfde proces voor het opnemen van alleen geverifieerde domeinen vindt ook plaats voor proxyAddresses, maar met enkele aanvullende logica. De controle voor geverifieerde domeinen wordt alleen uitgevoerd voor Postvak gebruikers. Een gebruiker e-mailadres of neem contact op met vertegenwoordigen een gebruiker in een andere Exchange-organisatie en u kunt alle waarden in proxyAddresses toevoegen aan deze objecten.

Voor de postvakgebruiker van een, on-premises of in Exchange Online, worden er alleen waarden voor geverifieerde domeinen weergegeven. Het kan er als volgt:

| Kenmerk | Value |
| --- | --- |
| on-premises proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

In dit geval **smtp:abbie.spencer@fabrikam.com** is verwijderd omdat dit domein is niet geverifieerd. Maar ook toegevoegd Exchange **SIP:abbie.spencer@fabrikamonline.com**. Fabrikam Lync/Skype on-premises, Azure AD, maar niet gebruikt en Exchange Online voorbereiden voor het.

Deze logica voor proxyAddresses wordt aangeduid als **ProxyCalc**. ProxyCalc bij elke wijziging van een gebruiker die wordt aangeroepen wanneer:

- De gebruiker is toegewezen een serviceabonnement met Exchange Online, zelfs als de gebruiker is geen licentie voor Exchange. Bijvoorbeeld, als de gebruiker de Office-E3-SKU is toegewezen, maar alleen aan is toegewezen SharePoint Online. Dit geldt zelfs als uw postvak nog steeds on-premises wordt.
- Het kenmerk msExchRecipientTypeDetails heeft een waarde.
- U aanbrengt een wijziging in proxyAddresses of userPrincipalName.

ProxyCalc kan even duren voor het verwerken van een gebruiker wordt gewijzigd en is niet synchroon met de Azure AD Connect-exportproces.

> [!NOTE]
> De logica ProxyCalc heeft enkele aanvullende gedrag voor geavanceerde scenario's die niet wordt vermeld in dit onderwerp. In dit onderwerp is bedoeld voor u meer inzicht in het gedrag en niet alle interne logica.

### <a name="quarantined-attribute-values"></a>In quarantaine geplaatste kenmerkwaarden
De kenmerken worden ook gebruikt als er dubbele kenmerkwaarden. Zie voor meer informatie, [tolerantie van dubbel kenmerk](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Zie ook
* [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
