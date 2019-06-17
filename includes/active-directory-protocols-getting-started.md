---
title: Overzicht van .NET-protocol voor Azure AD | Microsoft Docs
description: HTTP-berichten gebruiken om met behulp van Azure AD toegang toe te staan tot webtoepassingen en web-API's in uw tenant.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2019
ms.author: priyamo
ms.openlocfilehash: b6dd4cd55755ae2c92afd327ad72ffe6966b9a07
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121520"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uw toepassing registreren bij uw AD-tenant
Eerst moet u uw toepassing registreren bij uw tenant Azure Active Directory (Azure AD). Na registratie beschikt u over een toepassings-id voor uw toepassing en kan uw toepassing tokens ontvangen.

* Meld u aan bij [Azure Portal](https://portal.azure.com).
* Kies uw Azure AD-tenant door te klikken op uw account in de rechterbovenhoek van de pagina, gevolgd door te klikken op de **schakelen tussen mappen** navigatie en selecteer vervolgens de juiste tenant. 
  * Sla deze stap over als u maar één Azure tenant in uw account hebt, of als u al de juiste Azure Active Directory tenant hebt gekozen.
* Klik in het linkernavigatievenster op **Azure Active Directory**.
* Klik op **App-registraties** en klikt u op **registratie van nieuwe**.
* Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Het wordt niet als het een webtoepassing of een openbare client (mobiele en desktop)-toepassing voor deze zelfstudie is van belang, maar als u specifieke voorbeelden voor webtoepassingen of openbare clienttoepassingen, Bekijk onze [snelstartgidsen](../articles/active-directory/develop/v1-overview.md).
  * **Naam** is de naam van de toepassing en beschrijft de toepassing voor eindgebruikers.
  * Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
  * Geef de **omleidings-URI**. Voor webtoepassingen is dit de basis-URL van uw app waar gebruikers zich kunnen aanmelden.  Bijvoorbeeld `http://localhost:12345`. Voor openbare-client (mobiele en desktop), Azure AD gebruikt om tokenantwoorden te retourneren. Voer een specifieke waarde voor uw toepassing.  Bijvoorbeeld `http://MyFirstAADApp`.
    <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
* Als u de registratie hebt voltooid, toewijzen Azure AD uw toepassing een unieke client-id (de **toepassings-ID**). U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
* Uw toepassing in Azure portal, klik op **App-registraties**, en klik vervolgens op **alle toepassingen weergeven**.
