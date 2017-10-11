---
title: Overzicht van Enterprise State Roaming | Microsoft Docs
description: Bevat informatie over Enterprise State Roaming instellingen in Windows-apparaten. Enterprise State Roaming biedt gebruikers een uniforme ervaring via hun Windows-apparaten en minder tijd nodig voor het configureren van een nieuw apparaat.
services: active-directory
keywords: Wat is er Enterprise State Roaming, synchronisatie, enterprise, windows-cloud
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: b3c01f8d332d26e92dc3052681a4b2c95142d440
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-state-roaming-overview"></a>Overzicht van enterprise state roaming
Met Windows 10 [Azure Active Directory (Azure AD)](active-directory-whatis.md) gebruikers krijgen de mogelijkheid om te synchroniseren veilig hun gebruikersinstellingen en de instellingen van toepassingsgegevens naar de cloud. Enterprise State Roaming biedt gebruikers een uniforme ervaring via hun Windows-apparaten en minder tijd nodig voor het configureren van een nieuw apparaat. Enterprise State Roaming werkt vergelijkbaar met de standaard [consumer instellingen sync](http://windows.microsoft.com/en-US/windows-8/sync-settings-pcs) die is geïntroduceerd in Windows 8. Daarnaast biedt Enterprise State Roaming:

* **Scheiding van zakelijke en consumentgegevens** : organisaties zijn controle over hun gegevens en er is geen combinatie van zakelijke gegevens in een cloud-account van verbruiker of consumentgegevens in een enterprise cloud-account.
* **Verbeterde beveiliging** : gegevens worden automatisch versleuteld vóór het verlaten van de gebruiker Windows 10-apparaten met behulp van Azure Rights Management (Azure RMS) en gegevens blijven versleuteld in rust in de cloud. Alle inhoud blijft versleuteld in rust in de cloud, met uitzondering van de naamruimten, zoals namen van de instellingen en Windows-app.  
* **Beheer en controle voor een betere** – controle op en inzicht biedt ten opzichte van die instellingen in uw organisatie en op welke apparaten via de portal Azure AD-integratie wordt gesynchroniseerd. 

Enterprise State Roaming is beschikbaar in meerdere Azure-regio's. U kunt de bijgewerkte lijst met beschikbare regio's vinden op de [Azure-Services per regio's](https://azure.microsoft.com/regions/#services) pagina onder Azure Active Directory.

| Artikel | Beschrijving |
| --- | --- |
| [Enterprise State Roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md) |Enterprise State Roaming is beschikbaar voor elke organisatie met een Premium Azure Active Directory (Azure AD) abonnement. Zie voor meer informatie over het verkrijgen van een Azure AD-abonnement de [Azure AD-product](https://azure.microsoft.com/services/active-directory) pagina. |
| [Instellingen en veelgestelde vragen voor dataroaming](active-directory-windows-enterprise-state-roaming-faqs.md) |In dit onderwerp worden enkele vragen beantwoord IT-beheerders over de instellingen en synchroniseren van app-gegevens hebben kunnen. |
| [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) |Windows 10 biedt Groepsbeleid en beheer van mobiele apparaten (MDM)-beleidsinstellingen voor het beperken van synchronisatie van instellingen. |
| [Windows 10 zwervende naslaginformatie](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) |Hier volgt een volledige lijst met alle instellingen die zal worden zwerven en/of back-up in Windows 10. |
| [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md) |Dit onderwerp doorloopt enkele eenvoudige stappen voor probleemoplossing en bevat een lijst met bekende problemen. |

