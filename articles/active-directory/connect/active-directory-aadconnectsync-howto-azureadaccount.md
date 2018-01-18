---
title: 'Azure AD Connect-synchronisatie: het Azure AD-serviceaccount beheren | Microsoft Docs'
description: In dit onderwerp beschrijft het herstellen van het Azure AD-serviceaccount.
services: active-directory
keywords: AADSTS70002, AADSTS50054, het opnieuw instellen van het wachtwoord voor de Azure AD Connect-synchronisatie Connector-serviceaccount
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: cfd807706ebbf0bfa6ea699129cb197f1c79db8c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect-synchronisatie: het beheren van het Azure AD-serviceaccount
Het serviceaccount dat wordt gebruikt door de Azure AD-Connector moet zijn gratis. Als u terugzetten van de referenties wilt, wordt de in dit onderwerp voor u. Bijvoorbeeld, als een globale beheerder per ongeluk het wachtwoord opnieuw instellen op de serviceaccount met behulp van PowerShell.

## <a name="reset-the-credentials"></a>De referenties opnieuw instellen
Als het serviceaccount dat is gedefinieerd in de Azure AD-Connector geen contact met Azure AD vanwege verificatieproblemen, het wachtwoord opnieuw kan worden ingesteld.

1. Aanmelden bij de Azure AD Connect sync-server en start PowerShell.
2. Voer `Add-ADSyncAADServiceAccount` uit.  
   ![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Geef referenties op globale beheerder Azure AD.

Deze cmdlet het wachtwoord voor het serviceaccount opnieuw instellen en deze bijwerken in Azure AD en in de synchronisatie-engine.

## <a name="known-issues-these-steps-can-solve"></a>Bekende problemen met die deze stappen kunnen worden opgelost
Deze sectie is een lijst met fouten die zijn gerapporteerd door klanten die door een gebruikersreferenties instellen op het Azure AD-serviceaccount zijn opgelost.

- - -
Gebeurtenis 6900  
De server is een onverwachte fout opgetreden tijdens het verwerken van een wijzigingsmelding wachtwoord:  
AADSTS70002: Fout bij valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

- - -
Gebeurtenis 659  
Fout bij het ophalen van wachtwoord-beleidsconfiguratie synchronisatie. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fout bij valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

