---
title: MongoDB installeren op een Windows virtuele machine in Azure | Microsoft Docs
description: Informatie over het MongoDB installeren op een virtuele machine in Azure gemaakt met het klassieke implementatiemodel met Windows Server.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>MongoDB installeren op een Windows virtuele machine in Azure
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md).  In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Als u wilt installeren en configureren met het implementatiemodel van Resource Manager MongoDB, Zie [in dit artikel](../install-mongodb.md).

[MongoDB] [ MongoDB] is een populaire open-source, hoogwaardige NoSQL-database. In dit artikel begeleidt u bij het maken van een Windows Server-virtuele machine (VM) met de [Azure-portal][AzurePortal]. U maakt en een gegevensschijf koppelen aan de virtuele machine voor het installeren en configureren van MongoDB. Hebt u een bestaande virtuele machine in Azure die u wilt gebruiken, kunt u meteen naar gaan [installeren en configureren van MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Een virtuele machine maken waarop Windows Server wordt uitgevoerd
Volg deze instructies voor het maken van een virtuele machine.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> U kunt een eindpunt voor MongoDB toevoegen tijdens het maken van de virtuele machine en als volgt configureren: deze als de naam **Mongo**, gebruiken **TCP** als het protocol en de openbare en particuliere poorten instellen op **27017**.
>
>

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Als u wilt opslag bieden voor de virtuele machine, een gegevensschijf koppelen en initialiseren zodat Windows kan worden gebruikt. Als u een gegevensschijf al hebt, kunt u de bestaande schijf koppelen of kunt u een lege schijf koppelen.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Zie voor instructies voor het initialiseren van de schijf, ' How to: initialiseren van een nieuwe gegevensschijf in WindowsServer "in [hoe een gegevensschijf koppelen aan een virtuele Windows-machine](attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installeren en uitvoeren van MongoDB op de virtuele machine
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u geleerd hoe maakt u een virtuele machine met Windows Server op afstand verbinding mee maken en een gegevensschijf koppelen.  U hebt ook geleerd hoe installeren en configureren van MongoDB op Windows gebaseerde VM. U kunt nu toegang tot MongoDB op Windows gebaseerde virtuele machine aan de hand van de geavanceerde onderwerpen in de [MongoDB-documentatie][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

