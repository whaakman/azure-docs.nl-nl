---
title: Zelfstudie voor het gebruik van Azure App-configuratie voor het beheren van de functie vlaggen | Microsoft Docs
description: In deze zelfstudie leert u hoe u voor het beheren van functie vlaggen afzonderlijk van uw toepassing met behulp van Azure App-configuratie.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: b7fbf9add67a45c0db89fc11cee5c10bc537ab63
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393571"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Zelfstudie: Functie-vlaggen in Azure App-configuratie beheren

U kunt alle functie-vlaggen in Azure App-configuratie opslaan en beheren vanuit één locatie. App-configuratie is een portal UI met de naam **functie Manager** die speciaal ontworpen voor functie-vlaggen. App-configuratie biedt ook systeemeigen ondersteuning voor het schema van de .NET Core-functievlag.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Definiëren en beheren van de functie markeringen in App-configuratie.
> * Vlaggen voor toegang-functie van uw toepassing.

## <a name="create-feature-flags"></a>Vlaggen voor functie maken

De functie Manager in de Azure-portal voor App-configuratie biedt een gebruikersinterface voor het maken en beheren van de functie-markeringen die u in uw toepassingen gebruikt.

Om toe te voegen een nieuwe functievlag:

1. Selecteer **functie Manager** >  **+ toevoegen** om toe te voegen, een functievlag.

    ![Vlag functielijst](./media/azure-app-configuration-feature-flags.png)

1. Voer een unieke naam van de sleutel voor de functie-markering. U moet deze naam om te verwijzen naar de vlag in uw code.

1. Als u wilt, geeft u de functievlag een beschrijving.

1. De initiële status van de functievlag ingesteld. Deze status is meestal *uit* of *op*. De *op* status verandert in *voorwaardelijke* als u een filter aan de functievlag toevoegen.

    ![Functie vlag maken](./media/azure-app-configuration-feature-flag-create.png)

1. Wanneer de status is *op*, selecteer **+ filter toevoegen** om op te geven van eventuele aanvullende voorwaarden om de status te kwalificeren. Voer de filtersleutel van een ingebouwde of aangepaste en selecteer vervolgens **+ parameter toevoegen** om te koppelen van een of meer parameters aan het filter. Als u ingebouwde filters zijn onder andere:

    | Sleutel | JSON-parameters |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-100 procent} |
    | Microsoft.TimeWindow | {"Start": UTC-tijd, 'End': UTC-tijd} |

    ![Functie vlag filter](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Vlag voor de onderdeelstatus bijwerken

Een functievlag statuswaarde wijzigen:

1. Selecteer **functie Manager**.

1. Aan de rechterkant van de functievlag voor een die u wilt wijzigen, selecteer het weglatingsteken ( **...** ), en selecteer vervolgens **bewerken**.

1. Een nieuwe status voor de functievlag ingesteld.

## <a name="access-feature-flags"></a>Vlaggen voor toegang-functie

Functie-markeringen die zijn gemaakt door de functie Manager worden opgeslagen en opgehaald als normale sleutelwaarden. Ze worden opgeslagen onder een speciale naamruimtevoorvoegsel `.appconfig.featureflag`. Als u wilt weergeven van de onderliggende sleutelwaarden, de configuratie-Verkenner te gebruiken. Uw toepassing kan deze waarden ophalen met behulp van de App-configuratie configuratieproviders, SDK's, opdrachtregel-extensies en REST-API's.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u kunt de functie vlaggen en hun status beheren met behulp van App-configuratie. Zie voor meer informatie over de functie-ondersteuning in App-configuratie en ASP.NET Core, het volgende artikel:

* [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md)
