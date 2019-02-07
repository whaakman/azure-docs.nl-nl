---
title: Over het aanpassen van een regel voor synchronisatie in Azure AD Connect | Microsoft-Docs
description: Dit onderwerp bevat stappen voor het oplossen van problemen met Azure AD Connect installeert.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 1407df2dbc0cc590ea919ca0ead727959b1e08b4
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773376"
---
# <a name="how-to-customize-a-synchronization-rule"></a>Over het aanpassen van een regel voor synchronisatie

## <a name="recommended-steps"></a>**Aanbevolen stappen**

U kunt de regeleditor synchronisatie gebruiken om te bewerken of maken van een nieuwe regel voor synchronisatie. U moet een ervaren gebruiker wijzigingen aanbrengen in synchronisatieregels. Verkeerde wijzigingen kunnen leiden tot het verwijderen van objecten uit uw doeldirectory. Lees [aanbevolen documenten](#recommended-documents) expertise in synchronisatieregels krijgen. Om te wijzigen gaat u een regel voor synchronisatie via de volgende stappen uit:

* Start de synchronisatie-editor in het toepassingsmenu in desktop, zoals hieronder weergegeven:

    ![Menu Synchronization Rule Editor](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/syncruleeditormenu.png)

* Klonen om een standaardregel voor de synchronisatie aanpassen, de bestaande regel door te klikken op de knop 'Bewerken' op de Synchronization Rules Editor die wordt een kopie van de standaard-regel maken en deze uitschakelen. De gekloonde regel niet opslaan met een prioriteit van minder dan 100.  Prioriteit bepaalt welke regel wins (laagste numerieke waarde) een conflictoplossing als er een conflict kenmerk-stroom.

    ![Synchronization Rule Editor](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/clonerule.png)

* Als u een specifiek kenmerk wijzigt, moet in het ideale geval u alleen behouden het kenmerk wijzigen in de gekloonde regel.  Schakel vervolgens de standaardregel zodat gewijzigde kenmerk is afkomstig van de gekloonde regel en andere kenmerken van de standaardregel voor de standaard worden verzameld. 

* Houd er rekening mee dat in het geval waarin de berekende waarde van het aangepaste kenmerk is NULL in de gekloonde regel en is niet NULL zijn in de standaard-standaard regel vervolgens de not NULL-waarde wint en de NULL-waarde wordt vervangen. Als u niet wilt dat een NULL-waarde om te worden vervangen door een geen NULL-waarde vervolgens toewijzen AuthoritativeNull in uw gekloonde regel.

* Wijzig een **uitgaand** regel, filter wijzigen van de editor van de regel voor synchronisatie.

## <a name="recommended-documents"></a>**Aanbevolen documenten**
* [Azure AD Connect-synchronisatie: Technische concepten](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect-synchronisatie: Inzicht in de architectuur](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect-synchronisatie: Inzicht in declaratieve inrichting](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect-synchronisatie: Inzicht in verklarende inrichtingsexpressies](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect-synchronisatie: Inzicht in de standaardconfiguratie](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect-synchronisatie: Inzicht in gebruikers, groepen en contactpersonen](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect-synchronisatie: Schaduwkenmerken](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Volgende stappen
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md).
- [Wat is hybride identiteit? ](whatis-hybrid-identity.md).