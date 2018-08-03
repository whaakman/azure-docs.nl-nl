- Het VNet moet in dezelfde Azure-**regio** en hetzelfde **abonnement** voorkomen als de Batch-account.

- Voor pools die zijn gemaakt met een virtuele-machineconfiguratie, geldt dat alleen VNets op basis van Azure Resource Manager worden ondersteund. Voor pools die zijn gemaakt met een cloudserviceconfiguratie, worden alleen klassieke VNets ondersteund.
  
- Voor het gebruik van een klassiek VNET moet de `MicrosoftAzureBatch`-service-principal de toegangsbeheerrol `Classic Virtual Machine Contributor` hebben voor het betreffende VNet. Voor het gebruik van een op Azure Resource Manager gebaseerd VNet moet u gemachtigd zijn voor toegang tot het VNet en om virtuele machines te implementeren in het subnet.

- Het subnet dat is opgegeven voor de pool moet voldoende vrije IP-adressen hebben voor het aantal virtuele machines voor de pool, ofwel de som van de `targetDedicatedNodes`- en `targetLowPriorityNodes`-eigenschappen van de pool. Als het subnet onvoldoende vrije IP-adressen heeft, wijst de pool de rekenknooppunten gedeeltelijk toe en wordt een fout weergegeven voor het aanpassen van de grootte. 

- Pools in de configuratie van de virtuele machine die in Azure VNet is geïmplementeerd, wijzen automatisch extra Azure-netwerkresources toe. De volgende resources zijn nodig voor elk knooppunt met 50 pools in een VNet: 1 netwerkbeveiligingsgroep, 1 openbaar IP-adres en 1 load balancer. De resources zijn beperkt door [quota](../articles/batch/batch-quota-limit.md) in het abonnement dat het virtuele netwerk bevat en bij het maken van de Batch-pool is opgegeven.

- Het VNet moet communicatie vanuit de Batch-service toestaan om taken te kunnen plannen voor de rekenknooppunten. Dit kan worden geverifieerd door te controleren of het VNet bijbehorende netwerkbeveiligingsgroepen heeft. Als communicatie met de rekenknooppunten in het opgegeven subnet wordt geweigerd door een netwerkbeveiligingsgroep, zet de Batch-service de status van de rekenknooppunten op **Onbruikbaar**. 

- Als aan het opgegeven VNet netwerkbeveiligingsgroepen en/of een firewall zijn gekoppeld, configureert u de binnenkomende en uitgaande poorten zoals wordt weergegeven in de volgende tabellen:


  |    Doelpoort(en)    |    IP-adres van bron      |   Bronpoort    |    Voegt Batch NSG's toe?    |    Vereist opdat VM bruikbaar is?    |    Actie van gebruiker   |
  |---------------------------|---------------------------|----------------------------|----------------------------|-------------------------------------|-----------------------|
  |   <ul><li>Voor pools die zijn gemaakt met de virtuele-machineconfiguratie: 29876, 29877</li><li>Voor pools die zijn gemaakt met de cloudserviceconfiguratie: 10100, 20100, 30100</li></ul>        |    * voor aanvullende beveiliging kunt u ook IP-adressen van de Batch-service opgeven. Neem contact op met ondersteuning voor Azure om de lijst met IP-adressen van de Batch-service te verkrijgen. | * of 443 |    Ja. Batch voegt NSG's toe op het niveau van netwerkinterfaces (NIC) die zijn gekoppeld aan virtuele machines. Deze NSG's staan alleen verkeer vanuit IP-adressen van Batch-servicerollen toe. Zelfs als u deze poorten opent voor het gehele web, wordt het verkeer geblokkeerd op de NIC. |    Ja  |  U hoeft geen NSG op te geven, want Batch staat alleen Batch-IP-adressen toe. <br /><br /> Als u toch een NSG opgeeft, zorg dan dat deze poorten zijn geopend voor inkomend verkeer. <br /><br /> Als u * opgeeft als de bron-IP in uw NSG, voegt Batch nog steeds NSG's toe op het niveau van de NIC die is gekoppeld aan virtuele machines. |
  |    3389 (Windows), 22 (Linux)               |    Gebruikerscomputers die voor foutopsporing worden gebruikt, zodat u op afstand toegang hebt tot de virtuele machine.    |   *  | Nee                                    |    Nee                    |    Voeg NSG's toe als u externe toegang (RDP of SSH) tot de virtuele machine wilt toestaan.   |                                


  |    Uitgaande poort(en)    |    Doel    |    Voegt Batch NSG's toe?    |    Vereist opdat VM bruikbaar is?    |    Actie van gebruiker    |
  |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
  |    443    |    Azure Storage    |    Nee    |    Ja    |    Als u NSG's toevoegt, zorg dan dat deze poort is geopend voor uitgaand verkeer.    |

   Zorg er ook voor dat uw Azure Storage-eindpunt kan worden omgezet door alle aangepaste DNS-servers die u beschikbaar maakt voor uw VNet. Vooral URL's met de indeling `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` en `<account>.blob.core.windows.net` moeten kunnen worden omgezet. 

   Als u een op Resource Manager gebaseerde NSG (beveiligingsgroep) toevoegt, kunt u [servicetags](../articles/virtual-network/security-overview.md#service-tags) gebruiken om de Storage-IP-adressen voor de specifieke regio te selecteren voor uitgaande verbindingen. Let wel: de Storage-IP-adressen moeten zich in dezelfde regio bevinden als uw Batch-account en VNet. Servicetags zijn momenteel in preview-versie in de geselecteerde Azure-regio's.