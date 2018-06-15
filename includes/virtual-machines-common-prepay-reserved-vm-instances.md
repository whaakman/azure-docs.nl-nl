---
ms.topic: include
ms.openlocfilehash: 8e710bebf979b60f61552593ae550e95a8340d2b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34307563"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-exemplaren

Vooruitbetalen voor virtuele machines en geld besparen met exemplaren van Azure gereserveerde virtuele Machine (VM). Zie voor meer informatie [gereserveerde exemplaren van Azure-aanbieding](https://azure.microsoft.com/pricing/reserved-vm-instances/).

U kunt Azure gereserveerde exemplaren kopen de [Azure-portal](https://portal.azure.com). Een gereserveerde exemplaar aanschaffen:
-   U moet zich in een rol van eigenaar voor ten minste één onderneming of abonnement op gebruiksbasis.
-   Voor Enterprise-abonnementen aankopen gereserveerde exemplaar moeten zijn ingeschakeld in de [EA portal](https://ea.azure.com).
-   Voor het programma Cloud Solution Provider (CSP) aanschaffen alleen de beheerder of agenten verkopers de gereserveerde exemplaren.

## <a name="buy-a-reserved-instance"></a>Een gereserveerde exemplaar kopen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer **toevoegen** een nieuw exemplaar van de gereserveerde kopen.
4. Vul de vereiste velden in. Actieve virtuele machine-exemplaren die overeenkomen met de kenmerken die u selecteert om op te halen van de gereserveerde exemplaar korting te kwalificeren. Het werkelijke nummer van uw VM-exemplaren die de korting te krijgen, is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van dit gereserveerde exemplaar.| 
    |Abonnement|Het abonnement dat u gebruikt om te betalen voor het gereserveerde exemplaar. De betalingsmethode voor het abonnement wordt in rekening gebracht de opstartkosten voor het gereserveerde exemplaar. Het abonnementstype moet een enterprise agreement (bieden nummer: MS-AZR - 0017P) of betalen naar gebruik (bieden nummer: MS-AZR - 0003P). De kosten worden afgetrokken van de inschrijving bedrag saldo of in rekening gebracht als overschrijding voor een enterprise-abonnement. Voor abonnement met betalen naar gebruik, zijn de kosten in rekening gebracht op de creditcard of factuur betalingsmethode voor het abonnement.|    
    |Bereik       |Van de instantie van het gereserveerde bereik betrekking op één abonnement of meerdere abonnementen (gedeelde scope). Als u selecteert: <ul><li>Één abonnement - exemplaar met de gereserveerde korting wordt toegepast op virtuele machines in dit abonnement. </li><li>Gedeeld - wordt de gereserveerde exemplaar korting toegepast op virtuele machines die worden uitgevoerd in de abonnementen die binnen uw facturering context. Voor zakelijke klanten is het gedeelde bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van abonnementen ontwikkelen en testen) binnen de inschrijving. Voor klanten betalen naar gebruik is het gedeelde bereik alle betalen naar gebruik abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Locatie    |De Azure-regio die wordt gedekt door het gereserveerde exemplaar.|    
    |VM-grootte     |De grootte van de VM-exemplaren.|
    |Termijn        |Een jaar of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht binnen de gereserveerde exemplaar. Het aantal is het aantal actieve VM-exemplaren die de facturering korting kunnen ophalen. Bijvoorbeeld, als u 10 Standard_D2 VM's in de VS-Oost uitvoert, wilt u opgeven hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |
5. U kunt de kosten van het gereserveerde exemplaar weergeven wanneer u selecteert **kosten te berekenen**.

    ![Schermopname vóór het verzenden van de gereserveerde exemplaar aankoop](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecteer **aankoop**.
7. Selecteer **weergeven van deze reservering** voor de status van uw aankoop.

    ![Schermopname na de aankoop gereserveerde exemplaar verzenden](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Volgende stappen 
De gereserveerde exemplaar korting wordt automatisch toegepast op het aantal virtuele machines die overeenkomen met het exemplaar van het gereserveerde bereik en kenmerken worden uitgevoerd. U kunt het bereik van de gereserveerde exemplaar via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI of via de API. 

Zie voor meer informatie over het beheren van een gereserveerde exemplaar, [gereserveerde exemplaren Azure beheren](../articles/billing/billing-manage-reserved-vm-instance.md).

Zie voor meer informatie over gereserveerde exemplaren van Azure, de volgende artikelen:

- [Geld besparen op virtuele machines met gereserveerde exemplaren](../articles/billing/billing-save-compute-costs-reservations.md)
- [Azure-gereserveerde exemplaren beheren](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Gereserveerde exemplaren in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.