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
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7de7c87e5cf1747f4899f33d9e6b9cbf0bb430e1
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060584"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Aan de slag met de Azure Active Directory reporting API

Azure Active Directory biedt u tal van [rapporten](overview-reports.md). De gegevens van deze rapporten kunnen zeer nuttig zijn voor uw toepassingen, zoals SIEM-systemen, audit- en business intelligence-hulpprogramma's. 

Met behulp van de rapportage-API van Azure AD, krijgt u programmatische toegang tot de gegevens via een set op REST gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

Dit artikel bevat een roadmap voor toegang tot de rapportagegegevens met behulp van de gerelateerde API.

Als u problemen ondervindt, raadpleegt u [over het verkrijgen van ondersteuning voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Vereisten

Voor toegang tot de rapportage-API, zelfs als u van plan bent over de toegang tot de API met behulp van een script, moet u naar:

1. Toewijzen van rollen (Beveiligingslezer, Beveiligingsbeheerder, globale beheerder)
2. Een toepassing registreren
3. Machtigingen verlenen
4. Verzamelen van configuratie-instellingen

Zie voor gedetailleerde instructies, de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md).

## <a name="apis-with-graph-explorer"></a>API's met Graph Explorer

U kunt de [MSGraph explorer](https://developer.microsoft.com/graph/graph-explorer) Controleer uw aanmelding en analyseren van gegevens van de API. Zorg ervoor dat u zich aanmeldt bij uw account met behulp van de knoppen voor aanmelding bij in de gebruikersinterface van Graph Explorer en stel **AuditLog.Read.All** en **Directory.Read.All** machtigingen voor uw tenant, zoals wordt weergegeven.   

![Graph Explorer](./media/concept-reporting-api/graph-explorer.png)

![Machtigingen voor gebruikersinterface wijzigen](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Certificaten gebruiken voor toegang tot de rapportage-API van Azure AD 

Gebruik de Azure AD rapportage-API met certificaten als u van plan bent om op te halen van de rapportagegegevens zonder tussenkomst van de gebruiker.

Zie voor gedetailleerde instructies [ophalen van gegevens met behulp van de Reporting API van Azure AD met certificaten](tutorial-access-api-with-certificates.md).


## <a name="next-steps"></a>Volgende stappen

 * [Controle van de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
 * [Rapport van aanmeldingsactiviteiten API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
 * [Fouten oplossen in Azure AD rapportage-API](troubleshoot-graph-api.md)


