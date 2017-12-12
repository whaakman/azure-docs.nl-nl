---
title: Een verwijderde Office 365-groep in Azure Active Directory herstellen | Microsoft Docs
description: Het herstellen van een verwijderde groep, terug te zetten groepen weergeven en verwijderen van permamnently een groep in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 66b76b9bfd63f270c1bb86454908e2bae0897d04
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Een verwijderde Office 365-groep in Azure Active Directory herstellen

Wanneer u verwijdert een Office 365-groep in de Azure Active Directory (Azure AD), is de verwijderde groep behouden, maar niet zichtbaar voor 30 dagen van de datum van verwijdering. Dit is zodat de groep en de inhoud ervan kunnen worden hersteld indien nodig. Deze functionaliteit is beperkt uitsluitend tot Office 365-groepen in Azure AD. Het is niet beschikbaar voor beveiligingsgroepen en distributiegroepen.

> [!NOTE] 
> Gebruik geen `Remove-MsolGroup` omdat deze de groep permanent worden verwijderd. Gebruik altijd `Remove-AzureADMSGroup` een O365-groep verwijderen. 

De vereiste machtigingen voor het herstellen van een groep kunnen een van de volgende zijn:

Rol  | Machtigingen 
--------- | ---------
Bedrijfsbeheerder, Partner Tier2 ondersteuning en beheerders van InTune-Service | Een verwijderde groep voor Office 365 kunt herstellen 
Ondersteuning voor de beheerder van gebruiker en Partner Tier1 | Een verwijderde groep Office 365, behalve de toegewezen aan de Company Administrator-rol kunt herstellen 
Gebruiker | Een verwijderde Office 365-groep waarvan ze eigenaar kunt herstellen 


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>De verwijderde Office 365-groepen die beschikbaar zijn voor het herstellen weergeven
De volgende cmdlets kunnen worden gebruikt om de verwijderde groepen om te controleren dat de een- of die u geïnteresseerd bent in zijn niet nog permanent verwijderd. Deze cmdlets deel uitmaken van de [Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/). Meer informatie over deze module kunt u vinden de [Azure Active Directory PowerShell versie 2](/powershell/azure/install-adv2?view=azureadps-2.0) artikel.

1.  Voer de volgende cmdlet als u wilt weergeven van alle verwijderde Office 365-groepen in uw tenant die nog steeds beschikbaar zijn om te herstellen.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Als u de object-id van een specifieke groep weet (en u dit van de cmdlet in stap 1 downloaden kunt) Voer ook de volgende cmdlet om te controleren of de specifieke verwijderde groep is niet nog is permanent verwijderd.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-your-deleted-office-365-group"></a>Het herstellen van de verwijderde Office 365-groep
Zodra u hebt gecontroleerd of de groep nog steeds beschikbaar om te herstellen, herstelt u de verwijderde groep met een van de volgende stappen uit. Als de groep documenten, SP sites of andere permanente objecten bevat, duurt het tot 24 uur voor herstellen van een groep en de inhoud ervan.

1.  Voer de volgende cmdlet voor het herstellen van de groep en de inhoud ervan.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

U kunt ook kan de volgende cmdlet worden uitgevoerd als permanent wilt verwijderen de verwijderde groep.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Hoe weet u Hiermee gewerkte?
Om te bevestigen dat u hebt een Office 365-groep is hersteld, voer de `Get-AzureADGroup –ObjectId <objectId>` cmdlet om informatie over de groep weer te geven. Na het terugzetten van de is aanvraag voltooid:
- De groep wordt weergegeven in de linkernavigatiebalk op Exchange
- Het plan voor de groep wordt weergegeven in de Planner
- Alle Sharepoint-sites en de bijbehorende inhoud is beschikbaar
- De groep kan worden geopend vanuit een van de Exchange-eindpunten en andere Office 365-werkbelastingen die ondersteuning bieden voor Office 365-groepen


## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory-groepen.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
