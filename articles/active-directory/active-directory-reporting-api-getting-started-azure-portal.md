---
title: Aan de slag met Azure AD rapportage-API | Microsoft Docs
description: Hoe u aan de slag met de Azure Active Directory rapportage-API
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: df0b672a07575a0d26fff89c90008043d02818dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589097"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Aan de slag met de Azure Active Directory rapportage-API

Azure Active Directory biedt u tal van [rapporten](active-directory-reporting-azure-portal.md). De gegevens van deze rapporten kunnen zeer nuttig zijn voor uw toepassingen, zoals SIEM-systemen, audit- en business intelligence-hulpprogramma's. 

Met behulp van de Azure AD rapportage-API u programmatische toegang krijgen tot de gegevens via een set op basis van REST-API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

Dit artikel bevat een routekaart voor toegang tot de rapportagegegevens met de bijbehorende API.

Als u problemen ondervindt, raadpleegt u [ondersteuning voor Azure Active Directory krijgen](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Vereisten

Voor toegang tot de rapportage-API, zelfs als u van plan bent over de toegang tot de API met behulp van een script, moet u:

1. Toewijzen van rollen (Security Reader beveiliging Admin, globale beheerder)
2. Een toepassing registreren
3. Machtigingen verlenen
4. Verzamelen van configuratie-instellingen


 
Zie voor gedetailleerde instructies de [vereisten voor toegang tot de Azure Active Directory-rapportage API](active-directory-reporting-api-prerequisites-azure-portal.md).


## <a name="recommendation"></a>Aanbeveling 

Als u van plan bent op bij het ophalen van rapportgegevens zonder tussenkomst van de gebruiker, moet u rekening houden met behulp van de Azure AD rapportage-API met certificaten.

Zie voor gedetailleerde instructies [ophalen van gegevens met behulp van de Azure AD rapportage-API met certificaten](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Verkennen

Haal een eerste indruk van de rapportage-API's:
   
   - [Met behulp van de voorbeelden voor de API-controle](active-directory-reporting-api-audit-samples.md) 
 
   - [Met behulp van de voorbeelden voor het rapport aanmeldingsactiviteiten API](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Aanpassen  

Maak uw eigen oplossing: 
   
   - [Met behulp van de audit API-referentiemateriaal](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Met behulp van de aanmeldingsactiviteiten rapport API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



