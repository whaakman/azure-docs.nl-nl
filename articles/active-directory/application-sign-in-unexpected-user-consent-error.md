---
title: Onverwachte fout bij het uitvoeren van toestemming voor een toepassing | Microsoft Docs
description: Fouten die tijdens het proces optreden kunnen van een toepassing en wat u kunt doen over ze ermee akkoord dat wordt beschreven
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: fd500fdd4c8642bad96dcf71eebcf1fad461a35f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Onverwachte fout bij het uitvoeren van toestemming voor een toepassing

Dit artikel bevat fouten die kunnen optreden tijdens het proces om een toepassing met. Als u problemen met onverwachte toestemming wordt gevraagd dat niet eventuele foutberichten bevatten, Zie [verificatie scenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Veel toepassingen die zijn geïntegreerd met Azure Active Directory vereisen machtigingen voor toegang tot andere bronnen om te werken. Wanneer deze resources ook worden geïntegreerd met Azure Active Directory, toegang tot deze is vaak aangevraagd met het gemeenschappelijke kader voor toestemming. 

Dit resulteert in een instemmingsprompt wordt weergegeven, die treedt meestal op de eerste keer dat een toepassing wordt gebruikt, maar kan ook optreden op een later gebruik van de toepassing.

Bepaalde voorwaarden worden voldaan voor een gebruiker toe te staan de machtigingen die vereist zijn voor een toepassing. Als u deze voorwaarden niet wordt voldaan, kunnen er diverse fouten optreden. Deze omvatten:

## <a name="requesting-not-authorized-permissions-error"></a>Niet-geautoriseerde Machtigingsfout aanvragen
* **AADSTS90093:** &lt;clientAppDisplayName&gt; vraagt een of meer machtigingen die u bent niet gemachtigd om u te verlenen. Neem contact op met een beheerder, die u kunt deze toepassing namens jou toestemming.

Deze fout treedt op wanneer een gebruiker die geen beheerder van een bedrijf wil gebruik van een toepassing die de machtigingen die alleen een beheerder kan verlenen aanvraagt. Deze fout kan worden opgelost door een beheerder toegang verlenen tot de toepassing namens hun organisatie.

## <a name="policy-prevents-granting-permissions-error"></a>Beleid voorkomt dat machtigingen verlenen fout
* **AADSTS90093:** beheerder van &lt;tenantDisplayName&gt; heeft een beleid dat u niet verlenen ingesteld &lt;naam van de app&gt; de machtigingen die het aanvraagt. Neem contact op met de beheerder van &lt;tenantDisplayName&gt;, wie machtigingen kunt verlenen voor deze app namens jou.

Deze fout treedt op wanneer de beheerder de mogelijkheid voor gebruikers om toestemming aan toepassingen, uitgeschakeld en vervolgens een gebruiker die geen beheerder probeert te gebruiken van een toepassing die toestemming nodig. Deze fout kan worden opgelost door een beheerder toegang verlenen tot de toepassing namens hun organisatie.

## <a name="intermittent-problem-error"></a>Onregelmatige probleem fout
* **AADSTS90090:** het lijkt erop dat we een onregelmatig probleem opname van de machtigingen die u hebt geprobeerd te verlenen aan aangetroffen &lt;clientAppDisplayName&gt;. Probeer het later opnieuw.

Deze fout geeft aan dat er een probleem onregelmatige service aan de clientzijde is opgetreden. Deze kan worden opgelost door probeert toe te staan de toepassing opnieuw.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout
* **AADSTS65005:** de app &lt;clientAppDisplayName&gt; aangevraagd machtigingen voor toegang tot een resource &lt;resourceAppDisplayName&gt; die is niet beschikbaar. 

Neem contact op met de ontwikkelaar van toepassingen.

##  <a name="resource-not-available-in-tenant-error"></a>De resource is niet beschikbaar in de tenant-fout
* **AADSTS65005:** &lt;clientAppDisplayName&gt; toegang tot een bron aanvraagt &lt;resourceAppDisplayName&gt; die is niet beschikbaar in uw organisatie &lt; tenantDisplayName&gt;. 

Zorg ervoor dat deze resource beschikbaar is of neem contact op met de beheerder van &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>Fout met niet overeenkomende machtigingen
* **AADSTS65005:** de app aangevraagd toestemming voor toegang tot resources &lt;resourceAppDisplayName&gt;. Deze aanvraag is mislukt omdat deze komt niet overeen met hoe de app is vooraf geconfigureerd tijdens de registratie van de app. Neem contact op met de app vendor.* *

Deze fouten die alle optreden wanneer de toepassing die een gebruiker probeert toe te staan machtigingen vraagt voor toegang tot een resource-toepassing die niet is gevonden in de map van de organisatie (tenant). Dit kan verschillende redenen optreden:

-   De ontwikkelaar van de client-toepassing heeft de toepassing niet goed geconfigureerd, waardoor toegang vragen tot een ongeldige resource. De ontwikkelaar moet in dit geval wordt de configuratie van de clienttoepassing lost dit probleem bijwerken.

-   Een Service-Principal voor de doeltoepassing resource bestaat niet in de organisatie, of in het verleden bestond maar is verwijderd. U lost dit probleem, moet een Service-Principal voor de resource-toepassing in de organisatie worden ingericht zodat de clienttoepassing machtigingen kan aanvragen. Dit kan gebeuren in een aantal manieren, afhankelijk van het type toepassing, met inbegrip van:

    -   Ophalen van een abonnement voor de resource-toepassing (Microsoft gepubliceerde toepassingen)

    -   Ermee akkoord dat de resource-toepassing

    -   Machtigingen van de toepassing via Azure Portal

    -   Het toevoegen van de toepassing van de Azure AD-Toepassingsgalerie

## <a name="next-steps"></a>Volgende stappen 

[Apps, machtigingen en toestemming in Azure Active Directory (v1-eindpunt)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Scopes, machtigingen en toestemming in Azure Active Directory (v2.0-eindpunt)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


