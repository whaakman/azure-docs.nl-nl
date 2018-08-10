---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 08-07-2018
ms.openlocfilehash: 19a153a5cdc9d5f878494984313baebd12dbcbb5
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631160"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties

Betaal vooruit voor virtuele machines en bespaar geld met instanties voor Azure gereserveerde virtuele Machine (VM). Zie voor meer informatie, [Azure Reserved VM Instances aanbieding](https://azure.microsoft.com/pricing/reserved-vm-instances/).

U kunt een gereserveerde VM-instantie kopen de [Azure-portal](https://portal.azure.com). Een exemplaar aanschaffen:

- U moet zich in een rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
- Voor Enterprise-abonnementen, aankopen in de reservering moeten zijn ingeschakeld in de [EA-portal](https://ea.azure.com).
- Voor het programma Cloud Solution Provider (CSP) kunnen alleen de agents beheerder of de verkoop-agents de reserveringen kopen.

## <a name="determine-the-right-vm-size-before-purchase"></a>Bepaal de juiste VM-grootte voor aankoop

De velden metersubcategorie en Product in de gebruiksgegevens geen onderscheid maakt tussen VM-grootten die gebruikmaken van premiumopslag van VM-grootten die niet van premiumopslag gebruikmaken, met behulp van deze veld om te bepalen van de VM-grootte voor het kopen van reserveringen kan leiden tot onjuiste reservering kopen en geeft u reservering kortingen. Gebruik een van de onderstaande methoden om te bepalen van de juiste VM-grootte voor de reserveringsaankoop.

- Verwijzen naar het veld AdditionalInfo in uw van gebruiksbestand of de usage-API om te bepalen het juiste VM-grootte voor een aankoop van de reservering. Gebruik de waarden niet vanaf de velden metersubcategorie of Product, omdat deze velden geen onderscheid tussen S en niet-S-versies van een virtuele machine maken.
- U krijgt ook een nauwkeurige VM-grootte, met behulp van Powershell, Azure Resource Manager, of gegevens van virtuele machine in Azure portal.

Gereserveerde VM-instanties zijn beschikbaar voor de meeste VM-grootten met enkele uitzonderingen:

- Virtuele machines in de Preview-versie: een VM-reeks of de grootte die in de Preview-versie zijn niet beschikbaar voor het kopen van reserveringen.
- Clouds: reserveringen zijn niet beschikbaar voor aankoop in de regio's Azure US Government, Duitsland en China.
- Onvoldoende quotum: een reservering die is afgestemd op één abonnement vCPU-quotum is beschikbaar in het abonnement voor de nieuwe gereserveerde instantie moet hebben. Bijvoorbeeld, als het doelabonnement een limiet van 10 vcpu's voor uit de D-serie heeft, kan niet klikt u vervolgens u kopen een reservering voor 11 Standard_D1 exemplaren. De controle van het quotum voor reserveringen bevat de virtuele machines die al zijn geïmplementeerd in het abonnement. Bijvoorbeeld, als het abonnement een quotum van 10 vcpu's voor uit de D-serie heeft en twee standard_D1 instanties zijn geïmplementeerd, kunt klikt u vervolgens u kopen een reservering voor 10 standard_D1-exemplaren in dit abonnement. 
- Capaciteitsbeperkingen voor: In zeldzame gevallen kan Azure beperkt de aanschaf van nieuwe reserveringen voor een subset van de VM-grootten, vanwege onvoldoende capaciteit in een regio.

## <a name="buy-a-reserved-vm-instance"></a>Kopen van een gereserveerde VM-exemplaar

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer **toevoegen** naar een nieuwe reservering kopen.
4. Vul de vereiste velden in. Actieve virtuele machine-exemplaren die overeenkomen met de kenmerken die u in aanmerking komen voor het ophalen van de reserveringskorting. Het werkelijke nummer van uw VM-exemplaren die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van deze reservering.| 
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor de reservering. De betalingswijze voor het abonnement wordt in rekening gebracht de kosten vooraf voor de reservering. Het abonnement moet een Enterprise-overeenkomst (aanbieding: MS-AZR-0017P) of een Betalen-per-gebruik-abonnement (aanbieding: MS-AZR-0003P) zijn. Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|    
    |Bereik       |Bereik van de reservering kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - de reserveringskorting wordt toegepast op virtuele machines in dit abonnement. </li><li>Gedeeld - wordt de reserveringskorting toegepast op virtuele machines die worden uitgevoerd in alle abonnementen binnen de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van dev/test-abonnementen) binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Locatie    |De Azure-regio die wordt gedekt door de reservering.|    
    |VM-grootte     |De grootte van de VM-exemplaren.|
    |Optimaliseren voor     |VM-instantieflexibiliteit grootte van toepassing is de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Capaciteitsprioriteit reserveert datacenter-capaciteit voor uw implementaties. Dit biedt extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt. Capaciteitsprioriteit is alleen beschikbaar als het reserveringsbereik Eén abonnement is. |
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht in de reservering. Het aantal is het aantal actieve VM-exemplaren die de korting voor facturering krijgt. Bijvoorbeeld, als u 10 Standard_D2 VM's in de VS-Oost, geeft vervolgens u hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |
5. U kunt de kosten van de reservering weergeven wanneer u selecteert **kosten berekenen**.

    ![Schermafbeelding alvorens het kopen van reserveringen](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecteer **Aankoop**.
7. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

    ![Schermopname na de aankoop van de reservering verzenden](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Volgende stappen

De reserveringskorting wordt automatisch toegepast op het aantal actieve virtuele machines die overeenkomen met het reserveringsbereik en kenmerken. U kunt het bereik van de reservering via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI, of via de API.

Zie voor meer informatie over het beheren van een reservering, [beheren Azure reserveringen](../articles/billing/billing-manage-reserved-vm-instance.md).

Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

- [Wat zijn Azure reserveringen?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Reserveringen in Azure beheren](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
