---
title: In deze zelfstudie leert u hoe abonneren op een Azure-Stack-aanbieding | Microsoft Docs
description: Deze zelfstudie laat zien hoe u een nieuw abonnement op Azure-Stack-services maken en testen van de aanbieding door het maken van een virtuele machine.
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
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Zelfstudie: maken en testen van een abonnement
Deze zelfstudie laat zien hoe u een abonnement met een aanbieding maakt en test deze. Voor de test, wordt u zich aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external) als een cloudbeheerder abonneren op de aanbieding en maak vervolgens een virtuele machine.

> [!TIP]
> Voor meer met een meer geavanceerde evaluatie, kunt u [maken van een abonnement voor een bepaalde gebruiker](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) en meld u vervolgens als die gebruiker in de gebruikersportal. 

Deze zelfstudie leert u hoe u zich abonneert op de aanbieding gemaakt in de [vorige zelfstudie](asdk-offer-services.md).

Wat u leert:

> [!div class="checklist"]
> * Abonneren op een aanbieding 
> * Testen van de aanbieding

## <a name="subscribe-to-an-offer"></a>Abonneren op een aanbieding
Abonneren op een aanbieding als een gebruiker, moet u aanmelden bij de gebruikersportal van Azure-Stack voor het detecteren van de services die door de Azure-Stack-operator is aangeboden.

1. Meld u aan bij de [gebruikersportal](https://portal.local.azurestack.external) en klik op **Neem een abonnement op**.

   ![Een abonnement nemen](media/asdk-subscribe-services/get-subscription.png)

2. Typ in het veld **Weergavenaam** een naam voor uw abonnement. Klik vervolgens op **bieden** Selecteer een van de aanbiedingen beschikbaar in de **kiest u een aanbieding** sectie en klik vervolgens op **maken**.

   ![Een aanbieding maken](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > U moet nu de gebruikersportal aan de slag met uw abonnement kunt vernieuwen.

3. Als u wilt weergeven van het abonnement dat u zojuist hebt gemaakt, klikt u op **meer services**, klikt u op **abonnementen**, klik vervolgens op het nieuwe abonnement. Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien als nieuwe services opgenomen als onderdeel van het nieuwe abonnement zijn. In dit voorbeeld **virtuele machines** is toegevoegd.

   ![Abonnement weergeven](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testen van de aanbieding
Terwijl u aangemeld bent de [gebruikersportal](https://portal.local.azurestack.external), kunt u de aanbieding testen door een virtuele machine met de nieuwe mogelijkheden voor abonnement inrichten. 

> [!NOTE]
> Deze test vereist een installatiekopie van het Windows Server 2016 Datacenter VM die is toegevoegd aan de Stack van Azure marketplace in een [vorige zelfstudie](asdk-marketplace-item.md). 

1. Meld u aan bij de [gebruikersportal](https://portal.local.azurestack.external).

2. Klik in de gebruikersportal op **virtuele Machines** > **toevoegen** > **Windows Server 2016 Datacenter**, en klik vervolgens op **maken** .

3. In de **basisbeginselen** sectie, typt u een **naam**, **gebruikersnaam**, en **wachtwoord**, kies een **abonnement**, Maak een **resourcegroep** (of een bestaande set selecteren) en klik vervolgens op **OK**.

4. In de **een grootte kiezen** sectie, klikt u op **A1 standaard**, en klik vervolgens op **Selecteer**.  

5. Accepteer de standaardinstellingen op de blade instellingen en klik op **OK**.

6. In de **samenvatting** sectie, klikt u op **OK** voor het maken van de virtuele machine.  

7. Klik op een overzicht van de nieuwe virtuele machine **virtuele machines**, zoekt u naar de nieuwe virtuele machine en klik op de naam.

    ![Alle resources](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> Implementatie van de virtuele machine wordt enkele minuten duren.


## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd in deze zelfstudie:

> [!div class="checklist"]
> * Abonneren op een aanbieding 
> * Testen van de aanbieding

Ga naar de volgende zelfstudie voor meer informatie over het maken van een virtuele machine met een sjabloon.

> [!div class="nextstepaction"]
> [Een virtuele machine uit een sjabloon maken](asdk-create-vm-template.md)