---
title: Aan de slag met de rapportage-API van Azure AD | Microsoft Docs
description: Aan de slag met de Azure Active Directory reporting API
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 860d602ecba257ed9015d1e080e5dcb1aa5ab872
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624621"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Aan de slag met de Azure Active Directory reporting API

Azure Active Directory biedt u tal van [rapporten](overview-reports.md), bevat nuttige informatie voor toepassingen zoals SIEM-systemen, audit en hulpprogramma's voor business intelligence. 

Met behulp van de Microsoft Graph-API voor Azure AD-rapporten, krijgt u programmatische toegang tot de gegevens via een set op REST gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

In dit artikel biedt een overzicht van de rapportage-API, ook over methoden om deze te openen.

Als u problemen ondervindt, raadpleegt u [over het verkrijgen van ondersteuning voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="prerequisites"></a>Vereisten

Voor toegang tot de rapportage-API, met of zonder tussenkomst van de gebruiker, moet u naar:

1. Toewijzen van rollen (Beveiligingslezer, Beveiligingsbeheerder, globale beheerder)
2. Een toepassing registreren
3. Machtigingen verlenen
4. Verzamelen van configuratie-instellingen

Zie voor gedetailleerde instructies, de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>API-eindpunten 

De Microsoft Graph API-eindpunt voor de logboeken voor controle is `https://graph.microsoft.com/beta/auditLogs/directoryAudits` en het Microsoft Graph API-eindpunt voor aanmeldingen `https://graph.microsoft.com/beta/auditLogs/signIns`. Zie voor meer informatie de [API controleverwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) en [aanmelding bij de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn).

## <a name="apis-with-graph-explorer"></a>API's met Graph Explorer

U kunt de [MSGraph explorer](https://developer.microsoft.com/graph/graph-explorer) Controleer uw aanmelding en analyseren van gegevens van de API. Zorg ervoor dat u zich aanmeldt bij uw account met behulp van de knoppen voor aanmelding bij in de gebruikersinterface van Graph Explorer en stel **AuditLog.Read.All** en **Directory.Read.All** machtigingen voor uw tenant, zoals wordt weergegeven.   

![Graph Explorer](./media/concept-reporting-api/graph-explorer.png)

![Machtigingen voor gebruikersinterface wijzigen](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Certificaten gebruiken voor toegang tot de rapportage-API van Azure AD 

Gebruik de Azure AD rapportage-API met certificaten als u van plan bent om op te halen van de rapportagegegevens zonder tussenkomst van de gebruiker.

Zie voor gedetailleerde instructies [ophalen van gegevens met behulp van de Reporting API van Azure AD met certificaten](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Volgende stappen

 * [Vereisten voor toegang tot de rapportage-API](howto-configure-prerequisites-for-reporting-api.md) 
 * [Ophalen van gegevens met behulp van de Reporting API van Azure AD met certificaten](tutorial-access-api-with-certificates.md)
 * [Fouten oplossen in Azure AD rapportage-API](troubleshoot-graph-api.md)


