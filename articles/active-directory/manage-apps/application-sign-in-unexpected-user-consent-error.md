---
title: Onverwachte fout bij het uitvoeren van toestemming voor een toepassing | Microsoft Docs
description: Fouten die tijdens het proces optreden kunnen van een toepassing en wat u kunt doen over ze ermee akkoord dat wordt beschreven
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.openlocfilehash: 24cf2da101b3d72e985e9c797e61131d6755b989
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55959354"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Onverwachte fout bij het uitvoeren van toestemming voor een toepassing

Dit artikel worden de fouten die tijdens het proces optreden kunnen van stemt ermee in dat een toepassing. Als u problemen met onverwachte toestemming vragen die geen eventuele foutberichten bevatten, Zie [Verificatiescenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Veel toepassingen die kunnen worden geïntegreerd met Azure Active Directory vereist machtigingen voor toegang tot andere resources om te kunnen werken. Wanneer deze resources ook worden geïntegreerd met Azure Active Directory, machtigingen voor toegang tot deze is vaak aangevraagd met behulp van de algemene toestemmingsframework. Een vraag toestemming wordt weergegeven, dit gebeurt doorgaans de eerste keer dat een toepassing wordt gebruikt, maar kan ook gebeuren op een later gebruik van de toepassing.

Bepaalde voorwaarden worden voldaan voor een gebruiker akkoord gaan met de machtigingen die vereist dat een toepassing. Als niet aan deze voorwaarden wordt voldaan, kunnen de volgende fouten optreden.

## <a name="requesting-not-authorized-permissions-error"></a>Machtigingsfout met niet-geautoriseerde aanvragen
* **AADSTS90093:** &lt;clientAppDisplayName&gt; vraagt een of meer machtigingen die u bent niet gemachtigd om toegang te verlenen. Neem contact op met een beheerder, die toestemming voor deze toepassing uit uw naam geven kan.

Deze fout treedt op wanneer een gebruiker die geen beheerder van een bedrijf probeert te gebruiken van een toepassing die de machtigingen die alleen een beheerder kan verlenen aanvraagt. Deze fout kan worden opgelost door een beheerder toegang verlenen tot de toepassing namens hun organisatie.

## <a name="policy-prevents-granting-permissions-error"></a>Beleid voorkomt u dat het verlenen van machtigingen fout
* **AADSTS90093:** Een beheerder van &lt;tenantDisplayName&gt; heeft een beleid waarmee wordt voorkomen u verleent dat ingesteld &lt;naam van de app&gt; de aangevraagde machtigingen. Neem contact op met de beheerder van &lt;tenantDisplayName&gt;, die machtigingen kunt verlenen voor deze app op uw rekening.

Deze fout treedt op wanneer een bedrijfsbeheerder uitgeschakeld voor de gebruikers instemmen met toepassingen de mogelijkheid wordt en vervolgens een niet-beheerder-gebruiker probeert te gebruiken van een toepassing die toestemming nodig heeft. Deze fout kan worden opgelost door een beheerder toegang verlenen tot de toepassing namens hun organisatie.

## <a name="intermittent-problem-error"></a>Onregelmatig terugkerend probleem fout
* **AADSTS90090:** Het lijkt erop dat het aanmeldingsproces een onregelmatig terugkerend probleem op te nemen de machtigingen die u hebt geprobeerd te verlenen aan aangetroffen &lt;clientAppDisplayName&gt;. Probeer het later opnieuw.

Deze fout geeft aan dat er een probleem met onregelmatige service aan de clientzijde opgetreden. Het kan worden opgelost door akkoord gaan met de toepassing opnieuw probeert.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout
* **AADSTS65005:** De app &lt;clientAppDisplayName&gt; aangevraagd machtigingen voor toegang tot een resource &lt;resourceAppDisplayName&gt; die niet beschikbaar is. 

Neem contact op met de ontwikkelaar van de toepassing.

##  <a name="resource-not-available-in-tenant-error"></a>De resource is niet beschikbaar in de tenant-fout
* **AADSTS65005:** &lt;clientAppDisplayName&gt; toegang aanvraagt tot een resource &lt;resourceAppDisplayName&gt; die is niet beschikbaar in uw organisatie &lt;tenantDisplayName &gt;. 

Zorg ervoor dat deze resource beschikbaar is of neem contact op met de beheerder van &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>Fout met niet overeenkomende machtigingen
* **AADSTS65005:** De app toestemming voor toegang tot resource aangevraagd &lt;resourceAppDisplayName&gt;. Deze aanvraag is mislukt omdat deze komt niet overeen met hoe de app is vooraf geconfigureerd tijdens de appregistratie. Neem contact op met de app vendor.* *

Deze fouten die alle optreden wanneer de toepassing die een gebruiker probeert toegang te laten geven machtigingen aanvraagt voor toegang tot een resourcetoepassing die niet is gevonden in de map van de organisatie (tenant). Dit kan gebeuren om verschillende redenen:

-   De ontwikkelaar van de client-toepassing heeft de toepassing niet juist geconfigureerd, waardoor de toegang tot een ongeldige resource. Ontwikkelaar van de toepassing moet in dat geval wordt de configuratie van de clienttoepassing om dit probleem te verhelpen bijwerken.

-   Een Service-Principal voor de doeltoepassing resource bestaat niet in de organisatie, of in het verleden bestond maar werd verwijderd. U lost dit probleem, moet een Service-Principal voor de resourcetoepassing in de organisatie zijn ingericht, zodat de clienttoepassing kan machtigingen aanvragen. De Service-Principal kan worden ingericht op een aantal manieren, afhankelijk van het type toepassing, met inbegrip van:

    -   Ophalen van een abonnement voor de resourcetoepassing (Microsoft gepubliceerde toepassingen)

    -   Ermee akkoord dat de resourcetoepassing

    -   De toepassingsmachtigingen verleent via de Azure-portal

    -   De toepassing toe te voegen van de Azure AD-Toepassingsgalerie

## <a name="next-steps"></a>Volgende stappen 

[Apps, machtigingen en toestemming in Azure Active Directory (v1-eindpunt)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Bereiken, machtigingen en toestemming in Azure Active Directory (v2.0-eindpunt)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


