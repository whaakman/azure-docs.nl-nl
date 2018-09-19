---
title: 'Azure AD Connect-synchronisatie: het Azure AD-serviceaccount beheren | Microsoft Docs'
description: In dit onderwerp beschrijft het herstellen van de Azure AD-serviceaccount.
services: active-directory
keywords: AADSTS70002, AADSTS50054, het opnieuw instellen van het wachtwoord voor de Connector-serviceaccount van de Azure AD Connect-synchronisatie
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 93dcbe5ceac87d84db9638de6d5fe120de8c2d36
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46313647"
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect-synchronisatie: het Azure AD-serviceaccount beheren
Het serviceaccount dat wordt gebruikt door de Azure AD-Connector is mag service gratis. Als u de referenties opnieuw instellen wilt, klikt u vervolgens is in dit onderwerp voor u. Bijvoorbeeld, als een globale beheerder per ongeluk het wachtwoord opnieuw instellen op de serviceaccount met behulp van PowerShell.

## <a name="reset-the-credentials"></a>De referenties opnieuw instellen
Als het serviceaccount dat is gedefinieerd in de Azure AD-Connector geen contact krijgt met Azure AD vanwege problemen met de verificatie, kan het wachtwoord opnieuw worden ingesteld.

1. Aanmelden bij de Azure AD Connect-synchronisatieserver en start PowerShell.
2. Voer `Add-ADSyncAADServiceAccount` uit.  
   ![PowerShell-cmdlet addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD-hoofdbeheerder referenties opgeven.

Deze cmdlet Hiermee stelt u het wachtwoord voor het serviceaccount en deze bijwerken in Azure AD en in de synchronisatie-engine.

## <a name="known-issues-these-steps-can-solve"></a>Bekende problemen in die deze stappen op te lossen
In deze sectie wordt een lijst van fouten die zijn gerapporteerd door klanten die door een referenties opnieuw instellen op de Azure AD-serviceaccount zijn opgelost.

- - -
Gebeurtenis 6900  
De server heeft een onverwachte fout aangetroffen tijdens het verwerken van een melding voor wachtwoordwijziging:  
AADSTS70002: Fout bij valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

- - -
Gebeurtenis 659  
Fout bij het ophalen van wachtwoord-beleidsconfiguratie synchroniseren. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fout bij valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht in en synchronisatie aanpassen](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

