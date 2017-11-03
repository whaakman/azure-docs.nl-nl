
U kunt meer informatie over Azure Cosmos DB globale distributiepunten in deze Azure vrijdag video met Scott Hanselman en Principal Engineering Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Zie voor meer informatie over hoe globale databasereplicatie in Azure Cosmos DB werkt, [Distribueer gegevens globaal met Cosmos DB](../articles/cosmos-db/distribute-data-globally.md).

## <a id="addregion"></a>Globale database regio's met de Azure-Portal toevoegen
Azure Cosmos DB is beschikbaar in alle [Azure-regio's] [ azureregions] wereldwijd. Na het selecteren van het standaardniveau voor consistentie voor het account van uw database, koppelt u een of meer regio's (afhankelijk van uw keuze standaard consistentie niveau en globale distributie moet).

1. In de [Azure-portal](https://portal.azure.com/), klik in de linkerbalk op **Azure Cosmos DB**.
2. In de **Azure Cosmos DB** blade, selecteer de database-account te wijzigen.
3. Klik op de accountblade **globaal gegevens repliceren** in het menu.
4. In de **globaal gegevens repliceren** blade, selecteer de regio's toevoegen of verwijderen door te klikken op gebieden in de kaart en klik vervolgens op **opslaan**. Er is een kosten voor het toevoegen van regio's, raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) of de [gegevens globaal met Azure Cosmos DB distribueren](../articles/cosmos-db/distribute-data-globally.md) artikel voor meer informatie.
   
    ![Klik op de regio's in de kaart toevoegen of verwijderen][1]
    
Als u een tweede regio toevoegt de **handmatige Failover** optie is ingeschakeld op de **globaal gegevens repliceren** blade in de portal. Deze optie kunt u het failoverproces van testen of de regio primaire schrijven wijzigen. Als u een derde regio toevoegt de **Failover prioriteiten** optie is ingeschakeld op de blade dezelfde zodat u kunt de failovervolgorde voor leesbewerkingen wijzigen.  

### <a name="selecting-global-database-regions"></a>Globale database regio's selecteren
Er zijn twee algemene scenario's voor het configureren van twee of meer gebieden:

1. Lage latentie toegang leveren om gegevens naar eindgebruikers ongeacht waar ze zich overal ter wereld bevinden te
2. Toevoegen van regionale tolerantie voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR)

Voor het leveren van lage latentie voor eindgebruikers, is het raadzaam zowel de toepassing implementeren en toevoegen van Azure Cosmos-database in de regio's die overeenkomen met het is aan waar gebruikers van de toepassing zich bevinden.

Voor BCDR, wordt aanbevolen om toe te voegen regio's op basis van de regio-paren die wordt beschreven in de [zakelijke continuïteit en herstel na noodgevallen (BCDR): Azure-gebieden gekoppeld] [ bcdr] artikel.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
