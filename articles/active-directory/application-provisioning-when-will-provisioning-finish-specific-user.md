---
title: Nagaan wanneer een bepaalde gebruiker toegang tot een toepassing worden | Microsoft Docs
description: Nagaan wanneer een zeer belangrijk gebruiker de toegang tot een toepassing die u hebt geconfigureerd voor gebruikers inrichten met Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: fcefb31904cfb77022db0358e9feee6a0479db81
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a>Nagaan wanneer een bepaalde gebruiker toegang tot een toepassing worden
Wanneer u automatisch gebruikers inrichten met een toepassing, Azure AD automatisch inrichten en update gebruikersaccounts in een app op basis van items zoals [toewijzing van gebruikers en groepen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) op een regelmatig geplande tijdstip interval, doorgaans elke tien minuten.

## <a name="how-long-does-it-take"></a>Hoe lang duurt het?

De tijd die nodig is voor een bepaalde gebruiker moeten worden ingericht afhankelijk hoofdzakelijk of er een initiële synchronisatie 'volledige' al is gebeurd.

De eerste synchronisatie tussen Azure AD en een app kan overal van 20 minuten tot enkele uren duren, afhankelijk van de grootte van de Azure AD-directory en het aantal gebruikers in het bereik voor het inrichten. 

Volgende synchronisaties na de eerste synchronisatie sneller zijn (bijvoorbeeld binnen 10 minuten), zoals de inrichting service watermerken die de status van beide systemen na de eerste synchronisatie slaat, verbeterde prestaties van de volgende synchronisaties vertegenwoordigen.

## <a name="how-to-check-the-status-of-a-user"></a>Het controleren van de status van een gebruiker

Overzicht van de status van de inrichting van de geselecteerde gebruiker, raadpleegt u de controlelogboeken in Azure AD.

De inrichting controlelogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt; Audit logboeken** tabblad. Filter de logboeken op de **Account inrichten** categorie alleen de inrichting om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de 'overeenkomende ID' die is geconfigureerd voor deze in de kenmerktoewijzingen. 

Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomende kenmerk aan de kant van de Azure AD hebt geconfigureerd en de gebruiker niet inrichten heeft de waarde 'audrey@contoso.com', zoekt u in de auditlogboeken voor 'audrey@contoso.com' en bekijk vervolgens vermeldingen geretourneerd.

De inrichting controlelogboeken vastleggen alle bewerkingen die worden uitgevoerd door de inrichting service, waaronder:

* Uitvoeren van query's Azure AD voor toegewezen gebruikers die zich binnen het bereik van de inrichting
* Opvragen van de doel-app voor de aanwezigheid van deze gebruikers
* Vergelijking van de gebruikersobjecten tussen het systeem
* Toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen

## <a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
