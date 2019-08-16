---
title: Self-service uitwisselingen en terugstortingen voor Azure Reservations
description: Meer informatie over hoe u Azure Reservations kunt omruilen of terugbetalen.
author: yashesvi
manager: yashesvi
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/05/2019
ms.author: banders
ms.openlocfilehash: 828bc3784a118a81adc4391b1bf222c00ee2025a
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543017"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Self-service uitwisselingen en terugstortingen voor Azure Reservations

Azure Reservations flexibiliteit bieden om te voldoen aan uw evoluerende behoeften. U kunt een reservering inwisselen voor een andere reservering van hetzelfde type. U kunt ook een restitutie (tot USD 50.000 per jaar) voor een reservering aanvragen als u deze niet meer nodig hebt.

De selfserviceoptie voor inwisselen en annuleren is niet beschikbaar voor Enterprise Agreement-klanten van de Amerikaanse overheid. Andere abonnements typen van de Amerikaanse overheid, inclusief betalen per gebruik en CSP, worden ondersteund.

U moet beschikken over de machtiging voor de reserverings order voor het uitwisselen van een bestaande reserve ring of een restitutie.

## <a name="exchange-an-existing-reserved-instance"></a>Een bestaande gereserveerde instantie uitwisselen

U kunt uw reserve ring uitwisselen met drie snelle stappen in de [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

1. Selecteer de reserve ringen die u wilt terugbetalen en klik op **Exchange**.  
    ![Voorbeeld afbeelding van weer gave met te retour neren reserve ringen](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Selecteer het VM-product dat u wilt aanschaffen en geef een hoeveelheid op. Zorg ervoor dat het totaal van de nieuwe aankoop hoger is dan het retour totaal. [Bepaal het juiste formaat voordat u het aankoopt](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    ![Voorbeeld afbeelding van het te kopen VM-product bij een uitwisseling](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. De trans actie controleren en volt ooien.  
    ![Voorbeeld afbeelding van het te kopen VM-product bij een uitwisseling, waarbij de retour nering wordt voltooid](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Als u een reserve ring wilt terugbetalen, gaat u naar reserverings **Details** en klikt u op **terugbetaling**.

## <a name="how-transactions-are-processed"></a>Hoe trans acties worden verwerkt

Eerst annuleert micro soft de bestaande reserve ring en wordt de Pro-nominale hoeveelheid voor die reserve ring terugbetaald. Als er een Exchange is, wordt de nieuwe aankoop verwerkt. Micro soft verwerkt de terugbetalingen met een van de volgende methoden, afhankelijk van uw account type en betalings wijze:

### <a name="enterprise-agreement-customers"></a>Enter prise Agreement-klanten

Geld wordt toegevoegd aan de monetaire toezeg ging voor uitwisselingen en restituties als de oorspronkelijke aankoop is gemaakt met één. Overschrijding-facturen sinds de oorspronkelijke aankopen worden opnieuw geopend en geclassificeerd om ervoor te zorgen dat de monetaire toezeg ging wordt gebruikt. Als de monetaire toezeggings termijn die gebruikmaakt van de reserve ring, niet langer actief is, wordt tegoed toegevoegd aan uw huidige monetaire toezeggings periode van ENTER prise Agreement.

Als de oorspronkelijke aankoop is gemaakt als een overschrijding, geeft micro soft een credit nota.

### <a name="pay-as-you-go-invoice-payments-and-csp-program"></a>Betalen naar gebruik-factuur betalingen en CSP-programma

De oorspronkelijke inkoop factuur voor reserve ringen wordt geannuleerd en er wordt een nieuwe factuur gemaakt voor de terugbetaling. Voor uitwisselingen wordt in de nieuwe factuur de terugbetaling en de nieuwe aankoop weer gegeven. Het restitutie bedrag wordt aangepast op basis van de aankoop. Als u alleen een reserve ring terugrestitueert, blijft de evenredige hoeveelheid bij micro soft en wordt deze aangepast op basis van een toekomstige reserve ring.

### <a name="pay-as-you-go-credit-card-customers"></a>Betalen per gebruik-creditcard klanten

De oorspronkelijke factuur wordt geannuleerd en er wordt een nieuwe factuur gemaakt. Het geld wordt terugbetaald naar de credit card die is gebruikt voor de oorspronkelijke aankoop. Als u uw kaart hebt gewijzigd, [neemt u contact op met de ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="cancel-exchange-and-refund-policies"></a>Annulerings-, uitwisselings-en terugbetalings beleid

Azure heeft de volgende beleids regels voor annuleringen, uitwisselingen en restituties.

**Exchange-beleid**

- U kunt meerdere bestaande reserve ringen retour neren om een nieuwe reserve ring van hetzelfde type aan te schaffen. U kunt geen reserve ringen van het ene type uitwisselen voor een andere. U kunt bijvoorbeeld geen VM-reserve ring retour neren om een SQL-reserve ring aan te schaffen.
- Alleen eigen aars van reserve ringen kunnen een uitwisseling verwerken. [Meer informatie over het toevoegen of wijzigen van gebruikers die een reserve ring kunnen beheren](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Een uitwisseling wordt verwerkt als restitutie en opnieuw aankoop: er worden verschillende trans acties voor de annulering en de nieuwe aankoop gemaakt. De gefactureerde reserverings hoeveelheid wordt terugbetaald voor de reserve ringen waarvoor u de trans actie hebt ingewisseld. De kosten voor de nieuwe aankoop worden volledig in rekening gebracht. De evenredige reserverings hoeveelheid is de dagelijkse, evenredige rest waarde van de reserve ring die wordt geretourneerd.
- U kunt reserve ringen omruilen of terugbetalen, zelfs als de Enter prise Agreement voor het aanschaffen van de reserve ring is verlopen en is vernieuwd als een nieuwe overeenkomst.
- U kunt een gereserveerde eigenschap zoals grootte, regio, hoeveelheid en term wijzigen met een uitwisseling.
- Het nieuwe aankoop totaal moet gelijk zijn aan of groter zijn dan het geretourneerde bedrag.
- De nieuwe reserve ring die is gekocht als onderdeel van Exchange, heeft een nieuwe periode vanaf het moment van Exchange.
- Er zijn geen boeten of jaarlijkse limieten voor uitwisselingen.

**Restitutie beleid**
- Als u een reserve ring annuleert, kunnen er 12% openstaande ontslag kosten in rekening worden gebracht.
- De terugbetaling die u voor een annulering ontvangt, is het resterende Pro-nominale saldo min de kosten voor de vroege beëindiging van 12%. Als u wilt annuleren, gaat u naar de reserve ring in het Azure Portal en selecteert u **terugbetaling**.
- Uw totale restitutie bedrag mag niet groter zijn dan $50.000 USD in een voortschrijdende periode van 12 maanden.
- De restituties worden berekend op basis van de laagste prijs van de aanschaf prijs of de huidige prijs van de reserve ring.
- Alleen eigen aars van reserve ringen kunnen een terugbetaling verwerken. [Meer informatie over het toevoegen of wijzigen van gebruikers die een reserve ring kunnen beheren](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Micro soft behoudt zich het recht voor om een boete van 12% voor eventuele retouren te berekenen. Er worden momenteel geen kosten in rekening gebracht, maar deze worden in de toekomst in rekening gebracht.

## <a name="exchange-non-premium-storage-for-premium-storage"></a>Niet-Premium-opslag uitwisselen voor Premium-opslag

U kunt een gereserveerde reserve ring uitwisselen voor een VM-grootte die geen ondersteuning biedt voor Premium Storage naar een bijbehorende VM-grootte. Een voor beeld: een _F1_ voor een _D11_. Als u de uitwisseling wilt maken, gaat u naar reserverings gegevens en klikt u op **Exchange**. De periode van de gereserveerde instantie wordt niet opnieuw ingesteld door Exchange of er wordt een nieuwe trans actie gemaakt.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Reservations beheren](billing-manage-reserved-vm-instance.md)voor meer informatie over het beheren van een reserve ring.
- Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:
    - [Wat zijn Azure Reservations?](billing-save-compute-costs-reservations.md)
    - [Reserve ringen in azure beheren](billing-manage-reserved-vm-instance.md)
    - [Begrijpen hoe de reserverings korting wordt toegepast](billing-understand-vm-reservation-charges.md)
    - [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
    - [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
    - [Windows-software kosten niet inbegrepen bij reserve ringen](billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations in het Partner Center-programma van de Cloud Solution Provider (CSP)](/partner-center/azure-reservations)
