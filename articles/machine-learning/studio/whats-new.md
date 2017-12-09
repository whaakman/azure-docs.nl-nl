---
title: Wat is er nieuw in Azure Machine Learning | Microsoft Docs
description: Nieuwe functies die beschikbaar in Azure Machine Learning zijn.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 82ec23f4edba88ec75c2073e24cad24b66323d72
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="whats-new-in-azure-machine-learning"></a>Wat is nieuw in Azure Machine Learning?

### <a name="the-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-the-following-feature"></a>De maart 2017 release van Microsoft Azure Machine Learning-updates biedt de volgende functie:



* Toegewezen capaciteit voor Azure Machine Learning BES taken

    Machine Learning Batch-Pool verwerken maakt gebruik van de [Azure Batch](../../batch/batch-technical-overview.md) bieden door de klant beheerd schaal voor de Service Batch uitvoering van Azure Machine Learning-service. Batch-Pool verwerking kunt u Azure Batch-pools waarop kunt u batchtaken verzenden en ze worden uitgevoerd in een voorspelbare wijze maken.

    Zie voor meer informatie [Azure Batch-service voor Machine Learning-taken](dedicated-capacity-for-bes-jobs.md).


### <a name="the-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a>De augustus 2016-release van Microsoft Azure Machine Learning-updates bieden de volgende functies:
* Klassieke webservices kunnen nu worden beheerd in het nieuwe [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/) portal die één plaats voor het beheren van alle aspecten van uw Web-service biedt.    
  * Waarmee u webservice [gebruiksstatistieken](manage-new-webservice.md).
  * Vereenvoudigt het testen van Azure Machine Learning extern-Request-aanroepen met voorbeeldgegevens.
  * Biedt een nieuwe Batch-Service voor uitvoering testpagina voorbeeld gegevens en taak verzending geschiedenis.
  * Eenvoudiger beheer van het eindpunt bevat.

### <a name="the-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a>De juli 2016-release van Microsoft Azure Machine Learning-updates bieden de volgende functies:
* Web-services worden nu beheerd als Azure-resources die worden beheerd via [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) interfaces, waardoor de volgende verbeteringen:
  * Er zijn nieuwe [REST-API's](https://msdn.microsoft.com/library/azure/Dn950030.aspx) op basis van webservices te implementeren en beheren van uw Resource Manager.
  * Er is een nieuwe [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/) portal die één plaats voor het beheren van alle aspecten van uw Web-service biedt.
* Bevat een nieuwe op basis van abonnement, meerdere landen/regio web service-implementatiemodel met Resource Manager gebaseerde API's het gebruik van de Resource Manager Resource Provider voor webservices.
* Introduceert nieuwe [prijsstelling](https://azure.microsoft.com/pricing/details/machine-learning/) en plan de beheermogelijkheden met behulp van de nieuwe Resource Manager-RP voor facturering.
  * U kunt nu [uw webservice implementeren in meerdere regio's](how-to-deploy-to-multiple-regions.md) zonder een abonnement maken in elke regio.
* Biedt webservice [gebruiksstatistieken](manage-new-webservice.md).
* Vereenvoudigt het testen van Azure Machine Learning extern-Request-aanroepen met voorbeeldgegevens.
* Biedt een nieuwe Batch-Service voor uitvoering testpagina voorbeeld gegevens en taak verzending geschiedenis.

Bovendien de Machine Learning Studio is bijgewerkt zodat u kunt implementeren op het nieuwe model van de Web-service of doorgaan om te implementeren op het klassieke model van de Web-service. 

