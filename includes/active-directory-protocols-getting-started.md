---
title: Overzicht van .NET-protocol voor Azure AD | Microsoft Docs
description: HTTP-berichten gebruiken om met behulp van Azure AD toegang toe te staan tot webtoepassingen en web-API's in uw tenant.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: cb31bb91c80e4d5dd032b009b40d8e3fc435e0c8
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359456"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uw toepassing registreren bij uw AD-tenant
Eerst moet u uw toepassing registreren bij uw tenant Azure Active Directory (Azure AD). Na registratie beschikt u over een toepassings-id voor uw toepassing en kan uw toepassing tokens ontvangen.

* Meld u aan bij [Azure Portal](https://portal.azure.com).
* Kies uw Azure AD-tenant door te klikken op uw account in de rechterbovenhoek van de pagina, gevolgd door te klikken op de **schakelen tussen mappen** navigatie en selecteer vervolgens de juiste tenant. 
  * Deze stap overslaan als u slechts één Azure AD-tenant voor uw account hebt of als u al hebt geselecteerd de juiste Azure AD-tenant.
* Klik in het linkernavigatievenster op **Azure Active Directory**.
* Klik op **App-registraties** en klikt u op **nieuwe toepassing registreren**.
* Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Dit kan een webtoepassing of een systeemeigen toepassing zijn. Voor deze zelfstudie maakt dat geen verschil. Als u behoefte hebt aan specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, raadpleegt u onze [snelstartgidsen](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Voor webtoepassingen, geeft u de **aanmeldings-URL**, dit is de basis-URL van uw app, waar gebruikers zich kunnen in bijvoorbeeld `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Voor systeemeigen toepassingen bieden een **omleidings-URI**, die Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`.
* Als u de registratie hebt voltooid, toewijzen Azure AD uw toepassing een unieke client-id, de **toepassings-ID**. U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
* Uw toepassing in Azure portal, klik op **App-registraties**, en klik vervolgens op **alle toepassingen weergeven**.
