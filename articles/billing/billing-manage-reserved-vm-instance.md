---
title: Azure Reservations beheren
description: Meer informatie over hoe u Azure Reservations kunt beheren.
ms.service: billing
author: bandersmsft
manager: yashesvi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2019
ms.author: banders
ms.openlocfilehash: b161fc7cd4faa75dd87613c297c12f1edd862510
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840030"
---
# <a name="manage-reservations-for-azure-resources"></a>Reserve ringen voor Azure-resources beheren

Nadat u een Azure-reserve ring hebt gekocht, moet u mogelijk de reserve ring Toep assen op een ander abonnement, wijzigen wie de reserve ring kan beheren of het bereik van de reserve ring wijzigen. U kunt ook een reserve ring in twee reserve ringen splitsen om een aantal instanties toe te passen die u aan een ander abonnement hebt aangeschaft.

Als u Azure Reserved Virtual Machine Instances hebt gekocht, kunt u de instelling voor optimaliseren voor de reserve ring wijzigen. De reserverings korting kan worden toegepast op Vm's in dezelfde serie of u kunt de capaciteit van het Data Center reserveren voor een specifieke VM-grootte. En u moet de reserve ringen optimaliseren zodat ze volledig worden gebruikt.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Reserverings volgorde en reserve ring

Wanneer u een reserve ring koopt, worden er twee objecten gemaakt: **Reserverings volgorde** en **reserve ring**.

Op het moment van aankoop heeft een reserverings order één reserve ring. Acties zoals splitsen, samen voegen, gedeeltelijke terugbetaling of uitwisseling maken nieuwe reserve ringen onder de reserverings **order**.

Als u een reserverings order wilt weer geven, gaat u naar **reserve ringen** > selecteert u de reserve ring en klikt u op de reserverings **order-id**.

![Voor beeld van reserverings Order Details met reserverings Order-ID ](./media/billing-manage-reserved-vm-instance/reservation-order-details.png)

Een reserve ring neemt de machtigingen over van de reserverings order.

## <a name="change-the-reservation-scope"></a>Het reserverings bereik wijzigen

 Uw reserverings korting is van toepassing op virtuele machines, SQL-data bases, Azure Cosmos DB of andere resources die overeenkomen met uw reserve ring en worden uitgevoerd in het reserverings bereik. De facturerings context is afhankelijk van het abonnement dat is gebruikt om de reserve ring te kopen.

Het bereik van een reserve ring bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > -**reserve ringen**.
3. Selecteer de reserve ring.
4. Selecteer **Instellingen** > **Configuratie**.
5. Wijzig het bereik.

Als u overschakelt van gedeeld naar één bereik, kunt u alleen abonnementen selecteren waarvan u de eigenaar bent. Alleen abonnementen binnen dezelfde factureringscontext als de reservering kunnen worden geselecteerd.

Het bereik is alleen van toepassing op afzonderlijke abonnementen met betalen per gebruik-tarieven (MS-AZR-0003P of MS-AZR-0023P), Enter prise bieden MS-AZR-0017P of MS-AZR-0148P of CSP-abonnements typen.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Gebruikers toevoegen of wijzigen die een reservering kunnen beheren

U kunt reserverings beheer delegeren door personen toe te voegen aan rollen op de reserverings order of de reserve ring. De persoon die de reserverings order plaatst en de account Beheerder hebben standaard de rol van eigenaar op de reserverings order en de reserve ring.

U kunt de toegang tot reserve ringen orders en reserve ringen onafhankelijk van de abonnementen die de reserverings korting krijgen, beheren. Wanneer u iemand machtigingen geeft voor het beheren van een reserverings order of de reserve ring, geeft deze geen toestemming voor het beheren van het abonnement. En als u iemand machtigingen verleent voor het beheren van een abonnement in het bereik van de reserve ring, geeft het geen rechten voor het beheren van de reserverings order of de reserve ring.

Voor het uitvoeren van een uitwisseling of terugbetaling moet de gebruiker toegang hebben tot de reserverings order. Wanneer u iemand machtigingen verleent, is het het beste om machtigingen te verlenen aan de reserverings order, niet de reserve ring.


Toegangs beheer delegeren voor een reserve ring:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reservering** om de reserve ringen weer te geven waartoe u toegang hebt.
3. Selecteer de reserve ring waarvoor u de toegang tot andere gebruikers wilt delegeren.
4. Selecteer **toegangsbeheer (IAM)** .
5. Selecteer**eigenaar**van **functie toewijzings** > **functie** > toevoegen. Als u beperkte toegang wilt geven, selecteert u een andere rol.
6. Typ het e-mail adres van de gebruiker die u wilt toevoegen als eigenaar.
7. Selecteer de gebruiker en selecteer vervolgens **Opslaan**.

## <a name="split-a-single-reservation-into-two-reservations"></a>Eén reserve ring in twee reserve ringen splitsen

 Nadat u meer dan één resource-exemplaar binnen een reserve ring hebt gekocht, wilt u mogelijk instanties binnen die reserve ring toewijzen aan verschillende abonnementen. Standaard hebben alle exemplaren één bereik: één abonnement of gedeeld. Stel dat u 10 reserverings instanties hebt gekocht en de scope hebt opgegeven om abonnement A te zijn. U kunt nu het bereik wijzigen voor zeven reserve ringen tot abonnement A en de resterende drie van het abonnement B. door een reserve ring te splitsen, kunt u exemplaren distribueren voor een gedetailleerd bereik beheer. U kunt de toewijzing van abonnementen vereenvoudigen door gedeeld bereik te kiezen. Maar voor kosten beheer-of budgetterings doeleinden kunt u hoeveel heden toewijzen aan specifieke abonnementen.

 U kunt een reserve ring in twee reserve ringen splitsen, met Power shell, CLI of via de API.

### <a name="split-a-reservation-by-using-powershell"></a>Een reserve ring splitsen met behulp van Power shell

1. Haal de reserverings Order-ID op door de volgende opdracht uit te voeren:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. De details van een reserve ring ophalen:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Splits de reserve ring in twee en distribueer de instanties:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. U kunt het bereik bijwerken door de volgende opdracht uit te voeren:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancel-exchange-or-refund-reservations"></a>Reserve ringen annuleren, omruilen of terugbetalen

U kunt reserve ringen annuleren, vervangen of terugbetalen met bepaalde beperkingen. Zie [self-service-uitwisseling en terugbetalingen voor Azure Reservations](billing-azure-reservations-self-service-exchange-and-refund.md)voor meer informatie.

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Instelling voor optimaliseren wijzigen voor gereserveerde VM-instanties

 Wanneer u een gereserveerde VM-instantie koopt, kiest u de optie voor de grootte van het exemplaar of de capaciteits prioriteit. Flexibiliteit van de instantie grootte past de reserverings korting toe op andere virtuele machines in dezelfde [VM-grootte groep](https://aka.ms/RIVMGroups). Met capaciteits prioriteit wordt de capaciteit van het Data Center voor uw implementaties prioriteit. Deze optie biedt extra vertrouwen in uw vermogen om de VM-exemplaren te starten wanneer u ze nodig hebt.

Wanneer het bereik van de reserve ring wordt gedeeld, is de flexibiliteit van de instantie grootte standaard ingeschakeld. De capaciteit van het Data Center heeft geen prioriteit voor VM-implementaties.

Voor reserve ringen waarbij de scope één is, kunt u de reserve ring voor capaciteits prioriteit optimaliseren in plaats van de grootte van VM-instantie omvang.

De instelling optimaliseren voor de reserve ring bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > -**reserve ringen**.
3. Selecteer de reserve ring.
4. Selecteer **Instellingen** > **Configuratie**.
5. Wijzig de instelling **Optimize for** .

## <a name="optimize-reservation-use"></a>Reserverings gebruik optimaliseren

Azure reserverings besparingen zijn alleen van toepassing op het aanhoudende gebruik van resources. Wanneer u een reserverings aankoop maakt, betaalt u de kosten vooraf voor wat in wezen 100% mogelijk is voor resource gebruik gedurende een periode van één of drie jaar. Probeer uw reserve ring optimaal te benutten om zoveel mogelijk gebruik en besparingen te krijgen. In de volgende secties wordt uitgelegd hoe u een reserve ring bewaakt en het gebruik ervan optimaliseert.

### <a name="view-reservation-use-in-the-azure-portal"></a>Reserverings gebruik in de Azure Portal weer geven

Een manier om het reserverings gebruik weer te geven, bevindt zich in de Azure Portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **alle services** > -[**reserve ringen**](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) en noteer het **gebruik (%)** voor een reserve ring.  
  ![Afbeelding van de lijst met reserve ringen](./media/billing-manage-reserved-vm-instance/reservation-list.png)
3. Selecteer een reserve ring.
4. Bekijk de hand matige trend van de reserve ring over een bepaalde periode.  
  ![Afbeelding met reserverings gebruik ](./media/billing-manage-reserved-vm-instance/reservation-utilization-trend.png)

### <a name="view-reservation-use-with-api"></a>Reserverings gebruik met API weer geven

Als u een klant van Enterprise Agreement (EA) bent, kunt u programmatisch bekijken hoe de reserve ringen in uw organisatie worden gebruikt. U krijgt ongebruikte reserve ring via gebruiks gegevens. Wanneer u de reserverings kosten bekijkt, moet u er rekening mee houden dat de gegevens worden verdeeld over de werkelijke kosten en de afgeschreven kosten. De werkelijke kosten bieden gegevens voor het afstemmen van de maandelijkse factuur. De service heeft ook reserverings-aanschaf kosten en toepassings Details van de reserve ring. Afgeschreven kosten zijn vergelijkbaar met de werkelijke kosten, met uitzonde ring van de effectieve prijs voor het gebruik van reserve ringen. Ongebruikte reserverings uren worden weer gegeven in afgeschreven kosten gegevens. Zie voor meer informatie over gebruiks gegevens voor EA-klanten [Enterprise Agreement reserverings kosten en gebruik ophalen](billing-understand-reserved-instance-usage-ea.md).

Gebruik voor andere typen abonnementen de API [-reserve ringen samen vattingen: lijst per reserverings order en reserve ring](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation).

### <a name="optimize-your-reservation"></a>Uw reserve ring optimaliseren

Als u merkt dat de reserve ringen van uw organisatie onderworden gebruikt:

- Zorg ervoor dat de virtuele machines die uw organisatie maakt, overeenkomen met de VM-grootte die voor de reserve ring is.
- Zorg ervoor dat de flexibiliteit van de instantie grootte is ingeschakeld. Zie voor meer informatie [Manage Reservations-instelling Optimize wijzigen voor gereserveerde VM-instanties](#change-optimize-setting-for-reserved-vm-instances).
- Wijzig het bereik van de reserve ring naar _gedeeld_ zodat het uitgebreid van toepassing is. Zie [het bereik voor een reserve ring wijzigen](#change-the-reservation-scope)voor meer informatie.
- Overweeg het niet-gebruikte aantal te wisselen. Zie [annuleringen en uitwisselingen](#cancel-exchange-or-refund-reservations)voor meer informatie.


## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:

- [Wat zijn reserve ringen voor Azure?](billing-save-compute-costs-reservations.md)

Een service abonnement kopen:
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Vooruitbetalen voor Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)

Een software abonnement kopen:
- [Het vooruitbetalen voor Red Hat-Software plannen van Azure Reservations](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Vooruitbetalen voor SUSE-software-abonnementen vanuit Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md)

Inzicht in kortingen en gebruik:
- [Begrijpen hoe de korting voor VM-reservering wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Begrijpen hoe de korting op het Red Hat Enterprise Linux-software plan wordt toegepast](../billing/billing-understand-rhel-reservation-charges.md)
- [Inzicht krijgen in de manier waarop de SUSE Linux Enter prise software plan korting wordt toegepast](../billing/billing-understand-suse-reservation-charges.md)
- [Begrijpen hoe andere reserverings kortingen worden toegepast](billing-understand-reservation-charges.md)
- [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
- [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Windows-software kosten niet inbegrepen bij reserve ringen](billing-reserved-instance-windows-software-costs.md)
