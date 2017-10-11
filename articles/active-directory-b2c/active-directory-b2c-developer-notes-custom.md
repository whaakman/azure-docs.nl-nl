---
title: 'Azure Active Directory B2C: Opmerkingen voor ontwikkelaars op het gebruik van aangepast beleid | Microsoft Docs'
description: Opmerkingen voor ontwikkelaars over het configureren en onderhouden van Azure AD B2C met aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Releaseopmerkingen voor openbare preview van Azure Active Directory B2C aangepast beleid
De functieset aangepast beleid is nu beschikbaar voor evaluatie van de openbare preview voor alle Azure Active Directory B2C (Azure AD B2C) klanten. Deze functieset is gericht op geavanceerde identiteit ontwikkelaars oplossingen die het meest complexe identiteit.  

Vandaag de dag, vereist deze functieset ontwikkelaars voor het configureren van het Framework identiteit ervaring rechtstreeks via XML-bestand bewerken. Deze methode van de configuratie is een krachtige en complexe. Geavanceerde identiteit ontwikkelaars met behulp van het kader van de gebruikerservaring identiteit moeten wilt investeren enige tijd voor het voltooien van de zelfstudies en verwijzing naar documenten te lezen. 

## <a name="features-included-in-this-public-preview"></a>Functies in deze openbare preview
Met de nieuwe functies geïntroduceerd in de openbare preview, kunnen ontwikkelaars de volgende taken uitvoeren:<br>

* Maken en uploaden aangepaste verificatie gebruiker trajecten met behulp van aangepast beleid. 
   * Gebruikers reizen stapsgewijze als kunnen worden uitgewisseld tussen claimproviders beschrijven. 
   * Definieer Voorwaardelijke vertakkingen in trajecten van de gebruiker. 
* REST-API-diensten in uw aangepaste verificatie gebruiker trajecten worden geïntegreerd.  
* Federatie met de id-providers die compatibel met de standaard OpenIDConnect zijn toevoegen. <br>
* Toevoegen van Federatie met de id-providers die aan het SAML 2.0-protocol voldoen. 

## <a name="terms-of-the-public-preview"></a>Voorwaarden van de openbare preview

* We raden u aan de nieuwe functies gebruiken voor evaluatiedoeleinden.<br>
* De nieuwe functies zijn niet bedoeld voor gebruik in een productieomgeving.<br>
* Service level agreements (Sla) niet van toepassing op de nieuwe functies. <br>
* Ondersteuningsaanvragen kunnen worden ingediend via reguliere ondersteuningskanalen. <br>
* Er is geen toegezegde opgegeven voor algemene beschikbaarheid.<br>
* Onze goeddunken, en voor welke reden dan ook, kunt Microsoft markeren en weigeren of scenario's en gebruiker trajecten die groter is dan het bereik van de Azure AD B2C product Freelance moeten fungeren als een klant identiteits- en toegangsbeheer (CIAM) beheerplatform beperken.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>De verantwoordelijkheden van aangepast beleid functieset ontwikkelaars
Handmatige beleidsconfiguratie lager niveau toegang verleent tot het onderliggende platform van Azure AD B2C en resulteert in het maken van een unieke, volledig aanpasbare vertrouwensrelatie-framework. Het aantal mogelijke permutaties met aangepaste identiteitsproviders, vertrouwensrelaties, integraties met externe services en stapsgewijze werkstromen hogere eisen op plaatsen gevorderde ontwikkelaars die ze gebruiken.

Om volledig profiteren van de openbare preview, is het raadzaam dat ontwikkelaars de functieset aangepast beleid gebruiken in overeenstemming zijn met de volgende richtlijnen:
* Vertrouwd raken met de taal van de configuratie van de identiteit ervaring Engine en het beheer van de sleutel/geheimen.
* Eigenaar worden van scenario's en aangepaste integraties.
* Methodisch scenariotests uitvoeren.
* Ga als volgt software ontwikkelings- en staging-best practices met ten minste één ontwikkelings- en testomgeving en een productie-omgeving.
* Blijf op de hoogte over nieuwe ontwikkelingen in de id-providers en services die u met integreert. Bijvoorbeeld: bijhouden van wijzigingen in geheimen en geplande en ongeplande wijzigingen in de service.
* Actieve bewaking hebt ingesteld en bewaak de reactiesnelheid van productieomgevingen.
* Neem contact op met e-mailadressen actueel te houden en blijf reageert op de Microsoft live-site-team e-mailberichten.
* Maatregelen tijdige wanneer aangeraden om dit te doen door het team van Microsoft live-site. 


>[!NOTE]
>Deze functies worden uiteindelijk misschien opgenomen in Azure AD ingebouwde beleid, zodat ze beter toegankelijk voor alle ontwikkelaars.

## <a name="next-steps"></a>Volgende stappen
[Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
