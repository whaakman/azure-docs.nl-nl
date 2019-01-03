---
title: Functies - rendering Azure Batch
description: Het gebruik van mogelijkheden voor rendering in Azure Batch
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 2f423de63b7cec24d9d9c4541ddab0ac38b1076a
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542304"
---
# <a name="using-azure-batch-rendering"></a>Met behulp van Azure Batch rendering

Er zijn verschillende manieren waarop u met Azure Batch rendering:

* API's:
  * Schrijf code met behulp van een van de Batch-API's.  Ontwikkelaars kunnen Azure Batch-mogelijkheden integreren in hun bestaande toepassingen of de werkstroom, of cloud of op basis van on-premises.
* Vanaf de opdrachtregel-hulpprogramma's:
  * De [Azure vanaf de opdrachtregel](https://docs.microsoft.com/cli/azure/) of [PowerShell](https://docs.microsoft.com/powershell/azure/overview) kan worden gebruikt voor het gebruik van de Batch script.
  * In het bijzonder de [Batch CLI-ondersteuning voor sjabloon](https://docs.microsoft.com/azure/batch/batch-cli-templates) maakt het veel gemakkelijker te maken van pools en taken verzendt.
* Batch Explorer-gebruikersinterface:
  * [Batch Explorer](https://github.com/Azure/BatchLabs) is een platformoverschrijdende clienthulpprogramma waarmee ook Batch-accounts worden beheerd en bewaakt.
  * Voor elk van de rendering-toepassingen zijn een aantal sjablonen voor groep van toepassingen en -taak opgegeven die kunnen worden gebruikt om eenvoudig te maken van toepassingen en voor het verzenden van taken.  Een set van sjablonen wordt vermeld in de gebruikersinterface van de toepassing met de sjabloonbestanden die toegankelijk zijn vanuit GitHub.
  * Aangepaste sjablonen helemaal worden geschreven of de opgegeven sjablonen van GitHub kunnen worden gekopieerd en gewijzigd.
* Client-toepassing-plug-ins:
  * Invoegtoepassingen zijn beschikbaar die Batch rendering worden gebruikt als rechtstreeks in het ontwerp van de client en ontwerptoepassingen toestaan.  De plug-ins voornamelijk aanroepen van de Batch Explorer-toepassing met contextuele informatie over de huidige 3D-model en bevat functies voor het beheren van activa.

De beste manier om te proberen van Azure Batch rendering en eenvoudigste manier voor eindgebruikers die geen ontwikkelaars en niet Azure-experts, is met de Batch Explorer-toepassing, hetzij rechtstreeks of aangeroepen vanuit een clienttoepassing invoegtoepassing.

## <a name="using-batch-explorer"></a>Batch Explorer gebruiken

Voor een stapsgewijze zelfstudie voor het gebruik van Batch Explorer om uit te voeren rendering, Zie de [Blender zelfstudie](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender).

### <a name="download-and-install"></a>Downloaden en installeren

Batch Explorer [downloads zijn beschikbaar](https://azure.github.io/BatchExplorer/) voor Windows, OSX en Linux.

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Met behulp van sjablonen voor maken van pools en taken uitvoeren

Er is een uitgebreide set met sjablonen beschikbaar voor gebruik met Batch Explorer waarmee u eenvoudig toepassingen maken en verzenden van taken voor de verschillende rendering-toepassingen zonder te geven van alle eigenschappen die zijn vereist voor het maken van pools, jobs en taken rechtstreeks met Batch.  De sjablonen die beschikbaar zijn in de Batch Explorer worden opgeslagen en zichtbaar zijn in [een GitHub-opslagplaats](https://github.com/Azure/BatchExplorer-data/tree/master/ncj).

![Batch Explorer-galerie](./media/batch-rendering-using/batch-explorer-gallery.png)

Sjablonen zijn opgegeven die geschikt zijn voor alle toepassingen aanwezig op de VM-installatiekopieën van Marketplace-rendering.  Voor elke toepassing bestaan meerdere sjablonen, waaronder sjablonen van de pool dat ze voor CPU en GPU pools, Windows en Linux-pools; Taaksjablonen zijn volledig frame of naast elkaar Blender rendering en V-Ray gedistribueerde weergaven. Het aantal opgegeven zal worden uitgebreid na verloop van tijd die geschikt zijn voor andere functies van Batch, zoals automatisch schalen van toepassingen.

Het is ook mogelijk voor aangepaste sjablonen moeten worden geproduceerd, helemaal of door het wijzigen van de opgegeven sjablonen. Aangepaste sjablonen kunnen worden gebruikt door het selecteren van het item 'Lokale sjablonen' in de sectie 'Galerie' van de Batch Explorer.

### <a name="file-system-and-data-movement"></a>Systeem- en -verplaatsing van bestand

De sectie 'Gegevens' in de Batch Explorer kunt bestanden die moeten worden gekopieerd tussen een lokale bestandssysteem en Azure Storage-accounts.

## <a name="client-application-plug-ins"></a>Client-invoegtoepassingen voor toepassing

Invoegtoepassingen zijn beschikbaar voor enkele van de clienttoepassingen.  Invoegtoepassingen toestaan pools en taken rechtstreeks vanuit de toepassing worden gemaakt of aanroepen van Batch Explorer.

* [Blender](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)
* [Autodesk 3ds Max.](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
* [Autodesk Maya](https://github.com/Azure/azure-batch-maya)

## <a name="next-steps"></a>Volgende stappen

Voor voorbeelden van Batch rendering proberen de twee zelfstudies:

* [Rendering met de Azure CLI](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Rendering met behulp van Batch Explorer](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)