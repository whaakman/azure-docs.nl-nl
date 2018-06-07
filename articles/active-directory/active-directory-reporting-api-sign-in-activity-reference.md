---
title: Azure Active Directory aanmeldingsactiviteiten rapport API-referentiemateriaal | Microsoft Docs
description: Verwijzing voor de API van Azure Active Directory aanmeldingsactiviteiten rapport
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/08/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: c5d5e11c5178e5b90c2970e52dd1e6f47a78204d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588386"
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory aanmeldingsactiviteiten rapport API-referentiemateriaal

> [!TIP] 
> Bekijk de nieuwe Microsoft Graph-API voor [reporting](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit), die deze API uiteindelijk wordt vervangen. 

In dit artikel maakt deel uit van een verzameling artikelen over Azure Active Directory (Azure AD) rapportage-API. Rapportage van Azure AD biedt u een API waarmee u toegang krijgt tot controlegegevens met code of gerelateerde hulpprogramma's.
Het bereik van dit artikel is om u te bieden informatie over de **audit API**.

Zie:

* [Aanmelden activiteiten](active-directory-reporting-azure-portal.md#activity-reports) voor meer conceptuele informatie
* [Aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage-API.


## <a name="who-can-access-the-api-data"></a>Wie toegang heeft tot de API-gegevens?
* Gebruikers- en Service-Principals in de rol beheerder beveiliging of beveiliging lezer
* Globale beheerders
* Elke app die autorisatie is voor toegang tot de API (app autorisatie kan worden ingesteld alleen op basis van toestemming van de globale beheerder)

Wilt configureren toegang voor een toepassing te krijgen tot beveiliging API's, zoals een aanmeldingspagina gebeurtenissen, gebruikt u de volgende PowerShell om toe te voegen van de Service-Principal-toepassingen aan de rol Lezer beveiliging

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Vereisten
Voor toegang tot dit rapport via de API voor rapportage, moet u het volgende hebben:

* Een [Azure Active Directory Premium P1 of P2 edition](active-directory-whatis.md)
* Voltooid de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Toegang tot de API
U kunt de toegang tot deze API via de [grafiek Explorer](https://graphexplorer2.cloudapp.net) of programmatisch met bijvoorbeeld PowerShell. Gebruik de backtick (aka: ernstig accent) 'escape"$-teken om ervoor te zorgen dat de syntaxis van de OData-filter gebruikt in aanroepen van de REST van AAD-grafiek kan worden geïnterpreteerd door PowerShell-teken. Het teken backtick fungeert als [PowerShell van escape-teken](https://technet.microsoft.com/library/hh847755.aspx), zodat PowerShell een letterlijke interpretatie van het teken $ en te voorkomen dat deze verwarrend als PowerShell naam van een variabele (bijvoorbeeld $filter).

De focus van dit onderwerp is op de grafiek Explorer. Zie voor een voorbeeld van PowerShell [PowerShell-script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-eindpunt
U kunt toegang tot deze API met behulp van de volgende basis-URI:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Vanwege de hoeveelheid gegevens heeft deze API een limiet van 1.000.000 geretourneerde records. 

Deze aanroep retourneert de gegevens in batches. Elke batch heeft maximaal 1000 records. Als u de volgende batch met records, gebruikt u de volgende koppeling. Ophalen van de [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) gegevens uit de eerste set van de geretourneerde records. Het skip-token wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Ondersteunde filters
U kunt beperken het aantal records dat wordt geretourneerd door een API-aanroep in de vorm van een filter.  
Voor aanmelden API-gerelateerde gegevens, worden de volgende filters ondersteund:

* **$top =\<aantal records moeten worden geretourneerd\>**  : het aantal geretourneerde records te beperken. Dit is een dure bewerking. Gebruik dit filter geen als u wilt retourneren duizenden objecten.  
* **$filter =\<uw filterinstructie\>**  - om op te geven op basis van de ondersteunde filtervelden, het type van records die u belangrijk vindt

## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en -operators
Als u wilt opgeven welk type records die u belangrijk vindt, kunt u een filter-instructie die kan een bestand of een combinatie van de volgende filtervelden bevatten:

* [signinDateTime](#signindatetime) -definieert een datum of datumbereik
* [userId](#userid) -definieert een specifieke gebruiker op basis van de gebruikers-ID.
* [userPrincipalName](#userprincipalname) -definieert een specifieke gebruiker op basis van de gebruiker UPN (User Principal Name)
* [appId](#appid) -definieert een specifieke app op basis van de app-ID
* [appDisplayName](#appdisplayname) -definieert een specifieke app op basis van de app weergegeven naam
* [loginStatus](#loginStatus) -bepaalt de status van de aanmeldingen (geslaagd / fout)

> [!NOTE]
> Wanneer u grafiek Explorer gebruikt, is u hoeft te hoofdlettergevoelig gebruiken voor elke letter uw filter-velden.
> 
> 

Als u wilt beperken het bereik van de geretourneerde gegevens, kunt u combinaties van de ondersteunde filters en filtervelden maken. Bijvoorbeeld retourneert de volgende instructie de bovenste 10 records tussen 1 juli-2016 en juli 6 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Operators ondersteund**: eq, ge RP, gt, lt

**Voorbeeld**:

Met behulp van een specifieke datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Met behulp van een datumbereik    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notities**:

De datetime-parameter moet in de UTC-notatie 

- - -
### <a name="userid"></a>userId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notities**:

De waarde van de gebruikers-id is een string-waarde

- - -
### <a name="userprincipalname"></a>userPrincipalName
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notities**:

De waarde van userPrincipalName is de waarde van een tekenreeks

- - -
### <a name="appid"></a>AppId
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notities**:

De waarde van de appId is een string-waarde

- - -
### <a name="appdisplayname"></a>appDisplayName
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notities**:

De waarde van appDisplayName is een string-waarde

- - -
### <a name="loginstatus"></a>loginStatus
**Operators ondersteund**: eq

**Voorbeeld**:

    $filter=loginStatus+eq+'1'  


**Notities**:

Er zijn twee opties voor de loginStatus: 0 - geslaagd, 1 - fout

- - -
## <a name="next-steps"></a>Volgende stappen
* Wilt u ziet u voorbeelden van gefilterde activiteiten aanmelden? Bekijk de [Azure Active Directory aanmeldingsactiviteiten rapport API samples](active-directory-reporting-api-sign-in-activity-samples.md).
* Wilt u meer informatie over de Azure AD rapportage-API? Zie [aan de slag met Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

