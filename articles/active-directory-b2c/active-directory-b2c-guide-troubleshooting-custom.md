---
title: 'Azure Active Directory B2C: Aangepaste beleidsproblemen oplossen | Microsoft Docs'
description: Meer informatie over methoden voor het oplossen van fouten bij het werken met aangepaste beleidsregels in Azure Active Directory.
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: joroja
ms.openlocfilehash: 023390ba36a74217503ff8b3076ad17978fe3fb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Problemen oplossen met Azure AD B2C aangepaste beleidsregels en identiteit ervaring Framework

Als u gebruikmaakt van Azure Active Directory B2C (Azure AD B2C) aangepast beleid, kunt u het Framework identiteit optreden in het XML-indeling van de beleid-taal instellen uitdagingen tegenkomen.  Leren schrijven van aangepaste beleidsregels kan worden zoals leren werken met een andere taal. In dit artikel beschreven tools en tips waarmee u kunnen snel opsporen en oplossen van problemen. 

> [!NOTE]
> Dit artikel is gericht op de configuratie van uw Azure AD B2C aangepast beleid voor het oplossen van problemen. Het omgaan niet met de relying party-toepassing of de identity-bibliotheek.

## <a name="xml-editing"></a>XML-bewerken

De meest voorkomende fout bij het instellen van een aangepast beleid is niet goed opgemaakt XML. Er is een goede XML-editor bijna essentieel. Een goede XML-editor XML systeemeigen wordt weergegeven, gekleurd inhoud prefills algemene voorwaarden, houdt XML-elementen die zijn geïndexeerd en kunt valideren met schema. Hier vindt u twee van onze favoriete XML-editors:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Kladblok ++](https://notepad-plus-plus.org/)

XML-schemavalidatie identificeert fouten voordat u uw XML-bestand uploaden. Haal de XML-schemadefinitie TrustFrameworkPolicy_0.3.0.0.xsd in de hoofdmap van het starter pack. Voor meer informatie in de documentatie van uw XML-editor, zoekt u naar *XML-hulpprogramma's* en *XML-validatie*.

Mogelijk vindt u een overzicht van XML-regels handig zijn. Azure AD B2C wordt geweigerd eventuele XML opmaakfouten die worden gedetecteerd. In sommige gevallen kan een onjuiste indeling XML mogelijk foutberichten die misleidend zijn.

## <a name="upload-policies-and-policy-validation"></a>Beleid en de validatie van het uploaden

 XML-bestand uploaden validatie is automatisch. De meeste fouten veroorzaken het uploaden is mislukt. Validatie omvat het beleidsbestand dat u uploadt. Dit omvat ook de keten van bestanden die het uploadbestand naar (het bestand met beleidsregel van relying party, het bestand uitbreidingen en de base-bestand verwijst). 
 
 Algemene validatiefouten biedt de volgende.

Fout-fragment:`... makes a reference to ClaimType with id "displaName" but neither the policy nor any of its base policies contain such an element`
* De waarde ClaimType mogelijk verkeerd gespeld of bestaat niet in het schema.
* ClaimType waarden moeten worden gedefinieerd in ten minste één van de bestanden in het beleid. 
    Bijvoorbeeld: ` <ClaimType Id="socialIdpUserId">`
* Als ClaimType is gedefinieerd in het extensiebestand, maar kan ook worden gebruikt in een TechnicalProfile-waarde in de base-bestand, resulteert het base-bestand uploadt in een fout.

Fout-fragment:`...makes a reference to a ClaimsTransformation with id...`
* De oorzaken van de fout kunnen zijn dezelfde als de fout ClaimType.

Fout-fragment:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Controleer of de tenant-id-waarde in de  **\<TrustFrameworkPolicy\>**  en  **\<BasePolicy\>**  elementen overeen met de doel-Azure AD B2C-tenant.  

## <a name="troubleshoot-the-runtime"></a>De runtime oplossen

* Gebruik `Run Now` en `https://jwt.io` voor het testen van uw beleid onafhankelijk van uw web- of mobiele toepassing. Deze website fungeert als een relying party-toepassing. De inhoud van de JSON Web Token (JWT) die wordt gegenereerd door het beleid van uw Azure AD B2C wordt weergegeven. Voor het maken van een testtoepassing in identiteit ervaring Framework, gebruikt u de volgende waarden:
    * Naam: TestApp
    * Web-App of Web-API: Nee
    * Native client: Nee

* Als u wilt traceren de uitwisseling van berichten tussen de clientbrowser en de Azure AD B2C, gebruik [Fiddler](http://www.telerik.com/fiddler). Dit kunt u een indicatie van waar de gebruiker reis in stappen van de orchestration kan niet ophalen.

* In **Ontwikkelingsmodus**, gebruik **Application Insights** als u wilt traceren van de activiteit van uw identiteit ervaring Framework gebruiker reis. In **Ontwikkelingsmodus**, kunt u de uitwisseling van claims tussen identiteit ervaring Framework en de verschillende claimproviders die zijn gedefinieerd door technische profielen, zoals id-providers, op basis van een API-services, zien de Azure AD B2C-gebruikerslijst en andere services zoals Azure meerdere-multi-factor-verificatie.  

## <a name="recommended-practices"></a>Aanbevolen procedures

**Houd meerdere versies van uw scenario's. Ze te groeperen in een project met uw toepassing.** De base, uitbreidingen en relying party-bestanden zijn rechtstreeks van elkaar afhankelijk. Opslaan als een groep. Nieuwe functies worden toegevoegd aan uw beleid, houd aparte werkende versies. Fase werkende versies in uw eigen bestandssysteem met de toepassingscode die ze met communiceren.  Uw toepassingen mogelijk veel verschillende relying party beleidsregels in een tenant worden aangeroepen. Deze is mogelijk afhankelijk van de claims die ze van uw Azure AD B2C-beleid verwachten.

**Ontwikkelen en testen van technische profielen met bekende gebruikers reizen.** Geteste starter pack beleid gebruiken voor het instellen van uw technische profielen. Ze afzonderlijk testen voordat u ze in uw eigen trajecten gebruiker opnemen.

**Ontwikkelen en testen van de gebruiker trajecten met geteste technische profielen.** De orchestration-stappen van een gebruiker reis incrementeel wijzigen. Geleidelijk bouw uw beoogde scenario's.

## <a name="next-steps"></a>Volgende stappen

* Download het [active-directory-b2c-custom-policy-starterpack] (https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) ZIP-bestand in GitHub.
