---
title: Behouden van de bestanden in PowerShell in Azure Cloud-Shell (Preview) | Microsoft Docs
description: Overzicht van hoe Azure Cloud Shell zich blijft bestanden voordoen.
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: d0bc16bc951fce17235d8070012de44ebab89888
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>De werking van PowerShell in Azure Cloud-Shell (Preview)
PowerShell in de Cloud-Shell (Preview) aanhoudt bestanden met de volgende methode: 
* Koppelen van de opgegeven Azure-bestandsshare als `clouddrive` in uw `$Home` map voor directe bestandsshare interactie.

## <a name="list-cloud-drive-azure-file-shares"></a>Lijst met Cloud station Azure-bestandsshares
De `Get-CloudDrive` opdracht haalt de Azure-share bestandsgegevens op dit moment zijn gekoppeld met de Cloud-station in de Cloud-Shell. <br>
![Get-CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Cloud-station ontkoppelen
U kunt een Azure-bestandsshare die gekoppeld aan Cloud-Shell op elk gewenst moment ontkoppelen. Als de Azure-bestandsshare is verwijderd, wordt u gevraagd om te maken en koppelen van een nieuwe Azure-bestandsshare op de volgende sessie.

De `Dismount-CloudDrive` opdracht een Azure-bestandsshare ontkoppelt van het huidige opslagaccount. Ontkoppelen van het Cloud-station, wordt de huidige sessie beëindigd. De gebruiker wordt gevraagd om te maken en een nieuwe Azure-bestandsshare koppelen tijdens de volgende.
![Ontkoppeling CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Volgende stappen
[Snelstartgids voor PowerShell](quickstart-powershell.md) <br>
[Meer informatie over Azure-bestanden](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Meer informatie over opslag labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>