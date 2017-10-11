---
title: Azure Active Directory-App-registratie | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een toepassing met Azure Active Directory registreren met de Azure-portal
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 2f2817688beb2028fd0bba8522827d87a0097f21
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Uw toepassing registreren met uw Azure Active Directory-tenant

De Azure-portal kunt u uw toepassing registreren met uw Azure Active Directory (Azure AD)-tenant. Dit maakt een toepassings-ID voor de toepassing en ingeschakeld voor het ontvangen van tokens.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Uw account selecteren in de rechterbovenhoek van de pagina kiest u uw Azure AD-tenant.
3. Kies in het navigatiedeelvenster links **meer Services**, klikt u op **App registraties**, en klik op **toevoegen**.
4. Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. Als u specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen wilt, Bekijk onze [snelstartgidsen](active-directory-developers-guide.md).
  * Voor webtoepassingen, geeft u de **aanmeldings-URL**, dit is de basis-URL van uw app, waarbij kunnen gebruikers zich aanmelden bijvoorbeeld `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Voor systeemeigen toepassingen bieden een **omleidings-URI**, dat gebruikmaakt van Azure AD token antwoorden retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`.
5. Zodra u inschrijving hebt voltooid, wijst Azure AD van uw toepassing een unieke client-id, de toepassing-ID.

## <a name="update-application-settings-from-the-azure-portal"></a>Toepassingsinstellingen van de Azure-portal bijwerken

Instellingen van een bestaande toepassing met de Azure portal kunt u eenvoudig wijzigen. U wilt bijvoorbeeld een antwoord-URL is waar Azure AD geeft token antwoorden configureren. U kunt ook machtigingen voor andere toepassingen, bijvoorbeeld configureren om toe te staan van uw toepassing te krijgen tot de Microsoft Graph API. U kunt alle dit via de pagina met toepassingsinstellingen doen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Uw account selecteren in de rechterbovenhoek van de pagina kiest u uw Azure AD-tenant.
3. Kies in het navigatiedeelvenster links **meer Services**, klikt u op **App registraties**, en kies uw toepassing uit de lijst.
4. Klik op **instellingen** om de instellingenpagina voor de toepassing te openen.
  * De **eigenschappen** pagina kunt u de algemene informatie over de toepassing wijzigen. Dit omvat de toepassingsnaam, de aanmeldings-URL en de URL voor afmelden.
  * De **antwoord-URL's** pagina kunt u een antwoord-URL is waar Azure AD token antwoorden verzendt toevoegen.
  * De **eigenaars** pagina kunt u toepassingseigenaars toevoegen.
  * De **machtigingen** pagina kunt u machtigingen voor de app kunt configureren. Bijvoorbeeld voor toegang tot de Microsoft Graph-API, klikt u op **toevoegen** en selecteer **Microsoft Graph** Kies de machtiging is vereist, bijvoorbeeld in de API-selector **Directory-gegevens lezen**.
  * De **sleutels** pagina kunt u geheimen van de toepassing toevoegen. Het geheim wordt alleen weergegeven wanneer u onmiddellijk na het maken, verder in dat geval moet u ervoor zorgen te kopiëren voor gebruik.

## <a name="use-the-inline-manifest-editor"></a>De inline manifest-editor gebruiken

De inline-manifest editor kunt u bepaalde toepassingseigenschappen van de die niet worden weergegeven in de Azure-portal rechtstreeks wijzigen. Bijvoorbeeld, kunt u het wijzigen van de toepassing App ID URI of de impliciete stroom OAuth2.0 in plaats van de standaard grant-autorisatiecodestroom inschakelen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Uw account selecteren in de rechterbovenhoek van de pagina kiest u uw Azure AD-tenant.
3. Kies in het navigatiedeelvenster links **meer Services**, klikt u op **App registraties**, en kies uw toepassing uit de lijst.
4. Klik op **Manifest** vanaf de toepassingspagina om het manifest inline-editor te openen.
5. U kunt rechtstreeks wijzigingen aanbrengen in het manifest en sla deze wanneer u klaar bent. U kunt ook het manifest te openen in uw favoriete editor en het uploaden van het bijgewerkte manifest downloaden.

## <a name="next-steps"></a>Volgende stappen

1. Bekijk de [snelstartgidsen](active-directory-developers-guide.md) voor gedetailleerde scenario's van toepassingen die verificatie met behulp van Azure AD.
2. Bekijk onze volledige lijst van de codevoorbeelden in [GitHub](https://github.com/azure-samples).
