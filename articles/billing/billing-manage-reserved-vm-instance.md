---
title: Azure-reserveringen beheren | Microsoft Docs
description: Informatie over het bereik van abonnement wijzigen en beheren van toegang voor Azure-reserveringen.
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
ms.date: 08/08/2018
ms.author: yashesvi
ms.openlocfilehash: d47c85d4197f45db50f1974b6faea270e6761237
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628569"
---
# <a name="manage-reservations-for-resources-in-azure"></a>Reserveringen voor resources in Azure beheren

Nadat u een Azure-reservering koopt, kunt u de reservering van toepassing op een ander abonnement dan de versie die u hebt opgegeven tijdens de aankoop. U kunt ook als de overeenkomende virtuele machines, SQL-databases of andere bronnen zijn uitgevoerd in meerdere abonnementen, kunt u het reserveringsbereik gedeeld wijzigen. Als u wilt de reserveringskorting maximaliseren, zorg ervoor dat het aantal exemplaren dat u hebt gekocht overeenkomt met de kenmerken en het aantal resources die u hebt uitgevoerd. Zie voor meer informatie, [Azure reserveringen](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Het bereik voor een reservering wijzigen

 De reserveringskorting is van toepassing op virtuele machines, SQL-databases of andere bronnen die overeenkomen met uw reservering en binnen het reserveringsbereik worden uitgevoerd. Het bereik van een reservering kan worden voor één abonnement of voor alle abonnementen in de context van de facturering. Als u het bereik aan één abonnement hebt ingesteld, wordt de reservering wordt vergeleken met het uitvoeren van resources in het geselecteerde abonnement. Als u het bereik op gedeelde, Azure komt overeen met de reservering naar resources die worden uitgevoerd in alle abonnementen binnen de context van de facturering instellen. De context van de facturering is afhankelijk van het abonnement dat u gebruikt de reservering kopen.

Het bereik van een reservering bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer de reservering.
4. Selecteer **Instellingen** > **Configuratie**.
5. Het bereik wijzigen. Als u wijzigt van gedeeld op één scope, kunt u alleen abonnementen waar u de eigenaar bent. Alleen abonnementen binnen de context van de dezelfde facturering als de reservering kan worden geselecteerd. De context van de facturering wordt bepaald door het abonnement dat u hebt geselecteerd bij de reservering is gekocht. Het bereik is alleen van toepassing op abonnementen voor betalen per gebruik-aanbieding-MS-AZR - 0003P en Enterprise-aanbieding MS-AZR - 0017P-abonnementen. Dev/test-abonnementen zijn niet in aanmerking voor het ophalen van de reserveringskorting voor enterprise-overeenkomsten.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Toevoegen of wijzigen van gebruikers die een reservering kunnen beheren

U kunt beheer van een reservering delegeren door personen toe te voegen aan rollen voor de reservering. Standaard is de persoon die de reservering hebt gekocht en de accountbeheerder de rol van eigenaar hebben op de reservering.

U kunt onafhankelijk toegang tot reserveringen beheren van de abonnementen die aan de reserveringskorting. Als u iemand machtigingen geven voor het beheren van een reservering, die geen geeft deze rechten het abonnement te beheren. En als u iemand machtigingen voor het beheren van een abonnement binnen het bereik van de reservering geeft, dat deze rechten voor het beheren van de reservering niet geeft.

Om te delegeren van beheer van toegang voor een reservering:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reservering** aan lijst reserveringen waartoe u toegang hebt.
3. Selecteer de reservering die u wilt toegang tot andere gebruikers delegeren.
4. Selecteer **Access Control (IAM)** in het menu.
5. Selecteer **toevoegen** > **rol** > **eigenaar** (of een andere rol als u wilt om beperkt toegang te geven).
6. Typ het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. 
7. Selecteer de gebruiker en selecteer vervolgens **Opslaan**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Gereserveerde VM-instantie voor VM-grootte flexibiliteit of capaciteit prioriteit optimaliseren

 VM-instantieflexibiliteit past de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Standaard, wanneer het bereik van de reservering wordt gedeeld, de flexibiliteit van de grootte van exemplaar is ingeschakeld en datacenter-capaciteit met prioriteit wordt niet toegepast voor VM-implementaties. U kunt de reservering voor capaciteitsprioriteit in plaats van de VM-instantieflexibiliteit grootte optimaliseren voor reserveringen waar het bereik één is. Capaciteitsprioriteit reserveert Datacenter-capaciteit voor uw implementaties, waardoor extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt.

Het bereik van een reservering bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reserveringen**.
3. Selecteer de reservering.
4. Selecteer **Instellingen** > **Configuratie**.
5. De optimaliseren voor de instelling wijzigen.

## <a name="split-a-single-reservation-into-two-reservations"></a>Een enkele reservering in twee reserveringen splitsen

 Nadat u meer dan één instantie hebt gekocht, kunt u exemplaren binnen een reservering toewijzen aan verschillende abonnementen. Standaard worden alle exemplaren (opgegeven tijdens de aankoop hoeveelheid) één scope - u één abonnement hebt of gedeeld. Bijvoorbeeld, u 10 Standard D2-VM's die zijn aangeschaft en het bereik voor het abonnement A. worden opgegeven U kunt nu het bereik voor zeven reserveringen wijzigen in abonnement A en de resterende 3 B. splitsen van een reservering abonnement kunt u exemplaren voor gedetailleerde scopebeheer distribueren. U kunt de toewijzing aan abonnementen vereenvoudigen door te kiezen gedeeld bereik. Maar voor cost management of budgettering doeleinden, kunt u hoeveelheden voor specifieke abonnementen toewijzen.

 U kunt splitsen een reservering in twee reserveringen via PowerShell, CLI, of via de API.

### <a name="split-a-reservation-by-using-powershell"></a>Een reservering splitsen met behulp van PowerShell

1. De reserveringsorder-ID ophalen door het uitvoeren van de volgende opdracht uit:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```

2. De details van een reservering ophalen:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. De reservering in twee splitsen en distribueren van de exemplaren:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. U kunt het bereik bijwerken door de volgende opdracht uit:
    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

- [Wat zijn Azure reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Betaal vooruit voor SQL Database-compute-resources met Azure SQL Database gereserveerde capaciteit](../sql-database/sql-database-reserved-capacity.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
