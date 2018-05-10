---
title: Azure Machine Learning en FPGAs
description: Informatie over het versnellen van modellen en diep neural netwerken met FPGAs.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: 493b3f8de617146af534349e18ed49b83c46dee8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-a-field-programmable-gate-array-fpga"></a>Wat is een veld programmeerbare gate matrix (FPGA)?

Veld programmeerbare gate matrices (FPGA) zijn geïntegreerd circuits die opnieuw kunnen worden geconfigureerd als nodig is. U kunt een FPGA wijzigen indien nodig voor het implementeren van nieuwe logica. Azure Machine Learning Hardware versnelde modellen kunt u om te trained modellen implementeren op FPGAs in de Azure-cloud.

Deze functionaliteit wordt aangedreven door Project Brainwave die diep neural networks (DNN) vertalen in een programma FPGA verwerkt. 

## <a name="why-use-an-fpga"></a>Waarom een FPGA gebruiken?

FPGAs bieden zeer lage latentie en ze zijn met name nuttig voor score berekenen voor gegevens op batchgrootte een (Er is geen vereiste voor een grote batchgrootte).  Ze zijn rendabele en beschikbaar zijn in Azure.  Project Brainwave kunt vooraf getraind DNNs parallelize via deze FPGAs uit uw service te schalen.

## <a name="next-steps"></a>Volgende stappen

[Een model als een webservice op een FPGA implementeren](how-to-deploy-fpga-web-service.md)

[Informatie over het installeren van de SDK FPGA](reference-fpga-package-overview.md)