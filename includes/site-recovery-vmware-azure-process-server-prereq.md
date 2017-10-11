In dit artikel wordt van het volgende uitgegaan:

1. Er is reeds een **site-naar-site VPN**- of een **Express Route**-verbinding tot stand gebracht tussen uw on-premises netwerk en Azure Virtual Network.
2. Uw gebruikersaccount heeft machtigingen voor het maken van een nieuwe virtuele machine in het Azure-abonnement waar de virtuele machines tijdens een failover zijn geplaatst.
3. Uw abonnement heeft een minimum van 4 kernen beschikbaar voor de implementatie van een nieuwe virtuele machine voor de processerver.
4. U hebt de beschikking over de **wachtwoordzin voor de configuratieserver**.

> [!TIP]
> Zorg ervoor dat u verbinding hebt met poort 443 van de configuratieserver (wordt on-premises uitgevoerd) van Azure Virtual Network waar de virtuele machines tijdens een failover zijn geplaatst.
