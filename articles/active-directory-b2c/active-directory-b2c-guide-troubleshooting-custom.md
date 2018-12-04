---
title: Aangepaste beleidsproblemen oplossen in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over methoden voor het oplossen van fouten bij het werken met aangepaste beleidsregels in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8bb95ae776c329e67e9f9936213a9f4c2a0c8f62
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842591"
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Problemen oplossen met Azure AD B2C aangepaste beleidsregels en Identity-Ervaringsframework

Als u Azure Active Directory B2C (Azure AD B2C) aangepaste beleidsregels, kunt u ervaren uitdagingen instellen van het Framework identiteit ervaring in het XML-indeling van de beleid-taal.  Leren schrijven van aangepaste beleidsregels kan worden, zoals een nieuwe taal te leren. In dit artikel wordt beschreven hulpprogramma's en tips waarmee u kunnen snel opsporen en oplossen van problemen. 

> [!NOTE]
> In dit artikel gaat over het oplossen van uw aangepaste configuratie Azure AD B2C. Het niet de relying party-toepassing of de id-bibliotheek op te lossen.

## <a name="xml-editing"></a>XML-bewerken

De meest voorkomende fout bij het instellen van aangepaste beleidsregels is onjuist opgemaakt XML. Er is een goede XML-editor bijna essentieel. Een goede XML-editor XML systeemeigen wordt weergegeven, inhoud codeert, prefills algemene voorwaarden, houdt XML-elementen die zijn geïndexeerd en kan valideren met schema. Hier volgen twee van onze favoriete XML-editors:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Notepad++](https://notepad-plus-plus.org/)

XML-schemavalidatie identificeert fouten voordat u uw XML-bestand uploaden. In de hoofdmap van het starter-pack, de definitie van de XML-schema TrustFrameworkPolicy_0.3.0.0.xsd ophalen Zoek voor meer informatie in de documentatie van uw XML-editor, *XML-hulpprogramma's* en *XML-validatie*.

Een overzicht van XML-regels is wellicht handig zijn. Azure AD B2C geweigerd eventuele XML indelingsfouten die worden gedetecteerd. Af en toe een onjuiste indeling XML kan leiden tot foutberichten die misleidend zijn.

## <a name="upload-policies-and-policy-validation"></a>Upload-beleidsregels en validatie van het gebruikersbeleid

 Validatie van het XML-bestand uploaden is automatisch. De meeste fouten leiden tot het uploaden mislukt. Validatie bevat het beleidsbestand dat u uploadt. Dit omvat ook de keten van bestanden die het uploadbestand naar (het bestand met de relying party, het extensiebestand en de base-bestand verwijst). 
 
 Algemene validatiefouten omvatten het volgende.

Fout-fragment: `... makes a reference to ClaimType with id "displaName" but neither the policy nor any of its base policies contain such an element`
* De ClaimType-waarde is mogelijk verkeerd gespeld of bestaat niet in het schema.
* De ClaimType-waarden moeten worden gedefinieerd in ten minste een van de bestanden in het beleid. 
    Bijvoorbeeld: ` <ClaimType Id="socialIdpUserId">`
* Als de ClaimType is gedefinieerd in het extensiebestand, maar kan ook worden gebruikt in een waarde van het technische profiel in de base-bestand, resulteert het basis-bestand uploaden in een fout.

Fout-fragment: `...makes a reference to a ClaimsTransformation with id...`
* De oorzaken van de fout kunnen zijn dezelfde als de fout ClaimType.

Fout-fragment: `Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Controleer of de tenant-id de waarde in de **\<TrustFrameworkPolicy\>** en **\<BasePolicy\>** schermelementen overeenkomen met de doel-Azure AD B2C-tenant.  

## <a name="troubleshoot-the-runtime"></a>De runtime oplossen

* Gebruik `Run Now` en `https://jwt.io` voor het testen van uw beleid onafhankelijk van uw web- of mobiele toepassing. Deze website fungeert als een relying party-toepassing. De inhoud van de JSON Web Token (JWT) die wordt gegenereerd door uw Azure AD B2C-beleid wordt weergegeven. Voor het maken van een testtoepassing in Identiteitservaring-Framework, gebruikt u de volgende waarden:
    * Naam: TestApp
    * Web-App/Web-API: Nee
    * Systeemeigen client: Nee

* Als u wilt traceren de uitwisseling van berichten tussen de clientbrowser en de Azure AD B2C, gebruikt u [Fiddler](https://www.telerik.com/fiddler). U kunt een indicatie van waar de gebruikersbeleving is mislukt in uw indelingsstappen ophalen.

* In **Ontwikkelingsmodus**, gebruikt u **Application Insights** traceren van de activiteit van de gebruikersbeleving Identiteitservaring-Framework. In **Ontwikkelingsmodus**, ziet u de uitwisseling van claims tussen de Identiteitservaring-Framework en de verschillende claimproviders die zijn gedefinieerd door technische profielen, zoals id-providers, op basis van API-services, de Azure AD B2C-map voor gebruiker, en andere services, zoals Azure meerdere meervoudige verificatie.  

## <a name="recommended-practices"></a>Aanbevolen procedures

**Houd meerdere versies van uw scenario's. Ze te groeperen in een project met uw toepassing.** De base, uitbreidingen en relying party-bestanden zijn rechtstreeks van elkaar afhankelijk. Sla deze op als een groep. Wanneer er nieuwe functies worden toegevoegd aan uw beleid, houd u afzonderlijke werkende versies. Fase werkende versies in uw eigen systeem met de toepassingscode die ze met communiceren het bestand.  Uw toepassingen mogelijk nog vele andere relying party beleidsregels in een tenant worden aangeroepen. Ze kunnen worden afhankelijk van de claims die ze van uw Azure AD B2C-beleid verwachten.

**Ontwikkel en test technische profielen met bekende gebruikers reizen.** Geteste starter pack beleid gebruiken voor het instellen van uw technische profielen. Ze afzonderlijk testen voordat u ze in uw eigen reizen gebruiker opnemen.

**Ontwikkel en test gebruiker reizen met geteste technische profielen.** De orchestration-stappen van een gebruikersbeleving incrementeel wijzigen. Bouw geleidelijk uw beoogde scenario's.

## <a name="next-steps"></a>Volgende stappen

* Download in GitHub, de [active-directory-b2c-custom-policy-starterpack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) ZIP-bestand.
