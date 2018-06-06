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
ms.openlocfilehash: 72fb594265e69eb1dc16cb29ad4df6acb3a87720
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34663624"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uw toepassing registreren bij uw AD-tenant
Eerst moet u uw toepassing registreren met uw Azure Active Directory (Azure AD)-tenant. Na registratie beschikt u over een toepassings-id voor uw toepassing en kan uw toepassing tokens ontvangen.

* Meld u aan bij [Azure Portal](https://portal.azure.com).
* Selecteer de gewenste Azure AD-tenant door rechtsboven op de pagina op uw account te klikken.
* Klik in het linkernavigatievenster op **Azure Active Directory**.
* Klik op **App registraties** en klik op **registratie van de nieuwe toepassing**.
* Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Dit kan een webtoepassing of een systeemeigen toepassing zijn. Voor deze zelfstudie maakt dat geen verschil. Als u behoefte hebt aan specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, raadpleegt u onze [snelstartgidsen](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Voor webtoepassingen, geeft u de **aanmeldings-URL**, dit is de basis-URL van uw app, waarbij kunnen gebruikers zich aanmelden bijvoorbeeld `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Voor systeemeigen toepassingen bieden een **omleidings-URI**, Azure AD worden gebruikt bij het token antwoorden retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`.
* Zodra u inschrijving hebt voltooid, toewijzen Azure AD uw toepassing een unieke client-id, de **toepassings-ID**. U moet deze waarde in de volgende secties, dus kopiëren van de toepassingspagina.
* Voor uw toepassing in de Azure portal, klikt u **App registraties**, en klik vervolgens op **weergeven van alle toepassingen**.