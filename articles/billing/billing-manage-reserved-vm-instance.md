---
title: Exemplaren van de gereserveerde virtuele Machine in Azure beheren | Microsoft Docs
description: Ontdek hoe abonnementsbereik wijzigen en toegang beheren voor Azure-gereserveerde virtuele machine-exemplaren.
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/06/2017
ms.author: vikdesai
ms.openlocfilehash: e23eea52ff5d27beacf938a1ef153172e24f1aee
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-reserved-virtual-machine-instances"></a>Exemplaren van de gereserveerde virtuele Machine beheren

Nadat u een exemplaar van Azure gereserveerde virtuele machine hebt gekocht, kunt u de reservering van toepassing op een ander abonnement dan de opgegeven tijdens de aankoop. U kunt ook als uw overeenkomende virtuele machines worden uitgevoerd in meerdere abonnementen, kan u het bereik van de reservering gedeeld wijzigen. Als u wilt de reservering korting maximaliseren, zorg ervoor dat het aantal exemplaren dat u hebt gekocht overeenkomt met de kenmerken en het aantal virtuele machines die u hebt uitgevoerd. Zie voor meer informatie over gereserveerde virtuele Machine-exemplaren, [geld besparen door virtuele machines in Azure vooraf betalen](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Het bereik voor een reservering wijzigen
 Uw reservering korting geldt voor virtuele machines die overeenkomen met uw reservering en binnen het bereik van de reservering worden uitgevoerd. Het bereik van een reservering is op één abonnement of alle abonnementen in de context van de facturering. Als u het bereik aan één abonnement hebt ingesteld, wordt de reservering overeenkomen met virtuele machines worden uitgevoerd in het geselecteerde abonnement. Als u het bereik op gedeelde, Azure komt overeen met de reservering voor virtuele machines die worden uitgevoerd in de abonnementen in de context van de facturering instellen. De facturering context is afhankelijk van het abonnement dat u gebruikt de reservering aanschaffen. Zie voor meer informatie, [vooraf betalen voor VM's met VM-instanties van gereserveerde](https://go.microsoft.com/fwlink/?linkid=861721).

Het bereik van een reservering bijwerken: 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **meer Services** > **reserveringen**.
3. Selecteer de reservering.
4. Selecteer **instellingen** > **configuratie**.
5. Het bereik wijzigen. Als u wijzigt van gedeeld naar één scope, kunt u alleen abonnementen waar u de eigenaar bent. Alleen abonnementen in dezelfde facturering context als de reservering kan worden geselecteerd. De facturering context wordt bepaald door het abonnement dat u hebt geselecteerd tijdens de reservering is gekocht. Het bereik is alleen van toepassing op de aanbieding betalen naar gebruik MS-AZR - 0003P abonnementen en Enterprise aanbieding MS-AZR - 0017P abonnementen. Ontwikkelen en testen abonnementen zijn niet in aanmerking voor het ophalen van de reservering korting voor enterprise-overeenkomsten.

## <a name="split-a-single-reservation-into-two-reservations"></a>Een één-reservering in twee reserveringen splitsen
 Nadat u meer dan één exemplaar koopt, wilt u mogelijk exemplaren binnen een reservering toewijzen aan verschillende abonnementen behoren. Standaard worden alle exemplaren (hoeveelheid opgegeven tijdens de aankoop) één scope - beide één abonnement hebt of gedeeld. Bijvoorbeeld 10 standaard D2-VM's die zijn aangeschaft en het bereik voor het abonnement A. worden opgegeven U kunt nu het bereik voor 7 gereserveerde virtuele machine-exemplaren wijzigen in een abonnement en de resterende 3 aan B. splitsen van een reservering abonnement kunt u voor het distribueren van exemplaren voor gedetailleerde scopebeheer. U kunt de toewijzing aan abonnementen, met gedeelde bereik kiezen vereenvoudigen. Maar voor kosten management of budgetten doeleinden, kunt u specifieke abonnementen hoeveelheden toewijzen.

 U kunt verdelen een reservering in twee reserveringen via PowerShell, CLI of via de API.

### <a name="split-a-reservation-by-using-powershell"></a>Een reservering verdeeld met behulp van PowerShell
1. De reservering volgorde-ID niet ophalen met de volgende opdracht:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```
2. De details ophalen van een reservering:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. De reservering in twee splitsen en distribueren van de exemplaren:

    ```powershell
    # Split the reservation. The sum of the Reserved VM instances, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. U kunt het bereik bijwerken door de volgende opdracht uit te voeren:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Toevoegen of wijzigen van gebruikers die een reservering kunnen beheren
U kunt het beheer van een reservering delegeren door personen toe te voegen aan de rollen op de reservering. Standaard wordt de persoon die de reservering gekocht en de accountbeheerder de rol van eigenaar hebben op de reservering. 

U kunt toegang onafhankelijk tot reserveringen beheren vanuit de abonnementen die de reservering korting te krijgen. Als u iemand machtigingen voor het beheren van een reservering geeft die geeft geen deze rechten het abonnement te beheren. En als u iemand machtigingen voor het beheren van een abonnement binnen het bereik van de reservering geeft die hen rechten voor het beheren van de reservering niet geven.
 
Toegangsbeheer voor een reservering overdragen: 
1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer **meer Services** > **reservering** tot lijst reserveringen waartoe u toegang hebt.
3.  Selecteer de reservering die u wilt toegang aan andere gebruikers delegeren.
4.  Selecteer **Access Control (IAM)** in het menu.
5.  Selecteer **toevoegen** > **rol** > **eigenaar** (of een andere rol als u toegang wilt geven beperkt). 
6. Typ het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. 
7. Selecteer de gebruiker en selecteer vervolgens **opslaan**.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
