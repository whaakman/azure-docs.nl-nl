---
title: Naslaginformatie over Azure AD-SAML-Protocol | Microsoft Docs
description: "Dit artikel bevat een overzicht van de profielen voor eenmalige aanmelding en één Sign-Out SAML in Azure Active Directory."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: d5ffba5d0c409fe9de7a9e82c6faa4ca2702ab95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# Hoe het SAML-protocol wordt gebruikt in Azure Active Directory
Maakt gebruik van het SAML 2.0-protocol van de Azure Active Directory (Azure AD) zodat toepassingen kunnen een ervaring voor eenmalige aanmelding te bieden aan hun gebruikers. De [Single Sign-On](active-directory-single-sign-on-protocol-reference.md) en [één Afmelden](active-directory-single-sign-out-protocol-reference.md) SAML-profielen van Azure AD wordt uitgelegd hoe SAML asserties, protocollen en bindingen worden gebruikt in de id-provider-service.

SAML-Protocol is vereist voor de id-provider (Azure AD) en de serviceprovider (de toepassing) voor het uitwisselen van gegevens over zichzelf.

Wanneer een toepassing wordt geregistreerd bij Azure AD, registreert de ontwikkelaar van de app federation-gerelateerde informatie met Azure AD. Dit omvat de **omleidings-URI** en **Metadata URI** van de toepassing.

Azure AD gebruikt de **Metadata URI** van de cloudservice voor het ophalen van de ondertekeningssleutel en de afmelding URI van de cloudservice. Als de toepassing een metagegevens URI niet ondersteunt, de ontwikkelaar moet Neem contact op met Microsoft ondersteuning om het afmelden URI en sleutel ondertekening.

Azure Active Directory wordt tenantspecifieke en algemene (tenant-onafhankelijk) eenmalige aanmelding en één afmelden eindpunten. Deze URL's vertegenwoordigen adresseerbare locaties--ze zijn niet alleen een id's--zodat u naar het eindpunt gaat te lezen van de metagegevens.

* Het eindpunt tenantspecifieke bevindt zich op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  De <TenantDomainName> tijdelijke aanduiding voor een geregistreerde domeinnaam of TenantID GUID van een Azure AD-tenant vertegenwoordigt. De federatiemetagegevens van de tenant contoso.com is bijvoorbeeld op: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Het eindpunt van de Tenant-onafhankelijke bevindt zich op `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dit eindpuntadres **algemene** wordt weergegeven, in plaats van een tenant domeinnaam of -ID.

Zie voor meer informatie over de Federatiemetagegevens documenten die Azure AD publiceert [Federatiemetagegevens](active-directory-federation-metadata.md).
