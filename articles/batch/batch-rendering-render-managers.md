---
title: Renderen manager support - Azure Batch
description: Met behulp van Azure voor het renderen met behulp van Azure Batch rendering manager-integratie
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 6878d3b4fc8648db540d016389747eceb45d936a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436165"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Met behulp van Azure Batch met render farm managers

Als u een bestaande renderen on-premises-farm, en vervolgens is het zeer waarschijnlijk dat een render manager bepaalt de render farm capaciteit en taken weer te geven.

Azure biedt ingebouwde ondersteuning of invoegtoepassingen voor populaire render managers. U kunt vervolgens toevoegen en verwijderen van virtuele machines van Azure, met inbegrip van virtuele machines met de licentieverlening voor de toepassingen waarbij u betaalt voor gebruik en virtuele machines met lage prioriteit.

De volgende render-managers worden ondersteund:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](https://www.royalrender.de/)
* [Thinkbox Deadline](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Gebruik van Azure met PipelineFX Qube

Scripts en instructies voor het inschakelen van Azure Batch pool van virtuele machines moet worden gebruikt als Qube werknemers zijn [de GitHub-opslagplaats](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Gebruik van Azure met Royal Render

Royal Render heeft ingebouwde, integratie van Azure en Azure Batch zodat u kunt om uit te breiden een renderfarm met virtuele machines op basis van Azure. Zie voor een overzicht [de help-bestanden](https://www.royalrender.de/help8/index.html?Cloudrendering.html).

Zie voor een voorbeeld van een klant Royal renderen met behulp van de integratie van Azure, de [Jellyfish Pictures klantverhaal](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Gebruik van Azure met Thinkbox Deadline

Scripts en instructies voor het inschakelen van Azure Batch VM's moet worden gebruikt als de Deadline ondergeschikte servers zich in een pool [de GitHub-opslagplaats](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Volgende stappen

Probeer de Azure Batch-integratie voor uw render-manager, met behulp van de juiste invoegtoepassing en de instructies op GitHub, indien van toepassing.