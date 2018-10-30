---
title: Wat is er nieuw in Azure Machine Learning Studio | Microsoft Docs
description: Nieuwe functies die beschikbaar in Azure Machine Learning Studio zijn.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.openlocfilehash: 2f990ce30b66392e8a649e4e88b21457d9fe223b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212655"
---
# <a name="whats-new-in-azure-machine-learning-studio"></a>Wat is er nieuw in Azure Machine Learning Studio

## <a name="october-2018"></a>Oktober 2018

De engine voor het R-taal in de [R-Script uitvoeren](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script) module is een nieuwe versie van R-runtimeversie--Microsoft R Open (BHT) 3.4.4 toegevoegd. BHT 3.4.4 is gebaseerd op open-source CRAN R 3.4.4 en is daarom compatibel met pakketten die geschikt is voor die versie van R.  Meer informatie over de ondersteunde R-pakketten in het artikel '[R-pakketten ondersteund door Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning#bkmk_List)'.

## <a name="march-2017"></a>Maart 2017 
Deze versie van Microsoft Azure Machine Learning-updates biedt de volgende functie:

* Toegewezen capaciteit voor Azure Machine Learning BES-taken

    Machine Learning Batch-Pool gebruikt verwerken de [Azure Batch](../../batch/batch-technical-overview.md) service voor de klant beheerde scale voor de Service Azure Machine Learning Batch kan worden uitgevoerd. Batchverwerking van toepassingen kunt u Azure Batch-pools waarop u kunt batch-taken indienen en deze uitvoeren op een voorspelbare wijze maken.

    Zie voor meer informatie, [Azure Batch-service voor Machine Learning-taken](dedicated-capacity-for-bes-jobs.md).


## <a name="august-2016"></a>Augustus 2016 
Deze versie van Microsoft Azure Machine Learning-updates bieden de volgende functies:
* Klassieke webservices kunnen nu worden beheerd in de nieuwe [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/) portal één locatie biedt voor het beheren van alle aspecten van uw Web-service.    
  * Waarmee u webservice [gebruiksstatistieken](manage-new-webservice.md).
  * Vereenvoudigt het testen van de aanvraag is met Azure Machine Learning extern aanroepen met voorbeeldgegevens.
  * Biedt een nieuwe Batch-Service voor uitvoering van test-pagina met de geschiedenis van voorbeeld van gegevens en taak indienen.
  * Eenvoudiger beheer van het eindpunt biedt.

## <a name="july-2016"></a>Juli 2016 
Deze versie van Microsoft Azure Machine Learning-updates bieden de volgende functies:
* Web-services worden nu beheerd als Azure-resources die worden beheerd via [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) interfaces, waardoor de volgende verbeteringen:
  * Er zijn nieuwe [REST-API's](https://msdn.microsoft.com/library/azure/Dn950030.aspx) implementeren en beheren van uw Resource Manager gebaseerde webservices.
  * Er is een nieuwe [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/) portal één locatie biedt voor het beheren van alle aspecten van uw Web-service.
* Bevat een nieuwe op basis van een abonnement, meerdere regio's web service-implementatiemodel met behulp van Resource Manager op basis van gebruik te maken van de Resource Manager-Resourceprovider voor Web Services API's.
* Introduceert nieuwe [prijsplannen](https://azure.microsoft.com/pricing/details/machine-learning/) en mogelijkheden voor beheer met behulp van de nieuwe Resource Manager-RP voor facturering van plan bent.
  * U kunt nu [uw webservice implementeert in meerdere regio's](how-to-deploy-to-multiple-regions.md) hoeft te maken van een abonnement in elke regio.
* Biedt webservice [gebruiksstatistieken](manage-new-webservice.md).
* Vereenvoudigt het testen van de aanvraag is met Azure Machine Learning extern aanroepen met voorbeeldgegevens.
* Biedt een nieuwe Batch-Service voor uitvoering van test-pagina met de geschiedenis van voorbeeld van gegevens en taak indienen.

Bovendien de Machine Learning Studio is bijgewerkt zodat u kunt implementeren naar het nieuwe model van de Web-service of doorgaan om in het klassieke model voor de Web-service te implementeren. 

