In deze stap kunt u de beschikbaarheidsgroep-listener testen met behulp van een clienttoepassing die wordt uitgevoerd op hetzelfde netwerk.

Clientverbinding heeft de volgende vereisten:

* Clientverbindingen met de listener moeten afkomstig zijn van de machines die zich bevinden in een andere cloudservice dan de versie die als host fungeert voor de beschikbaarheidsreplica's altijd op.
* Als wordt altijd op replica's in verschillende subnetten, moeten clients opgeven *MultisubnetFailover = True* in de verbindingstekenreeks. Deze voorwaarde resulteert in parallelle verbindingspogingen met replica's in de verschillende subnetten. Dit scenario bevat een regio-overschrijdende altijd op beschikbaarheid groep-implementatie.

Een voorbeeld hiervan is verbinding maken met de listener voor een van de virtuele machines in hetzelfde virtuele netwerk van Azure (maar niet een die als host fungeert voor een replica). Er is een eenvoudige manier om te voltooien van deze test om te proberen verbinding maken met SQL Server Management Studio de beschikbaarheidsgroep-listener. Een andere eenvoudige methode is het uitvoeren van [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)als volgt:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Als de waarde EndpointPort *1433*, u niet opgeven in de aanroep zijn vereist. De vorige aanroep wordt ervan uitgegaan dat de clientcomputer wordt gekoppeld aan hetzelfde domein en dat de aanroeper heeft machtigingen zijn toegekend op de database met behulp van Windows-verificatie.
> 
> 

Wanneer u de listener test, moet u de beschikbaarheidsgroep om ervoor te zorgen dat clients verbinding met de listener via failovers maken kunnen failover.

