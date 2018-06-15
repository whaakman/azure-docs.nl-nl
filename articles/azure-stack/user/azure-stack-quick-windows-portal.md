---
title: Azure Stack Quick Start - een virtuele Windows-machine maken
description: Azure Stack Quick Start - maken van een Windows-VM met de portal
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 5776fc472483018eb2c9e4f8962d0b1e8bce8081
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32155578"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Snelstartgids: een virtuele machine van Windows server maken met de Stack van Azure-portal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Windows Server 2016 virtuele machine maken met behulp van de Stack van Azure-portal. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine.

## <a name="sign-in-to-the-azure-stack-portal"></a>Aanmelden bij de Azure-Stack-portal

Meld u aan de Stack van Azure-portal. Het adres van de Stack van Azure-portal is afhankelijk van welke Azure-Stack-product waarmee u verbinding:

* Voor de Azure Stack Development Kit (ASDK) gaat u naar: https://portal.local.azurestack.external.
* Ga naar de URL die uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **nieuwe** > **Compute** > **Windows Server 2016 Datacenter Eval** > **maken**. Als er geen **Windows Server 2016 Datacenter Eval** -item, neem contact op met uw Azure-Stack-operator. Vragen die ze aan de marketplace toevoegen zoals wordt beschreven in de [de Windows Server 2016 VM-installatiekopie toevoegen aan de Stack van Azure marketplace](../azure-stack-add-default-image.md) artikel.

    ![Stappen voor het maken van een virtuele Windows-machine in de portal](media/azure-stack-quick-windows-portal/image01.png)
2. Onder **basisbeginselen**, typ een **naam**, **gebruikersnaam**, en **wachtwoord**. Kies een **abonnement**. Maak een **resourcegroep**, of Selecteer een bestaande één, selecteer een **locatie**, en klik vervolgens op **OK**.

    ![Basisinstellingen configureren](media/azure-stack-quick-windows-portal/image02.png)
3. Onder **een grootte kiezen**, klikt u op **D1 standaard** > **Selecteer**.
    ![De grootte van virtuele machine kiezen](media/azure-stack-quick-windows-portal/image03.png)
4. Onder **instellingen**, accepteer de standaardinstellingen en klik op **OK**.
    ![Virtuele machine-instellingen configureren](media/azure-stack-quick-windows-portal/image04.png)
5. Onder **samenvatting**, klikt u op **OK** voor het maken van de virtuele machine.
    ![Overzicht weergeven en de virtuele machine maken](media/azure-stack-quick-windows-portal/image05.png)
6. Klik op een overzicht van de nieuwe virtuele machine **alle resources**, zoekt u de naam van de virtuele machine en klik op de naam ervan in de zoekresultaten.
    ![Zie de virtuele machine](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met behulp van de virtuele machine, verwijdert u de virtuele machine en de bijbehorende bronnen. Om dit te doen, selecteert u de resourcegroep op de pagina van de virtuele machine en klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, moet u een eenvoudige virtuele machine voor Windows Server geïmplementeerd. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
