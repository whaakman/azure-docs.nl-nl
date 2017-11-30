---
title: Azure migreren assessment instellingen aanpassen | Microsoft Docs
description: Hierin wordt beschreven hoe instellen en uitvoeren van een beoordeling voor VMware-machines migreren naar Azure met de Azure-migratie-Planner
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a068b9c7-5f87-4fe1-90b9-3be48d91aa3f
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 20aeb2073ad307952f92c8377bc9d78169f1756c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="customize-an-assessment"></a>Een beoordeling aanpassen

[Azure migreren](migrate-overview.md) beoordelingen maakt met de standaardinstellingen. U kunt deze standaardinstellingen van de instructies in dit artikel kunt wijzigen na het maken van een beoordeling.


## <a name="edit-assessment-values"></a>Assessment waarden bewerken

1. In Azure migreren project **beoordelingen** pagina, selecteer de beoordeling en klikt u op **eigenschappen bewerken**.
2. Wijzig de instellingen in overeenstemming met de onderstaande tabel.

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **TARGET-locatie** | De Azure-locatie waarnaar u wilt migreren. |  VS West 2 is de standaardlocatie.
    **Redundantie van gegevensopslag** | Het type opslag die door de Azure VM's na de migratie wordt gebruikt. | Alleen [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replicatie wordt momenteel ondersteund.
    **Comfort factor** | Comfort factor is een buffer die wordt gebruikt tijdens de evaluatie. Gebruik dit voor het account voor zaken zoals seizoensgebonden gebruik, geschiedenis van geheugenprestaties korte, waarschijnlijk in de toekomst het gebruik van verhoogd. | Standaard wordt 1.3 x.
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
