---
title: Azure API Management-paginabesturingselementen | Microsoft Docs
description: Meer informatie over de paginabesturingselementen beschikbaar voor gebruik in sjablonen voor ontwikkelaarsportals in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: apimpm
ms.openlocfilehash: d87293d89e4009512494bf47f9742ea5901f909a
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445359"
---
# <a name="azure-api-management-page-controls"></a>Azure API Management-pagina controles
Azure API Management levert de volgende besturingselementen voor gebruik in de developer portal-sjablonen.  
  
Voor het gebruik van een besturingselement, plaatst u het in de gewenste locatie in de sjabloon van developer-portal. Sommige besturingselementen, zoals de [acties voor app](#app-actions) toe, parameters hebben, zoals wordt weergegeven in het volgende voorbeeld:  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 De waarden voor de parameters worden doorgegeven als onderdeel van het gegevensmodel voor de sjabloon. In de meeste gevallen kunt u gewoon plakken in het opgegeven voorbeeld voor elk besturingselement voor het correct te laten werken. Voor meer informatie over de parameterwaarden ziet u de sectie voor het model van gegevens voor elke sjabloon waarin een besturingselement kan worden gebruikt.  
  
 Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
## <a name="developer-portal-template-page-controls"></a>Developer portal sjabloon paginabesturingselementen  
  
-   [App-acties](#app-actions)  
-   [Basic-aanmelding](#basic-signin)  
-   [besturingselement voor paginering](#paging-control)  
-   [Providers](#providers)  
-   [besturingselement voor zoeken](#search-control)  
-   [Meld u aan](#sign-up)  
-   [abonnement-knop](#subscribe-button)  
-   [abonnement annuleren](#subscription-cancel)  
  
##  <a name="app-actions"></a> App-acties  
 De `app-actions` controle biedt een gebruikersinterface voor interactie met toepassingen op de gebruikersprofielpagina in de portal voor ontwikkelaars.  
  
 ![App&#45;acties besturingselement](./media/api-management-page-controls/APIM-app-actions-control.png "APIM-acties voor app-beheer")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parameters  
  
|Parameter|Beschrijving|  
|---------------|-----------------|  
|appId|De id van de toepassing.|  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `app-actions` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Toepassingen](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a> Basic-aanmelding  
 De `basic-signin` controle biedt een besturingselement voor het verzamelen van aanmelding bij gebruikersgegevens op de pagina aanmelden in de portal voor ontwikkelaars.  
  
 ![Basic&#45;aanmelding besturingselement](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `basic-signin` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Aanmelden](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a> besturingselement voor paginering  
 De `paging-control` biedt wisselbestand-functionaliteit op developer portal-pagina's die een lijst met items weergeven.  
  
 ![besturingselement voor het wisselbestand](./media/api-management-page-controls/APIM-paging-control.png "APIM-besturingselement voor paginering")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `paging-control` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [API-lijst](api-management-api-templates.md#APIList)  
  
-   [Lijst met probleem](api-management-issue-templates.md#IssueList)  
  
-   [Lijst met producten](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a> Providers  
 De `providers` controle biedt een besturingselement voor de selectie van verificatieproviders op de pagina aanmelden in de portal voor ontwikkelaars.  
  
 ![providers besturingselement](./media/api-management-page-controls/APIM-providers-control.png "APIM-providers beheren")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `providers` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Aanmelden](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a> besturingselement voor zoeken  
 De `search-control` biedt functionaliteit voor het zoeken op developer portal-pagina's die een lijst met items weergeven.  
  
 ![besturingselement voor zoeken](./media/api-management-page-controls/APIM-search-control.png "APIM-besturingselement voor zoeken")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `search-control` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [API-lijst](api-management-api-templates.md#APIList)  
  
-   [Lijst met producten](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a> Meld u aan  
 De `sign-up` controle biedt een besturingselement voor het verzamelen van informatie over gebruikersprofielen in de pagina voor het registreren in de portal voor ontwikkelaars.  
  
 ![Meld u&#45;omhoog besturingselement](./media/api-management-page-controls/APIM-sign-up-control.png "aanmelding APIM-besturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `sign-up` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Aanmelden](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a> abonnement-knop  
 De `subscribe-button` biedt een besturingselement voor een gebruiker aan een product abonneren.  
  
 ![abonneren&#45;besturingselement voor knop](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abonneren knopbesturingselement")  
  
### <a name="usage"></a>Gebruik  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parameters  
 Geen.  
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `subscribe-button` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Product](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a> abonnement annuleren  
 De `subscription-cancel` controle biedt een besturingselement voor het annuleren van een abonnement op een product in de gebruikersprofielpagina in de portal voor ontwikkelaars.  
  
 ![abonnement&#45;besturing annuleren](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM-abonnement annuleren besturingselement")  
  
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
  
### <a name="developer-portal-templates"></a>Sjablonen voor ontwikkelaarsportals  
 De `subscription-cancel` beheer kan worden gebruikt in de volgende sjablonen van de developer-portal:  
  
-   [Product](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).