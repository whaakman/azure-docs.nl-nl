---
title: Meerdere IP-adressen voor virtuele machines in Azure - Portal | Microsoft Docs
description: Meer informatie over meerdere IP-adressen toewijzen aan een virtuele machine met de Azure portal | Resource Manager.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: d264bd47d76db8015a64f09248c57c94572e2693
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Meerdere IP-adressen toewijzen aan virtuele machines met de Azure portal

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met de Azure portal. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt met behulp van het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen de [begrijpen implementatiemodellen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

Als u maken van een virtuele machine met meerdere IP-adressen of een statisch privé IP-adres wilt, moet u deze met PowerShell of Azure CLI maken. Klik op de opties PowerShell of CLI aan het begin van dit artikel voor meer informatie over hoe. U kunt een virtuele machine maken met één dynamische particuliere IP-adres en (optioneel) op één openbaar IP-adres met behulp van de portal door de stappen in de [maken van een virtuele machine van Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [maken van een Linux-VM](../virtual-machines/linux/quick-create-portal.md) artikelen. Nadat u de virtuele machine hebt gemaakt, kunt u het type IP-adres wijzigen van dynamische in statische en toevoegen van extra IP-adressen met de portal door stap in de [toevoegen IP-adressen voor een virtuele machine](#add) sectie van dit artikel.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt persoonlijke en openbare IP-adressen toevoegen aan een NIC via de stappen volgen. De voorbeelden in de volgende secties wordt ervan uitgegaan dat er al een virtuele machine met de drie IP-configuraties beschreven in de [scenario](#Scenario) in dit artikel, maar het is niet vereist dat u doen.

### <a name="coreadd"></a>Core stappen

1. Blader naar de Azure portal op https://portal.azure.com en meld u in de App, indien nodig.
2. Klik in de portal op **meer services** > type *virtuele machines* in het filtervak en klik vervolgens op **virtuele machines**.
3. In de **virtuele machines** blade, klikt u op de virtuele machine die u wilt toevoegen, IP-adressen aan. Klik op **netwerkinterfaces** in de virtuele machine blade die wordt weergegeven en selecteer vervolgens de netwerkinterface die u wilt toevoegen het IP-adressen aan. In het voorbeeld weergegeven in de volgende afbeelding wordt de NIC met de naam *myNIC* van de virtuele machine met de naam *myVM* is ingeschakeld:

    ![Netwerkinterface](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. In de blade die wordt weergegeven voor de NIC die u hebt geselecteerd, klikt u op **IP-configuraties**.

Voer de stappen in een van de secties die volgen, op basis van het type IP-adres die toe te voegen.

### <a name="add-a-private-ip-address"></a>**Een persoonlijke IP-adres toevoegen**

Voer de volgende stappen voor het toevoegen van een nieuwe persoonlijke IP-adres:

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op **Add**. In de **toevoegen IP-configuratie** blade die wordt weergegeven, maak een IP-configuratie met de naam *IPConfig 4* met *10.0.0.7* als een *statische* privé-IP adres en klik vervolgens op **OK**.

    > [!NOTE]
    > Wanneer u een statisch IP-adres toevoegt, moet u een ongebruikte en een geldig adres op de NIC is verbonden met subnet. Als het adres dat u selecteert, niet beschikbaar is, de portal een X voor het IP-adres wordt weergegeven en u moet een andere selecteren.

3. Wanneer u op OK klikt, wordt de blade wordt gesloten en ziet u de nieuwe IP-configuratie weergegeven. Klik op **OK** sluiten de **toevoegen IP-configuratie** blade.
4. U kunt klikken op **toevoegen** toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen.
5. De particuliere IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel.

### <a name="add-a-public-ip-address"></a>Een openbaar IP-adres toevoegen

Een openbaar IP-adres wordt toegevoegd door het koppelen van een resource met openbare IP-adres aan een nieuwe IP-configuratie of een bestaande IP-configuratie.

> [!NOTE]
> Openbare IP-adressen hebben een nominaal kosten. Lees meer informatie over prijzen voor IP-adres, de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).
> 

### <a name="create-public-ip"></a>Een openbare IP-adres-resource maken

Een openbaar IP-adres is een instelling voor de bron van een openbare IP-adres. Als u een openbare IP-adres resource die is momenteel niet gekoppeld aan een IP-configuratie die u wilt koppelen aan een IP-configuratie hebt, wordt de volgende stappen overslaan en voer de stappen in een van de secties die volgen, die u nodig hebt. Als u een beschikbare resource voor openbare IP-adres niet hebt, voert u de volgende stappen uit om een:

1. Blader naar de Azure portal op https://portal.azure.com en meld u in de App, indien nodig.
3. Klik in de portal op **nieuw** > **Networking** > **openbaar IP-adres**.
4. In de **openbare IP-adres maken** blade die wordt weergegeven, voer een **naam**, selecteer een **IP-adrestoewijzing** type, een **abonnement**, een **resourcegroep**, en een **locatie**, klikt u vervolgens op **maken**, zoals wordt weergegeven in de volgende afbeelding:

    ![Een openbare IP-adres-resource maken](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Voltooi de stappen in een van de volgende secties om te koppelen van het openbare IP-adres resource aan een IP-configuratie.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Koppel het openbare IP-adres resource naar een nieuwe IP-configuratie

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op **Add**. In de **toevoegen IP-configuratie** blade die wordt weergegeven, maak een IP-configuratie met de naam *IPConfig 4*. Schakel de **openbaar IP-adres** en selecteer een bestaande, beschikbare openbare IP-adres resource van de **openbare IP-adres kiezen** blade die wordt weergegeven.

    Nadat u het openbare IP-adres resource hebt geselecteerd, klikt u op **OK** en de blade wordt gesloten. Als u een bestaand openbaar IP-adres niet hebt, kunt u een door de stappen in de [maken van een openbare IP-adres resource](#create-public-ip) sectie van dit artikel. 

3. Bekijk het nieuwe IP-configuratie. Hoewel een particulier IP-adres is niet expliciet toegewezen, is een automatisch toegewezen aan de IP-configuratie, omdat alle IP-configuraties moeten een particulier IP-adres hebben.
4. U kunt klikken op **toevoegen** toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen.
5. De privé IP-adres toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adres niet aan het besturingssysteem toevoegen.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Koppel het openbare IP-adres resource aan een bestaande IP-configuratie

1. Voer de stappen in de [Core stappen](#coreadd) sectie van dit artikel.
2. Klik op de IP-configuratie die u wilt de openbare IP-adres resource toevoegen.
3. Klik op de blade IPConfig **IP-adres**.
4. In de **openbare IP-adres kiezen** blade die wordt weergegeven, selecteert u een openbaar IP-adres.
5. Klik op **opslaan** en de blades worden gesloten. Als u een bestaand openbaar IP-adres niet hebt, kunt u een door de stappen in de [maken van een openbare IP-adres resource](#create-public-ip) sectie van dit artikel.
3. Bekijk het nieuwe IP-configuratie.
4. U kunt klikken op **toevoegen** toevoegen van extra IP-configuraties of sluit alle geopende blades voor het voltooien van de IP-adressen toe te voegen. Het openbare IP-adres niet aan het besturingssysteem toevoegen.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
