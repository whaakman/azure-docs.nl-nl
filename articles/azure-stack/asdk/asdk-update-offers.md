---
title: In deze zelfstudie leert u hoe Azure-Stack aanbiedingen en plannen bijwerken | Microsoft Docs
description: In dit artikel wordt beschreven hoe weergeven en wijzigen van bestaande Azure-Stack aanbiedingen en plannen.
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
ms.openlocfilehash: 3b5d2ab5e924f578f5838d11b0d2058aacf67dec
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-update-offers-and-plans"></a>Zelfstudie: update biedt en plannen
Als een Azure-Stack-operator, kunt u plannen met de gewenste services en de toepasselijke quota's voor uw gebruikers zich abonneren op maken. Deze *baseren plannen* bevatten de kernservices aan uw gebruikers moeten worden aangeboden en u kunt slechts één basisplan per aanbieding hebben. Als u uw aanbieding wijzigen wilt, kunt u *invoegtoepassing plannen* die kunt u het plan uit te breiden computer, opslag, wijzigen of quota in eerste instantie wordt aangeboden met het basisplan netwerk. 

Hoewel alles combineren in één abonnement mogelijk optimale in sommige gevallen, is het raadzaam om een base plannen en bieden extra services met behulp van de invoegtoepassing plannen. U kan bijvoorbeeld bepalen aanbieden van IaaS-services als onderdeel van een basisplan met alle PaaS-services behandeld als invoegtoepassing plannen. Plannen kunnen ook worden gebruikt voor het verbruik van bronnen in uw beperkte ASDK-omgeving beheren. Bijvoorbeeld: als u wilt dat uw gebruikers Houd ook rekening met het gebruik van hun bronnen, kan er een relatief klein basisplan (afhankelijk van de services die zijn vereist) en als gebruikers capaciteit bereiken, ze zou gewaarschuwd dat ze de toewijzing van bronnen al hebben gebruikt op basis van hun toegewezen plan. De gebruikers mogelijk Selecteer een beschikbare aanvullende planning voor aanvullende bronnen. 

> [!NOTE]
> Wanneer u een plan voor de invoegtoepassing een gebruiker toevoegt aan een bestaand abonnement van de aanbieding, de aanvullende bronnen kunnen een uur duren om te worden weergegeven. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een invoegtoepassing maken 
> * Abonneren op het plan invoegtoepassing

## <a name="create-an-add-on-plan"></a>Een invoegtoepassing maken
**Plannen van de invoegtoepassing** zijn gemaakt door het wijzigen van een bestaande aanbieding.

1. Aanmelden bij de [Stack Azure portal](https://adminportal.local.azurestack.external) als een beheerder van de cloud.
2. Volg dezelfde stappen eerder opgeslagen [maken van een basisplan](asdk-offer-services.md) voor het maken van een nieuw plan aanbieden van services die niet eerder aangeboden. In dit voorbeeld worden Sleutelkluis (Microsoft.KeyVault)-services opgenomen in het plan.
3. Klik in de beheerdersportal op **biedt** en selecteer vervolgens de aanbieding moet worden bijgewerkt met een invoegtoepassing plan.

   ![](media/asdk-update-offers/1.PNG)

4.  Ga naar de onderkant van de aanbieding eigenschappen en selecteer **invoegtoepassing plannen**. Klik op **Add**.
   
    ![](media/asdk-update-offers/2.PNG)

5. Selecteer het plan om toe te voegen. In dit voorbeeld wordt het plan wordt aangeroepen **sleutelkluis plan**, en klik vervolgens op **Selecteer** het plan toevoegen aan de aanbieding. U ontvangt een melding dat het plan is toegevoegd aan de aanbieding.
   
    ![](media/asdk-update-offers/3.PNG)

6. Bekijk de lijst van invoegtoepassing plannen die deel uitmaakt van de aanbieding om te controleren dat de nieuwe invoegtoepassing plannen die worden vermeld.
   
    ![](media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>Abonneren op het plan invoegtoepassing
Moet u aanmelden bij de gebruikersportal van Azure-Stack uitbreiden van een bestaand Azure-Stack-abonnement met een invoegtoepassing plan.

Volg deze stappen om aanvullende bronnen detecteren die zijn die worden aangeboden door de Azure-Stack-operator en een plan invoegtoepassing toevoegen aan een aanbieding die u eerder hebt geabonneerd op.

1. Meld u aan bij de [gebruikersportal](https://portal.local.azurestack.external).
2. Voor het abonnement uit te breiden met extra planning, klikt u **meer services**, klikt u op **abonnementen**, selecteer vervolgens uw abonnement.
   
    ![](media/asdk-update-offers/5.PNG)

3.  Klik in het overzicht van het abonnement op **toevoegen plan**.
   
    ![](media/asdk-update-offers/6.PNG)

4. In het tabblad van het plan toevoegen door het plan invoegtoepassing toevoegen aan het abonnement te selecteren. In dit voorbeeld **sleutelkluis plan** is geselecteerd. U ontvangt vervolgens een melding dat het plan voor de invoegtoepassing is geslaagd verkregen en moet u de portal voor toegang tot de bijgewerkte abonnement te vernieuwen.
   
    ![](media/asdk-update-offers/7.PNG)

5. Controleer ten slotte de invoegtoepassing plannen die zijn gekoppeld aan het abonnement om ervoor te zorgen dat het plan voor de invoegtoepassing is toegevoegd.
   
    ![](media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een invoegtoepassing maken 
> * Abonneren op het plan invoegtoepassing

> [!div class="nextstepaction"]
> [Een virtuele machine uit een sjabloon maken](asdk-create-vm-template.md)

