---
title: Cloud Partner Portal-API-verwijzing | Azure Marketplace
description: Beschrijving van de vereisten voor het gebruik en lijst met marketplace API-bewerkingen.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 9626fdb5dbdf720a65217421565fc559a0cee684
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935464"
---
<a name="cloud-partner-portal-api-reference"></a>Cloud Partner Portal API-verwijzing
==================================

REST API's voor de Cloud Partner-Portal toestaan het programma voor het ophalen en manipuleren van werkbelastingen, aanbiedingen en uitgever profielen. De API's gebruiken op rollen gebaseerd toegangsbeheer (RBAC) om af te dwingen de juiste machtigingen aan verwerkingstijd.

Deze referentie bevat de technische details voor de Cloud Partner Portal REST API's. De nettolading voorbeelden in dit document zijn alleen ter informatie en zijn onderhevig aan wijzigingen, nieuwe functionaliteit wordt toegevoegd.


<a name="prerequisites-and-considerations"></a>Vereisten en overwegingen
-------------------------------

Voordat u de API's gebruikt, moet u het volgende controleren:

- De [vereisten](./cloud-partner-portal-api-prerequisites.md) artikel voor meer informatie over een service-principal toevoegen aan uw account en een Azure Active Directory (Azure AD) toegangstoken ophalen voor de verificatie. 
- De twee [gelijktijdigheidsbeheer](./cloud-partner-portal-api-concurrency-control.md).
strategieën voor het aanroepen van deze API's beschikbaar.
- Extra API [overwegingen met betrekking tot](./cloud-partner-portal-api-considerations.md), zoals versiebeheer en foutafhandeling.


<a name="common-tasks"></a>Algemene taken
------------
Deze referentie details API's voor het uitvoeren van de volgende algemene taken.


### <a name="offers"></a>Aanbiedingen

-   [Alle aanbiedingen ophalen](./cloud-partner-portal-api-retrieve-offers.md)
-   [Een specifieke aanbiedings ophalen](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Aanbiedingsstatus ophalen](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Een aanbieding maken](./cloud-partner-portal-api-creating-offer.md)
-   [Een aanbieding publiceren](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>Bewerkingen

-   [Bewerkingen ophalen](./cloud-partner-portal-api-retrieve-operations.md)
-   [Bewerkingen annuleren](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Een app publiceren

-   [Live gaan](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Andere taken

-   [Prijzen voor aanbiedingen van de virtuele machine instellen](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Problemen oplossen

-   [Problemen met verificatiefouten oplossen](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
