---
title: Verificatie in twee stappen en AD FS - Azure MFA | Microsoft Docs
description: Dit is de Azure Multi-Factor Authentication-pagina waarop wordt beschreven hoe u met Azure MFA en AD FS aan de slag kunt gaan.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: 7b597f45a13c4f31c662a7832d9571dcc6b25e52
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Aan de slag met Azure Multi-Factor Authentication en Active Directory Federation Services
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Als uw organisatie met behulp van AD FS uw on-premises Active Directory heeft gefedereerd met Azure Active Directory, zijn er twee opties voor het gebruik van Azure Multi-Factor Authentication.

* Cloudresources beveiligen met Azure Multi-Factor Authentication of Active Directory Federation Services
* Cloudresources en on-premises resources beveiligen met behulp van de Azure Multi-Factor Authentication-server

De volgende tabel bevat een overzicht van de verschillen tussen de verificatie-ervaring bij het beveiligen van resources met Azure Multi-Factor Authentication en AD FS

| Verificatie-ervaring - op browser gebaseerde apps | Verificatie-ervaring - niet op browser gebaseerde apps |
|:--- |:--- |:--- |
| Beveiligen van Azure AD-resources met Azure Multi-Factor Authentication |<li>De eerste verificatiestap wordt uitgevoerd on-premises met AD FS.</li> <li>De tweede stap is een telefonische methode die met behulp van cloudverificatie wordt uitgevoerd.</li> |
| Azure AD-resources beveiligen met behulp van Active Directory Federation Services |<li>De eerste verificatiestap wordt uitgevoerd on-premises met AD FS.</li><li>De tweede stap wordt on-premises uitgevoerd door de claim toe te kennen.</li> |

Valkuilen met app-wachtwoorden voor federatieve gebruikers:

* App-wachtwoorden worden gecontroleerd met cloudverificatie en daarom worden federaties omzeild. Federatie wordt alleen actief gebruikt bij het instellen van het app-wachtwoord.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door app-wachtwoorden.
* U raakt on-premises mogelijkheden voor logboekregistratie bij verificatie voor app-wachtwoorden kwijt.
* Account uitschakelen/verwijderen kan met Directory-synchronisatie tot drie uur duren, waardoor uitschakelen/verwijderen van app-wachtwoorden in de cloudidentiteit wordt vertraagd.

Zie voor meer informatie over het instellen van Azure Multi-Factor Authentication of van de Azure Multi-Factor Authentication-server met AD FS de volgende artikelen:

* [Cloudresources beveiligen met behulp van Azure Multi-Factor Authentication en AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
* [Cloudresources en on-premises resources beveiligen met behulp van de Azure Multi-Factor Authentication-server met Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
* [Cloudresources en on-premises resources beveiligen met behulp van de Azure Multi-Factor Authentication-server met AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
