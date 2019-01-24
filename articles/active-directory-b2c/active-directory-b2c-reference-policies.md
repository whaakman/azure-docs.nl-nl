---
title: Gebruikersstromen in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over de uitbreidbaar beleidsframework van Azure Active Directory B2C en hoe u verschillende gebruikersstromen maken.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8da3f5b86ccf1d6038ab410fe7fc6e6840fd0404
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845500"
---
# <a name="user-flows-in-azure-active-directory-b2c"></a>Gebruikersstromen in Azure Active Directory B2C

Het framework uitbreidbaar beleid van Azure Active Directory (Azure AD) B2C, is de belangrijkste sterkte van de service. Volledig beschrijven identiteitservaringen zoals beleid voor registreren, aanmelden of profiel bewerken. Instellen om u te helpen de meest algemene taken voor identiteit, bevat vooraf gedefinieerde, configureerbare beleidsregels met de naam van de Azure AD B2C-portal **gebruikersstromen**. 

## <a name="what-are-user-flows"></a>Wat zijn gebruikersstromen?

Een beleid kunt u gedrag in uw toepassingen beheren met het configureren van de volgende instellingen:

- Account typen die wordt gebruikt voor aanmelding bij, zoals sociale accounts, zoals een Facebook- of lokale accounts
- Kenmerken worden verzameld van de consumenten, zoals de voornaam, postcode, en schoen grootte
- Azure Multi-Factor Authentication
- Aanpassing van de gebruikersinterface
- Informatie die de toepassing als claims in een token ontvangt 

U kunt veel gebruikersstromen van verschillende typen maken in uw tenant en ze in uw toepassingen indien nodig kunnen gebruiken. Gebruikersstromen kunnen worden hergebruikt binnen toepassingen. Deze flexibiliteit kunt u definiëren en ervaring van de identiteit met minimale of geen wijzigingen in uw code te wijzigen. Uw toepassing wordt een gebruikersstroom geactiveerd met behulp van een standaard HTTP-verificatieaanvraag met de parameter van een gebruiker-stroom. Een aangepaste [token](active-directory-b2c-reference-tokens.md) als reactie wordt ontvangen. 

De volgende voorbeelden ziet de parameter 'p' query tekenreeks die Hiermee geeft u het beleid moet worden gebruikt:

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow
```

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow
```

## <a name="user-flow-versions"></a>Gebruikersstroomversies

In de Azure-portal, nieuwe [versies van gebruikersstromen](user-flow-versions.md) voortdurend worden toegevoegd. Wanneer u aan de slag met Azure AD B2C, getest gebruiker stromen worden aanbevolen voor gebruik. Wanneer u een nieuwe gebruikersstroom maakt, kiest u de gebruikersstroom die u nodig hebt uit de **aanbevolen** tabblad.

De volgende gebruikersstromen worden momenteel aanbevolen:

- **Registreren en aanmelden** -beide van de ervaringen van de registratie en aanmelden met een configuratie voor één verwerkt. Gebruikers worden in de juiste weg afhankelijk van de context geleid. Het verdient aanbeveling dat u deze gebruikersstroom via gebruiken een **aanmelding** gebruikersstroom of een **aanmelden** gebruikersstroom.
- **Bewerken van profielen** -kunnen gebruikers hun profielgegevens bewerken.
- **Wachtwoord opnieuw instellen** -kunt u configureren of en hoe gebruikers hun wachtwoord opnieuw kunnen instellen.

## <a name="linking-user-flows"></a>Koppelen gebruikersstromen

Een **registreren of aanmelden** de gebruikersstroom met lokale accounts bevat een **wachtwoord vergeten?** koppeling op de eerste pagina van de ervaring. Op deze koppeling te klikken niet automatisch trigger een wachtwoord opnieuw instellen van beleid. 

In plaats daarvan de foutcode `AADB2C90118` wordt geretourneerd naar uw toepassing. De toepassing nodig heeft voor het afhandelen van deze foutcode door het uitvoeren van een specifieke gebruikersstroom die het wachtwoord opnieuw instellen. Als u wilt zien, kijk eens een [eenvoudige ASP.NET-voorbeeld](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) die laat zien het koppelen van de gebruikersstromen.

## <a name="email-address-storage"></a>E-mailadres opslag

Een e-mailadres is vereist als onderdeel van een beleid. Als de gebruiker wordt geverifieerd met een sociale id-provider, het e-mailadres is opgeslagen in de **otherMails** eigenschap. Als een lokaal account is gebaseerd op de gebruikersnaam van een, wordt het e-mailadres opgeslagen in een eigenschap van de detail sterke verificatie. Als een lokaal account is gebaseerd op een e-mailadres, wordt het e-mailadres is opgeslagen in de **signInNames** eigenschap.
 
Het e-mailadres wordt niet gegarandeerd in al deze gevallen worden geverifieerd. Een tenantbeheerder kan de e-mailverificatie in het algemene beleid voor lokale accounts uitschakelen. Zelfs als e-mailadresverificatie is ingeschakeld, adressen worden niet geverifieerd als ze afkomstig van een sociale id-provider zijn en ze nog niet hebt gewijzigd.
 
Alleen de **otherMails** en **signInNames** eigenschappen worden weergegeven via het Active Directory Graph API. Het e-mailadres in de eigenschap van de detail sterke verificatie is niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen

Volg de instructies in voor het maken van de aanbevolen gebruikersstromen [zelfstudie: Een gebruikersstroom maken](tutorial-create-tenant.md).


