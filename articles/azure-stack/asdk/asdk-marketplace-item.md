---
title: Een Azure-Stack marketplace-item toevoegen vanuit Azure | Microsoft Docs
description: Beschrijft hoe de installatiekopie van een Windows-Server op basis van een Azure-virtuele machine toevoegen aan de Stack Azure Marketplace.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 25f179f825526e20377c0e94ccb38a788cadd898
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>Zelfstudie: een Azure-Stack marketplace-item toevoegen van Azure

Als een Azure-Stack-Operator, wordt het eerste wat dat u moet uitvoeren om gebruikers te implementeren van een virtuele Machine (VM) is een VM-installatiekopie toevoegen aan de Stack van Azure marketplace. Standaard niets wordt gepubliceerd op de Stack van Azure marketplace, maar u kunt items uit downloaden [een samengestelde lijst met items voor de Azure marketplace](.\.\azure-stack-marketplace-azure-items.md) die zijn vooraf geteste worden uitgevoerd op Azure-Stack. Gebruik deze optie als u in een scenario voor verbonden werkt en u uw Azure-Stack-exemplaar bij Azure hebt geregistreerd.

In deze zelfstudie maakt toevoegen u de installatiekopie van het Windows Server 2016 VM vanuit Azure marketplace aan de Stack van Azure marketplace.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure-Stack voor Windows Server-VM marketplace-item toevoegen
> * Controleer of dat de VM-installatiekopie is beschikbaar 
> * Een installatiekopie van de virtuele machine testen

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer de [Azure Stack-compatibele Azure PowerShell-modules](asdk-post-deploy.md#install-azure-stack-powershell)
- Download de [Azure Stack-hulpprogramma's](asdk-post-deploy.md#download-the-azure-stack-tools)
- [Registreren van de ASDK](asdk-register.md) met uw Azure-abonnement

## <a name="add-a-windows-server-vm-image"></a>Een Windows Server-VM-installatiekopie toe te voegen
U kunt een installatiekopie van Windows Server 2016 toevoegen aan de Stack van Azure marketplace door het downloaden van de installatiekopie van de Azure marketplace. Gebruik deze optie als u in een scenario voor verbonden werkt en u al hebt [uw Azure-Stack-exemplaar geregistreerd](asdk-register.md) met Azure.

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management** > **toevoegen uit Azure**. 

   ![Toevoegen van Azure](media/asdk-marketplace-item/azs-marketplace.png)

3. Zoeken of zoeken naar de **Windows Server 2016 Datacenter** installatiekopie.

4. Selecteer de **Windows Server 2016 Datacenter** installatiekopie en selecteer vervolgens **downloaden**.

   ![Afbeelding van Azure downloaden](media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> Het duurt ongeveer een uur aan de VM-installatiekopie te downloaden en deze publiceren naar de Stack van Azure marketplace. 

Wanneer het downloaden is voltooid, de installatiekopie worden gepubliceerd en beschikbaar is onder **Marketplace Management**. De installatiekopie van het is ook beschikbaar onder **Compute** nieuwe virtuele machines maken.

## <a name="verify-the-marketplace-item-is-available"></a>Controleer of dat het marketplace-item beschikbaar is
Volg deze stappen om te controleren of de nieuwe VM-installatiekopie is beschikbaar in de Stack van Azure marketplace.

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management**. 

3. Controleer of de installatiekopie van het Windows Server 2016 Datacenter VM zijn toegevoegd.

   ![Image is gedownload van Azure](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Een installatiekopie van de virtuele machine testen
Als een Azure-Stack-operator, kunt u de [beheerdersportal](https://adminportal.local.azurestack.external) voor het maken van een test VM voor het valideren van de installatiekopie van het beschikbaar is gesteld is. 

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).

2. Klik op **nieuwe** > **Compute** > **Windows Server 2016 Datacenter** > **maken**.  
 ![Virtuele machine maken vanuit marketplace-installatiekopie](media/asdk-marketplace-item/new-compute.png)

3. In de **basisbeginselen** blade, voer de volgende informatie en klik vervolgens op **OK**:
  - **Naam**: test-vm-1
  - **Gebruikersnaam**: AdminTestUser
  - **Wachtwoord**: AzS TestVM01
  - **Abonnement**: Accepteer de standaard Provider-abonnement
  - **Resourcegroep**: test-vm-rg
  - **Locatie**: lokale

4. In de **een grootte kiezen** blade, klikt u op **A1 standaard**, en klik vervolgens op **Selecteer**.  

5. In de **instellingen** blade, accepteer de standaardinstellingen en klik op **OK**

6. In de blade **Samenvatting** klikt u op **OK** om de virtuele machine te maken.  
> [!NOTE]
> Implementatie van de virtuele machine duurt een paar minuten.

7. Als u wilt weergeven met de nieuwe virtuele machine **virtuele machines**, zoekt u naar **test-vm-1** en klik op de naam ervan.
    ![Eerste testinstallatiekopie voor de virtuele machine weergegeven](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Resources opschonen
Nadat u hebt aangemeld bij de virtuele machine, en geverifieerd dat de testafbeelding correct werkt, moet u de test-resourcegroep verwijderen. Hierdoor wordt beperkte bronnen beschikbaar zijn naar één knooppunt ASDK-installaties vrijgemaakt.

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep, VM en alle gerelateerde resources met de volgende stappen:

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).
2. Klik op **resourcegroepen** > **test-vm-rg** > **resourcegroep verwijderen**.
3. Type **test-vm-rg** als de naam van een resourcegroep en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure-Stack voor Windows Server-VM marketplace-item toevoegen
> * Controleer of dat de VM-installatiekopie is beschikbaar 
> * Een installatiekopie van de virtuele machine testen

Ga naar de volgende zelfstudie voor meer informatie over het maken van een Azure-Stack aanbieding en plannen.

> [!div class="nextstepaction"]
> [Azure-Stack IaaS-services bieden](asdk-offer-services.md)