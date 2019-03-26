---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/22/2019
ms.openlocfilehash: f7fbbb421a01b268b784a6d6c875cd959a5d1d42
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407861"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties

Betaal vooruit voor virtuele machines en bespaar geld met instanties voor Azure gereserveerde virtuele Machine (VM). Zie voor meer informatie, [Azure Reserved VM Instances aanbieding](https://azure.microsoft.com/pricing/reserved-vm-instances/).

U kunt een gereserveerde VM-instantie kopen de [Azure-portal](https://portal.azure.com). Een exemplaar aanschaffen:

- U moet zich in een rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
- Voor Enterprise-abonnementen, **gereserveerde instanties toevoegen** moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com). Of, als deze instelling is uitgeschakeld, moet u een EA-beheerder voor het abonnement zijn.
- Voor het programma Cloud Solution Provider (CSP) kopen alleen de agents beheerder of de verkoop agents reserveringen.

De reserveringskorting wordt automatisch toegepast op het aantal actieve virtuele machines die overeenkomen met het reserveringsbereik en kenmerken. U kunt het bereik van de reservering via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI, of via de API.

## <a name="determine-the-right-vm-size-before-you-buy"></a>De juiste VM-grootte bepalen voordat u aanschaft

Voordat u een reservering koopt, moet u de grootte van de virtuele machine die u moet bepalen. De volgende secties ziet u de juiste VM-grootte bepalen.

### <a name="use-reservation-recommendations"></a>Aanbevelingen voor gebruik in reservering

U kunt de reservering aanbevelingen gebruiken om te bepalen van de reserveringen die u moet aanschaffen.

- Aanschafaanbevelingen en de aanbevolen hoeveelheid worden weergegeven bij de aankoop van een gereserveerde instantie van de virtuele machine in Azure portal.
- Azure Advisor biedt aanschafaanbevelingen voor afzonderlijke abonnementen.  
- Aankoop aanbevelingen ophalen voor zowel gedeeld bereik en bereik van één abonnement kunt u de API's. Zie voor meer informatie, [gereserveerd exemplaar aankoop aanbeveling API's voor zakelijke klanten](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).
- Kopen voor EA-klanten, aanbevelingen voor gedeelde en één abonnement bereiken zijn beschikbaar met de [Azure Consumption Insights Power BI-inhoudspakket](/power-bi/service-connect-to-azure-consumption-insights).

### <a name="analyze-your-usage-information"></a>De informatie over het gebruik analyseren
U moet uw informatie over het gebruik om te bepalen welke reserveringen die u moet aanschaffen analyseren.

Gebruiksgegevens is beschikbaar in het gebruik van bestands- en API's. Gebruik deze samen om te bepalen welke reservering te kopen. U moet controleren voor VM-exemplaren met intensief gebruik dagelijks om te bepalen het aantal reserveringen om aan te schaffen.

Voorkomen dat de `Meter` subcategorie en `Product` velden in gegevens over het gebruik. Ze geen onderscheid maken tussen VM-grootten die gebruikmaken van premium storage. Als u deze velden om te bepalen van de VM-grootte voor het kopen van reserveringen, kunt u de juiste grootte kan kopen. Vervolgens ontvangen geen de reserveringskorting die u verwacht. In plaats daarvan verwijzen naar de `AdditionalInfo` veld in uw van gebruiksbestand of de usage-API om te bepalen van de juiste VM-grootte.

### <a name="purchase-restriction-considerations"></a>Overwegingen met betrekking tot aankoop-beperking

Gereserveerde VM-instanties zijn beschikbaar voor de meeste VM-grootten met enkele uitzonderingen. Reservering kortingen zijn niet van toepassing voor de volgende virtuele machines:

- **VM-reeks** -A-serie, Av2-serie en G-serie.

- **Virtuele machines in de Preview-versie** -een VM-reeks of de grootte die is beschikbaar als preview.

- **Clouds** -reserveringen zijn niet beschikbaar voor aankoop in Duitsland en China-regio's.

- **Onvoldoende quotum** -vCPU-quotum is beschikbaar in het abonnement voor de nieuwe gereserveerde instanties beschikken over een reservering die is afgestemd op één abonnement. Bijvoorbeeld, als het doelabonnement een limiet van 10 vcpu's voor uit de D-serie heeft, kan niet klikt u vervolgens u kopen een reservering voor 11 Standard_D1 exemplaren. De controle van het quotum voor reserveringen bevat de virtuele machines die al zijn geïmplementeerd in het abonnement. Bijvoorbeeld, als het abonnement een quotum van 10 vcpu's voor uit de D-serie heeft en twee standard_D1 instanties zijn geïmplementeerd, kunt klikt u vervolgens u kopen een reservering voor 10 standard_D1-exemplaren in dit abonnement. U kunt [offerte toename van de aanvraag maken](../articles/azure-supportability/resource-manager-core-quotas-request.md) om dit probleem te verhelpen.

- **Capaciteitsbeperkingen** - In zeldzame gevallen, Azure-limieten de aanschaf van nieuwe reserveringen voor een subset van VM-, vanwege onvoldoende capaciteit in een regio grootten.

## <a name="buy-a-reserved-vm-instance"></a>Kopen van een gereserveerde VM-exemplaar

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer **toevoegen** naar een nieuwe reservering kopen.
4. Vul de vereiste velden in. Actieve virtuele machine-exemplaren die overeenkomen met de kenmerken die u in aanmerking komen voor het ophalen van de reserveringskorting. Het werkelijke nummer van uw VM-exemplaren die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Description|
    |------------|--------------|
    |Name        |De naam van deze reservering.|
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor de reservering. De betalingswijze voor het abonnement wordt in rekening gebracht de kosten vooraf voor de reservering. Het abonnementstype moet een enterprise agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P) en betalen per gebruik (getallen bieden: MS-AZR-0003P of MS-AZR - 0023 P). Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|    
    |Bereik       |Bereik van de reservering kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - de reserveringskorting wordt toegepast op virtuele machines in dit abonnement. </li><li>Gedeeld - wordt de reserveringskorting toegepast op virtuele machines die worden uitgevoerd in alle abonnementen binnen de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Regio    |De Azure-regio die wordt gedekt door de reservering.|    
    |VM-grootte     |De grootte van de VM-exemplaren.|
    |Optimaliseren voor     |VM-instantieflexibiliteit grootte van toepassing is de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Capaciteitsprioriteit bepaalt de volgorde van Datacenter-capaciteit voor uw implementaties. Dit biedt extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt. Capaciteitsprioriteit is alleen beschikbaar als het reserveringsbereik Eén abonnement is. |
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht in de reservering. Het aantal is het aantal actieve VM-exemplaren die de korting voor facturering krijgt. Bijvoorbeeld, als u 10 Standard_D2 VM's in de VS-Oost, geeft vervolgens u hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |
5. U kunt de kosten van de reservering weergeven wanneer u selecteert **kosten berekenen**.

    ![Schermafbeelding alvorens het kopen van reserveringen](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecteer **Aankoop**.
7. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

    ![Schermopname na de aankoop van de reservering verzenden](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="change-a-reservation-after-purchase"></a>Een reservering na de aankoop wijzigen

U kunt de volgende typen wijzigingen aanbrengen in een reservering na de aankoop:

- Reserveringsbereik bijwerken
- Grootte-instantieflexibiliteit (indien van toepassing)
- Eigendom

U kunt ook een reservering in kleinere segmenten en merge al gesplitst reserveringen splitsen. Geen van de wijzigingen die ertoe leiden dat een nieuwe commerciële transactie of wijzigt u de einddatum van de reservering.

U niet de volgende typen wijzigingen na de aankoop wordt rechtstreeks:

- Een bestaande reservering regio
- SKU
- Hoeveelheid
- Duur

U kunt echter *exchange* een reservering als u wijzigingen wilt aanbrengen.

## <a name="cancellations-and-exchanges"></a>Annuleringen en uitwisselingen

Als u uw reservering wilt annuleren, worden er mogelijk kosten voor vroegtijdige beëindiging van 12% in rekening gebracht. Restituties worden gebaseerd op de laagste prijs, zijnde de aankoopprijs of de huidige prijs van de reservering, welke het laagste is. Restituties zijn beperkt tot $ 50.000 per jaar. De restitutie die u ontvangt, is het resterende bedrag naar rato saldo minus de kosten voor vroegtijdige beëindiging van 12%. Om aan te vragen bij een annulering, gaat u naar de reservering in de Azure portal en selecteer **gerestitueerd** een ondersteuningsaanvraag wilt maken.

Als u de reservering van gereserveerde VM-instanties wilt wijzigen in een andere regio, groep voor VM-grootte of periode, kunt u de reservering uitwisselen voor een andere reservering van gelijke of grotere waarde. De begindatum van de periode voor de nieuwe reservering wordt niet meegenomen naar de uitgewisselde reservering. De term 1 of 3 jaar wordt gestart wanneer u de nieuwe reservering maakt. Om aan te vragen bij een uitwisseling, gaat u naar de reservering in Azure portal en selecteer **Exchange** een ondersteuningsaanvraag wilt maken.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van een reservering, [Azure-reserveringen beheren](../articles/billing/billing-manage-reserved-vm-instance.md).
- Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:
    - [Wat zijn Azure-reserveringen?](../articles/billing/billing-save-compute-costs-reservations.md)
    - [Reserveringen in Azure beheren](../articles/billing/billing-manage-reserved-vm-instance.md)
    - [Begrijpen hoe de reserveringskorting wordt toegepast](../articles/billing/billing-understand-vm-reservation-charges.md)
    - [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../articles/billing/billing-understand-reserved-instance-usage.md)
    - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
    - [Kosten van de Windows-software is niet opgenomen in de reserveringen](../articles/billing/billing-reserved-instance-windows-software-costs.md)
    - [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)
