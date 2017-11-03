---
title: Maken van een exemplaar van Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor het maken van een nieuw exemplaar van Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 6433ea1f0eb6ad375402b998b4dfa80bded35c4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Maak een nieuw exemplaar van Azure API Management-service

Deze zelfstudie worden de stappen beschreven voor het maken van een nieuwe API Management exemplaar met de [Azure-portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Vereisten

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-new-service"></a>Een nieuwe service maken

1. In de [Azure-portal](https://portal.azure.com/), selecteer **nieuw** > **Enterprise Integration** > **API management**.

    U kunt ook kiezen **nieuw**, type `API management` in het zoekvak en druk op Enter. Klik op **Create**.

2. In de **API Management-service** venster, voer een unieke **naam** voor uw API Management-service. Deze naam kan niet later worden gewijzigd.

    > [!TIP]
    > De naam van de service wordt gebruikt voor het genereren van een standaard-domeinnaam in de vorm van *{name} .azure-api.net.* Als u gebruiken een aangepaste domeinnaam wilt, Zie [een aangepast domein configureren](configure-custom-domain.md). <br/>
    > De naam van de service wordt gebruikt om te verwijzen naar de service en de bijbehorende Azure-resource.

5. Selecteer een **abonnement** tussen de verschillende Azure-abonnementen waartoe u toegang hebt.
6. Selecteer in **Resourcegroep** de nieuwe of bestaande resource.  Een resourcegroep is een verzameling resources met dezelfde levenscyclus, dezelfde machtigingen en hetzelfde beleid. Klik [hier](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
7. In **locatie**, selecteer de geografische regio waar de API Management-service wordt gemaakt. Alleen de beschikbare API Management-service regio's worden weergegeven in het vak vervolgkeuzelijst. 
9. Voer een **organisatienaam**. Deze naam wordt gebruikt in een aantal locaties. De titel van de portal voor ontwikkelaars en de afzender van e-mailmeldingen.
10. In **beheerder e**stelt e-mailadres op welke alle meldingen van **API Management** wordt verzonden.
11. In **prijscategorie**stelt **Developer** laag evalueren van de service. Deze laag is niet beschikbaar voor gebruik in productieomgevingen. Zie voor meer informatie over het schalen van de API Management-categorieën [upgraden en schalen](upgrade-and-scale.md).
12. Kies **Maken**.

    > [!TIP]
    > Het duurt meestal tussen 20-30 minuten om een API Management-service te maken. Selecteren **vastmaken aan dashboard** maakt een nieuwe service gemakkelijker vinden.

## <a name="next-steps"></a>Volgende stappen

[Publiceren van een API met Azure API Management](#api-management-getstarted-publish-api.md)