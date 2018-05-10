---
title: Azure Active Directory-audit API-referentiemateriaal | Microsoft Docs
description: Hoe u aan de slag met de Azure Active Directory-audit-API
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 1bf86a9190039cdf0fe8dc435bdee4308b28cf29
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory-audit API-referentiemateriaal

> [!TIP] 
> Bekijk de nieuwe Microsoft Graph-API voor [reporting](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit), die deze API uiteindelijk wordt vervangen. 


In dit artikel maakt deel uit van een verzameling artikelen over Azure Active Directory (Azure AD) rapportage-API. Rapportage van Azure AD biedt u een API waarmee u toegang krijgt tot controlegegevens met code of gerelateerde hulpprogramma's.
Het bereik van dit artikel is om u te bieden informatie over de **audit API**.

Zie:

* [Controlelogboeken](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie

* [Aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.


Voor:

- Veelgestelde vragen, lees de [Veelgestelde vragen](active-directory-reporting-faq.md) 

- Problemen [een ondersteuningsticket bestand](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
* Gebruikers met de rol Beveiligingsbeheerder of Beveiligingslezer
* Globale beheerders
* Elke app die autorisatie is voor toegang tot de API (app autorisatie kan worden ingesteld alleen op basis van toestemming van de globale beheerder)

## <a name="prerequisites"></a>Vereisten
Voor toegang tot dit rapport via de rapportage-API, moet u het volgende hebben:

* Een [Azure Active Directory gratis of betere edition](active-directory-editions.md)
* Voltooid de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Toegang tot de API
U kunt de toegang tot deze API via de [grafiek Explorer](https://graphexplorer2.cloudapp.net) of programmatisch met bijvoorbeeld PowerShell. Gebruik de backtick (aka: ernstig accent) 'escape"$-teken om ervoor te zorgen dat de syntaxis van de OData-filter gebruikt in aanroepen van de REST van AAD-grafiek kan worden geïnterpreteerd door PowerShell-teken. Het teken backtick fungeert als [PowerShell van escape-teken](https://technet.microsoft.com/library/hh847755.aspx), zodat PowerShell een letterlijke interpretatie van het teken $ en te voorkomen dat deze verwarrend als PowerShell naam van een variabele (bijvoorbeeld $filter).

De focus van dit artikel is op de grafiek Explorer. Zie voor een voorbeeld van PowerShell [PowerShell-script](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-eindpunt

U kunt toegang tot deze API met behulp van de volgende URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Er is geen limiet voor het aantal records dat wordt geretourneerd door de API van Azure AD audit (met behulp van de OData-paginering). Zie voor de grenzen van de bewaartermijn van de rapportagegegevens, [bewaarbeleidsregels Reporting](active-directory-reporting-retention.md).

De aanroep retourneert de gegevens in batches. Elke batch heeft maximaal 1000 records. Als u de volgende batch met records, gebruikt de **volgende** koppeling. De token skip-informatie ophalen uit de eerste reeks geretourneerde records. Het skip-token wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Ondersteunde filters

U kunt beperken het aantal records dat wordt geretourneerd door een API-aanroep met een filter.  
Voor aanmelden API-gerelateerde gegevens, worden de volgende filters ondersteund:

* **$top =\<aantal records moeten worden geretourneerd\>**  : het aantal geretourneerde records te beperken. Dit is een dure bewerking. Gebruik dit filter geen als u wilt retourneren duizenden objecten.     
* **$filter =\<uw filterinstructie\>**  - om op te geven op basis van de ondersteunde filtervelden, het type van records die u belangrijk vindt

## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en -operators
Als u wilt opgeven welk type records die u belangrijk vindt, kunt u een filter-instructie met één of een combinatie van de volgende filtervelden:

* [ActiviteitDatum](#activitydate) -definieert een datum of datumbereik
* [categorie](#category) -definieert de categorie die u filteren wilt op.
* [activityStatus](#activitystatus) -bepaalt de status van een activiteit
* [activityType](#activitytype) -definieert het type van een activiteit
* [activiteit](#activity) -definieert u de activiteit als tekenreeks  
* [naam van deacteur/](#actorname) -definieert de actor in de vorm van de naam van de actor
* [acteur/objectid](#actorobjectid) -definieert de actor in de vorm van de actor-ID   
* [acteur/upn](#actorupn) -definieert de actor in de vorm van de actor principe (user Principal name) 
* [doelnaam/](#targetname) -definieert het doel in de vorm van de naam van de actor
* [doel/objectid](#targetobjectid) -definieert het doel in de vorm van de doel-ID  
* [doel/upn](#targetupn) -definieert de actor in de vorm van de actor principe (user Principal name)   

- - -
### <a name="activitydate"></a>activityDate
**Operators ondersteund**: eq, ge RP, gt, lt

**Voorbeeld**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Notities**:

DateTime-waarde moet in UTC-notatie

- - -
### <a name="category"></a>category

**Ondersteunde waarden**:

| Category                         | Waarde     |
| :--                              | ---       |
| Hoofddirectory                   | Directory |
| Selfservice voor wachtwoordbeheer | SSPR      |
| Self-service voor groepsbeheer    | SSGM      |
| Account inrichten             | Sync      |
| Automatische wachtwoordoverschakeling      | Automatische wachtwoordoverschakeling |
| Identiteitsbeveiliging              | IdentityProtection |
| Uitgenodigde gebruikers                    | Uitgenodigde gebruikers |
| MIM-service                      | MIM-service |



**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Ondersteunde waarden**:

| Activiteitsstatus | Waarde |
| :--             | ---   |
| Geslaagd         | 0     |
| Fout         | - 1   |

**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=activityType eq 'User'    

**Notities**:

hoofdlettergevoelig

- - -
### <a name="activity"></a>activiteit
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Notities**:

hoofdlettergevoelig

- - -
### <a name="actorname"></a>naam van de acteur /
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Notities**:

Niet-hoofdlettergevoelige

- - -
### <a name="actorobjectid"></a>acteur/objectId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>doelnaam /
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=targets/any(t: t/name eq 'some name')    

**Notities**:

Niet-hoofdlettergevoelige

- - -
### <a name="targetupn"></a>doel/upn
**Operators ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Notities**:

* Niet-hoofdlettergevoelige
* De volledige naamruimte tijdens het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity toevoegen

- - -
### <a name="targetobjectid"></a>doel/objectId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>acteur/upn
**Operators ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Notities**:

* Niet-hoofdlettergevoelige 
* De volledige naamruimte tijdens het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity toevoegen

- - -
## <a name="next-steps"></a>Volgende stappen

- Wilt u ziet u voorbeelden van gefilterde systeemactiviteiten? Bekijk de [voorbeelden van Azure Active Directory-audit API](active-directory-reporting-api-audit-samples.md).

- Wilt u meer informatie over de Azure AD rapportage-API? Zie [aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

