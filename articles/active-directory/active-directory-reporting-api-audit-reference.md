---
title: Azure Active Directory-audit API-referentiemateriaal | Microsoft Docs
description: Hoe u aan de slag met de Azure Active Directory-audit-API
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5cdf80ff1cc49b1582302d411ee6fcc8f193c021
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory-audit API-referentiemateriaal
In dit onderwerp maakt deel uit van een verzameling van onderwerpen over de Azure Active Directory rapportage-API.  
Rapportage van Azure AD biedt u een API waarmee u toegang krijgt tot controlegegevens met code of gerelateerde hulpprogramma's.
Het bereik van dit onderwerp is om u te bieden informatie over de **audit API**.

Zie:

* [Controlelogboeken](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie

* [Aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.


Voor:

- Veelgestelde vragen, Lees onze [Veelgestelde vragen](active-directory-reporting-faq.md) 

- Uitgeeft, neem [een ondersteuningsticket bestand](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
* Gebruikers met de rol Beveiligingsbeheerder of Beveiligingslezer
* Globale beheerders
* Elke app die autorisatie is voor toegang tot de API (app autorisatie worden instellingen slechts op basis van toestemming van de globale beheerder)

## <a name="prerequisites"></a>Vereisten
Als u dit rapport opent via de rapportage-API, moet u het volgende hebben:

* Een [Azure Active Directory gratis of betere edition](active-directory-editions.md)
* Voltooid de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Toegang tot de API
U kunt de toegang tot deze API via de [grafiek Explorer](https://graphexplorer2.cloudapp.net) of programmatisch met bijvoorbeeld PowerShell. In de volgorde voor PowerShell correct te kunnen interpreteren de syntaxis van de OData-filter gebruikt in AAD grafiek REST-aanroepen, moet u de backtick (aka: ernstig accent) 'escape' van het teken $-teken. Het teken backtick fungeert als [PowerShell van escape-teken](https://technet.microsoft.com/library/hh847755.aspx), zodat PowerShell een letterlijke interpretatie van het teken $ en te voorkomen dat deze als de naam van een PowerShell-variabele verwarrend (ie: $filter).

De focus van dit onderwerp is op de grafiek Explorer. Zie voor een voorbeeld van PowerShell [PowerShell-script](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-eindpunt
U kunt toegang tot deze API met behulp van de volgende URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Er is geen limiet voor het aantal records dat wordt geretourneerd door de Azure AD-audit-API (OData paginering met).
Bekijk voor ondergrenzen voor de bewaarperiode op rapportagegegevens [bewaarbeleidsregels Reporting](active-directory-reporting-retention.md).

Deze aanroep retourneert de gegevens in batches. Elke batch heeft maximaal 1000 records.  
Als u de volgende batch met records, gebruikt u de volgende koppeling. De informatie skiptoken ophalen uit de eerste reeks geretourneerde records. Het skip-token wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Ondersteunde filters
U kunt beperken het aantal records dat wordt geretourneerd door een API-aanroep in de vorm van een filter.  
Voor aanmelden API van de bijbehorende gegevens, de volgende filters worden ondersteund:

* **$top =\<aantal records moeten worden geretourneerd\>**  : het aantal geretourneerde records te beperken. Dit is een dure bewerking. U moet dit filter niet gebruiken als u wilt retourneren duizenden objecten.     
* **$filter =\<uw filterinstructie\>**  - om op te geven op basis van de ondersteunde filtervelden, het type van records die u belangrijk vindt

## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en -operators
Als u wilt opgeven welk type records die u belangrijk vindt, kunt u een filter-instructie die kan een bestand of een combinatie van de volgende filtervelden bevatten:

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

**Opmerkingen bij de**:

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
### <a name="activitystatus"></a>activityStatus

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

**Opmerkingen bij de**:

hoofdlettergevoelig

- - -
### <a name="activity"></a>activiteit
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Opmerkingen bij de**:

hoofdlettergevoelig

- - -
### <a name="actorname"></a>naam van de acteur /
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Opmerkingen bij de**:

Niet-hoofdlettergevoelige

- - -
### <a name="actorobjectid"></a>actor/objectId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>doelnaam /
**Operators ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=targets/any(t: t/name eq 'some name')    

**Opmerkingen bij de**:

Niet-hoofdlettergevoelige

- - -
### <a name="targetupn"></a>doel/upn
**Operators ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Opmerkingen bij de**:

* Niet-hoofdlettergevoelige
* U moet de volledige naamruimte toevoegen bij het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

- - -
### <a name="targetobjectid"></a>target/objectId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>acteur/upn
**Operators ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Opmerkingen bij de**:

* Niet-hoofdlettergevoelige 
* U moet de volledige naamruimte toevoegen bij het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

- - -
## <a name="next-steps"></a>Volgende stappen
* Wilt u ziet u voorbeelden van gefilterde systeemactiviteiten? Bekijk de [voorbeelden van Azure Active Directory-audit API](active-directory-reporting-api-audit-samples.md).
* Wilt u meer informatie over de Azure AD rapportage-API? Zie [aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

