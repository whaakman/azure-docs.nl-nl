---
title: De VM-installatiekopie van een gebruiker maken voor de Azure Marketplace
description: Bevat de stappen en de referenties die zijn vereist voor het maken van een gebruiker VM-installatiekopie.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 11/29/2018
ms.author: pabutler
ms.openlocfilehash: 0005ab517d38903b87889b67449569495e396265
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938330"
---
# <a name="create-a-user-vm-image"></a>Een VM-installatiekopie voor gebruikers maken

In dit artikel wordt uitgelegd dat de twee algemene stappen vereist voor het maken van een niet-beheerde installatiekopie vanaf een gegeneraliseerde VHD.  Verwijzingen worden geleverd om u te begeleiden u bij elke stap: installatiekopie van vastlegt en generaliseer de installatiekopie.


## <a name="capture-the-vm-image"></a>De VM-installatiekopie vastleggen

Volg de instructies in het volgende artikel over het vastleggen van de virtuele machine die overeenkomt met de methode met toegang:

-  PowerShell: [Een niet-beheerde VM-installatiekopie maken van een Azure-VM](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [Hoe u een installatiekopie van een virtuele machine of een VHD maken](../../../virtual-machines/linux/capture-image.md)
-  API: [Virtuele Machines - vastleggen](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>Generaliseren van de VM-installatiekopie

Omdat u hebt de gebruikersinstallatiekopie gegenereerd op basis van een eerder gegeneraliseerde VHD, moet ook worden gegeneraliseerd.  Selecteer opnieuw het volgende artikel die overeenkomt met uw toegangsmechanisme voor.  (U mag hebben al gegeneraliseerd uw schijf wanneer u het vastgelegd.)

-  PowerShell: [De virtuele machine generaliseren](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [Stap 2: VM-installatiekopie maken](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Virtuele Machines - generaliseren](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>Volgende stappen

Vervolgens wordt u [maken van een certificaat](cpp-create-key-vault-cert.md) en op te slaan in een nieuwe Azure Key Vault.  Dit certificaat is vereist voor het tot stand brengen van een beveiligde WinRM-verbinding met de virtuele machine.
