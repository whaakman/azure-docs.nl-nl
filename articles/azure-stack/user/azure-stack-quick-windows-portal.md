---
title: Azure Stack Quick Start - een virtuele Windows-machine maken
description: Azure Stack Quick Start - maken van een Windows-VM met de portal
services: azure-stack
author: ErikjeMS
manager: byronr
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/15/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: 64a623b3f5cb443e0676e7d89d7040837be95737
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>Een virtuele Windows-machine maken met de Stack van Azure-portal

U kunt een virtuele Windows-machine maken met behulp van de Stack van Azure-portal. De portal is een browser gebaseerde gebruikersinterface waar u kunt maken, configureren en beheren van resources.

## <a name="sign-in-to-the-azure-stack-portal"></a>Aanmelden bij de Azure-Stack-portal

Meld u aan de Stack van Azure-portal. Het adres van de Stack van Azure-portal is afhankelijk van welk Azure-Stack-product u een verbinding maakt:

* Voor de Azure Stack Development Kit (ASDK) gaat u naar: https://portal.local.azurestack.external.
* Ga naar de URL die uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **nieuwe** > **Compute** > **Windows Server 2016 Datacenter Eval** > **maken**. Als er geen **Windows Server 2016 Datacenter Eval** -item, neem contact op met uw Azure-Stack-operator. Vragen die ze aan de marketplace toevoegen zoals wordt beschreven in de [de Windows Server 2016 VM-installatiekopie toevoegen aan de Stack van Azure marketplace](../azure-stack-add-default-image.md) artikel. 
    ![](media/azure-stack-quick-windows-portal/image01.png)
2. Onder **basisbeginselen**, typ een **naam**, **gebruikersnaam**, en **wachtwoord**. Kies een **abonnement**. Maak een **resourcegroep**, of Selecteer een bestaande één, selecteer een **locatie**, en klik vervolgens op **OK**.

    ![](media/azure-stack-quick-windows-portal/image02.png)
3. Onder **een grootte kiezen**, klikt u op **D1 standaard** > **Selecteer**.
    ![](media/azure-stack-quick-windows-portal/image03.png)
4. Onder **instellingen**, accepteer de standaardinstellingen en klik op **OK**.
    ![](media/azure-stack-quick-windows-portal/image04.png)
5. Onder **samenvatting**, klikt u op **OK** voor het maken van de virtuele machine. 
    ![](media/azure-stack-quick-windows-portal/image05.png)
6. Klik op een overzicht van de nieuwe virtuele machine **alle resources**, zoekt u naar de virtuele machine en klik op de naam.
    ![](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de virtuele machine niet meer nodig, de resourcegroep, virtuele machine en alle gerelateerde resources verwijderd. Om dit te doen, selecteert u de resourcegroep van de pagina van de virtuele machine en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
U kunt een eenvoudige Windows virtuele machine hebt geïmplementeerd in deze snel starten. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
