---
title: Project akoestische Azure Batch account instellen
titlesuffix: Azure Cognitive Services
description: In deze procedure wordt beschreven hoe u een Azure Batch account kunt instellen voor gebruik met de geluids eenheid van het project en de integratie van de Unreal-Engine.
services: cognitive-services
author: ashtat
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: f11dfda62ebb53aba6254f2db4eace7c524141d4
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704884"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Project akoestische Azure Batch account instellen
In deze procedure wordt beschreven hoe u een Azure Batch account kunt instellen voor gebruik met de geluids eenheid van het project en de integratie van de Unreal-Engine.

## <a name="get-an-azure-subscription"></a>Een Azure-abonnement nemen
Een [Azure-abonnement](https://azure.microsoft.com/free/) is vereist voordat u batch-en opslag accounts instelt. Als u zich voor de eerste keer aanmeldt, biedt Azure een beperkt aantal tijdrovende, gratis bronnen en $200 tegoed.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch-en opslag accounts maken
Volg vervolgens [deze instructies](https://docs.microsoft.com/azure/batch/batch-account-create-portal) voor het instellen van uw Azure batch en gekoppelde Azure Storage accounts.

Kies standaard opties voor zowel batch-als opslag accounts:
  
  ![Scherm opname van Azure Batch nieuwe account opties standaard instellingen weer geven](media/new-batch-account-create.png)

  ![Scherm opname van Azure Storage nieuwe account opties standaard instellingen weer geven](media/batch-storage-account-create.png)

Het duurt enkele minuten voordat de accounts zijn geïmplementeerd in Azure. Zoek in de rechter bovenhoek van de portal naar een voltooiings bericht.
  
  ![Scherm afbeelding van geïmplementeerde melding van Azure-accounts](media/batch-accounts-deploy-notification.png)

Uw accounts moeten nu zichtbaar zijn op uw dash board.
  
  ![Scherm afbeelding van Azure Portal dash board met een batch-en opslag account](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Akoestische maken-gebruikers interface instellen met Azure-referenties
Klik op de koppeling batch-account op het dash board en klik vervolgens op de koppeling **sleutels** op de pagina batch-account om toegang te krijgen tot uw referenties.
  
  ![Scherm afbeelding van Azure Batch account met een koppeling naar de pagina sleutels gemarkeerd](media/batch-access-keys.png)

  ![Scherm afbeelding van de pagina Azure Batch account sleutels met toegangs toetsen](media/batch-keys-info.png)

Klik op de koppeling naar het **opslag account** op de pagina om toegang te krijgen tot de referenties van uw Azure Storage-account.
  
  ![Scherm afbeelding van de pagina Azure Storage account sleutels met toegangs toetsen](media/storage-keys-info.png)

Geef deze referenties op in de [maken unit eenheid](unity-baking.md) of [Unreal maken-invoeg toepassing](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Knooppunt typen en regio ondersteuning
Voor project akoestische is een Azure VM-knoop punt met Fsv2-en H-serie reken optimalisatie vereist die mogelijk niet worden ondersteund in alle Azure-regio's. Controleer [deze tabel](https://azure.microsoft.com/global-infrastructure/services) om te controleren of u de juiste locatie voor uw batch-account hebt gekozen.
![Scherm afbeelding met Azure Virtual Machines per regio](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Uw quotum bijwerken
Azure Batch accounts worden ingericht bij het maken van een account met een limiet van 20 reken kernen. We willen mogelijk deze limiet verhogen voor snellere maken tijden, omdat u de werk belasting van uw akoestische op verschillende knoop punten kunt parallelliseren, tot het aantal test punten in uw scène. U kunt een quotum verhoging aanvragen door te klikken op de **quotum** koppeling op uw Azure batch portal pagina en vervolgens op **aanvraag quotum toename**te klikken:

![Scherm opname van de Azure-quotum pagina](media/azure-quotas.png)

## <a name="next-steps"></a>Volgende stappen
* De invoeg toepassing voor project akoestische in uw [eenheids](unity-integration.md) -of [Unreal](unreal-integration.md) -project integreren

