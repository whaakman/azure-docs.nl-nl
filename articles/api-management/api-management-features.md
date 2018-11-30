---
title: Vergelijking van de Azure API Management-lagen op basis van functie | Microsoft Docs
description: In dit artikel worden vergeleken op basis van de functies van API Management-categorieën.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: a00328608c582dcd28dbc78b5b56829f9d1ab500
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585534"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Vergelijking van de Azure API Management-lagen op basis van functie

Elke API Management [prijscategorie](https://aka.ms/apimpricing) vindt u een afzonderlijke set met functies en per eenheid [capaciteit](api-management-capacity.md). De volgende tabel geeft een overzicht van de belangrijkste functies die beschikbaar zijn in elk van de lagen. Sommige functies mogelijk werken anders of hebt verschillende mogelijkheden, afhankelijk van de laag. In dergelijke gevallen zijn de verschillen genoemd in de documentatieartikelen met een beschrijving van deze afzonderlijke functies.

| Functie                                                                                      | Verbruik<sup>Preview-versie</sup> | Developer      | Basic          | Standard       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD-integratie<sup>1</sup>                                                             | Nee                            | Ja            | Nee             | Ja            | Ja            |
| Ondersteuning voor Virtual Network (VNet)                                                               | Nee                            | Ja            | Nee             | Nee             | Ja            |
| Implementatie in meerdere regio's                                                                      | Nee                            | Nee             | Nee             | Nee             | Ja            |
| Meerdere aangepaste domeinnamen                                                                 | Nee                            | Nee             | Nee             | Nee             | Ja            |
| Portal voor ontwikkelaars<sup>2</sup>                                                                 | Nee                            | Ja            | Ja            | Ja            | Ja            |
| Ingebouwde cache                                                                               | Nee                            | Ja            | Ja            | Ja            | Ja            |
| Ingebouwde Analysemiddelen                                                                           | Nee                            | Ja            | Ja            | Ja            | Ja            |
| [SSL-instellingen](api-management-howto-manage-protocols-ciphers.md)                             | Nee                            | Ja            | Ja            | Ja            | Ja            |
| [Externe cache](https://aka.ms/apimbyoc)                                                    | Ja                           | Geen<sup>3</sup> | Geen<sup>3</sup> | Geen<sup>3</sup> | Geen<sup>3</sup> |
| [Verificatie van clientcertificaten](api-management-howto-mutual-certificates-for-clients.md) | Geen<sup>4</sup>                | Ja            | Ja            | Ja            | Ja            |
| [Back-up en herstel](api-management-howto-disaster-recovery-backup-restore.md)               | Nee                            | Ja            | Ja            | Ja            | Ja            |
| [Management via Git](api-management-configuration-repository-git.md)                        | Nee                            | Ja            | Ja            | Ja            | Ja            |
| Direct beheer API                                                                        | Nee                            | Ja            | Ja            | Ja            | Ja            |
| Azure Monitor-logboeken en metrische gegevens                                                               | Geen<sup>5</sup>                | Ja            | Ja            | Ja            | Ja            |

<sup>1</sup> maakt het gebruik van Azure AD (en Azure AD B2C) als een id-provider voor gebruikersaanmelding op de portal voor ontwikkelaars.<br/>
<sup>2</sup> met inbegrip van verwante functionaliteit bijvoorbeeld gebruikers, groepen, problemen, applicationsn en e-sjablonen en meldingen.<br/>
<sup>3</sup> externe cache-ondersteuning voor deze laag is binnenkort beschikbaar.<br/>
<sup>4</sup> verificatie van clientcertificaten wordt toegevoegd aan de laag verbruik vóór de algemene beschikbaarheid.<br/>
<sup>5</sup> volledige Azure Monitor-ondersteuning wordt toegevoegd aan de laag verbruik.
