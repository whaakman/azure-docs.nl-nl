---
title: Fouten oplossen in Azure Active Directory reporting API | Microsoft Docs
description: Biedt u een oplossing voor fouten tijdens het aanroepen van Azure Active Directory Reporting API's.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 33c382c8b28549e1b2be83322b910ca1a885e45d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622753"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Fouten oplossen in Azure Active Directory reporting API

In dit artikel geeft een lijst van de veelvoorkomende foutberichten die u ondervindt mogelijk bij het openen van de activiteitenrapporten met behulp van de MS Graph API en de stappen voor de oplossing.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 HTTP-interne serverfout opgetreden tijdens het openen van Microsoft Graph-V2-eindpunt

We ondersteunen momenteel geen de v2-eindpunt voor Microsoft Graph - Zorg ervoor dat u toegang tot de activiteitenlogboeken met behulp van de Microsoft Graph-v1-eindpunt.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Fout: Kan geen gebruikersrollen ophalen uit AD Graph

U mag deze foutmelding krijgt bij het openen van aanmeldingen met behulp van Graph Explorer. Zorg ervoor dat u bent aangemeld bij uw account met behulp van de knoppen voor aanmelding bij in de Graph Explorer-gebruikersinterface, zoals wordt weergegeven in de volgende afbeelding. 

![Graph Explorer](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Fout: Kan geen premium-licentie om uit te voeren vanuit AD Graph 

Als u dit foutbericht opgetreden tijdens het openen van aanmeldingen met behulp van Graph Explorer, kies **wijzigingsmachtigingen** onder uw account in het navigatievenster aan de linkerkant en selecteer **Tasks.ReadWrite** en **Directory.Read.All**. 

![Machtigingen voor gebruikersinterface wijzigen](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Fout: Geen tenant is B2C of tenant heeft geen premium-licentie

Toegang tot rapporten-aanmelding vereist een Azure Active Directory premium 1 (P1) licentie. Als u dit foutbericht ziet tijdens het openen van aanmeldingen, zorg ervoor dat uw tenant in licentie met een Azure AD P1-licentie gegeven wordt.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Fout: Gebruiker is niet in de toegestane rollen 

Als u dit foutbericht ziet opgetreden tijdens het openen van de logboeken voor controle of aanmeldingen met behulp van de API, zorg ervoor dat uw account deel van uitmaakt de **Beveiligingslezer** of **rapportlezer** rol in uw Azure Active Directory tenant. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Fout: Toepassing AAD 'directory-gegevens lezen-machtiging ontbreekt 

Volg de stappen in de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) om te controleren of uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Fout: Toepassing MSGraph API 'Lezen alle logboekgegevens controleren' machtiging ontbreekt

Volg de stappen in de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) om te controleren of uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

## <a name="next-steps"></a>Volgende stappen

[Gebruik de API-verwijzing-controle](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[gebruikt u de aanmeldingsactiviteiten rapport API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)