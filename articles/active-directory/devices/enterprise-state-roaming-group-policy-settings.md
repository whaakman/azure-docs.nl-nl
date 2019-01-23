---
title: Beleid en de MDM-instellingen | Microsoft Docs
description: Bevat informatie over Groepsbeleid en het mobiele apparaat management (MDM)-instellingen die moeten worden gebruikt op apparaten die eigendom. Deze beleidsregels worden toegepast op het hele apparaat van de gebruiker.
services: active-directory
keywords: Wat zijn groep beleid en de MDM-instellingen voor Enterprise State Roaming, Enterprise State Roaming, windows cloud
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: curtand
ms.component: devices
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.openlocfilehash: 5ce3597bb52a22cfba6103b1d6cc5d0ff6371579
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447656"
---
# <a name="group-policy-and-mdm-settings"></a>Instellingen voor Groepsbeleid en MDM
Gebruik deze instellingen voor Groepsbeleid en het management (MDM)-instellingen voor mobiele apparaten alleen op apparaten in Bedrijfseigendom omdat deze beleidsregels worden toegepast op het hele apparaat van de gebruiker. Toepassen van een MDM-beleid om uit te schakelen van synchronisatie van instellingen voor een persoonlijk apparaat eigendom van gebruikers wordt negatieve invloed hebben op het gebruik van het apparaat. De accounts van andere gebruikers op het apparaat wordt bovendien ook worden beïnvloed door het beleid.

Ondernemingen die u wilt beheren voor persoonlijke (niet-beheerde) apparaten roaming kunnen Azure portal gebruiken voor in- of uitschakelen van roaming, in plaats van met behulp van Groepsbeleid of MDM
De volgende tabellen beschrijven de beschikbare instellingen.

## <a name="mdm-settings"></a>MDM-instellingen
De MDM-beleidsinstellingen van toepassing op zowel Windows 10 en Windows 10 Mobile.  Ondersteuning voor Windows 10 Mobile bestaat alleen voor Microsoft-account op basis van roaming via OneDrive-account van gebruiker.  Raadpleeg [apparaten en eindpunten](enterprise-state-roaming-windows-settings-reference.md) voor meer informatie over welke apparaten worden ondersteund voor het synchroniseren van Azure AD op basis van.

| Name | Description |
| --- | --- |
| Microsoft-Account verbinding toestaan |Gebruikers kunnen worden geverifieerd met behulp van een Microsoft-account op het apparaat |
| Synchronisatie van instellingen toestaan |Hiermee kunnen gebruikers Windows-instellingen en app-gegevens; roamen Als u dit beleid uitschakelt, wordt uitgeschakeld sync, evenals een back-ups op mobiele apparaten |

## <a name="group-policy-settings"></a>Instellingen voor groepsbeleid
De instellingen voor Groepsbeleid van toepassing op Windows 10-apparaten die zijn gekoppeld aan een Active Directory-domein. De tabel bevat ook verouderde instellingen die wordt weergegeven voor het beheren van synchronisatie-instellingen, maar die niet werken voor Enterprise State Roaming voor Windows 10, die worden vermeld bij 'Gebruik niet' in de beschrijving.

Deze instellingen bevinden zich op: `Computer Configuration > Administrative Templates > Windows Components > Sync your settings` 

| Name | Description |
| --- | --- |
| Accounts: Microsoft-Accounts blokkeren |Met deze beleidsinstelling wordt voorkomen dat gebruikers nieuwe Microsoft-accounts op deze computer toevoegen |
| Worden niet gesynchroniseerd |Hiermee voorkomt u dat gebruikers Windows-instellingen en app-gegevens kunnen worden gebruikt |
| Synchroniseer niet aanpassen |Uitschakelen van de groep thema's synchroniseren |
| Worden de browserinstellingen niet gesynchroniseerd. |Hiermee schakelt synchroniseren van de Internet Explorer-groep |
| Worden wachtwoorden niet gesynchroniseerd |Uitschakelen van de groep wachtwoorden synchroniseren |
| Andere Windows-instellingen worden niet worden gesynchroniseerd |Uitschakelen van de groep van andere Windows-instellingen synchroniseren |
| Bureaublad en persoonlijke instellingen worden niet worden gesynchroniseerd |Gebruik geen; heeft geen effect |
| Worden niet gesynchroniseerd op verbindingen met een datalimiet |Hiermee schakelt het roamen op gemeten verbindingen, zoals mobiele 3 G |
| Worden de apps niet gesynchroniseerd. |Gebruik geen; heeft geen effect |
| App-instellingen worden niet worden gesynchroniseerd |Hiermee schakelt roaming van app-gegevens |
| Worden de instellingen van start niet gesynchroniseerd. |Gebruik geen; heeft geen effect |

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht [enterprise State Roaming overzicht](enterprise-state-roaming-overview.md).


