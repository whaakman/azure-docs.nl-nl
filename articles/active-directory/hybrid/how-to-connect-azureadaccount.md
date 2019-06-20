---
title: Wijzig het wachtwoord voor het Azure AD-Connector | Microsoft Docs
description: In dit onderwerp beschrijft het herstellen van de Azure AD-Connector-account.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ea151ee79fccd66f1d9422744d8f57829677ec0
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204537"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Het wachtwoord voor het Azure AD Connector-account wijzigen
Het account van Azure AD-Connector is mag service gratis. Als u de referenties opnieuw instellen wilt, klikt u vervolgens is in dit onderwerp voor u. Bijvoorbeeld, als een globale beheerder per ongeluk het wachtwoord opnieuw instellen op het account met behulp van PowerShell.

## <a name="reset-the-credentials"></a>De referenties opnieuw instellen
Als het account van Azure AD-Connector geen contact krijgt met Azure AD vanwege problemen met de verificatie, kan het wachtwoord opnieuw worden ingesteld.

1. Aanmelden bij de Azure AD Connect-synchronisatieserver en start PowerShell.
2. Voer `Add-ADSyncAADServiceAccount` uit.  
   ![PowerShell-cmdlet addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD-hoofdbeheerder referenties opgeven.

Deze cmdlet Hiermee stelt u het wachtwoord voor het serviceaccount en deze bijwerken in Azure AD en in de synchronisatie-engine.

## <a name="known-issues-these-steps-can-solve"></a>Bekende problemen in die deze stappen op te lossen
In deze sectie wordt een lijst van fouten die zijn gerapporteerd door klanten die door een referenties opnieuw instellen van de Azure AD-Connector-account zijn opgelost.

---
Event 6900  
De server heeft een onverwachte fout aangetroffen tijdens het verwerken van een melding voor wachtwoordwijziging:  
AADSTS70002: Fout bij het valideren van referenties. AADSTS50054: Er is een oud wachtwoord gebruikt voor verificatie.

---
Event 659  
Fout bij het ophalen van wachtwoord-beleidsconfiguratie synchroniseren. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fout bij het valideren van referenties. AADSTS50054: Er is een oud wachtwoord gebruikt voor verificatie.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

