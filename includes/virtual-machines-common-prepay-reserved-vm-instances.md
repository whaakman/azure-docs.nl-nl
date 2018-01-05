# <a name="prepay-for-virtual-machines-with-reserved-vm-instances"></a>Vooruitbetalen voor virtuele Machines met een gereserveerde VM-exemplaren

Vooruitbetalen voor virtuele machines en geld besparen met gereserveerde virtuele Machine-exemplaren. Zie voor meer informatie [gereserveerde virtuele Machine-exemplaren aanbieding](https://azure.microsoft.com/pricing/reserved-vm-instances/).

U kunt gereserveerde virtuele Machine-exemplaren kopen de [Azure-portal](https://portal.azure.com). Een instantie van de gereserveerde virtuele Machine aanschaffen:
-   U moet zich in een rol van eigenaar voor ten minste één onderneming of abonnement op gebruiksbasis.
-   Voor Enterprise-abonnementen reservering aankopen moeten zijn ingeschakeld in de [EA portal](https://ea.azure.com).

## <a name="buy-a-reserved-virtual-machine-instance"></a>Een exemplaar van de gereserveerde virtuele Machine kopen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **meer Services** > **reserveringen**.
3. Selecteer **toevoegen** om aan te schaffen van een nieuwe reservering.
4. Vul de vereiste velden in. Actieve virtuele machine-exemplaren die overeenkomen met de kenmerken die u selecteert om op te halen van de reservering korting te kwalificeren. Het werkelijke nummer van uw VM-exemplaren die de korting te krijgen, is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van deze reservering.| 
    |Abonnement|Het abonnement dat wordt gebruikt om de reservering te betalen. De betalingsmethode voor het abonnement wordt in rekening gebracht de opstartkosten voor de reservering. Het abonnementstype moet een enterprise agreement (bieden nummer: MS-AZR - 0017P) of betalen naar gebruik (bieden nummer: MS-AZR - 0003P). De kosten worden afgetrokken van de inschrijving bedrag saldo of in rekening gebracht als overschrijding voor een enterprise-abonnement. Voor abonnement met betalen naar gebruik, zijn de kosten in rekening gebracht op de creditcard of factuur betalingsmethode voor het abonnement.|    
    |Bereik       |De reservering bereik betrekking op één abonnement of meerdere abonnementen (gedeelde scope). Als u selecteert: <ul><li>Één abonnement - de reservering korting wordt toegepast op virtuele machines in dit abonnement. </li><li>Gedeeld - wordt de reservering korting toegepast op virtuele machines die worden uitgevoerd in de abonnementen die binnen uw facturering context. Voor zakelijke klanten is het gedeelde bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van abonnementen ontwikkelen en testen) binnen de inschrijving. Voor klanten betalen naar gebruik is het gedeelde bereik alle betalen naar gebruik abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Locatie    |De Azure-regio die wordt gedekt door de reservering.|    
    |VM-grootte     |De grootte van de VM-exemplaren.|
    |Termijn        |Een jaar of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht in de reservering. Het aantal is het aantal actieve VM-exemplaren die de facturering korting kunnen ophalen. Bijvoorbeeld, als u 10 Standard_D2 VM's in VS-Oost uitvoert, wilt u opgeven hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |
5. U kunt de kosten van de reservering weergeven wanneer u selecteert **kosten te berekenen**.

    ![Schermopname vóór het verzenden van de reservering aankoop](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecteer **aankoop**.
7. Selecteer **weergeven van deze reservering** voor de status van uw aankoop.

    ![Schermopname vóór het verzenden van de reservering aankoop](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps-after-buying-a-reservation"></a>Volgende stappen na de aankoop van een reservering
De reservering korting wordt automatisch toegepast op het aantal virtuele machines die overeenkomen met het bereik van de reservering en de kenmerken die worden uitgevoerd. U kunt het bereik van de reservering via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI of via de API. 

Zie voor meer informatie over het beheren van een reservering, [beheren Azure gereserveerde virtuele Machine-exemplaren](../articles/billing/billing-manage-reserved-vm-instance.md).

