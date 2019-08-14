---
title: Problemen oplossen in de API voor Azure Active Directory rapportage | Microsoft Docs
description: Biedt een oplossing voor fouten tijdens het aanroepen van Azure Active Directory rapportage-Api's.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6f1f34dcece9acb20d0db091152b24b26cb9fa2
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989537"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Problemen in Azure Active Directory rapportage-API oplossen

Dit artikel bevat een overzicht van de algemene fout berichten die u kunt uitvoeren tijdens het openen van activiteiten rapporten met behulp van de MS Graph API en stappen voor de oplossing.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 interne HTTP-server fout bij het openen van Microsoft Graph v2-eind punt

Het Microsoft Graph v2-eind punt wordt momenteel niet ondersteund. Zorg ervoor dat u toegang hebt tot de activiteiten logboeken met behulp van het Microsoft Graph v1-eind punt.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Fout: Kan de gebruikers rollen niet ophalen uit de AD-grafiek

Dit fout bericht kan worden weer gegeven wanneer u probeert toegang te krijgen tot aanmeldingen met Graph Explorer. Zorg ervoor dat u bent aangemeld bij uw account met behulp van beide aanmeld knoppen in de interface van Graph Verkenner, zoals wordt weer gegeven in de volgende afbeelding. 

![Graph Explorer](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Fout: Kan geen Premium-licentie controle uitvoeren vanuit de AD-grafiek 

Als u in dit fout bericht wordt weer gegeven terwijl u probeert toegang te krijgen tot aanmeldingen met behulp van Graph Explorer, kiest u **machtigingen voor wijzigen** onder uw account in het navigatie venster links en selecteert u **taken. readwrite** en **Directory. Read. all**. 

![Machtigingen voor gebruikers interface wijzigen](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Fout: Geen van de tenants is B2C of de Tenant heeft geen Premium-licentie

Voor toegang tot aanmeldings rapporten is een licentie voor Azure Active Directory Premium 1 (P1) vereist. Als dit fout bericht wordt weer gegeven tijdens het openen van de aanmeldingen, moet u ervoor zorgen dat uw Tenant is gelicentieerd met een Azure AD P1-licentie.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Fout: De gebruiker bevindt zich niet in de toegestane rollen 

Als dit fout bericht wordt weer gegeven tijdens het openen van controle Logboeken of-aanmeldingen met behulp van de API, moet u ervoor zorgen dat uw account deel uitmaakt van de rol **beveiligings lezer** of **rapport lezer** in uw Azure Active Directory Tenant. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Fout: De machtiging voor het lezen van mapgegevens van de toepassing AAD ontbreekt 

Volg de stappen in de [vereisten voor toegang tot de API voor Azure Active Directory rapportage](howto-configure-prerequisites-for-reporting-api.md) om ervoor te zorgen dat uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Fout: MSGraph-API voor ontbrekende toepassing alle controle logboek gegevens lezen

Volg de stappen in de [vereisten voor toegang tot de API voor Azure Active Directory rapportage](howto-configure-prerequisites-for-reporting-api.md) om ervoor te zorgen dat uw toepassing wordt uitgevoerd met de juiste set machtigingen. 

## <a name="next-steps"></a>Volgende stappen

[Gebruik de controle-API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[gebruiken de aanmeld activiteit rapport API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
