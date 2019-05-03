---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/06/2019
ms.openlocfilehash: cf35651f7dd839e8792029851b9bfe278036624c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028513"
---
Een experiment wordt uitgevoerd op een compute-doel, een compute-resource die is gekoppeld aan uw werkruimte.  Zodra u een compute-doel hebt gemaakt, kunt u het opnieuw gebruiken voor toekomstige planningen.

1. Selecteer **uitvoeren** onder aan het experiment uit te voeren.

     ![Experiment uit te voeren](./media/aml-ui-create-training-compute/run-experiment.png)

1. Wanneer de **Setup Compute-doelen** dialoogvenster wordt weergegeven, als uw werkruimte is al een compute-resource, kunt u dit nu.  Selecteer anders **nieuw**.

    > [!NOTE]
    > De visuele interface kunt alleen met het uitvoeren van experimenten voor Machine Learning-Computing doelen. Andere compute-doelen worden niet weergegeven.

1. Geef een naam voor de compute-resource.

1. Selecteer **Uitvoeren**.

    ![Setup compute-doel](./media/aml-ui-create-training-compute/set-compute.png)

    De compute-resource wordt nu gemaakt. De status bekijken in de rechterbovenhoek van het experiment. 

    > [!NOTE]
    > Het duurt ongeveer vijf minuten om een compute-resource te maken. Nadat de resource is gemaakt, kunt u deze kunt gebruiken en overslaan van deze wachttijd voor toekomstige planningen.
    >
    > De compute-resource wordt automatisch geschaald op 0 knooppunten wanneer deze niet-actieve om kosten te besparen.  Wanneer u deze na een vertraging opnieuw gebruikt, kan ongeveer 5 minuten wachttijd opnieuw optreden tijdens het vorige omhoog schalen.
