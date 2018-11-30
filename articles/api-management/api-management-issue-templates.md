---
title: Uitgeven van sjablonen in Azure API Management | Microsoft Docs
description: Informatie over het aanpassen van de inhoud van de probleem-pagina's in de ontwikkelaarsportal in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: f099c27c55b817d6d9217a614ee66bf1d414a4dd
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446365"
---
# <a name="issue-templates-in-azure-api-management"></a>Probleem-sjablonen in Azure API Management
Met Azure API Management biedt u de mogelijkheid om aan te passen van de inhoud van ontwikkelaarsportalpagina's met behulp van een set van sjablonen die hun inhoud hebt geconfigureerd. Met behulp van [DotLiquid](http://dotliquidmarkup.org/) syntaxis en de editor van uw keuze, zoals [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), en een opgegeven set gelokaliseerde [tekenreeks resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), en [pagina besturingselementen](api-management-page-controls.md), hebt u geweldige flexibiliteit voor het configureren van de inhoud van de pagina's naar eigen inzicht met behulp van deze sjablonen.  
  
 De sjablonen in deze sectie kunnen u de inhoud van de probleem-pagina's in de ontwikkelaarsportal aanpassen.  
  
-   [Lijst met probleem](#IssueList)  
  
> [!NOTE]
>  Standaard-voorbeeldsjablonen zijn opgenomen in de volgende documentatie, maar kunnen worden gewijzigd vanwege de continue verbeteringen. U kunt de live standaardsjablonen weergeven in de portal voor ontwikkelaars door te navigeren naar de gewenste afzonderlijke sjablonen. Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="IssueList"></a> Lijst met probleem  
 De **probleem lijst** sjabloon kunt u de hoofdtekst van de pagina van de lijst met probleem in de ontwikkelaarsportal aanpassen.  
  
 ![Lijst-Ontwikkelaarsportal uitgeven](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM probleem lijst Developer-Portal")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
<div class="row">  
  <div class="col-md-9">  
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>  
  </div>  
</div>  
<div class="row">  
  <div class="col-md-12">  
    {% if issues.size > 0 %}  
    <ul class="list-unstyled">  
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}  
      {% assign replaceString0 = '{0}' %}  
      {% assign replaceString1 = '{1}' %}  
      {% for issue in issues %}  
      <li>  
        <h3>  
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>  
        </h3>  
        <p>{{issue.description}}</p>  
        <em>  
          {% capture state %}{{issue.issueState}}{% endcapture %}  
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}  
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}  
          {{ str1 | replace : replaceString1, devName }}  
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>  
        </em>  
      </li>  
      {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    {% if canReportIssue %}  
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>  
    {% elsif isAuthenticated %}  
    <hr />  
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>  
    {% else %}  
    <hr />  
    <p>  
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}  
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}  
      {{ signIntext | replace : replaceString0, link }}  
    </p>  
    {% endif %}  
  </div>  
</div>  
```  
  
### <a name="controls"></a>Besturingselementen  
 De `Issue list` sjabloon mogelijk gebruikt u de volgende [pagina besturingselementen](api-management-page-controls.md).  
  
-   [besturingselement voor paginering](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Gegevensmodel  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Problemen|Verzameling van [probleem](api-management-template-data-model-reference.md#Issue) entiteiten.|De problemen die zichtbaar is voor de huidige gebruiker.|  
|Zoekresultaten oproepen|[Voor het wisselbestand](api-management-template-data-model-reference.md#Paging) entiteit.|De informatie paginering voor de verzameling van toepassingen.|  
|IsAuthenticated|booleaans|Of de huidige gebruiker is aangemeld bij het ontwikkelaarsportal.|  
|CanReportIssues|booleaans|Bepaalt of de huidige gebruiker heeft machtigingen voor een probleem melden.|  
|Search|tekenreeks|Deze eigenschap is afgeschaft en mag niet worden gebruikt.|  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how to connect my application to the API",  
            "Description": "I'm having trouble connecting my application to the backend API.",  
            "SubscriptionDeveloperName": "Clayton",  
            "IssueState": "Proposed",  
            "ReportedOn": "2016-04-04T18:46:35.64",  
            "Comments": null,  
            "Attachments": null,  
            "Services": null  
        }  
    ],  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "IsAuthenticated": true,  
    "CanReportIssue": true,  
    "Search": null  
}  
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).