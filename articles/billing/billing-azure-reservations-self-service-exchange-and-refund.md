---
title: Self-servicegebruikers worden uitgewisseld en terugbetalingen voor Azure-reserveringen | Microsoft Docs
description: Leer hoe u kunt exchange- of Azure reserveringen terugbetaling.
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
ms.date: 03/28/2019
ms.author: banders
ms.openlocfilehash: 229b5a0948a81c9b5d78dd49f304e7b6891f95ee
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58653098"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Self-servicegebruikers worden uitgewisseld en terugbetalingen voor Azure-reserveringen

Azure-reserveringen bieden flexibiliteit om te voldoen aan uw groeiende behoeften. U kunt een reservering voor een andere reservering van hetzelfde type kan uitwisselen. Als u deze niet meer nodig hebt, kunt u ook een reservering, maximaal 50.000 USD per jaar, terugbetaling.

## <a name="exchange-an-existing-reserved-instance"></a>Een bestaande gereserveerde instantie uitwisselen

U kunt uw reservering met drie snelle stappen in exchange de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

1. Selecteer de reserveringen die u wilt een restitutie en klikt u op **Exchange**.  
    ![Voorbeeldafbeelding van de reserveringen om terug te keren ](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Selecteer het VM-product dat u wilt kopen en typ een aantal. Zorg ervoor dat het nieuwe aankoop totaal meer dan het geretourneerde totaal. [De juiste grootte te bepalen voordat u aanschaft](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    ![Van de voorbeeldafbeelding van het VM-product te kopen met een exchange ](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. Bekijk en de transactie te voltooien.  
    ![Van de voorbeeldafbeelding van het VM-product te kopen met een exchange-, het rendement voltooien ](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Een reservering voor restitutie, gaat u naar **Reserveringsdetails** en klikt u op **restitutie**.

## <a name="how-return-and-exchange-transactions-are-processed"></a>Hoe Ga terug en exchange-transacties worden verwerkt

Eerst Microsoft Hiermee annuleert u de bestaande reservering en het bedrag naar rato voor deze reservering restituties. Als er een exchange, worden de nieuwe aankoop wordt verwerkt. Microsoft verwerkt met behulp van een van de volgende methoden, afhankelijk van het accounttype en de betalingswijze restituties:

### <a name="enterprise-agreement-customers"></a>Enterprise agreement-klanten

Wordt toegevoegd aan de monetaire toezegging voor de uitwisseling en als de oorspronkelijke aankoop gedaan met behulp van een restitutie geld. Eventuele overschrijding facturen omdat de oorspronkelijke aankoop heropend en rerated om te controleren of de monetaire toezegging wordt gebruikt. Als de monetaire toezegging termijn met behulp van die de reservering is gekocht niet meer actief is, klikt u vervolgens tegoed toegevoegd aan uw huidige periode van enterprise agreement monetaire toezegging.

Als de oorspronkelijke aankoop gedaan als overschrijding, verstrekt Microsoft een factuur.

### <a name="pay-as-you-go-invoice-payment-customers-and-cloud-solution-provider-program"></a>Klanten van betalen per gebruik factureren betaling en Cloud solution provider-programma

De oorspronkelijke aankoop factuur voor reservering is geannuleerd en vervolgens een nieuwe factuur voor restitutie is gemaakt. Voor de uitwisseling toont de nieuwe factuur de terugbetaling en de nieuwe aankoop. De terug te betalen bedrag wordt aangepast op basis van de aankoop. Als u alleen een reservering gerestitueerd, het bedrag naar rato blijft van toepassing op Microsoft en deze wordt aangepast op basis van een toekomstige reserveringsaankoop.

### <a name="pay-as-you-go-credit-card-customers"></a>Klanten van betalen per gebruik creditcard

De oorspronkelijke factuur wordt geannuleerd en een nieuwe factuur wordt gemaakt. De geld wordt gerestitueerd aan de creditcard die is gebruikt voor de oorspronkelijke aankoop. Als u de kaart hebt gewijzigd [contact op met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="exchange-policies"></a>Exchange-beleid

- U kunt meerdere bestaande reserveringen om aan te schaffen van een nieuwe reservering van hetzelfde type geretourneerd. U kan geen reserveringen van hetzelfde type voor een andere uitwisselen. U kunt bijvoorbeeld een reservering VM om aan te schaffen van een SQL-reservering niet retourneren.
- Alleen de eigenaren van de reservering kunnen bij een uitwisseling verwerken. [Meer informatie over het toevoegen of wijzigen van de gebruikers die een reservering kunnen beheren](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).
- Een exchange wordt verwerkt als een restitutie en repo – verschillende transacties worden gemaakt voor de annulering en de nieuwe aankoop. Het bedrag naar rato reservering wordt gerestitueerd voor de reserveringen die inruilwaarde u. U betaalt volledig voor de nieuwe aankoop. Het bedrag naar rato reservering is de dagelijks naar rato resterende waarde van de reservering wordt geretourneerd.
- U kunt exchange- of restitutie reserveringen, zelfs als de enterprise agreement gebruikt voor het kopen van de reservering is verlopen en als een nieuwe overeenkomst is vernieuwd.
- U kunt een eigenschap van de reservering, zoals de grootte, regio, hoeveelheid en termijn met een exchange wijzigen.
- Het nieuwe inkooporder-totale aantal moet gelijk zijn aan of groter zijn dan de hoeveelheid geretourneerde.
- De nieuwe reservering gekocht als onderdeel van exchange heeft een nieuwe term vanaf het moment van exchange.
- Er is geen boete of jaarlijkse limieten voor de uitwisseling.

## <a name="refund-policies"></a>Restitutiebeleid

- Uw totale terug te betalen bedrag mag niet meer dan 50.000 USD in een rolling periode van 12 maanden.
- Alleen de eigenaren van de reservering kunnen een restitutie verwerken. [Meer informatie over het toevoegen of wijzigen van de gebruikers die een reservering kunnen beheren](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Microsoft behoudt zich het recht om rekening te brengen van een boete 12% voor alle retourneert, hoewel de boete is niet op dit moment in rekening gebracht.

## <a name="exchange-a-non-premium-storage-vm-reservation-for-a-premium-storage-reservation"></a>Wisselen van een niet-premium-opslag reservering van de virtuele machine voor een premium storage-reservering

U kunt een reservering voor een VM-grootte die biedt geen ondersteuning voor premiumopslag naar een overeenkomende VM-grootte die is gekocht kan uitwisselen. Bijvoorbeeld, een _F1_ voor een _F1s_. Als u de exchange, gaat u naar de Details van de reservering en klikt u op **Exchange**. De uitwisseling niet opnieuw instellen van de termijn van de gereserveerde instantie of maak een nieuwe transactie.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van een reservering, [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).
- Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:
    - [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
    - [Reserveringen in Azure beheren](billing-manage-reserved-vm-instance.md)
    - [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
    - [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
    - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
    - [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
    - [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](/partner-center/azure-reservations)
