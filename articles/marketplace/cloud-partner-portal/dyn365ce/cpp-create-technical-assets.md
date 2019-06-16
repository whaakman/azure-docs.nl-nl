---
title: Dynamics 365 voor Customer Engagement technische activa maken | Azure Marketplace
description: De technische assets voor een Dynamics 365 voor Customer Engagement toepassing aanbieding maken.
services: Dynamics 365 for Customer Engagement, Azure, Marketplace, Cloud Partner Portal, AppSource
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 12/29/2018
ms.author: pabutler
ms.openlocfilehash: eff175264677d6b8ffb885229b5e68b306424335
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943085"
---
# <a name="create-technical-assets-for-azure-application-offer"></a>Technische activa voor de aanbieding van Azure-toepassing maken

Doorgaans het ontwikkelen van oplossingen met behulp van de [SDK voor Dynamics 365 voor Customer Engagement-apps](https://docs.microsoft.com/dynamics365/customer-engagement/developer/get-started-sdk).  Oplossingen worden verschillende vormen, zoals beschreven in [programmeringsmodellen voor Dynamics 365 voor Customer Engagement-apps](https://docs.microsoft.com/dynamics365/customer-engagement/developer/programming-models).  Ervoor gekozen het formulier dat het beste voldoet aan de vereisten van uw oplossing.  Wanneer u een oplossing ontwikkelt, moet u er een aantal problemen die u, zoals uitbreidbaarheid keuzes oplossingsonderdelen en compatibiliteit van de versie oplossen moet zijn.  Zie voor meer informatie, [Inleiding tot oplossingen](https://docs.microsoft.com/dynamics365/customer-engagement/developer/introduction-solutions).

De meeste van de Dynamics 365-oplossingen die zijn gepubliceerd naar AppSource zijn beheerde toepassingen die zijn gedistribueerd als pakketbestanden.


## <a name="creating-and-storing-the-package"></a>Het maken en opslaan van het pakket

De parallelle documentatie bij het maken van Dynamics 365 voor Customer Engagement biedt is gevonden in de sectie [Publiceer uw app op AppSource](https://docs.microsoft.com/dynamics365/customer-engagement/developer/publish-app-appsource).  Het volgende bevat details van de onderwerpen over het maken van het pakketbestand van de oplossing en dit uploaden naar Azure storage:

- [Stap 4: Maken van een pakket AppSource voor uw app](https://docs.microsoft.com/dynamics365/customer-engagement/developer/create-package-app-appsource) -wordt uitgelegd hoe u een gecomprimeerd (zip)-bestand dat bevat en dat staat voor uw beheerde toepassing te maken: map van de activa, aangepaste code dll-bestand, MIME-type bestand, AppSource pakketpictogram, licentie voorwaarden (HTML)-bestand en de inhoudsbestand (XML).
- [Stap 5: Uw pakket AppSource Store in Azure Storage en het genereren van een URL met SAS-sleutel](https://docs.microsoft.com/dynamics365/customer-engagement/developer/store-appsource-package-azure-storage) -wordt uitgelegd hoe u een pakketbestand AppSource opslaan in een Microsoft Azure Blob storage-account en de sleutel van een Shared Access Signature (SAS) gebruiken voor het delen van het pakketbestand. Het pakketbestand wordt opgehaald uit uw Azure-opslaglocatie voor certificering, en selecteer bij AppSource proefversies en publicatie.


## <a name="next-steps"></a>Volgende stappen

Als u hebt nog niet gedaan, [maken van uw Dynamics 365 voor Customer Engagement aanbieding](./cpp-create-offer.md).  Vervolgens kunt u zich kunt [uw aanbieding publiceren](./cpp-publish-offer.md).
