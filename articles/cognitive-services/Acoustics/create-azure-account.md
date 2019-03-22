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
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 7a7f5f6738b4bc96b6248deb062c7b3f63048148
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58309670"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Project akoestische Azure Batch-Account instellen
Deze instructies beschrijft het instellen van een Azure Batch-account voor gebruik met Project akoestische Unity en Unreal engine-integraties.

## <a name="get-an-azure-subscription"></a>Neem een abonnement op Azure
Een [Azure-abonnement](https://azure.microsoft.com/free/) is vereist voordat het instellen van de Batch- en Storage-accounts. Als u bent aangemeld voor de eerste keer, biedt Azure een paar gedurende beperkte tijd gratis resources en $200 aan tegoed.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch- en storage-accounts maken
Vervolgens gaat u als volgt [deze instructies](https://docs.microsoft.com/azure/batch/batch-account-create-portal) aan uw Azure-Batch instellen en Azure Storage-accounts die zijn gekoppeld.

Kies de standaardopties voor Batch- en Storage-accounts:
  
  ![Schermopname van Azure Batch nieuwe accounts opties met standaardinstellingen](media/new-batch-account-create.png)

  ![Schermopname van Azure-nieuwe opslagaccounts die opties met standaardinstellingen](media/batch-storage-account-create.png)

Het duurt een paar minuten voor Azure voor het implementeren van de accounts. Zoek naar een melding voor voltooiing van in de rechterbovenhoek van de portal.
  
  ![Schermopname van het Azure-accounts geïmplementeerd-melding](media/batch-accounts-deploy-notification.png)

Uw accounts worden nu weergegeven op uw dashboard.
  
  ![Schermafbeelding van de Azure portal-dashboard met een Batch- en Storage-account](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Akoestische bake gebruikersinterface met Azure-referenties instellen
Klik op de koppeling van het Batch-account op het dashboard en klik op de **sleutels** koppeling op de pagina van de Batch-account voor toegang tot uw referenties.
  
  ![Schermopname van Azure Batch-account met een koppeling naar de pagina sleutels is gemarkeerd](media/batch-access-keys.png)

  ![Schermopname van Azure Batch-account-pagina voor sleutels met toegangssleutels](media/batch-keys-info.png)

Klik op de **Opslagaccount** koppeling op de pagina voor toegang tot de referenties van uw Azure Storage-account.
  
  ![Schermopname van Azure Storage-account-pagina voor sleutels met toegangssleutels](media/storage-keys-info.png)

Voer deze referenties in de [bake-invoegtoepassing voor Unity](unity-baking.md) of [Unreal bake invoegtoepassing](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Knooppunttypen en ondersteuning voor regio
Project akoestische vereist dat fsv2 - en H-serie berekenen geoptimaliseerde virtuele machine van Azure-knooppunten die niet kunnen worden ondersteund in alle Azure-regio's. Controleer of [deze tabel](https://azure.microsoft.com/global-infrastructure/services) om ervoor te zorgen alsof u de juiste locatie voor uw Batch-account.
![Schermopname van Azure Virtual Machines per regio](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Een upgrade van uw quotum aanvragen
Azure Batch-accounts worden ingericht voor het account maken met een limiet van 20 compute-resources. Wilt u mogelijk verhogen van deze limiet voor snellere bake momenten, omdat u kunt uw workload akoestische parallel over veel knooppunten, tot maximaal het aantal punten in uw scène test. U kunt een verhoging aanvragen door te klikken op de **quotum** koppelen op de portalpagina van uw Azure Batch en vervolgens te klikken op **aanvraag Quota verhogen**:

![Schermafbeelding van de Quota van de Azure-pagina](media/azure-quotas.png)

## <a name="next-steps"></a>Volgende stappen
* Integratie van de invoegtoepassing Project akoestische in uw [Unity](unity-integration.md) of [Unreal](unreal-integration.md) project

