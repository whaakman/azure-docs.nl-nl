---
title: In dit artikel leert u hoe Azure-Stack aanbiedingen en plannen bijwerken | Microsoft Docs
description: In dit artikel wordt beschreven hoe weergeven en wijzigen van bestaande Azure-Stack aanbiedingen en plannen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a84148a3ac31d51ff30cebffab00e5fec8fdaa87
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238417"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack invoegtoepassing plannen
Als een Azure-Stack-operator, kunt u plannen met de gewenste services en de toepasselijke quota's voor uw gebruikers zich abonneren op maken. Deze [ *baseren plannen* ](azure-stack-create-plan.md) bevatten de kernservices aan uw gebruikers moeten worden aangeboden en u kunt slechts één basisplan per aanbieding hebben. Als u uw aanbieding wijzigen wilt, kunt u *invoegtoepassing plannen* die kunt u het plan uit te breiden computer, opslag, wijzigen of quota in eerste instantie wordt aangeboden met het basisplan netwerk. 

Hoewel alles combineren in één abonnement mogelijk optimale in sommige gevallen, is het raadzaam om een base plannen en bieden extra services met behulp van de invoegtoepassing plannen. U kan bijvoorbeeld bepalen aanbieden van IaaS-services als onderdeel van een basisplan met alle PaaS-services behandeld als invoegtoepassing plannen. Plannen kunnen ook worden gebruikt voor het verbruik van bronnen in uw Azure-Stack-omgeving beheren. Bijvoorbeeld: als u wilt dat uw gebruikers Houd ook rekening met het gebruik van hun bronnen, kan er een relatief klein basisplan (afhankelijk van de services die zijn vereist) en als gebruikers capaciteit bereiken, ze zou gewaarschuwd dat ze de toewijzing van bronnen al hebben gebruikt op basis van hun toegewezen plan. De gebruikers mogelijk Selecteer een beschikbare aanvullende planning voor aanvullende bronnen. 

> [!NOTE]
> Wanneer u een plan voor de invoegtoepassing een gebruiker toevoegt aan een bestaand abonnement van de aanbieding, de aanvullende bronnen kunnen een uur duren om te worden weergegeven. 

## <a name="create-an-add-on-plan"></a>Een invoegtoepassing maken
Plannen van de invoegtoepassing zijn gemaakt door het wijzigen van een bestaande aanbieding:

1. Aanmelden bij de Azure-Stack-beheerdersportal als een beheerder van de cloud.
2. Volg dezelfde stappen waarmee [maken van een nieuwe basisplan](azure-stack-create-plan.md) voor het maken van een nieuw plan aanbieden van services die niet eerder aangeboden. In dit voorbeeld wordt de Sleutelkluis (Microsoft.KeyVault)-services worden opgenomen in het nieuwe plan.
3. Klik in de beheerdersportal op **biedt** en selecteer vervolgens de aanbieding moet worden bijgewerkt met een invoegtoepassing plan.

   ![](media/create-add-on-plan/1.PNG)

4.  Ga naar de onderkant van de aanbieding eigenschappen en selecteer **invoegtoepassing plannen**. Klik op **Add**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Selecteer het plan om toe te voegen. In dit voorbeeld wordt het plan wordt aangeroepen **sleutelkluis plan**, en klik vervolgens op **Selecteer** het plan toevoegen aan de aanbieding. U ontvangt een melding dat het plan is toegevoegd aan de aanbieding.
   
    ![](media/create-add-on-plan/3.PNG)

6. Bekijk de lijst van invoegtoepassing plannen die deel uitmaakt van de aanbieding om te controleren dat de nieuwe invoegtoepassing plannen die worden vermeld.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Volgende stappen
[Een aanbieding maken](azure-stack-create-offer.md)