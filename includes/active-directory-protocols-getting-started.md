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
ms.openlocfilehash: 5a4f08718175dd7ef369d801807e6c8e1ff28fd9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39485583"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uw toepassing registreren bij uw AD-tenant
Eerst moet u uw toepassing registreren bij uw tenant Azure Active Directory (Azure AD). Na registratie beschikt u over een toepassings-id voor uw toepassing en kan uw toepassing tokens ontvangen.

* Meld u aan bij [Azure Portal](https://portal.azure.com).
* Kies uw Azure AD-tenant door te klikken op uw account in de rechterbovenhoek van de pagina, gevolgd door te klikken op de **schakelen tussen mappen** navigatie en selecteer vervolgens de juiste tenant. 
  * Deze stap overslaan als u slechts één Azure AD-tenant voor uw account hebt of als u al hebt geselecteerd de juiste Azure AD-tenant.
* Klik in het linkernavigatievenster op **Azure Active Directory**.
* Klik op **App-registraties** en klikt u op **nieuwe toepassing registreren**.
* Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Dit kan een webtoepassing of een systeemeigen toepassing zijn. Voor deze zelfstudie maakt dat geen verschil. Als u behoefte hebt aan specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, raadpleegt u onze [snelstartgidsen](../articles/active-directory/develop/azure-ad-developers-guide.md).
  * Voor webtoepassingen, geeft u de **aanmeldings-URL**, dit is de basis-URL van uw app, waar gebruikers zich kunnen in bijvoorbeeld `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Voor systeemeigen toepassingen bieden een **omleidings-URI**, die Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`.
* Als u de registratie hebt voltooid, toewijzen Azure AD uw toepassing een unieke client-id, de **toepassings-ID**. U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
* Uw toepassing in Azure portal, klik op **App-registraties**, en klik vervolgens op **alle toepassingen weergeven**.
