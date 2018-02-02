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
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: 74488b85ec524e4ad4c06a639a16ddbfd54b3154
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>De werking van PowerShell in Azure Cloud-Shell (Preview)
PowerShell in de Cloud-Shell (Preview) aanhoudt bestanden met de volgende methode: 
* Koppelen van de opgegeven Azure-bestandsshare als `clouddrive` in uw `$Home` map voor directe bestandsshare interactie.

## <a name="list-cloud-drive-azure-file-shares"></a>Lijst met Cloud station Azure-bestandsshares
De `Get-CloudDrive` opdracht haalt de Azure-share bestandsgegevens op dit moment zijn gekoppeld met de Cloud-station in de Cloud-Shell. <br>
![Get-CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Cloud-station ontkoppelen
U kunt een Azure-bestandsshare die gekoppeld aan Cloud-Shell op elk gewenst moment ontkoppelen. Als de Azure-bestandsshare is verwijderd, wordt u gevraagd om te maken en koppelen van een nieuwe Azure-bestandsshare op de volgende sessie.

De `Dismount-CloudDrive` opdracht een Azure-bestandsshare ontkoppelt van het huidige opslagaccount. Ontkoppelen van het Cloud-station, wordt de huidige sessie beëindigd. De gebruiker wordt gevraagd om te maken en een nieuwe Azure-bestandsshare koppelen tijdens de volgende.
![Running Dismount-CloudDrive](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Volgende stappen
[Snelstartgids voor PowerShell](quickstart-powershell.md) <br>
[Meer informatie over Azure-bestanden](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Meer informatie over opslag labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>