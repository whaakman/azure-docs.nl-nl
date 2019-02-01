---
title: Upgraden van DirSync en Azure AD Sync | Microsoft Docs
description: Beschrijving van het upgraden van DirSync en Azure AD Sync naar Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b95fa5b5336db62bdf64ebd0dc254160d1a9282d
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490597"
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Upgrade van Windows Azure Active Directory Sync en Azure Active Directory Sync
Azure AD Connect is de beste manier om verbinding te maken met uw on-premises directory met Azure AD en Office 365. Dit is een goed moment om te upgraden naar Azure AD Connect van Windows Azure Active Directory Sync (DirSync) of Azure AD Sync omdat deze hulpprogramma's zijn afgeschaft en vanaf 13 April 2017 niet langer worden ondersteund.

De twee identiteit synchronisatie-hulpprogramma's die zijn afgeschaft zijn beschikbaar voor klanten met één forest (DirSync) en voor multi-forest en andere geavanceerde klanten (Azure AD Sync). Deze oudere hulpprogramma's zijn vervangen door een enkele oplossing die beschikbaar is voor alle scenario's: Azure AD Connect. Biedt nieuwe functionaliteit, verbeteringen en ondersteuning voor nieuwe scenario's. Om te kunnen doorgaan met het synchroniseren van uw on-premises id-gegevens naar Azure AD en Office 365, wordt aangeraden dat u een upgrade naar Azure AD Connect uitvoert. Microsoft is geen garantie voor deze oudere versies werken na 31 December 2017.

De laatste versie van DirSync in juli 2014 is uitgebracht en de laatste versie van Azure AD Sync werd uitgebracht in mei 2015.

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD Connect?
Azure AD Connect is de opvolger van DirSync en Azure AD Sync. Het combineert alle scenario's deze twee ondersteund. U kunt meer lezen over het in [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).

## <a name="deprecation-schedule"></a>Schema
| Date | Opmerking |
| --- | --- |
| 13 april 2016 |Windows Azure Active Directory Sync (DirSync') en Microsoft Azure Active Directory Sync ('Azure AD Sync') zijn aangekondigd als afgeschaft. |
| Vanaf 13 april 2017 |Ondersteuning wordt beëindigd. Klanten worden niet langer een ondersteuningsaanvraag openen zonder eerst een upgrade naar Azure AD Connect. |
|En met 31 december 2017|Azure AD kan communicatie van Windows Azure Active Directory Sync (DirSync') en Microsoft Azure Active Directory Sync ('Azure AD Sync') niet meer accepteren.

## <a name="how-to-transition-to-azure-ad-connect"></a>Hoe u de overgang naar Azure AD Connect
Als u DirSync worden uitgevoerd, zijn er twee manieren voor het upgraden: In-place upgrade en parallelle implementatie. Een in-place upgrade wordt aanbevolen voor de meeste klanten en als er een recente besturingssysteem en minder dan 50.000 objecten. In andere gevallen is het aanbevolen voor een parallelle implementatie waar uw DirSync-configuratie wordt verplaatst naar een nieuwe server met Azure AD Connect.

| Oplossing | Scenario |
| --- | --- |
| [Upgraden van DirSync](how-to-dirsync-upgrade-get-started.md) |<li>Als u een bestaande DirSync-server al wordt uitgevoerd.</li> |
| [Een upgrade uitvoeren voor Azure AD Sync](how-to-upgrade-previous-version.md) |<li>Als u van Azure AD Sync overstapt.</li> |

Als u zien hoe u een in-place upgrade van DirSync naar Azure AD Connect doet wilt, klikt u vervolgens deze Channel 9-video te bekijken:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>Veelgestelde vragen
**V: Ik heb een e-mailbericht hebt ontvangen van het Azure-Team en/of een bericht van het Office 365-berichtencentrum, maar ik ben met behulp van Connect.**  
De melding is ook verzonden naar klanten die gebruikmaken van Azure AD Connect met een build-nummer 1.0. \*.0 (met behulp van een pre-1.1-versie). Microsoft adviseert klanten actueel blijven met Azure AD Connect-releases. De [Automatische upgrade](how-to-connect-install-automatic-upgrade.md) functie die is geïntroduceerd in 1.1 kunt u eenvoudig altijd een recente versie van Azure AD Connect geïnstalleerd hebt.

**V: DirSync/Azure AD Sync niet meer zal werken op 13 April 2017?**  
DirSync/Azure AD Sync blijven werken vanaf 13 April 2017.  Azure AD kan echter niet meer accepteren voor communicatie van DirSync/Azure AD Sync na 31 December 2017.

**V: Welke versies van het DirSync kan ik upgraden van?**  
Het wordt ondersteund voor een upgrade uitvoert van een DirSync-release die momenteel wordt gebruikt. 

**V: Hoe zit het met de Azure AD-Connector voor FIM/MIM?**  
De Azure AD-Connector voor FIM/MIM is **niet** is aangekondigd als afgeschaft. Het is aan **functie stilzetten**; er is geen nieuwe functionaliteit wordt toegevoegd en ontvangt geen oplossingen voor problemen. Microsoft adviseert klanten deze gebruiken voor het plannen van het verplaatsen naar Azure AD Connect. Het is raadzaam niet starten met behulp van deze nieuwe implementaties. Deze Connector worden aangekondigd in de toekomst afgeschaft.

## <a name="additional-resources"></a>Aanvullende resources
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
