---
title: Azure Active Directory-toestemmingsframework
description: Meer informatie over het toestemmingsframework in Azure Active Directory en hoe u het gemakkelijk om meerdere tenants web en native client-toepassingen te ontwikkelen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: zachowd, lenalepa, jesakowi
ms.custom: aaddev
ms.openlocfilehash: b01cd43d1c38a9db50937a6cff9b416b8c6acd0e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946473"
---
# <a name="azure-active-directory-consent-framework"></a>Azure Active Directory-toestemmingsframework

Het toestemmingsframework Azure Active Directory (Azure AD) kunt eenvoudig meerdere tenants web- en native clienttoepassingen ontwikkelen. Deze toepassingen kunnen aanmelden door gebruikersaccounts van een Azure AD-tenant die anders is dan het account waar de toepassing is geregistreerd. Ze kunnen ook nodig voor toegang tot web-API's, zoals de Microsoft Graph-API (voor Azure AD, Intune, en services in Office 365) en andere Microsoft-services-API's, naast uw eigen web-API's.

Het framework is gebaseerd op een gebruiker of beheerder toestemming verlenen tot een toepassing die wordt gevraagd om te worden geregistreerd in de directory, waarbij toegang tot directorygegevens. Bijvoorbeeld, als een web-clienttoepassing moet agenda om informatie te lezen over de gebruiker van Office 365, is die gebruiker vereist voor het eerst toestemming geven voor de clienttoepassing. Nadat toestemming is opgegeven, wordt de clienttoepassing kan de Microsoft Graph-API aanroepen namens de gebruiker en gebruik van de agenda-informatie, indien nodig worden. De [Microsoft Graph API](https://graph.microsoft.io) biedt toegang tot gegevens in Office 365 (zoals agenda's en berichten van Exchange, sites en -lijsten van SharePoint, OneDrive, van OneNote-notitieblokken, taken Planner en werkmappen van documenten Excel), evenals de gebruikers en groepen van Azure AD en andere objecten van meer Microsoft-cloudservices.

Het toestemmingsframework is gebouwd op OAuth 2.0 en de verschillende stromen, zoals code verlenen en de client referenties voor autorisatie verlenen, met behulp van openbare of vertrouwelijke clients. Met behulp van OAuth 2.0, maakt Azure AD het mogelijk te veel verschillende typen clienttoepassingen, zoals op een telefoon, tablet, server of een web-App bouwen en toegang krijgen tot de vereiste resources.

Zie voor meer informatie over het gebruik van het toestemmingsframework met OAuth 2.0-autorisatietoekenningen [toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure AD](v1-protocols-oauth-code.md) en [Verificatiescenario's voor Azure AD](authentication-scenarios.md). Zie voor informatie over het ophalen van geautoriseerde toegang tot Office 365 via Microsoft Graph [verificatie van apps met Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

## <a name="consent-experience---an-example"></a>Toestemming-ervaring - voorbeeld

De volgende stappen laten zien hoe de toestemming werkt ervaren voor zowel de ontwikkelaar van de toepassing en de gebruiker.

1. Stel dat u hebt een web-clienttoepassing die nodig zijn om aan te vragen van specifieke machtigingen voor toegang tot een resource/API. Leert u hoe u deze configuratie in de volgende sectie, maar in feite de Azure-portal wordt gebruikt om te declareren machtigingsaanvragen tijdens de configuratie. Net als andere configuratie-instellingen worden ze onderdeel van Azure AD-registratie van de toepassing:

  ![Machtigingen voor andere toepassingen](./media/quickstart-v1-integrate-apps-with-azure-ad/requiredpermissions.png)

1. Houd rekening met dat van uw toepassing machtigingen zijn bijgewerkt, de toepassing wordt uitgevoerd en een gebruiker is gebruikt voor de eerste keer. Eerst de toepassing moet een autorisatiecode ophalen uit Azure AD `/authorize` eindpunt. De autorisatiecode kan vervolgens worden gebruikt bij het aanschaffen van een nieuwe toegang en vernieuwen van het token.

1. Als de gebruiker nog niet is geverifieerd, Azure AD van `/authorize` eindpunt vraagt de gebruiker zich aanmeldt.

  ![Gebruiker of beheerder aanmelden bij Azure AD](./media/quickstart-v1-integrate-apps-with-azure-ad/usersignin.png)

1. Nadat de gebruiker is aangemeld, wordt Azure AD te bepalen of de gebruiker moet een instemmingspagina weergegeven. Deze beslissing is gebaseerd op of de gebruiker (of de beheerder van hun organisatie) heeft al verleend de toestemming van de toepassing. Als toestemming nog niet is gedaan, wordt Azure AD de gebruiker om toestemming wordt gevraagd en geeft de vereiste machtigingen die nodig is om te werken. De reeks machtigingen die worden weergegeven in het dialoogvenster overeenkomen met de die u hebt geselecteerd de **overgedragen machtigingen** in Azure portal.

  ![Gebruikerservaring voor toestemming](./media/quickstart-v1-integrate-apps-with-azure-ad/consent.png)

1. Nadat de gebruiker toestemming verleent, wordt een autorisatiecode voor uw toepassing, die wordt ingewisseld voor een toegangstoken verkrijgen en vernieuwen van het token geretourneerd. Zie voor meer informatie over deze stroom, [Web-API-app-type](web-api.md).

1. Als beheerder, kunt u ook toestemming voor gedelegeerde machtigingen namens alle gebruikers van een toepassing in uw tenant. Toestemming van een beheerder voorkomt u het dialoogvenster worden weergegeven voor elke gebruiker in de tenant en kan worden uitgevoerd in de [Azure-portal](https://portal.azure.com) door gebruikers met de beheerdersrol. Uit de **instellingen** -pagina voor uw toepassing, selecteert **vereiste machtigingen** en klik op de **machtigingen verlenen** knop.

  ![Machtigingen verlenen voor expliciete beheerderstoestemming](./media/quickstart-v1-integrate-apps-with-azure-ad/grantpermissions.png)

  > [!IMPORTANT]
  > Expliciete verleent toestemming met behulp van de **machtigingen verlenen** knop is momenteel vereist voor toepassingen met één pagina (SPA) die gebruikmaken van ADAL.js. Anders mislukt de toepassing wanneer het toegangstoken wordt aangevraagd.

## <a name="next-steps"></a>Volgende stappen

* Zie [het converteren van een app voor meerdere tenants worden](howto-convert-app-to-be-multi-tenant.md)
* Lees voor meer diepgang [hoe toestemming op het niveau van de OAuth 2.0-protocol wordt ondersteund tijdens de toewijzingsstroom voor autorisatie.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)
