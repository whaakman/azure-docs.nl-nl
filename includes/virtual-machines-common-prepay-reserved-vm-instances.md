---
ms.topic: include
ms.openlocfilehash: c30dbe25252a18e1edaed5ec81d86cc1dd976250
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513782"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties

Betaal vooruit voor virtuele machines en bespaar geld met instanties voor Azure gereserveerde virtuele Machine (VM). Zie voor meer informatie, [Azure Reserved VM Instances aanbieding](https://azure.microsoft.com/pricing/reserved-vm-instances/).

U kunt gereserveerde instanties van Azure kopen de [Azure-portal](https://portal.azure.com). Een gereserveerde instantie kopen:
-   U moet zich in een rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
-   Voor Enterprise-abonnementen, aankopen in de gereserveerde instantie moeten zijn ingeschakeld in de [EA-portal](https://ea.azure.com).
-   Voor het programma Cloud Solution Provider (CSP) kunnen alleen de agents beheerder of de verkoop-agents de gereserveerde instanties kopen.

## <a name="determine-the-right-vm-size-before-purchase"></a>Bepaal de juiste VM-grootte voor aankoop
De velden metersubcategorie en Product in de gebruiksgegevens geen onderscheid maakt tussen VM-grootten die gebruikmaken van premiumopslag van VM-grootten die niet van premiumopslag gebruikmaken, met behulp van deze veld om te bepalen van de VM-grootte voor het kopen van reserveringen kan leiden tot onjuiste reservering kopen en geeft u reservering kortingen. Gebruik een van de onderstaande methoden om te bepalen van de juiste VM-grootte voor de reserveringsaankoop.
- Raadpleeg de aanvullende informatie > ServiceType veld in uw van gebruiksbestand of de gegevens over gebruik API. Dit veld biedt de juiste VM-grootte bij het implementeren van virtuele machines die van premium storage gebruikmaken.
- U krijgt ook een nauwkeurige VM-grootte, met behulp van Powershell, Azure Resource Manager of gegevens van virtuele machine in Azure portal.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Kopen van een gereserveerde VM-exemplaar
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer **toevoegen** naar een nieuwe gereserveerde instantie aanschaft.
4. Vul de vereiste velden in. Actieve virtuele machine-exemplaren die overeenkomen met de kenmerken die u in aanmerking komen voor het ophalen van de gereserveerde instantie korting. Het werkelijke nummer van uw VM-exemplaren die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van dit gereserveerde instantie.| 
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor de gereserveerde instantie. Via de betalingswijze voor het abonnement worden de voorafgaande kosten voor de gereserveerde instantie in rekening gebracht. Het abonnement moet een Enterprise-overeenkomst (aanbieding: MS-AZR-0017P) of een Betalen-per-gebruik-abonnement (aanbieding: MS-AZR-0003P) zijn. Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|    
    |Bereik       |Bereik van de gereserveerde instantie kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - korting van de gereserveerde instantie wordt toegepast op virtuele machines in dit abonnement. </li><li>Gedeeld - wordt korting van de gereserveerde instantie toegepast op virtuele machines die worden uitgevoerd in alle abonnementen binnen de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van dev/test-abonnementen) binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Locatie    |De Azure-regio die wordt gedekt door de gereserveerde instantie.|    
    |VM-grootte     |De grootte van de VM-exemplaren.|
    |Optimaliseren voor     |VM-instantieflexibiliteit grootte van toepassing is de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Capaciteitsprioriteit reserveert datacenter-capaciteit voor uw implementaties. Dit biedt extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt. Capaciteitsprioriteit is alleen beschikbaar als het reserveringsbereik Eén abonnement is. |
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht in de gereserveerde instantie. Het aantal is het aantal actieve VM-exemplaren die de korting voor facturering krijgt. Bijvoorbeeld, als u 10 Standard_D2 VM's in de VS-Oost, geeft vervolgens u hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |
5. U kunt de kosten van de gereserveerde instantie weergeven wanneer u selecteert **kosten berekenen**.

    ![Schermafbeelding voor het indienen van de aangeschafte gereserveerde instantie](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecteer **Aankoop**.
7. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

    ![Schermopname na het indienen van de aangeschafte gereserveerde instantie](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Volgende stappen 
De gereserveerde instantie korting wordt automatisch toegepast op het aantal actieve virtuele machines die overeenkomen met het bereik van de gereserveerde instantie en de kenmerken. U kunt het bereik van de gereserveerde instantie via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI of via de API. 

Zie voor meer informatie over het beheren van een gereserveerde instantie, [gereserveerde instanties in Azure beheren](../articles/billing/billing-manage-reserved-vm-instance.md).

Zie voor meer informatie over gereserveerde instanties van Azure, de volgende artikelen:

- [Wat zijn Azure Reserved VM Instances?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Gereserveerde instanties in Azure beheren](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Inzicht in hoe korting op gereserveerde instanties wordt toegepast](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Inzicht in het gebruik van gereserveerde instanties voor betalen per gebruik](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in het gebruik van gereserveerde instanties voor Enterprise Enrollment](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Kosten van Windows-software zijn niet inbegrepen in gereserveerde instanties](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Gereserveerde instanties in CSP-programma (Cloud Solution Provider) van partnercentrum](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
