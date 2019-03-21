---
title: Project akoestische Azure Batch-Account instellen
titlesuffix: Azure Cognitive Services
description: Deze instructies beschrijft het instellen van een Azure Batch-account voor gebruik met Project akoestische Unity en Unreal engine-integraties.
services: cognitive-services
author: ashtat
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d3b761630124ef7f72269fe0712bf22647968d59
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137025"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Project akoestische Azure Batch-Account instellen
Deze instructies beschrijft het instellen van een Azure Batch-account voor gebruik met Project akoestische Unity en Unreal engine-integraties.

## <a name="get-an-azure-subscription"></a>Neem een abonnement op Azure
Een [Azure-abonnement](https://azure.microsoft.com/free/) is vereist voordat het instellen van de Batch- en Storage-accounts. Als u bent aangemeld voor de eerste keer, biedt Azure een paar gedurende beperkte tijd gratis resources en $200 aan tegoed.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch- en storage-accounts maken
Vervolgens gaat u als volgt [deze instructies](https://docs.microsoft.com/azure/batch/batch-account-create-portal) aan uw Azure-Batch instellen en Azure Storage-accounts die zijn gekoppeld.

Kies de standaardopties voor Batch- en Storage-accounts:
  
  ![Nieuw Batch-Account](media/new-batch-account-create.png)

  ![Nieuwe Storage-Account](media/batch-storage-account-create.png)

Het duurt een paar minuten voor Azure voor het implementeren van de accounts. Zoek naar een melding voor voltooiing van in de rechterbovenhoek van de portal.
  
  ![Accounts die zijn geïmplementeerd](media/batch-accounts-deploy-notification.png)

Uw accounts worden nu weergegeven op uw dashboard.
  
  ![Portal-Dashboard](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Akoestische bake gebruikersinterface met Azure-referenties instellen
Klik op de koppeling van het Batch-account op het dashboard en klik op de **sleutels** koppeling op de pagina van de Batch-account voor toegang tot uw referenties.
  
  ![Koppeling van batch-sleutels](media/batch-access-keys.png)

  ![De referenties van de batch-Account](media/batch-keys-info.png)

Klik op de **Opslagaccount** koppeling op de pagina voor toegang tot de referenties van uw Azure Storage-account.
  
  ![Opslagaccountreferenties](media/storage-keys-info.png)

Voer deze referenties in de [bake-invoegtoepassing voor Unity](unity-baking.md) of [Unreal bake invoegtoepassing](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Knooppunttypen en ondersteuning voor regio
Project akoestische vereist dat fsv2 - en H-serie berekenen geoptimaliseerde virtuele machine van Azure-knooppunten die niet kunnen worden ondersteund in alle Azure-regio's. Controleer of [deze tabel](https://azure.microsoft.com/global-infrastructure/services) om ervoor te zorgen alsof u de juiste locatie voor uw Batch-account.
![Azure virtuele Machines per regio](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Een upgrade van uw quotum aanvragen
Azure Batch-accounts worden ingericht voor het account maken met een limiet van 20 compute-resources. Wilt u mogelijk verhogen van deze limiet voor snellere bake momenten, omdat u kunt uw workload akoestische parallel over veel knooppunten, tot maximaal het aantal punten in uw scène test. U kunt een verhoging aanvragen door te klikken op de **quotum** koppelen op de portalpagina van uw Azure Batch en vervolgens te klikken op **aanvraag Quota verhogen**:

![Azure verhoging](media/azure-quotas.png)

## <a name="next-steps"></a>Volgende stappen
* Integratie van de invoegtoepassing Project akoestische in uw [Unity](unity-integration.md) of [Unreal](unreal-integration.md) project

