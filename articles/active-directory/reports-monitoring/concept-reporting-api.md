---
title: Aan de slag met de rapportage-API van Azure AD | Microsoft Docs
description: Aan de slag met de Azure Active Directory reporting API
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ff3e530dae3a6db4b7c84292a25e83c11000baf
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437761"
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

Bovendien kunt u de [risicogebeurtenissen Identity Protection API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) programmatische toegang tot beveiligingsdetecties met behulp van Microsoft Graph. Zie voor meer informatie, [aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](../identity-protection/graph-get-started.md). 

> [!NOTE]
>  De **https:\/\/domeinen graph.windows.net\/\<-tenantnaam\>\/rapporten\/**  eindpunt is afgeschaft. Gebruik de nieuwe API-eindpunten die hierboven worden beschreven programmatisch toegang verkrijgen tot de rapporten van activiteit en beveiliging.
  
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


