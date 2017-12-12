---
title: Instellingen voor beleid en MDM groep | Microsoft Docs
description: Bevat informatie over Groepsbeleid en het mobiele apparaat management (MDM)-instellingen die moeten worden gebruikt op apparaten in Bedrijfseigendom. Deze beleidsregels worden toegepast op het hele apparaat van de gebruiker.
services: active-directory
keywords: Wat zijn groep beleid en de MDM-instellingen voor Enterprise State Roaming, Enterprise State Roaming, windows cloud
documentationcenter: 
author: tanning
manager: mtillman
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 588084481ffc5cbbeed34e9527271179fa359ed5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="group-policy-and-mdm-settings"></a>Instellingen voor Groepsbeleid en MDM
Gebruik deze instellingen voor Groepsbeleid en het management (MDM)-instellingen voor mobiele apparaten alleen op apparaten in Bedrijfseigendom omdat deze beleidsregels worden toegepast op het hele apparaat van de gebruiker. Toepassen van een MDM-beleid voor het uitschakelen van synchronisatie van instellingen voor een persoonlijke invloed apparaat is eigendom van gebruiker negatieve op het gebruik van het apparaat. Bovendien worden de accounts van andere gebruikers op het apparaat ook beïnvloed door het beleid.

Ondernemingen die u wilt beheren voor persoonlijke (onbeheerde) apparaten roaming kunt de Azure-portal in- of uitschakelen van roaming in plaats van met behulp van Groepsbeleid of MDM
De volgende tabellen beschrijven de beleidsinstellingen die beschikbaar is.

## <a name="mdm-settings"></a>MDM-instellingen
De MDM-beleidsinstellingen van toepassing op Windows 10- en Windows 10 Mobile.  Windows 10 Mobile-ondersteuning geldt alleen voor Microsoft-account op basis van roaming via OneDrive-account van de gebruiker.  Raadpleeg de sectie 'Apparaten en eindpunten' voor meer informatie over welke apparaten worden ondersteund voor het synchroniseren van Azure AD op basis.

| Naam | Beschrijving |
| --- | --- |
| Microsoft-Account verbinding toestaan |Gebruikers worden geverifieerd via een Microsoft-account op het apparaat |
| Synchronisatie van instellingen toestaan |Hiermee kunnen gebruikers roaming van Windows-instellingen en app-gegevens; Als u dit beleid uitschakelt, wordt uitgeschakeld sync, evenals een back-ups op mobiele apparaten |

## <a name="group-policy-settings"></a>Instellingen voor Groepsbeleid
De instellingen voor Groepsbeleid van toepassing op Windows 10-apparaten die zijn gekoppeld aan een Active Directory-domein. De tabel bevat ook verouderde instellingen die wordt weergegeven voor het beheren van synchronisatie-instellingen, maar die niet werken voor Enterprise status zwervende voor Windows 10, die worden vermeld bij 'Gebruik niet' in de beschrijving.

| Naam | Beschrijving |
| --- | --- |
| Accounts: Blok Microsoft-Accounts |Met deze beleidsinstelling wordt voorkomen dat gebruikers nieuwe Microsoft-accounts op deze computer toevoegen |
| Synchroniseer niet |Voorkomt dat gebruikers voor het Windows-instellingen en app-gegevens kunnen worden gebruikt |
| Synchroniseer niet aanpassen |Schakelt synchroniseren van de groep thema 's |
| Synchroniseer niet browserinstellingen |Schakelt synchroniseren van de groep van Internet Explorer |
| Synchroniseert geen wachtwoorden |Schakelt synchronisatie van wachtwoorden groep |
| Synchroniseert geen andere Windows-instellingen |Schakelt synchroniseren van de groep van andere Windows-instellingen |
| Synchroniseer niet bureaubladinstellingen |Gebruik geen; heeft geen effect |
| Synchroniseer niet op verbindingen met datalimieten |Schakelt roaming op verbindingen, zoals mobiele 3 G datalimiet |
| Synchroniseer de apps niet |Gebruik geen; heeft geen effect |
| Synchroniseert geen app-instellingen |Schakelt roaming van app-gegevens |
| Synchroniseert geen instellingen van start |Gebruik geen; heeft geen effect |

## <a name="related-topics"></a>Verwante onderwerpen
* [Overzicht van Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Enterprise-status roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
* [Instellingen en veelgestelde vragen voor dataroaming](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Windows 10 zwervende naslaginformatie](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

