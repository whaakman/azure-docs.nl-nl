---
title: Upgrade van DirSync en Azure AD Sync | Microsoft Docs
description: Beschrijft hoe een upgrade van DirSync en Azure AD Sync naar Azure AD Connect.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3674670e10500d2992539ab60fbdb31b666fcf9a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Upgrade van Windows Azure Active Directory-synchronisatie en Azure Active Directory-synchronisatie
Azure AD Connect is de beste manier om verbinding te maken met uw on-premises directory met Azure AD en Office 365. Dit is een fantastische tijd om te upgraden naar Azure AD Connect van Windows Azure Active-directorysynchronisatie (DirSync) of Azure AD Sync omdat deze hulpprogramma's zijn gedeprecieerd en vanaf 13 april 2017 niet langer worden ondersteund.

De identiteit van de twee synchronisatie-hulpprogramma's die zijn afgeschaft zijn beschikbaar voor klanten met één forest (DirSync) en voor meerdere forests en andere geavanceerde klanten (Azure AD Sync). Deze oudere hulpprogramma's zijn vervangen door een enkele oplossing die beschikbaar is voor alle scenario's: Azure AD Connect. Dit biedt nieuwe functionaliteit, verbeteringen en ondersteuning voor nieuwe scenario's. Om te kunnen doorgaan met het synchroniseren van uw on-premises identity-gegevens naar Azure AD en Office 365, wordt aangeraden dat u een upgrade naar Azure AD Connect uitvoert.

De laatste versie van DirSync in juli 2014 is uitgebracht en de laatste versie van Azure AD Sync werd uitgebracht in mei 2015.

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD Connect?
Azure AD Connect is de opvolger van DirSync en Azure AD Sync. Het combineert alle scenario's deze twee ondersteund. U kunt meer lezen over in [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Afschaffing planning
| Date | Opmerking |
| --- | --- |
| 13 april 2016 |Windows Azure Active Directory-synchronisatie (DirSync') en Microsoft Azure Active Directory-synchronisatie ('Azure AD Sync) worden vermeld als afgeschaft. |
| 13 april 2017 |Ondersteuning eindigt. Klanten wordt niet langer een ondersteuningsaanvraag openen zonder eerst upgraden naar Azure AD Connect. |
|31 december 2017|Azure AD wordt communicatie van Windows Azure Active Directory-synchronisatie (DirSync') en Microsoft Azure Active Directory-synchronisatie ('Azure AD Sync') niet meer accepteren.

## <a name="how-to-transition-to-azure-ad-connect"></a>Het overstappen naar Azure AD Connect
Als u DirSync worden uitgevoerd, er zijn twee manieren waarop u een upgrade kunt uitvoeren: In-place upgrade en parallelle implementatie. Een in-place upgrade wordt aanbevolen voor de meeste klanten en als er een recent besturingssysteem en minder dan 50.000 objecten. In andere gevallen is het aanbevolen voor een parallelle implementatie waar uw DirSync-configuratie wordt verplaatst naar een nieuwe server met Azure AD Connect.

Als u Azure AD Sync gebruikt, wordt een in-place upgrade aanbevolen. Als u maken wilt, is het mogelijk te installeren van een nieuwe Azure AD Connect-server parallel en voer een migratie swing van uw Azure AD Sync-server naar Azure AD Connect.

| Oplossing | Scenario |
| --- | --- |
| [Upgraden van DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Als u een bestaande DirSync-server al wordt uitgevoerd.</li> |
| [Upgrade uitvoeren vanaf Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>Als u Azure AD Sync overstapt.</li> |

Als u zien hoe u een in-place upgrade van DirSync naar Azure AD Connect wilt, raadpleegt u vervolgens deze Channel 9 video:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>Veelgestelde vragen
**V: ik heb een e-mailbericht ontvangen van het Team van Azure en/of een bericht van het Office 365-beheercentrum bericht, maar ik gebruik verbinding maken.**  
De melding is ook verzonden naar klanten die gebruikmaken van Azure AD Connect met een build-nummer 1.0. \*.0 (met een pre-1.1-release). Microsoft raadt aan klanten kunnen de nieuwste versies van Azure AD Connect. De [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) 1.1 nieuwe functie kunt u gemakkelijk altijd een recente versie van Azure AD Connect geïnstalleerd hebt.

**V: wordt DirSync/Azure AD Sync niet meer werken op 13 April 2017?**  
DirSync/Azure AD Sync blijven werken op 13 April-2017.  Azure AD wordt echter niet meer communicatie van DirSync/Azure AD Sync accepteren op December 31 2017.

**V: welke versies DirSync kan ik upgraden van**  
Dit wordt ondersteund voor een upgrade uitvoert van een DirSync-release momenteel wordt gebruikt.

**V: hoe zit het Azure AD-Connector voor FIM/MIM?**  
De Azure AD-Connector voor FIM/MIM heeft **niet** is aangekondigd als afgeschaft. Het is ingesteld op **functie bevriezing**; er is geen nieuwe functionaliteit is toegevoegd en het ontvangt geen oplossingen voor problemen. Microsoft raadt aan klanten die gebruikmaken van het plannen voor het verplaatsen van deze naar Azure AD Connect. Het is raadzaam nieuwe implementaties met behulp van het niet starten. Deze Connector worden aangekondigd in de toekomst wordt afgeschaft.

## <a name="additional-resources"></a>Aanvullende resources
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
