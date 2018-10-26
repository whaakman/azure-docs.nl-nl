---
title: Toestemming van de gebruiker aan een toepassing - Azure Active Directory configureren | Microsoft Docs
description: Informatie over het beheren van de manier waarop gebruikers toestemming voor machtigingen van de toepassing geven. Verleent toestemming van een beheerder kunt u de gebruikerservaring vereenvoudigen. Deze methoden zijn van toepassing op alle gebruikers in uw tenant Azure Active Directory (Azure AD).
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: 25f5d79b900cf15a9bdcb8f037fa95494c63b7e1
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088833"
---
# <a name="configure-the-way-end-users-consent-to-an-application-in-azure-active-directory"></a>De manier waarop eindgebruikers toestemming voor een toepassing in Azure Active Directory geven configureren
Informatie over het configureren van de manier waarop gebruikers toestemming voor machtigingen van de toepassing geven. Verleent toestemming van een beheerder kunt u de gebruikerservaring vereenvoudigen. Dit artikel bevat de verschillende manieren waarop die u toestemming van de gebruiker kunt configureren. De methoden van toepassing op alle gebruikers in uw tenant Azure Active Directory (Azure AD). 

Zie voor meer informatie over het stemt ermee in dat toepassingen [Azure Active Directory-toestemmingsframework](../develop/consent-framework.md).

## <a name="prerequisites"></a>Vereisten

Verlenen van toestemming van een beheerder, moet u zich aanmeldt als hoofdbeheerder, een beheerder van de toepassing of een beheerder van de cloudtoepassing.

## <a name="grant-admin-consent-to-enterprise-apps-in-the-azure-portal"></a>Beheerder toestemming voor enterprise-apps in Azure portal

Beheerderstoestemming voor een zakelijke app verlenen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een globale beheerder, toepassingsbeheerder of een beheerder van de cloudtoepassing.
2. Klik op **alle services** boven aan het navigatiemenu aan. De **Azure Active Directory-extensie** wordt geopend.
3. Typ in het zoekvak filter **'Azure Active Directory'** en selecteer de **Azure Active Directory** item.
4. Klik in het navigatiemenu op **bedrijfstoepassingen**.
5. Klik op **verlenen Beheerderstoestemming**. U wordt gevraagd zich aanmeldt bij het beheren van de toepassing.
6. Meld u aan met een account met machtigingen voor het verlenen van toestemming van een beheerder voor de toepassing. 
7. Instemming met de machtigingen van de toepassing.

Deze optie werkt alleen als de toepassing is: 

- Geregistreerd in uw tenant, of
- Geregistreerd in een andere Azure AD-tenant en toegestaan door ten minste één gebruiker. Wanneer een gebruiker heeft ingestemd met een toepassing, Azure AD een lijst met de toepassing onder **bedrijfsapps** in Azure portal.

## <a name="grant-admin-consent-when-registering-an-app-in-the-azure-portal"></a>Verlenen van toestemming van een beheerder bij het registreren van een app in Azure portal

Beheerderstoestemming verlenen bij het registreren van een app: 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Navigeer naar de **App-registraties** blade.
3. Selecteer de toepassing om de toestemming.
4. Selecteer **vereiste machtigingen**.
5. Klik op **machtigingen verlenen** aan de bovenkant van de blade.


## <a name="grant-admin-consent-through-a-url-request"></a>Verlenen van toestemming van een beheerder via een URL-aanvraag

Verlenen van toestemming van een beheerder via een URL-aanvraag:

1. Een aanvraag voor het samenstellen *login.microsoftonline.com* met uw app-configuraties en toe te voegen op `&prompt=admin_consent`. 
2. Na het aanmelden met beheerdersreferenties, heeft toestemming voor alle gebruikers op de app zijn verleend.


## <a name="force-user-consent-through-a-url-request"></a>Toestemming van de gebruiker via een URL-aanvraag afdwingen

Om te vereisen dat eindgebruikers toestemming geven voor een toepassing telkens wanneer ze worden geverifieerd, append `&prompt=consent` verificatie aanvraag-URL.

## <a name="next-steps"></a>Volgende stappen

[Toestemming en het integreren van Apps aan AzureAD](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

[De toestemming en rollen voor AzureAD v2.0 geconvergeerde Apps](../develop/active-directory-v2-scopes.md)

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
