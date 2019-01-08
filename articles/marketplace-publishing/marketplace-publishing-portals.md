---
title: Overzicht van de verschillende portals nodig voor het maken van een aanbieding voor Marketplace | Microsoft Docs
description: Overzicht van de verschillende portals voor het maken van een aanbieding voor de Marketplace
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 89ce82b3-c28a-4b0d-b37a-db3112160a4e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: d1cc0efa1da90d90bd035eaa495a1336b6ff96c2
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074177"
---
# <a name="portals-you-will-need"></a>U moet portals

Voordat u het proces voor het publiceren van een aanbieding, je krijgt zo kennismaken met de verschillende portals die u nodig hebt. Hieronder vindt u de korte samenvatting van de portals--Developer Center, Azure Portal voor publiceren en Azure portal in de volgorde die u met hen communiceren.                                                                            

## <a name="developer-center"></a>Ontwikkelaarscentrum
[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)

### <a name="description"></a>Description
Het maken van uw Microsoft Developer Center-account is een eenmalige taak. Zorg ervoor dat het bedrijf nog geen een Developer Center-account voordat u probeert te maken. Gedurende dit proces verzamelen we bankgegevens, belastinggegevens en bedrijfsadres.

> [!NOTE]
> Als u alleen gratis aanbiedingen publiceert (of bring-your-own-license biedt), hebben we fiscale en bankgegevens niet nodig.
> 
> 

### <a name="identityaccount-used"></a>Identiteit/account dat wordt gebruikt
In het ideale geval deze waarde is van een beveiligingsgroep of een distributielijst (bijvoorbeeld azurepublishing @*partnercompany*.com). De lijst of de beveiliging van een distributiegroep **moet** worden geregistreerd als een Microsoft-account.

> [!TIP]
> Het is raadzaam om met behulp van een beveiligingsgroep of een distributielijst omdat dit de afhankelijkheid van een individu elimineert, hoewel een afzonderlijk account kan ook worden gebruikt.
> 
> 

## <a name="publishing-portal"></a>Portal voor publiceren
[https://publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Description

Deze waarde is de portal die u gebruiken om aan de aanbieding te werken en te publiceren (marketing, prijzen, publiceren, certificering indien van toepassing, enz.).

### <a name="identityaccount-used"></a>Identiteit/account dat wordt gebruikt
De bovenstaande lijst of beveiliging distributiegroep moet worden gebruikt voor de eerste keer aanmelden bij de portal voor publiceren. Andere gebruikers kunnen later worden toegevoegd als co-beheerders. Deze lijst is de wijze waarop deze wordt toegewezen aan de registratiegegevens van de Developer Center.

## <a name="azure-portal"></a>Azure Portal
[Azure Portal](https://portal.azure.com)

### <a name="description"></a>Description
Deze waarde is de portal waar u uw aanbiedingen via gefaseerde en gepubliceerde in de Azure Marketplace (van toepassing voor virtuele machines, oplossingssjablonen en services voor ontwikkelaars op basis van een Azure Resource Manager bekijken kunt).

### <a name="identityaccount-used"></a>Identiteit/account dat wordt gebruikt
Terwijl u bent de tijdelijke opslag voor een aanbieding van de portal voor publiceren, moet een abonnements-ID in de whitelist opgenomen. Hetzelfde abonnement (Er is een gebruikersnaam en wachtwoord die zijn gekoppeld aan deze) moet worden gebruikt voor aanmelding bij deze portal voor het testen van de gefaseerde aanbieding.

## <a name="see-also"></a>Zie ook
* [Aan de slag: Het publiceren van een aanbieding voor Azure Marketplace](marketplace-publishing-getting-started.md)

