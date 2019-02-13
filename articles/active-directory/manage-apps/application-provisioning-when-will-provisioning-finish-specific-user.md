---
title: Ontdek wanneer een specifieke gebruiker toegang krijgen tot een toepassing worden | Microsoft Docs
description: Nagaan wanneer een cruciaal gebruiker toegang heeft tot een toepassing die u hebt geconfigureerd voor het inrichten van gebruikers met Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d2bb3b7385467d2606a2a4fa0afb43b9440ab79
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56163101"
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a>Ontdek wanneer een specifieke gebruiker toegang heeft tot een toepassing
Wanneer u automatisch inrichten van gebruikers met een toepassing, Azure AD automatisch inrichten en bijwerken van gebruikersaccounts in een app op basis van items zoals [gebruikers- en groepstoewijzing](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) op een regelmatig geplande tijdstip interval, doorgaans elke tien minuten.

## <a name="how-long-does-it-take"></a>Hoe lang duurt het voordat?

De tijd die nodig zijn voor een bepaalde gebruiker moeten worden ingericht, is afhankelijk van voornamelijk op of er een eerste synchronisatie 'volledig' al is gebeurd.

De eerste synchronisatie tussen Azure AD en een app kunt tussen 20 minuten duren tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. 

Volgende synchronisaties na de initiële synchronisatie worden sneller (bijvoorbeeld binnen 10 minuten), zoals de inrichtingsservice watermerken die staan voor de status van beide systemen na de initiële synchronisatie, het verbeteren van de prestaties van de volgende synchronisatie worden opgeslagen.

## <a name="how-to-check-the-status-of-a-user"></a>Het controleren van de status van een gebruiker

Als u wilt zien de Inrichtingsstatus voor de geselecteerde gebruiker, raadpleegt u de controlelogboeken in Azure AD.

De inrichting auditlogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; controleren logboeken** tabblad. De logboeken filteren op de **Accountinrichting** categorie alleen de inrichten om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de "overeenkomende ID" die is geconfigureerd voor deze in de kenmerktoewijzingen. 

Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomstige kenmerk aan de Azure AD geconfigureerd en de gebruiker niet ingericht heeft een waarde van "audrey@contoso.com', zoekt u de auditlogboeken voor 'audrey@contoso.com' en bekijk vervolgens vermeldingen geretourneerd.

De inrichting auditlogboeken Noteer alle bewerkingen die worden uitgevoerd door de provisioning-service, met inbegrip van:

* Een query uitvoeren op Azure AD voor toegewezen gebruikers die zich in het bereik vallen voor inrichting
* Uitvoeren van query's de doel-app voor de aanwezigheid van deze gebruikers
* Vergelijking van de gebruikersobjecten van het systeem
* Toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen

## <a name="next-steps"></a>Volgende stappen
[Gebruiker-inrichting en ongedaan maken van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
