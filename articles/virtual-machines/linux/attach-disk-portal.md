---
title: Een gegevensschijf koppelen aan een Linux-VM | Microsoft Docs
description: De portal gebruiken voor het nieuwe of bestaande gegevensschijf koppelen aan een Linux-VM.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 4acfe53d68db3192c1f6c3c9e5f91b55bd5df7b8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30832497"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>De portal gebruiken voor een gegevensschijf koppelen aan een Linux-VM 
In dit artikel leest u hoe nieuwe en bestaande schijven koppelen aan een virtuele Linux-machine via de Azure portal. U kunt ook [een gegevensschijf koppelen aan een virtuele machine van Windows in de Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Voordat u schijven aan de virtuele machine koppelt, controleert u de volgende tips:

* De omvang van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Premium-opslag gebruiken, moet u een virtuele machine DS-serie- of GS-serie. U kunt zowel Premium en Standard schijven met deze virtuele machines. Premium-opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../windows/premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Schijven die zijn gekoppeld aan virtuele machines zijn daadwerkelijk .vhd bestanden die zijn opgeslagen in Azure. Zie voor meer informatie [over schijven en virtuele harde schijven voor virtuele machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-the-virtual-machine"></a>De virtuele machine gevonden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Aan de virtuele machines u pagina **Essentials**, klikt u op **schijven**.
   
    ![Instellingen voor de schijf openen](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>Een nieuwe schijf koppelen

1. Op de **schijven** deelvenster, klikt u op **+ gegevensschijf toevoegen**.
2. Klik op de vervolgkeuzelijst voor **naam** en selecteer **maken-schijf**:

    ![Maken van Azure beheerde schijven](./media/attach-disk-portal/create-new-md.png)

3. Voer een naam voor de beheerde schijf. Controleer de standaardinstellingen, indien nodig bijwerken en klik vervolgens op **maken**.
   
   ![Controleer de Schijfinstellingen](./media/attach-disk-portal/create-new-md-settings.png)

4. Klik op **opslaan** schijf voor de beheerde maken en bijwerken van de VM-configuratie:

   ![Nieuwe Azure beheerd schijf opslaan](./media/attach-disk-portal/confirm-create-new-md.png)

5. Nadat Azure de schijf wordt gemaakt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen voor de schijf van de virtuele machine onder **gegevensschijven**. Als beheerde schijven een bron op het hoogste niveau zijn, de schijf wordt weergegeven in de hoofdmap van de resourcegroep:

   ![Azure-schijf beheerd in de resourcegroep](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen
1. Op de **schijven** deelvenster, klikt u op **+ gegevensschijf toevoegen**.
2. Klik op de vervolgkeuzelijst voor **naam** om een lijst met bestaande beheerde schijven toegankelijk voor uw Azure-abonnement weer te geven. Selecteer de beheerde schijf koppelen:

   ![Bestaande Azure beheerd schijf koppelen](./media/attach-disk-portal/select-existing-md.png)

3. Klik op **opslaan** voor het koppelen van de bestaande beheerde schijf en de VM-configuratie bij te werken:
   
   ![Beheerde Azure-schijf updates opslaan](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Nadat Azure de schijf is gekoppeld aan de virtuele machine, wordt deze weergegeven in de instellingen voor de schijf van de virtuele machine onder **gegevensschijven**.



## <a name="next-steps"></a>Volgende stappen
U kunt ook [een gegevensschijf koppelen](add-disk.md) met de Azure CLI.
