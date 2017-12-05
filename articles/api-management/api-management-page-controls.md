---
title: Azure API Management-paginabesturingselementen | Microsoft Docs
description: Meer informatie over de paginabesturingselementen beschikbaar voor gebruik in developer portal sjablonen in Azure API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: apimpm
ms.openlocfilehash: 4fd91ae079ff054932f4572874001dd69dd848e7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="azure-api-management-page-controls"></a>Azure API Management-paginabesturingselementen
Azure API Management biedt de volgende besturingselementen voor gebruik in het developer portal sjablonen.  
  
Plaats deze in de gewenste locatie in de portal developer-sjabloon voor het gebruik van een besturingselement. Sommige besturingselementen, zoals de [acties voor app](#app-actions) bepalen, parameters hebben, zoals wordt weergegeven in het volgende voorbeeld:  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 De waarden voor de parameters worden doorgegeven als onderdeel van het gegevensmodel voor de sjabloon. In de meeste gevallen kunt u gewoon plakken in het opgegeven voorbeeld voor elk besturingselement om correct te laten werken. Voor meer informatie over de parameterwaarden ziet u de sectie voor het model van gegevens voor elke sjabloon waarin een besturingselement kan worden gebruikt.  
  
 Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Paginabesturingselementen voor ontwikkelaars sjabloon  
  
-   [App-acties](#app-actions)  
-   [Basic-aanmelding](#basic-signin)  
-   [besturingselement voor paginering](#paging-control)  
-   [providers](#providers)  
-   [besturingselement voor zoeken](#search-control)  
-   [aanmelden](#sign-up)  
-   [abonneren knop](#subscribe-button)  
-   [abonnement annuleren](#subscription-cancel)  
  
##  <a name="app-actions"></a>App-acties  
 De `app-actions` beheer biedt een gebruikersinterface voor interactie met toepassingen op de pagina gebruikersprofiel in de portal voor ontwikkelaars.  
  
 ![App- &#45; acties besturingselement](./media/api-management-page-controls/APIM-app-actions-control.png "APIM acties voor app beheer")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parameters  
  
|Parameter|Beschrijving|  
|---------------|-----------------|  
|AppId|De id van de toepassing.|  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `app-actions` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Toepassingen](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>Basic-aanmelding  
 De `basic-signin` control biedt een besturingselement voor het verzamelen van aanmelden gebruikersgegevens in de aanmeldingspagina in de portal voor ontwikkelaars.  
  
 ![Basic &#45; signin-besturingselement](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic signin-besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `basic-signin` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Aanmelden](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>besturingselement voor paginering  
 De `paging-control` biedt pagineringsfunctionaliteit moet worden ingeschakeld op developer portal-pagina's die een lijst met items weergeven.  
  
 ![besturingselement van het wisselbestand](./media/api-management-page-controls/APIM-paging-control.png "APIM paginering besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `paging-control` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [API-lijst](api-management-api-templates.md#APIList)  
  
-   [Lijst met kwesties](api-management-issue-templates.md#IssueList)  
  
-   [Lijst met producten](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>providers  
 De `providers` beheer biedt een controle voor selectie van verificatieproviders op de pagina aanmelden in de portal voor ontwikkelaars.  
  
 ![providers besturingselement](./media/api-management-page-controls/APIM-providers-control.png "APIM providers besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `providers` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Aanmelden](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>besturingselement voor zoeken  
 De `search-control` biedt functionaliteit voor het zoeken op developer portal-pagina's die een lijst met items weergeven.  
  
 ![besturingselement zoeken](./media/api-management-page-controls/APIM-search-control.png "APIM zoekbesturing")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `search-control` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [API-lijst](api-management-api-templates.md#APIList)  
  
-   [Lijst met producten](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>aanmelden  
 De `sign-up` control biedt een besturingselement voor het verzamelen van informatie over gebruikersprofielen in de aanmeldingspagina in de portal voor ontwikkelaars.  
  
 ![aanmelding &#45; besturingselement up](./media/api-management-page-controls/APIM-sign-up-control.png "APIM aanmelding besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `sign-up` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Aanmelden](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>abonneren knop  
 De `subscribe-button` biedt een controle voor een gebruiker op een product abonneren.  
  
 ![abonneren &#45; knopbesturingselement](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abonneren knopbesturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `subscribe-button` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Product](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>abonnement annuleren  
 De `subscription-cancel` control biedt een besturingselement voor het annuleren van een abonnement op een product in de pagina gebruikersprofiel in de portal voor ontwikkelaars.  
  
 ![abonnement &#45; besturing annuleren](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM abonnement annuleren besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parameters  
  
|Parameter|Beschrijving|  
|---------------|-----------------|  
|subscriptionId|De id van het abonnement te annuleren.|  
|cancelUrl|Het abonnement wordt geannuleerd URL.|  
  
### <a name="developer-portal-templates"></a>Developer portal-sjablonen  
 De `subscription-cancel` besturingselement kan worden gebruikt in de volgende developer portal-sjablonen:  
  
-   [Product](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](api-management-developer-portal-templates.md).