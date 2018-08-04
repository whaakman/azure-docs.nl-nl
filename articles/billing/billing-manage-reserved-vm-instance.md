---
title: Beheren Azure gereserveerde VM-instanties | Microsoft Docs
description: Informatie over het bereik van abonnement wijzigen en toegang voor Azure gereserveerde VM-instanties beheren.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: yashesvi
ms.openlocfilehash: 589afc736faaa210fdcd5cf6c79d10af4af3c0e9
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502522"
---
# <a name="manage-reserved-instances-in-azure"></a>Gereserveerde instanties in Azure beheren

Nadat u een Azure gereserveerde VM-instantie hebt gekocht, kunt u de gereserveerde instantie van toepassing op een ander abonnement dan de versie die is opgegeven tijdens de aankoop. U kunt ook als de overeenkomende virtuele machines worden uitgevoerd in meerdere abonnementen, kunt u het bereik van de gereserveerde instantie gedeeld wijzigen. Als u wilt de gereserveerde instantie korting te maximaliseren, zorg ervoor dat het aantal exemplaren dat u hebt gekocht, komt overeen met de kenmerken en het aantal virtuele machines die u hebt uitgevoerd. Zie voor meer informatie over gereserveerde instanties van Azure, [geld besparen door vooraf betalen voor virtuele machines van Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Het bereik voor een gereserveerde instantie wijzigen
 Uw gereserveerde instantie korting geldt voor virtuele machines die overeenkomen met uw gereserveerde instantie en binnen het bereik van de gereserveerde instantie worden uitgevoerd. Het bereik van een gereserveerde instantie kan worden voor één abonnement of voor alle abonnementen in de context van de facturering. Als u het bereik aan één abonnement hebt ingesteld, wordt de gereserveerde instantie overeen met het uitvoeren van virtuele machines in het geselecteerde abonnement. Als u het bereik op gedeelde, Azure komt overeen met de gereserveerde instantie aan virtuele machines die worden uitgevoerd in alle abonnementen binnen de context van de facturering instellen. De context van de facturering is afhankelijk van het abonnement dat wordt gebruikt voor het kopen van de gereserveerde instantie. Zie voor meer informatie, [vooraf betalen voor virtuele machines met gereserveerde instanties](https://go.microsoft.com/fwlink/?linkid=861721).

Bijwerken van het bereik van een gereserveerde instantie: 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reserveringen**.
3. Selecteer de gereserveerde instantie.
4. Selecteer **Instellingen** > **Configuratie**.
5. Het bereik wijzigen. Als u wijzigt van gedeeld op één scope, kunt u alleen abonnementen waar u de eigenaar bent. Alleen abonnementen binnen de context van de dezelfde facturering als de gereserveerde instantie kan worden geselecteerd. De context van de facturering wordt bepaald door het abonnement dat u hebt geselecteerd bij de gereserveerde instantie is gekocht. Het bereik is alleen van toepassing op abonnementen voor betalen per gebruik-aanbieding-MS-AZR - 0003P en Enterprise-aanbieding MS-AZR - 0017P-abonnementen. Dev/test-abonnementen zijn niet in aanmerking voor het ophalen van de korting voor gereserveerde instanties voor enterprise-overeenkomsten.

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Toevoegen of wijzigen van gebruikers die een gereserveerde instantie kunnen beheren
U kunt beheer van een gereserveerde instantie delegeren door gebruikers aan rollen op de gereserveerde instantie toe te voegen. Standaard is de persoon die de gereserveerde instantie hebt gekocht en de accountbeheerder de rol van eigenaar hebben op de gereserveerde instantie. 

U kunt onafhankelijk toegang tot de gereserveerde instanties beheren van de abonnementen die aan de gereserveerde instantie korting. Als u iemand machtigingen geven voor het beheren van een gereserveerde instantie, die geen geeft deze rechten het abonnement te beheren. En als u iemand machtigingen voor het beheren van een abonnement binnen het bereik van de gereserveerde instantie, die deze rechten voor het beheren van de gereserveerde instantie geen geeft.
 
Om te delegeren van beheer van toegang voor een gereserveerde instantie: 
1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer **alle Services** > **reservering** om gereserveerde instanties die u toegang tot hebt weer te geven.
3.  Selecteer de gereserveerde instantie die u wilt toegang tot andere gebruikers delegeren.
4.  Selecteer **Access Control (IAM)** in het menu.
5.  Selecteer **toevoegen** > **rol** > **eigenaar** (of een andere rol als u wilt om beperkt toegang te geven). 
6. Typ het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. 
7. Selecteer de gebruiker en selecteer vervolgens **opslaan**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Gereserveerde VM-instantie voor VM-grootte flexibiliteit of capaciteit prioriteit optimaliseren
 VM-instantieflexibiliteit past de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Standaard, wanneer het bereik van de reservering wordt gedeeld, de flexibiliteit van de grootte van exemplaar is ingeschakeld en datacenter-capaciteit met prioriteit wordt niet toegepast voor VM-implementaties. U kunt de reservering voor capaciteitsprioriteit in plaats van de VM-instantieflexibiliteit grootte optimaliseren voor reserveringen waar het bereik één is. Capaciteitsprioriteit reserveert Datacenter-capaciteit voor uw implementaties, waardoor extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt.

Bijwerken van het bereik van een gereserveerde instantie: 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reserveringen**.
3. Selecteer de gereserveerde instantie.
4. Selecteer **Instellingen** > **Configuratie**.
5. De optimaliseren voor de instelling wijzigen.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Splitsen in twee gereserveerde instanties één gereserveerde instantie
 Nadat u meer dan één instantie hebt gekocht, kunt u exemplaren binnen een gereserveerde instantie toewijzen aan verschillende abonnementen. Standaard worden alle exemplaren (opgegeven tijdens de aankoop hoeveelheid) één scope - u één abonnement hebt of gedeeld. Bijvoorbeeld, u 10 Standard D2-VM's die zijn aangeschaft en het bereik voor het abonnement A. worden opgegeven U kunt nu het bereik voor zeven gereserveerde instanties wijzigen in abonnement A en de resterende 3 B. splitsen van een gereserveerde instantie abonnement kunt u exemplaren voor gedetailleerde scopebeheer distribueren. U kunt de toewijzing aan abonnementen vereenvoudigen door te kiezen gedeeld bereik. Maar voor cost management of budgettering doeleinden, kunt u hoeveelheden voor specifieke abonnementen toewijzen.

 U kunt splitsen een gereserveerde instantie in twee gereserveerde instanties echter PowerShell, CLI, of via de API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Een gereserveerde instantie splitsen met behulp van PowerShell
1. De gereserveerde instantie order-ID ophalen door het uitvoeren van de volgende opdracht uit:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```

2. De details van een gereserveerde instantie ophalen:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. De gereserveerde instantie in twee splitsen en distribueren van de exemplaren:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```

1. U kunt het bereik bijwerken door de volgende opdracht uit:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gereserveerde instanties van Azure, de volgende artikelen:

- [Wat zijn Azure Reserved VM Instances?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Inzicht in hoe korting op gereserveerde instanties wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Inzicht in het gebruik van gereserveerde instanties voor betalen per gebruik](billing-understand-reserved-instance-usage.md)
- [Inzicht in het gebruik van gereserveerde instanties voor Enterprise Enrollment](billing-understand-reserved-instance-usage-ea.md)
- [Kosten van Windows-software zijn niet inbegrepen in gereserveerde instanties](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
