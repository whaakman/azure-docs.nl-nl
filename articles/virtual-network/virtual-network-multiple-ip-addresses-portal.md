---
title: Meerdere IP-adressen voor virtuele machines van Azure - Portal | Microsoft Docs
description: Leer hoe u meerdere IP-adressen toewijzen aan een virtuele machine met behulp van de Azure portal | Resource Manager.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: b1873b770a6b4280b7098c68ecb75cc1411fe453
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650561"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Meerdere IP-adressen toewijzen aan virtuele machines met behulp van de Azure-portal

> [!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
> 
> In dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met behulp van de Azure portal. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt via het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen, de [-implementatiemodellen begrijpen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

Als u maken van een virtuele machine met meerdere IP-adressen of een statisch privé IP-adres wilt, moet u maken met behulp van PowerShell of Azure CLI. Voor meer informatie over hoe, klikt u op de PowerShell of CLI opties aan de bovenkant van dit artikel. U kunt een virtuele machine maken met een enkele dynamisch privé IP-adres en (optioneel) een enkel openbaar IP-adres. De portal te gebruiken met de volgende stappen in de [maken van een Windows-VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [een Linux-VM maken](../virtual-machines/linux/quick-create-portal.md) artikelen. Nadat u de virtuele machine hebt gemaakt, kunt u het type IP-adres van dynamisch wijzigt in statisch en toevoegen van extra IP-adressen met behulp van de portal door de volgende stappen in de [toevoegen IP-adressen aan een virtuele machine](#add) sectie van dit artikel.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt persoonlijke en openbare IP-adressen toevoegen aan een Azure-netwerk-interface via de stappen die volgen. De voorbeelden in de volgende secties wordt ervan uitgegaan dat u al een virtuele machine met de drie IP-configuraties die worden beschreven hebt in de [scenario](#scenario), maar het is niet vereist.

### <a name="coreadd"></a>Core stappen

1. Blader naar de Azure portal op https://portal.azure.com en meld u aan deze, indien nodig.
2. Klik in de portal op **meer services** > type *virtuele machines* in het filtervak in en klik vervolgens op **virtuele machines**.
3. In de **virtuele machines** deelvenster, klikt u op de virtuele machine die u wilt toevoegen van IP-adressen aan. Klik op **netwerkinterfaces** in de virtuele machine deelvenster die wordt weergegeven, en selecteer vervolgens de netwerkinterface die u wilt toevoegen van de IP-adressen aan. In het voorbeeld in de volgende afbeelding, de NIC met de naam *myNIC* van de virtuele machine met de naam *myVM* is geselecteerd:

    ![Netwerkinterface](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. In het deelvenster dat wordt weergegeven voor de NIC die u hebt geselecteerd, klikt u op **IP-configuraties**.

Voer de stappen in een van de secties die volgen, op basis van het type IP-adres die toe te voegen.

### <a name="add-a-private-ip-address"></a>**Een privé IP-adres toevoegen**

Voer de volgende stappen uit om een nieuwe privé IP-adres:

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op **Add**. In de **toevoegen IP-configuratie** deelvenster dat verschijnt, maak een IP-configuratie met de naam *IPConfig-4* met *10.0.0.7* als een *statische* privé-IP-adres en klik vervolgens op **OK**.

    > [!NOTE]
    > Bij het toevoegen van een statisch IP-adres, moet u een niet-gebruikte, geldig adres op de NIC is verbonden met subnet. Als het adres dat u selecteert niet beschikbaar is, de portal wordt weergegeven een X voor het IP-adres en moet u een ander account.

3. Zodra u op OK klikt, wordt het deelvenster wordt gesloten en ziet u de nieuwe IP-configuratie die worden vermeld. Klik op **OK** sluiten de **toevoegen IP-configuratie** deelvenster.
4. U kunt klikken op **toevoegen** voor het toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen.
5. De privé IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel.

### <a name="add-a-public-ip-address"></a>Een openbaar IP-adres toevoegen

Een openbaar IP-adres is toegevoegd door het koppelen van een openbare IP-adresresource aan een nieuwe IP-configuratie of een bestaande IP-configuratie.

> [!NOTE]
> Openbare IP-adressen hebben nominale kosten in rekening. Lees voor meer informatie over prijzen voor IP-adres, de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).
> 

### <a name="create-public-ip"></a>Een openbare IP-adresresource maken

Een openbaar IP-adres is een instelling voor een openbare IP-adresresource. Als u een openbare IP-adresresource die niet op dit moment is gekoppeld aan een IP-configuratie die u wilt koppelen aan een IP-configuratie hebt, kunt u de volgende stappen overslaan en de stappen in een van de secties die volgen, die u nodig hebt. Als u een beschikbare openbare IP-adresresource geen hebt, voert u de volgende stappen uit voor het maken van een:

1. Blader naar de Azure portal op https://portal.azure.com en meld u aan deze, indien nodig.
3. Klik in de portal op **een resource maken** > **netwerken** > **openbaar IP-adres**.
4. In de **openbare IP-adres maken** deelvenster dat verschijnt, voer een **naam**, selecteer een **IP-adrestoewijzing** type, een **abonnement**, een **Resourcegroep**, en een **locatie**, klikt u vervolgens op **maken**, zoals wordt weergegeven in de volgende afbeelding:

    ![Een openbare IP-adresresource maken](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Voltooi de stappen in een van de volgende secties om te koppelen van de openbare IP-adresresource aan een IP-configuratie.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Koppel het openbare IP-adresresource aan een nieuwe IP-configuratie

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op **Add**. In de **toevoegen IP-configuratie** deelvenster dat verschijnt, maak een IP-configuratie met de naam *IPConfig-4*. Inschakelen de **openbaar IP-adres** en selecteer een bestaande, beschikbare openbare IP-adresresource uit de **openbare IP-adres kiezen** deelvenster dat wordt weergegeven.

    Nadat u het openbare IP-adresresource hebt geselecteerd, klikt u op **OK** en het venster wordt gesloten. Als u een bestaande openbare IP-adres niet hebt, kunt u maken door de stappen in de [maken van een openbare IP-adresresource](#create-public-ip) sectie van dit artikel. 

3. Bekijk de nieuwe IP-configuratie. Hoewel een privé IP-adres is niet expliciet zijn toegewezen, is een automatisch toegewezen aan de IP-configuratie, omdat alle IP-configuraties moeten een privé IP-adres hebben.
4. U kunt klikken op **toevoegen** voor het toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen.
5. Het privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel. Voeg geen openbaar IP-adres toe aan het besturingssysteem.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Koppel het openbare IP-adresresource aan een bestaande IP-configuratie

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op de IP-configuratie die u wilt de openbare IP-adresresource aan toevoegen.
3. In het deelvenster van het IP-configuratie die wordt weergegeven, klikt u op **IP-adres**.
4. In de **openbare IP-adres kiezen** deelvenster dat verschijnt, selecteert u een openbaar IP-adres.
5. Klik op **opslaan** en de deelvensters te sluiten. Als u een bestaande openbare IP-adres niet hebt, kunt u maken door de stappen in de [maken van een openbare IP-adresresource](#create-public-ip) sectie van dit artikel.
3. Bekijk de nieuwe IP-configuratie.
4. U kunt klikken op **toevoegen** voor het toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen. Voeg geen openbaar IP-adres toe aan het besturingssysteem.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
