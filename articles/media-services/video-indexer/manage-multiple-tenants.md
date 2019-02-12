---
title: Beheren van meerdere tenants met Video Indexer - Azure
description: Dit artikel bevat verschillende integratie-opties voor het beheren van meerdere tenants met Video Indexer.
services: media-services
documentationcenter: ''
author: ika-microsoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/10/2019
ms.author: ikbarmen
ms.openlocfilehash: 3b6daa60e8c8945d49170a00da5bd3ffb57b48ac
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000111"
---
# <a name="manage-multiple-tenants"></a>Meerdere tenants beheren

Dit artikel worden verschillende opties voor het beheren van meerdere tenants met Video Indexer. Kies een methode die het meest geschikt is voor uw scenario:

* Video Indexer-account per tenant
* Enkele Video Indexer-account voor alle tenants
* Azure-abonnement per tenant

## <a name="video-indexer-account-per-tenant"></a>Video Indexer-account per tenant

Wanneer u deze architectuur gebruikt, wordt voor elke tenant een Video Indexer-account gemaakt. De tenants hebben volledige isolatie in de permanente en compute-laag.  

![Video Indexer-account per tenant](./media/manage-multiple-tenants/video-indexer-account-per-tenant.png)

### <a name="considerations"></a>Overwegingen

* Klanten delen geen storage-accounts (tenzij handmatig worden geconfigureerd door de klant).
* Klanten delen geen compute (gereserveerde eenheden) en niet van invloed zijn op taken verwerkingstijden van elkaar.
* U kunt gemakkelijk een tenant verwijderen uit het systeem door het verwijderen van de Video Indexer-account.
* Er is geen mogelijkheid voor het delen van aangepaste modellen tussen tenants.

    Zorg ervoor dat er geen bedrijfsvereiste voor het delen van aangepaste modellen.
* Moeilijker vanwege meerdere Video Indexer (en bijbehorende Media Services) om accounts te beheren per tenant.

> [!TIP]
> Maak een gebruiker met beheerdersrechten voor uw systeem in [Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/) en gebruik van de autorisatie-API voor uw tenants de relevante [toegangstoken voor account](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).

## <a name="single-video-indexer-account-for-all-users"></a>Enkele Video Indexer-account voor alle gebruikers

Wanneer u deze architectuur, is de klant verantwoordelijk voor isolatie van tenants. Alle tenants hebben voor het gebruik van een enkele Video Indexer-account met één Azure Media Service-account. Tijdens het uploaden, moet te zoeken of verwijderen van inhoud, de klant de juiste resultaten filteren voor de tenant.

![Enkele Video Indexer-account voor alle gebruikers](./media/manage-multiple-tenants/single-video-indexer-account-for-all-users.png)

Met deze optie kunnen aanpassing modellen (persoon, taal en merken) worden gedeeld of geïsoleerd tussen tenants door het filteren van de modellen door de tenant.

Wanneer [video's uploaden](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?), kunt u een andere partitie kenmerk per tenant. Hierdoor kunnen isolatie in de [zoeken-API](https://api-portal.videoindexer.ai/docs/services/operations/operations/Search-videos?). Door het kenmerk partitie op te geven in de search-API wordt u alleen de resultaten van de opgegeven partitie ophalen. 

 ### <a name="considerations"></a>Overwegingen

* De mogelijkheid voor het delen van inhoud en aanpassing modellen tussen tenants.
* Een tenant heeft gevolgen voor de prestaties van andere tenants.
* Klant nodig heeft om te maken van een complexe beheerscenario laag bovenop Video Indexer.

> [!TIP]
> U kunt de [prioriteit](upload-index-videos.md) kenmerk om tenants taken prioriteit te geven.

## <a name="azure-subscription-per-tenant"></a>Azure-abonnement per tenant 

Wanneer u deze architectuur gebruikt, wordt elke tenant zijn of haar eigen Azure-abonnement hebt. Voor elke gebruiker maakt u een nieuwe Video Indexer-account in de tenantabonnement.

![Azure-abonnement per tenant](./media/manage-multiple-tenants/azure-subscription-per-tenant.png)

### <a name="considerations"></a>Overwegingen

* Dit is de enige optie waarmee facturering scheiding.
* Deze integratie heeft meer beheer-overhead vereist dan Video Indexer-account per tenant. Als de facturering is geen vereiste, wordt het aanbevolen gebruik van een van de andere opties die in dit artikel wordt beschreven.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)
