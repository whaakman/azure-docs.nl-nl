---
title: Renderen manager support - Azure Batch
description: Met behulp van Azure voor het renderen met behulp van Azure Batch rendering manager-integratie
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 4eeece4946b4f957d9f864da7c46d77d119863b5
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539919"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Met behulp van Azure Batch met render farm managers

Als u een bestaande renderen on-premises-farm, en vervolgens is het zeer waarschijnlijk dat een render manager bepaalt de render farm capaciteit en taken weer te geven.

Azure biedt ingebouwde ondersteuning of invoegtoepassingen voor populaire render managers. U kunt vervolgens toevoegen en verwijderen van virtuele machines van Azure, met inbegrip van virtuele machines met de licentieverlening voor de toepassingen waarbij u betaalt voor gebruik en virtuele machines met lage prioriteit.

De volgende render-managers worden ondersteund:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](http://www.royalrender.de/)
* [Thinkbox Deadline](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Gebruik van Azure met PipelineFX Qube

Scripts en instructies voor het inschakelen van Azure Batch pool van virtuele machines moet worden gebruikt als Qube werknemers zijn [de GitHub-opslagplaats](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Gebruik van Azure met Royal Render

Royal Render heeft ingebouwde, integratie van Azure en Azure Batch zodat u kunt om uit te breiden een renderfarm met virtuele machines op basis van Azure. Zie voor een overzicht [de help-bestanden](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Zie voor een voorbeeld van een klant Royal renderen met behulp van de integratie van Azure, de [Jellyfish Pictures klantverhaal](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Gebruik van Azure met Thinkbox Deadline

Scripts en instructies voor het inschakelen van Azure Batch VM's moet worden gebruikt als de Deadline ondergeschikte servers zich in een pool [de GitHub-opslagplaats](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Volgende stappen

Probeer de Azure Batch-integratie voor uw render-manager, met behulp van de juiste invoegtoepassing en de instructies op GitHub, indien van toepassing.