---
title: Azure gereserveerde VM-exemplaren beheren | Microsoft Docs
description: Ontdek hoe abonnementsbereik wijzigen en toegang beheren voor Azure-gereserveerde virtuele machine-exemplaren.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikramdesai01
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: vikdesai
ms.openlocfilehash: ddb9d46dc2689b0dbcd8734e276916f7cd9d2728
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063891"
---
# <a name="manage-reserved-instances-in-azure"></a>Gereserveerde exemplaren in Azure beheren

Nadat u een exemplaar van Azure gereserveerde virtuele machine hebt gekocht, wilt u mogelijk het gereserveerde exemplaar naar een ander abonnement dan de opgegeven tijdens de aankoop van toepassing. U kunt ook als de overeenkomstige virtuele machines worden uitgevoerd in meerdere abonnementen, kan u het exemplaar van het gereserveerde bereik gedeeld wijzigen. Als u wilt de gereserveerde exemplaar korting maximaliseren, zorg ervoor dat het aantal exemplaren dat u hebt gekocht overeenkomt met de kenmerken en het aantal virtuele machines die u hebt uitgevoerd. Zie voor meer informatie over Azure gereserveerde instanties, [geld besparen door virtuele machines in Azure vooraf betalen](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Het bereik voor een exemplaar van de gereserveerde wijzigen
 Uw gereserveerde exemplaar korting geldt voor virtuele machines die overeenkomen met uw exemplaar van de gereserveerde en binnen het bereik van de gereserveerde exemplaar worden uitgevoerd. Het bereik van een gereserveerde exemplaar kan niet één abonnement of alle abonnementen in de context van de facturering. Als u het bereik aan één abonnement hebt ingesteld, wordt de gereserveerde instantie overeen met de virtuele machines worden uitgevoerd in het geselecteerde abonnement. Als u het bereik op gedeelde, Azure komt overeen met het gereserveerde-exemplaar voor virtuele machines die worden uitgevoerd in de abonnementen in de context van de facturering instellen. De facturering context is afhankelijk van het abonnement dat u gebruikt het gereserveerde exemplaar aanschaffen. Zie voor meer informatie, [vooraf betalen voor virtuele machines met gereserveerde exemplaren](https://go.microsoft.com/fwlink/?linkid=861721).

Het bereik van een gereserveerde exemplaar bijwerken: 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reserveringen**.
3. Selecteer de gereserveerde exemplaar.
4. Selecteer **instellingen** > **configuratie**.
5. Het bereik wijzigen. Als u wijzigt van gedeeld naar één scope, kunt u alleen abonnementen waar u de eigenaar bent. Alleen abonnementen in dezelfde facturering context als de gereserveerde instantie kan worden geselecteerd. De facturering context wordt bepaald door het abonnement dat u hebt geselecteerd toen het gereserveerde exemplaar is gekocht. Het bereik is alleen van toepassing op de aanbieding betalen naar gebruik MS-AZR - 0003P abonnementen en Enterprise aanbieding MS-AZR - 0017P abonnementen. Ontwikkelen en testen abonnementen zijn niet in aanmerking voor het ophalen van de gereserveerde exemplaar korting voor enterprise-overeenkomsten.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Gereserveerde één exemplaar in twee exemplaren van de gereserveerde splitsen
 Nadat u meer dan één exemplaar koopt, wilt u mogelijk exemplaren binnen een gereserveerd exemplaar toewijzen aan verschillende abonnementen behoren. Standaard worden alle exemplaren (hoeveelheid opgegeven tijdens de aankoop) één scope - beide één abonnement hebt of gedeeld. Bijvoorbeeld 10 standaard D2-VM's die zijn aangeschaft en het bereik voor het abonnement A. worden opgegeven U kunt nu het bereik voor zeven gereserveerde exemplaren wijzigen in een abonnement en de resterende 3 aan B. splitsen van een gereserveerde exemplaar abonnement kunt u voor het distribueren van exemplaren voor gedetailleerde scopebeheer. U kunt de toewijzing aan abonnementen, met gedeelde bereik kiezen vereenvoudigen. Maar voor kosten management of budgetten doeleinden, kunt u specifieke abonnementen hoeveelheden toewijzen.

 U kunt verdelen een gereserveerde exemplaar in twee exemplaren van de gereserveerde via PowerShell, CLI of via de API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Een gereserveerde exemplaar met behulp van PowerShell splitsen
1. De gereserveerde exemplaar volgorde-ID ophalen met de volgende opdracht:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. De details ophalen van een gereserveerde exemplaar:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. Splits de gereserveerde exemplaar in twee en distribueren van de exemplaren:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. U kunt het bereik bijwerken door de volgende opdracht uit te voeren:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Toevoegen of wijzigen van gebruikers die een gereserveerde exemplaar kunnen beheren
U kunt beheer van een gereserveerde exemplaar delegeren door personen toe te voegen aan de rollen op de gereserveerde exemplaar. Standaard de persoon die het gereserveerde exemplaar gekocht en de accountbeheerder de rol van eigenaar hebben op het gereserveerde exemplaar. 

U kunt onafhankelijk toegang tot gereserveerde exemplaren beheren van de abonnementen die de gereserveerde exemplaar korting te krijgen. Als u iemand machtigingen voor het beheren van een gereserveerde exemplaar geeft geen die ze rechten voor het beheren van het abonnement geven. En als u iemand machtigingen voor het beheren van een abonnement binnen het gereserveerde exemplaar bereik geeft die hen rechten voor het beheren van het gereserveerde exemplaar niet geven.
 
Beheer van toegang voor een exemplaar van de gereserveerde overdragen: 
1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer **alle Services** > **reservering** voor een lijst met gereserveerde exemplaren waartoe u toegang hebt.
3.  Selecteer de gereserveerde instantie die u wilt toegang aan andere gebruikers delegeren.
4.  Selecteer **Access Control (IAM)** in het menu.
5.  Selecteer **toevoegen** > **rol** > **eigenaar** (of een andere rol als u toegang wilt geven beperkt). 
6. Typ het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. 
7. Selecteer de gebruiker en selecteer vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-gereserveerde exemplaren, de volgende artikelen:

- [Wat zijn gereserveerd VM-exemplaren van Azure?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-exemplaren](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen](billing-understand-reserved-instance-usage-ea.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
