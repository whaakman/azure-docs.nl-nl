---
title: Azure migreren assessment instellingen aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een beoordeling voor VMware-machines migreren naar Azure met de Azure-migratie-Planner
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: ce47790f6214864afdba33eb5cbe3a9e49b81cd5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure migreren](migrate-overview.md) beoordelingen maakt met de standaardinstellingen. U kunt deze standaardinstellingen van de instructies in dit artikel kunt wijzigen na het maken van een beoordeling.


## <a name="edit-assessment-values"></a>Assessment waarden bewerken

1. In Azure migreren project **beoordelingen** pagina, selecteer de beoordeling en klikt u op **eigenschappen bewerken**.
2. Wijzig de instellingen in overeenstemming met de onderstaande tabel.

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren. |  VS West 2 is de standaardlocatie.
    **Opslagredundantie** | Het type opslag dat na de migratie door de virtuele Azure-machines wordt gebruikt. | Alleen [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replicatie wordt momenteel ondersteund.
    **Comfortfactor** | Comfort factor is een buffer die wordt gebruikt tijdens de evaluatie. Gebruik dit voor het account voor zaken zoals seizoensgebonden gebruik, geschiedenis van geheugenprestaties korte, waarschijnlijk in de toekomst het gebruik van verhoogd. | Standaard wordt 1.3 x.
    **Geschiedenis van leiden** | De tijd die wordt gebruikt bij het evalueren van de prestatiegeschiedenis. | De standaardwaarde is één maand.
    **Gebruik percentiel** | De percentielwaarde in overweging moet nemen voor de geschiedenis van leiden. | Standaard is 95%.
    **Prijscategorie** | U kunt opgeven de [prijscategorie](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) voor een virtuele machine.  | Standaard de [standaard](../virtual-machines/windows/sizes-general.md) laag wordt gebruikt.
    **Aanbieding** | [Azure biedt](https://azure.microsoft.com/support/legal/offer-details/) die van toepassing is. | [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) is de standaardinstelling.
    **Valuta** | Factureringsvaluta. | De standaardwaarde is VS bedragen.
    **Korting (%)** | Abonnement-specifieke korting verschijnt boven op een aanbieding. | De standaardinstelling is 0%.
    **Azure hybride gebruik Benefit** | Geeft aan of u bent geregistreerd de [Azure hybride gebruik voordeel](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als op Ja instelt, Windows Azure-prijzen zijn kunt u overwegen voor VM's van Windows. | Standaard is Ja.

3. Klik op **opslaan** bijwerken van de evaluatie.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.
