---
title: Hoe Azure AD het SAML-protocol gebruikt | Microsoft Docs
description: Dit artikel bevat een overzicht van de SAML-profielen eenmalige aanmelding en eenmalige afmelding in Azure Active Directory.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c53cb7ee7bc76b07059d68962c606cdcf866196
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835268"
---
# <a name="how-azure-ad-uses-the-saml-protocol"></a>Hoe Azure AD het SAML-protocol gebruikt

Azure Active Directory (Azure AD) maakt gebruik van het SAML 2,0-protocol om toepassingen in staat te stellen een eenmalige aanmelding voor hun gebruikers te bieden. Met de SAML-profielen eenmalige [aanmelding](single-sign-on-saml-protocol.md) en [eenmalige afmelding](single-sign-out-saml-protocol.md) van Azure AD wordt uitgelegd hoe SAML-beweringen,-protocollen en-bindingen worden gebruikt in de id-Provider service.

Voor het SAML-protocol zijn de ID-provider (Azure AD) en de service provider (de toepassing) vereist voor het uitwisselen van informatie over zichzelf.

Wanneer een toepassing is geregistreerd bij Azure AD, registreert de app-ontwikkelaar aan de Federatie gerelateerde informatie met Azure AD. Deze informatie omvat de omleidings- **URI** en de **meta gegevens-URI** van de toepassing.

Azure AD maakt gebruik van de **meta gegevens-URI** van de Cloud service om de handtekening sleutel en de afmeldings-URI op te halen. De klant kan de app openen in **Azure AD-> app-registratie** en vervolgens in **Instellingen-> Eigenschappen**, kunnen ze de afmeldings-URL bijwerken. Op deze manier kan Azure AD het antwoord naar de juiste URL verzenden. 

Azure Active Directory stelt Tenant-specifieke en algemene (Tenant onafhankelijke) eenmalige aanmelding en eind punten voor eenmalige afmeldingen in. Deze Url's vertegenwoordigen adresseer bare locaties: ze zijn niet alleen id's, dus u kunt naar het eind punt gaan om de meta gegevens te lezen.

* Het Tenant-specifieke eind punt bevindt zich op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`. De tijdelijke aanduiding Tenant domainname > vertegenwoordigt een geregistreerde domein naam of TenantID-GUID van een Azure AD-Tenant.  *\<* De federatieve meta gegevens van de contoso.com-Tenant bevindt zich bijvoorbeeld op: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Het Tenant-onafhankelijke eind punt bevindt zich op `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dit adres van het eind punt wordt **common** weer gegeven in plaats van de domein naam of-id van een Tenant.

Zie [federatieve meta gegevens](azure-ad-federation-metadata.md)voor meer informatie over de federatieve meta gegevens documenten die door Azure AD worden gepubliceerd.
