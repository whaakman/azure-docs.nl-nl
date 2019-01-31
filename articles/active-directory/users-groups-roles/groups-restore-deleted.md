---
title: Een verwijderde Office 365-groep herstellen in Azure AD | Microsoft Docs
description: Een verwijderde groep herstellen, groepen weergeven die kunnen worden hersteld, en een groep definitief verwijderen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: ef18897d00e29a21dd602dc7213706bfc30a68bc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179375"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Een verwijderde Office 365-groep herstellen in Azure Active Directory

Wanneer u een Office 365-groep verwijdert in Azure AD (Azure Active Directory), blijft de verwijderde groep behouden maar is deze gedurende 30 dagen na de verwijderingsdatum niet meer zichtbaar. Zo kunnen de groep en bijbehorende inhoud indien nodig nog worden hersteld. Deze functionaliteit is exclusief beperkt tot Office 365-groepen in Azure AD. Het is niet beschikbaar voor beveiligingsgroepen en distributiegroepen.

> [!NOTE]
> Gebruik `Remove-MsolGroup` niet omdat de groep dan definitief wordt leeggemaakt. Gebruik `Remove-AzureADMSGroup` altijd om een O365-groep te verwijderen.

De machtigingen die zijn vereist om een groep te herstellen, kunnen zijn:

Rol | Machtigingen
--------- | ---------
Bedrijfsbeheerder, Laag2-ondersteuning voor partner en InTune-servicebeheerders | Kan elke willekeurige Office 365-groep herstellen
Beheerder van gebruikersaccounts en Laag1-ondersteuning voor partner | Kan elke verwijderde Office 365-groep herstellen behalve de groepen die zijn toegewezen aan de rol Bedrijfsbeheerder
Gebruiker | Kan elke willekeurige Office 365-groep herstellen waarvan ze eigenaar zijn


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>De verwijderde Office 365-groepen weergeven die beschikbaar zijn om te herstellen

De volgende cmdlets kunnen worden gebruikt om de verwijderde groepen weer te geven. Zo kunt u controleren of de groep of groepen waarin u geïnteresseerd bent, nog niet definitief zijn leeggemaakt. Deze cmdlets vormen onderdeel van de [Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/). In het artikel [Azure Active Directory PowerShell-versie 2](/powershell/azure/install-adv2?view=azureadps-2.0) vindt u meer informatie over deze module.

1.  Voer de volgende cmdlet uit om alle verwijderde Office 365-groepen in de tenant weer te geven die nog beschikbaar zijn voor herstellen.
   
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Als u de object-id van een specifieke groep weet (u kunt deze ophalen uit de cmdlet in stap 1) , kunt u de ook volgende cmdlet uitvoeren om te controleren dat de specifieke verwijderde groep nog niet definitief is leeggemaakt.
  
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```

## <a name="how-to-restore-your-deleted-office-365-group"></a>Uw verwijderde Office 365-groep herstellen
Zodra u hebt gecontroleerd of de groep nog steeds beschikbaar is voor herstellen, herstelt u de verwijderde groep met een van de volgende stappen. Als de groep documenten, SP-sites of andere permanente objecten bevat, duurt het maximaal 24 uur om deze groep en de bijbehorende inhoud te herstellen.

1.  Voer de volgende cmdlet uit om de groep en de bijbehorende inhoud te herstellen.
 
 ```
 Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
 ``` 

U kunt ook de volgende cmdlet uitvoeren om de verwijderde groep definitief te verwijderen.
 ```
 Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
 ```

## <a name="how-do-you-know-this-worked"></a>Hoe weet u of dit heeft gewerkt?

Voer de cmdlet `Get-AzureADGroup –ObjectId <objectId>` uit om informatie over de groep weer te geven als u wilt controleren of het herstellen van een Office 365-groep is geslaagd. Nadat de herstelaanvraag is voltooid, ziet u het volgende:

- De groep wordt in de linkernavigatiebalk in Exchange weergegeven
- Het plan voor de groep wordt weergegeven in Planner
- Alle SharePoint-sites en alle bijbehorende inhoud zijn beschikbaar
- Toegang tot de groep kan worden verkregen via elk Exchange-eindpunt en andere Office 365-workload die ondersteuning biedt voor Office 365-groepen

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory-groepen.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
