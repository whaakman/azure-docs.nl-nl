---
title: Zelfstudie voor het gebruik van Azure App-configuratie voor het beheren van de functie vlaggen | Microsoft Docs
description: In deze zelfstudie leert u hoe u voor het beheren van de functie vlaggen afzonderlijk van uw toepassing met behulp van Azure App-configuratie
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
ms.openlocfilehash: d995a2e9f0d05dc3b0853036e8fb3c04ccdfab96
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412341"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Zelfstudie: Functie-vlaggen in Azure App-configuratie beheren

U kunt alle functie-vlaggen in Azure App-configuratie opslaan en beheren vanuit één locatie. Er is een portal-gebruikersinterface, met de naam **functie Manager**, dat wil zeggen speciaal ontworpen voor de functie vlaggen. Bovendien, App-configuratie biedt ondersteuning voor .NET Core functie vlag gegevensschema systeemeigen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Definiëren en beheren van de functie markeringen in App-configuratie.
> * Vlaggen voor toegang-functie van uw toepassing.

## <a name="create-feature-flags"></a>Vlaggen voor functie maken

De **functie Manager** in de Azure-portal voor App-configuratie een gebruikersinterface voor het maken biedt en beheren van functie-die markeringen u in uw toepassing. Volg de volgende stappen uit om een nieuwe functievlag.

1. Selecteer **functie Manager** > **+ maken** om toe te voegen, een functievlag.

    ![Vlag functielijst](./media/azure-app-configuration-feature-flags.png)

2. Voer een unieke naam van de sleutel voor de functie-markering. U moet deze naam om te verwijzen naar de vlag in uw code.

3. (Optioneel) Geef functievlag een meer human-vriendelijk-beschrijving.

4. De initiële status van de functievlag ingesteld. Meestal is dit net *op* of *uit*.

    ![Functievlag maken](./media/azure-app-configuration-feature-flag-create.png)

5. Wanneer de status is *op*, (optioneel) Geef een extra voorwaarde om te kwalificeren met **filter toevoegen**. Geef een filter ingebouwde of aangepaste sleutel en koppelen van de parameter (s). Als u ingebouwde filters zijn onder andere:

    | Sleutel | JSON-parameters |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-100 procent} |
    | Microsoft.TimeWindow | {"Start": UTC-tijd, 'End': UTC-tijd} |

    ![Functie vlag filter](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Vlag voor de onderdeelstatus bijwerken

Volg de volgende stappen uit om een functievlag statuswaarde te wijzigen.

1. Selecteer **functie Manager**.

2. Klik op **...**   >  **Bewerken** aan de rechterkant van de markering van een functie die u wilt wijzigen.

3. Een nieuwe status voor de functievlag ingesteld.

## <a name="access-feature-flags"></a>Vlaggen voor toegang-functie

Functie vlaggen die zijn gemaakt door de **functie Manager** worden opgeslagen en opgehaald als normale sleutel-waarden. Ze worden opgeslagen onder een speciale naamruimtevoorvoegsel *. appconfig.featureflag*. U vindt de onderliggende sleutel-waarden met behulp van de **configuratie Explorer**. Uw toepassing kunt ophalen met behulp van de App-configuratie configuratieproviders, SDK's, opdrachtregel-extensies en REST-API's.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u voor het beheren van de functie vlaggen en hun status met behulp van App-configuratie. Zie de volgende bronnen voor meer informatie over ondersteuning voor het beheer van functies in de configuratie van Apps en ASP.NET Core.

* [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md)
