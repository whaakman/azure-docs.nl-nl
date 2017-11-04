---
title: Overzicht van .NET-protocol voor Azure AD | Microsoft Docs
description: HTTP-berichten gebruiken om met behulp van Azure AD toegang toe te staan tot webtoepassingen en web-API's in uw tenant.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 42ce8dfd30cda7d4085778954350550fd9fdf13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
## Uw toepassing registreren bij uw AD-tenant
U moet eerst uw toepassing registreren bij uw AD-tenant (Azure Active Directory-tenant). Na registratie beschikt u over een toepassings-id voor uw toepassing en kan uw toepassing tokens ontvangen.

* Meld u aan bij [Azure Portal](https://portal.azure.com).
* Selecteer de gewenste Azure AD-tenant door rechtsboven op de pagina op uw account te klikken.
* Klik in het linkernavigatievenster op **Azure Active Directory**.
* Klik op **App-registraties** en vervolgens op **Toevoegen**.
* Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Dit kan een webtoepassing of een systeemeigen toepassing zijn. Voor deze zelfstudie maakt dat geen verschil. Als u behoefte hebt aan specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, raadpleegt u onze [snelstartgidsen](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Als u een webtoepassing maakt, geeft u de **aanmeldings-URL** op. Dit is de basis-URL van uw toepassing, waar gebruikers zich kunnen aanmelden. Bijvoorbeeld: `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Als u een systeemeigen toepassing maakt, geeft u een **omleidings-URI** op. Deze wordt door Azure AD gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`.
* Nadat u de registratie hebt voltooid, wijst Azure AD aan uw toepassing een unieke client-id toe. Dit is de toepassings-id. U hebt de toepassings-id in de volgende gedeelten nodig. Kopieer deze waarde daarom vanaf de toepassingspagina.
