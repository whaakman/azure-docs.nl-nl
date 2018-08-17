---
title: Instellen van Azure-accounts voor akoestische - Cognitive Services
description: Volg deze handleiding voor het instellen van Azure Batch- en Storage-accounts die nodig zijn voor het werken met akoestische.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181557"
---
# <a name="create-an-azure-batch-account"></a>Azure Batch-account maken
Volg deze handleiding voor het instellen van Azure Batch- en Storage-accounts die nodig zijn voor het werken met akoestische. Zie voor meer informatie over de Unity-invoegtoepassing die is ontwikkeld als onderdeel van het Project akoestische [wat is er akoestische](what-is-acoustics.md). Zie voor meer informatie over het akoestische opnemen in uw Unity-project [aan de slag](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Neem een abonnement op Azure
Een [Azure-abonnement](https://azure.microsoft.com/free/) is vereist voordat het instellen van de Batch- en Storage-accounts. Als u bent aangemeld voor de eerste keer, biedt Azure een paar gedurende beperkte tijd gratis resources en $200 aan tegoed.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch- en storage-accounts maken
Vervolgens gaat u als volgt [deze instructies](https://docs.microsoft.com/azure/batch/batch-account-create-portal) aan uw Azure-Batch instellen en Azure Storage-accounts die zijn gekoppeld.

Kies de standaardopties voor Batch- en Storage-accounts:
  
  ![Nieuw Batch-Account](media/NewBatchAccountCreate.png)

  ![Nieuwe Storage-Account](media/BatchStorageAccountCreate.png)

Het duurt een paar minuten voor Azure voor het implementeren van de accounts. Zoek naar een melding voor voltooiing van in de rechterbovenhoek van de portal.
  
  ![Accounts die zijn geïmplementeerd](media/BatchAccountsDeployNotification.png)

Uw accounts worden nu weergegeven op uw dashboard.
  
  ![Portal-Dashboard](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Akoestische bake gebruikersinterface met Azure-referenties instellen
Klik op de koppeling van het Batch-account op het dashboard en klik op de **sleutels** koppeling op de pagina van de Batch-account voor toegang tot uw referenties.
  
  ![Koppeling van batch-sleutels](media/BatchAccessKeys.png)

  ![De referenties van de batch-Account](media/BatchKeysInfo.png)

Klik op de **Opslagaccount** koppeling op de pagina voor toegang tot de referenties van uw Azure Storage-account.
  
  ![Opslagaccountreferenties](media/StorageKeysInfo.png)

Deze referenties invoeren in het tabblad Bake, zoals beschreven in de [UI scenario verdient](bake-ui-walkthrough.md).

## <a name="node-types-and-region-support"></a>Knooppunttypen en ondersteuning voor regio
Project akoestische vereist dat f - en H-serie berekenen geoptimaliseerde virtuele machine van Azure-knooppunten die niet kunnen worden ondersteund in alle Azure-regio's. Controleer of [deze tabel](https://azure.microsoft.com/global-infrastructure/services) om ervoor te zorgen alsof u de juiste locatie voor uw Batch-account. Virtuele machines worden ondersteund op dit moment-H-serie in VS-Oost, Noord-centraal VS, Zuid-centraal VS, VS-West, VS-West 2, Noord-Europa, West-Europa en Japan-West.

## <a name="upgrading-your-quota"></a>Een upgrade van uw quotum aanvragen
Azure Batch-accounts worden ingericht voor het account maken met een limiet van 20 compute-resources. Kunt u deze overschrijdt voor snellere bake momenten, omdat u kunt uw workload akoestische parallel over veel knooppunten, tot maximaal het aantal punten in uw scène test. U kunt een verhoging aanvragen door te klikken op de **quotum** koppelen op de portalpagina van uw Azure Batch en vervolgens te klikken op **aanvraag Quota verhogen**:

![Azure verhoging](media/azurequotas.png)

## <a name="next-steps"></a>Volgende stappen
* Aan de slag [akoestische integreren in uw Unity-project](getting-started.md)
* Verken de [voorbeeld scène](sample-walkthrough.md)

