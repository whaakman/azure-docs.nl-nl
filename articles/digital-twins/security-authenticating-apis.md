---
title: Meer informatie over Azure Digital Apparaatdubbels API-verificatie | Microsoft Docs
description: Azure Digital Apparaatdubbels gebruiken om verbinding te maken en te verifiëren met Api's
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: lyhughes
ms.openlocfilehash: c0b4b6a13143f613bec64c8507f1726e2450be44
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815544"
---
# <a name="connect-and-authenticate-to-apis"></a>Verbinding maken met Api's en deze verifiëren

Azure Digital Apparaatdubbels maakt gebruik van Azure Active Directory (Azure AD) om gebruikers te verifiëren en toepassingen te beveiligen. Azure AD biedt ondersteuning voor verificatie voor verschillende moderne architecturen. Ze zijn allemaal gebaseerd op de industrie standaard protocollen OAuth 2,0 of OpenID Connect Connect. Daarnaast kunnen ontwikkel aars Azure AD gebruiken om single tenant-en LOB-toepassingen (line-of-Business) te bouwen. Ontwikkel aars kunnen ook Azure AD gebruiken voor het ontwikkelen van multi tenant-toepassingen.

Ga voor een overzicht van Azure AD naar de [pagina fundamentals](https://docs.microsoft.com/azure/active-directory/fundamentals/index) voor stapsgewijze hand leidingen, concepten en Quick starts.

Om een toepassing of service met Azure Active Directory te integreren, moet een ontwikkelaar de toepassing eerst bij Azure Active Directory registreren. Zie voor gedetailleerde instructies en scherm afbeeldingen [deze Snelstartgids](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Er worden [vijf primaire toepassings scenario's](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) ondersteund door Azure AD:

* Toepassing met één pagina (SPA): Een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD.
* Webbrowser voor webtoepassing: Een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD.
* Systeem eigen toepassing op Web-API: Een systeem eigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker verifiëren om bronnen te verkrijgen van een web-API die wordt beveiligd door Azure AD.
* Webtoepassing voor web-API: Een webtoepassing moet resources ophalen van een web-API die wordt beveiligd door Azure AD.
* Daemon of server toepassing op Web-API: Een daemon-toepassing of een server toepassing zonder webgebruikersinterface moet resources ophalen van een web-API die wordt beveiligd door Azure AD.

De Windows Azure-verificatie bibliotheek biedt een groot aantal manieren om Active Directory-tokens te verkrijgen. Raadpleeg [dit artikel](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki)voor meer informatie over de bibliotheek en code voorbeelden.

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Digital Apparaatdubbels aanroepen vanuit een web-API in de middelste laag

Wanneer ontwikkel aars oplossingen voor digitale Apparaatdubbels ontwikkelen, maken ze doorgaans een toepassing op middelste laag of API. De app of API roept vervolgens de Digital Apparaatdubbels API downstream aan. Als u deze standaard architectuur voor weboplossing wilt ondersteunen, moet u ervoor zorgen dat gebruikers eerst het volgende doen:

1. Verifiëren met de toepassing voor middelste laag

1. Er is een OAuth 2,0--of-token voor het verkrijgen van validatie tijdens verificatie

1. Het aangeschafte token wordt vervolgens gebruikt voor verificatie met of aanroepen van Api's die verder downstream worden gebruikt met behulp van de namens-stroom

Zie [OAuth 2,0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)namens stroom voor instructies over het indelen van de namens-stroom. U kunt ook code voorbeelden bekijken in [het aanroepen van een stroomafwaartse Web-API](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="next-steps"></a>Volgende stappen

Lees [Configure postman](./how-to-configure-postman.md)om Azure Digital apparaatdubbels te configureren en te testen met behulp van de OAuth 2,0 impliciete toekennings stroom.

Voor meer informatie over de beveiliging van Azure Digital Apparaatdubbels leest u [roltoewijzingen maken en beheren](./security-create-manage-role-assignments.md).