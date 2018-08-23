---
title: Azure Stack-Quick Start - een Windows-machine maken
description: Azure Stack-Quick Start - een Windows VM maken via de portal
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: efe6213e5c0261fb26ac40e74c2b0f6e0c9252dd
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139534"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Snelstartgids: een Windows server-machine maken met de Azure Stack-portal

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt een Windows Server 2016 virtuele machine maken met behulp van de Azure Stack-portal. Volg de stappen in dit artikel te maken en gebruiken van een virtuele machine.

## <a name="sign-in-to-the-azure-stack-portal"></a>Aanmelden bij de Azure Stack-portal

Aanmelden bij de Azure Stack-portal. Het adres van de Azure Stack-portal, is afhankelijk van welke Azure Stack-product waarmee u verbinding:

* Voor de Azure Stack Development Kit (ASDK) gaat u naar: https://portal.local.azurestack.external.
* Ga naar de URL die uw Azure Stack-operator opgegeven voor een geïntegreerde Azure Stack-systeem.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **nieuwe** > **Compute** > **Windows Server 2016 Datacenter Eval** > **maken**. Als er geen **Windows Server 2016 Datacenter Eval** vermelding, neem contact op met uw Azure Stack-operators. Vragen die ze aan de marketplace toevoegen, zoals wordt beschreven de [de Windows Server 2016-VM-installatiekopie toevoegen aan de Azure Stack marketplace](../azure-stack-add-default-image.md) artikel.

    ![Stappen voor het maken van een Windows-machine in de portal](media/azure-stack-quick-windows-portal/image01.png)
2. Onder **basisbeginselen**, typ een **naam**, **gebruikersnaam**, en **wachtwoord**. Kies een **abonnement**. Maak een **resourcegroep**, of een bestaande één, selecteer een **locatie**, en klik vervolgens op **OK**.

    ![Basisinstellingen configureren](media/azure-stack-quick-windows-portal/image02.png)
3. Onder **Kies een grootte**, klikt u op **Standard D1** > **Selecteer**.
    ![Kies een grootte van virtuele machine](media/azure-stack-quick-windows-portal/image03.png)
4. Onder **instellingen**, accepteer de standaardinstellingen en klik op **OK**.
    ![Instellingen voor virtuele machines configureren](media/azure-stack-quick-windows-portal/image04.png)
5. Onder **samenvatting**, klikt u op **OK** te maken van de virtuele machine.
    ![Overzicht weergeven en de virtuele machine maken](media/azure-stack-quick-windows-portal/image05.png)
6. Voor uw nieuwe virtuele machine, klikt u op **alle resources**, zoekt u de naam van de virtuele machine en klik vervolgens op de naam ervan in de lijst met zoekresultaten.
    ![Zie de virtuele machine](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar met de virtuele machine bent, verwijdert u de virtuele machine en de daarbij behorende bronnen. Om dit te doen, selecteert u de resourcegroep op de pagina van de virtuele machine en klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quick start hebt u een eenvoudige virtuele machine voor Windows Server geïmplementeerd. Voor meer informatie over virtuele machines van Azure Stack, blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
