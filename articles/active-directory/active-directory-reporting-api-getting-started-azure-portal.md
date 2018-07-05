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
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 93532f4b0b2d527a4d5c79e2ee1b2810394b2f11
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442080"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Aan de slag met de Azure Active Directory reporting API

Azure Active Directory biedt u tal van [rapporten](active-directory-reporting-azure-portal.md). De gegevens van deze rapporten kunnen zeer nuttig zijn voor uw toepassingen, zoals SIEM-systemen, audit- en business intelligence-hulpprogramma's. 

Met behulp van de rapportage-API van Azure AD, krijgt u programmatische toegang tot de gegevens via een set op REST gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

Dit artikel bevat een roadmap voor toegang tot de rapportagegegevens met behulp van de gerelateerde API.

Als u problemen ondervindt, raadpleegt u [over het verkrijgen van ondersteuning voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Vereisten

Voor toegang tot de rapportage-API, zelfs als u van plan bent over de toegang tot de API met behulp van een script, moet u naar:

1. Toewijzen van rollen (Beveiligingslezer, Beveiligingsbeheerder, globale beheerder)
2. Een toepassing registreren
3. Machtigingen verlenen
4. Verzamelen van configuratie-instellingen


 
Zie voor gedetailleerde instructies, de [vereisten voor toegang tot de Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).

## <a name="apis-with-graph-explorer"></a>API's met Graph Explorer

U kunt de [MSGraph explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) Controleer uw aanmelding en analyseren van gegevens van de API. Zorg ervoor dat u zich aanmeldt bij uw account met behulp van de knoppen voor aanmelding bij in de gebruikersinterface van Graph Explorer en stel **Tasks.ReadWrite** en **Directory.ReadAll** machtigingen voor uw tenant, zoals wordt weergegeven.   

![Graph Explorer](./media/active-directory-reporting-api-getting-started-azure-portal/graph-explorer.png)

![Machtigingen voor gebruikersinterface wijzigen](./media/active-directory-reporting-api-getting-started-azure-portal/modify-permissions.png)

## <a name="recommendation"></a>Aanbeveling 

Als u van plan bent over het ophalen van rapportgegevens zonder tussenkomst van de gebruiker, moet u overwegen de Reporting API van Azure AD met certificaten.

Zie voor gedetailleerde instructies [ophalen van gegevens met behulp van de Reporting API van Azure AD met certificaten](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Verkennen

Haal een eerste indruk van de rapportage-API's:
   
   - [Met behulp van de voorbeelden voor de API-controle](active-directory-reporting-api-audit-samples.md) 
 
   - [Met behulp van de voorbeelden voor het rapport van aanmeldingsactiviteiten API](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Aanpassen  

Maak uw eigen oplossing: 
   
   - [Met behulp van de audit API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Met behulp van de aanmeldingsactiviteiten rapport API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



