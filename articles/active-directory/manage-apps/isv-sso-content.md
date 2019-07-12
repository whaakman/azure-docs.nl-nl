---
title: SSO inschakelen voor uw toepassing met meerdere tenants
description: Richtlijnen voor onafhankelijke softwareleveranciers over de integratie met Azure active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c89a83ade6305579e700afb86f0b9e3aca2695e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795186"
---
# <a name="enable-single-sign-on-for-your-multi-tenant-application"></a>Eenmalige aanmelding inschakelen voor uw toepassing met meerdere tenants  

Wanneer u uw toepassing voor gebruik door andere bedrijven via een aankoop of abonnement aanbieden, maakt u uw toepassing beschikbaar voor klanten binnen hun eigen Azure-tenants. Dit staat bekend als het maken van een toepassing met meerdere tenants. Zie voor een overzicht van dit concept, [Multitenant-toepassingen in Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications) en [Tenants in Azure Active Directory](../develop/single-and-multi-tenant-apps.md).

## <a name="what-is-single-sign-on"></a>Wat is eenmalige aanmelding

Eenmalige aanmelding (SSO) voegt beveiliging en gemak wanneer gebruikers zich aanmelden bij toepassingen met behulp van Azure Active Directory en andere identiteiten. Wanneer een toepassing eenmalige aanmelding ingeschakeld is, hebben gebruikers geen afzonderlijke referenties voor toegang tot deze toepassing in te voeren. Voor een volledige uitleg van eenmalige aanmelding. [Zie eenmalige aanmelding voor toepassingen in Azure Active Directory](what-is-single-sign-on.md).

## <a name="why-enable-single-sign-on-in-your-application"></a>Waarom eenmalige aanmelding in uw toepassing inschakelen?

Er zijn tal van voordelen op het inschakelen van eenmalige aanmelding in uw toepassing met meerdere tenants. Als u eenmalige aanmelding inschakelen voor uw toepassing:

* Uw toepassing kan worden weergegeven in de Azure Marketplace, waar uw app kunnen worden gedetecteerd door miljoenen organisaties met behulp van Azure Active Directory is.
  * Kunnen klanten snel configureren van de toepassing met Azure AD.

* Uw toepassing kan worden gedetecteerd in de galerie in Office 365-App, het startprogramma voor Office 365 en binnen Microsoft Search op Office.com

* Uw toepassing kan de Microsoft Graph REST-API gebruiken voor toegang tot de gegevens die de productiviteit van de gebruiker die beschikbaar is in de Microsoft Graph-stations.

* U verlagen ondersteuning door waardoor het gemakkelijker voor uw klanten.
  * Documentatie van toepassingsspecifieke coproduced met het team van Azure AD voor de acceptatie van onze gezamenlijke klanten versnellingen.
  * Als één muisklik SSO is ingeschakeld, hebt geen IT-beheerders voor uw klanten voor meer informatie over het configureren van uw toepassing voor gebruik in hun organisatie.

* U bieden uw klanten de mogelijkheid voor het beheren van alle verificatie en autorisatie van hun werknemers en gasten identiteiten.

  * Als u alle account-beheer en naleving verantwoordelijkheid met de eigenaar van de klant van deze identiteiten plaatst.

  * Bieden de mogelijkheid SSO voor specifieke id-providers, groepen of gebruikers om te voldoen aan de behoeften van uw bedrijf uit te schakelen.

* U verhogen uw liquiditeit en adoptability. Veel grote organisaties die is vereist (of aspire naar) hun werknemers beschikken over een naadloze ervaring voor eenmalige aanmelding voor alle toepassingen. Eenmalige aanmelding zodat u eenvoudig is het belangrijk.

* U verminderen door eindgebruikers moeilijkheden, die kan eindgebruikers gebruik verhogen en uw inkomsten te verhogen.

## <a name="how-to-enable-single-sign-on-in-your-published-application"></a>Het inschakelen van eenmalige aanmelding in uw gepubliceerde toepassing

1. [Kies de juiste federation-protocol voor uw toepassing met meerdere tenants](isv-choose-multi-tenant-federation.md).
1. Eenmalige aanmelding implementeren in uw toepassing
   - Zie [hulp bij het verificatiepatronen](../develop/v2-app-types.md)
   - Zie [Azure active Directory-codevoorbeelden](../develop/sample-v2-code.md) voor OIDC en OAuth-protocollen
1. [Uw Azure-Tenant maken](isv-tenant-multi-tenant-app.md) en uw toepassing testen
1. [Maken en publiceren van SSO-documentatie op uw site](isv-create-sso-documentation.md).
1. [De aanbieding van uw toepassing verzenden](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx) en werken samen met Microsoft om documentatie te maken op de site van Microsoft.
1. [Deelnemen aan het Microsoft Partner Network (gratis) en het maken van uw gebruik van een plan markt](https://partner.microsoft.com/en-us/explore/commercial#gtm).
