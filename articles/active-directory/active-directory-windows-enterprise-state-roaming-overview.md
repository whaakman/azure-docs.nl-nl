---
title: Overzicht van Enterprise State Roaming | Microsoft Docs
description: Bevat informatie over Enterprise State Roaming-instellingen in het Windows-apparaten. Enterprise State Roaming biedt gebruikers een uniforme ervaring op hun Windows-apparaten en vermindert de tijd die nodig is voor het configureren van een nieuw apparaat.
services: active-directory
keywords: Wat is Enterprise State Roaming, synchronisatie, enterprise, windows-cloud
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: markvi
ms.openlocfilehash: c1332679af57a4994f4337e241845a87bf16b341
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059159"
---
# <a name="enterprise-state-roaming-overview"></a>Overzicht van enterprise state roaming
Met Windows 10, [Azure Active Directory (Azure AD)](fundamentals/active-directory-whatis.md) gebruikers krijgen de mogelijkheid hun gebruikersinstellingen en gegevens van de toepassing-instellingen naar de cloud veilig te synchroniseren. Enterprise State Roaming biedt gebruikers een uniforme ervaring op hun Windows-apparaten en vermindert de tijd die nodig is voor het configureren van een nieuw apparaat. Enterprise State Roaming werkt die vergelijkbaar is met de standaard [consumer-instellingen synchroniseren](http://windows.microsoft.com/windows-8/sync-settings-pcs) die is geïntroduceerd in Windows 8. Daarnaast biedt Enterprise State Roaming:

* **Scheiding van zakelijke en consumentgegevens** : organisaties zijn controle over hun gegevens en er is geen combinatie van zakelijke gegevens in een cloudopslagaccount consument of consumentgegevens in een enterprise cloud-account.
* **Verbeterde beveiliging** : gegevens worden automatisch versleuteld vóór het verlaten van de gebruiker Windows 10-apparaat met behulp van Azure Rights Management (Azure RMS) en gegevens blijven versleuteld in rust in de cloud. Alle inhoud blijft versleuteld in rust in de cloud, met uitzondering van de naamruimten, zoals de namen van de instellingen en Windows-app.  
* **Beheer en controle voor een betere** – controle en inzicht biedt ten opzichte van die worden gesynchroniseerd met de instellingen in uw organisatie en op welke apparaten via de integratie van Azure AD-portal. 

Enterprise State Roaming is beschikbaar in meerdere Azure-regio's. U kunt de bijgewerkte lijst met beschikbare regio's vinden op de [Azure-Services per regio's](https://azure.microsoft.com/regions/#services) pagina onder Azure Active Directory.

| Artikel | Beschrijving |
| --- | --- |
| [Enterprise State Roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md) |Enterprise State Roaming is beschikbaar voor elke organisatie met een Premium Azure Active Directory (Azure AD)-abonnement. Zie voor meer informatie over het verkrijgen van een Azure AD-abonnement, de [Azure AD-product](https://azure.microsoft.com/services/active-directory) pagina. |
| [Instellingen en gegevensroaming Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md) |In dit onderwerp vindt u antwoorden op enkele vragen die IT-beheerders mogelijk over de instellingen en app-gegevens synchroniseren. |
| [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) |Windows 10 biedt Groepsbeleid en beheer van mobiele apparaten (MDM)-beleidsinstellingen om te beperken van synchronisatie van instellingen. |
| [Windows 10 naslaginformatie over roaminginstellingen voor](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) |Hier volgt een volledige lijst van alle instellingen die zal worden zwerven en/of back-up in Windows 10. |
| [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md) |Dit onderwerp doorloopt enkele eenvoudige stappen voor het oplossen van problemen en bevat een lijst met bekende problemen. |

