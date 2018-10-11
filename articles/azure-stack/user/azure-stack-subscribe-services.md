---
title: In deze zelfstudie leert u hoe u zich kunt abonneren op een aanbieding van Azure Stack | Microsoft Docs
description: Deze zelfstudie leert u hoe u een nieuw abonnement op Azure Stack-services maken en testen van de aanbieding door te maken van een virtuele testmachine.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: aff20cba3bc07924f669420a8c367613b41111a1
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076097"
---
# <a name="tutorial-create-and-test-a-subscription"></a>Zelfstudie: maken en testen van een abonnement
Deze zelfstudie leert u hoe u een abonnement met een aanbieding maakt en test het vervolgens. Voor het testen, wordt u zich aanmelden bij de gebruikersportal van Azure Stack als de beheerder van een cloud, Abonneer u op de aanbieding en vervolgens een virtuele machine te maken.

> [!TIP]
> Voor meer een meer geavanceerde evaluatie-ervaring, kunt u [maken van een abonnement voor een bepaalde gebruiker](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) en meld u vervolgens aan als die gebruiker in de gebruikersportal. 

Deze zelfstudie leert u hoe u zich kunt abonneren op een Azure Stack-aanbieding.

Wat u leert:

> [!div class="checklist"]
> * Abonneren op een aanbieding 
> * Testen van de aanbieding

## <a name="subscribe-to-an-offer"></a>Abonneren op een aanbieding
Om u te abonneren op een aanbieding als een gebruiker, moet u zich aanmelden bij de gebruikersportal van Azure Stack voor het detecteren van de services die door de Azure Stack-operator is aangeboden.

1. Meld u aan bij de gebruikersportal aanmeldt en op **Neem een abonnement**.

   ![Een abonnement nemen](media/azure-stack-subscribe-services/get-subscription.png)

2. Typ in het veld **Weergavenaam** een naam voor uw abonnement. Klik vervolgens op **bieden** en selecteer een van de beschikbare aanbiedingen in de **Kies een aanbieding** sectie en klik vervolgens op **maken**.

   ![Een aanbieding maken](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > Nu moet u de gebruikersportal om te beginnen met uw abonnement vernieuwen.

3. Als u het abonnement dat u hebt gemaakt, klikt u op **alle services**.  Klik vervolgens onder de **algemene** categorie selecteren **abonnementen**, en selecteer vervolgens het nieuwe abonnement. Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien als nieuwe services opgenomen als onderdeel van het nieuwe abonnement zijn. In dit voorbeeld **virtuele machines** is toegevoegd.

   ![Abonnement weergeven](media/azure-stack-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testen van de aanbieding
Terwijl u bent aangemeld bij de gebruikersportal aanmeldt, kunt u de aanbieding kunt testen door een virtuele machine met behulp van de nieuwe mogelijkheden voor het abonnement in te richten. 

> [!NOTE]
> Deze test is vereist dat een virtuele machine met Windows Server 2016 Datacenter eerst is toegevoegd aan de Azure Stack marketplace. 

1. Meld u aan bij de gebruikersportal aanmeldt.

2. Klik in de gebruikersportal op **virtuele Machines** > **toevoegen** > **Windows Server 2016 Datacenter**, en klik vervolgens op **maken** .

3. In de **basisbeginselen** sectie, typt u een **naam**, **gebruikersnaam**, en **wachtwoord**, kiest u een **abonnement**, Maak een **resourcegroep** (of Selecteer een bestaande resourcegroep), en klik vervolgens op **OK**.

4. In de **Kies een grootte** sectie, klikt u op **Standard A1**, en klik vervolgens op **Selecteer**.  

5. In de blade instellingen de standaardwaarden te accepteren en op **OK**.

6. In de **samenvatting** sectie, klikt u op **OK** te maken van de virtuele machine.  

7. Voor uw nieuwe virtuele machine, klikt u op **virtuele machines**, zoek vervolgens naar de nieuwe virtuele machine en klik op de naam.

    ![Alle resources](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> Implementatie van de virtuele machine duurt een paar minuten om te voltooien.


## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd in deze zelfstudie:

> [!div class="checklist"]
> * Abonneren op een aanbieding 
> * Testen van de aanbieding


> [!div class="nextstepaction"]
> [Een virtuele machine maken van een communitysjabloon](azure-stack-create-vm-template.md)