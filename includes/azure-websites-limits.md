| Resource | Gratis | Gedeeld (preview) | Basic | Standard | Premium </th> |
| --- | --- | --- | --- | --- | --- |
| [Web, mobiele Apps of API apps](https://azure.microsoft.com/services/app-service/) per [App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |
| [Logische apps](https://azure.microsoft.com/services/app-service/logic/) per [App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 per kern |20 per kern |
| [App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 per regio |10 per resourcegroep |100 per resourcegroep |100 per resourcegroep |100 per resourcegroep |
| COMPUTE exemplaartype |Gedeeld |Gedeeld |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup></p> |
| [Scale-Out](../articles/app-service/web-sites-scale.md) (max. exemplaren) |1 gedeeld |1 gedeeld |3 toegewezen<sup>3</sup> |10 toegewezen<sup>3</sup> |20 toegewezen (50 in as-omgeving)<sup>3,4</sup> |
| Storage<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| CPU-tijd (5 min)<sup>6</sup> |3 minuten |3 minuten |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaardtarieven |Onbeperkt, betalen tegen standaardtarieven |
| CPU-tijd (dag)<sup>6</sup> |60 minuten |240 minuten |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaardtarieven |Onbeperkt, betalen tegen standaardtarieven |
| Geheugen (1 uur) |1024 MB per App Service-plan |1024 MB per app |N/A |N/A |N/A |
| Bandbreedte |165 MB |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkt, gelden de tarieven voor overdracht van gegevens |Onbeperkt, gelden de tarieven voor overdracht van gegevens |Onbeperkt, gelden de tarieven voor overdracht van gegevens |
| Toepassingsarchitectuur |32-bits |32-bits |32-bits/64-bits |32-bits/64-bits |32-bits/64-bits |
| Web-Sockets per exemplaar<sup>7</sup> |5 |35 |350 |Onbeperkt |Onbeperkt |
| Gelijktijdige [verbindingen het foutopsporingsprogramma](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) per toepassing |1 |1 |1 |5 |5 |
| [azurewebsites.NET-subdomein met FTP/S en SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Aangepast domein](../articles/app-service/app-service-web-tutorial-custom-domain.md) ondersteunen | |X |X |X |X |
| Aangepast domein [SSL-ondersteuning](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Onbeperkt aantal SNI SSL-verbindingen |Onbeperkt aantal SNI SSL en 1 IP SSL-verbindingen opgenomen |Onbeperkt aantal SNI SSL en 1 IP SSL-verbindingen opgenomen |
| Geïntegreerde belastingverdeler | |X |X |X |X |
| [Altijd ingeschakeld](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Geplande back-ups](../articles/app-service/web-sites-backup.md) | | | | Geplande back-ups elke 2 uur maximaal 12 back-ups per dag (handmatige + gepland) | Geplande back-ups elk uur een maximum van 50 back-ups per dag (handmatige + gepland) |
| [Automatisch schalen](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [WebJobs](../articles/app-service/web-sites-create-web-jobs.md)<sup>8</sup> |X |X |X |X |X |
| [Azure Scheduler](https://azure.microsoft.com/services/scheduler/) ondersteunen | |X |X |X |X |
| [Eindpuntbewaking](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Faseringssleuven](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Aangepaste domeinen per app</a> | |500 |500 |500 |500 |
| SLA | |<p> |99,9% |99.95%<sup>10</sup> |99.95%<sup>9</sup> |

<sup>1</sup>apps en opslagquota zijn per App Service-plan, tenzij anders vermeld.  
<sup>2</sup>het werkelijke aantal apps die u op deze machines hosten kunt is afhankelijk van de activiteit van de apps, de grootte van de machine-instanties en het bijbehorende gebruik van bronnen.  
<sup>3</sup>toegewezen instanties van verschillende grootten kunnen zijn. Zie [prijzen voor App Service](https://azure.microsoft.com/pricing/details/app-service/) voor meer informatie.  
<sup>4</sup>premium-laag staat maximaal 50 rekenprocessen toe (afhankelijk van beschikbaarheid) en 500 GB schijfruimte wanneer App Service-omgevingen, en 20 berekeningsinstanties en 250 GB opslag anders.  
<sup>5</sup>de limiet voor opslag is de totale grootte van de inhoud in alle apps in hetzelfde App Service-plan. Meer opties voor opslag zijn beschikbaar in [App Service-omgeving](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>deze resources worden beperkt door fysieke resources op de specifieke instanties (de grootte van het exemplaar en het aantal exemplaren).  
<sup>7</sup>als u een app in de Basic-laag naar twee exemplaren schalen, hebt u 350 gelijktijdige verbindingen voor elk van de twee exemplaren.  
<sup>8</sup>voert aangepaste uitvoerbare bestanden en/of scripts op aanvraag of volgens een schema of continu uitvoeren als een achtergrondtaak van uw App Service-exemplaar. Altijd beschikbaar is vereist voor de continue uitvoering van WebJobs. Azure Scheduler Gratis of Standaard is vereist voor geplande WebJobs. Er is geen vooraf gedefinieerde limiet voor het nummer van webtaken die kunnen worden uitgevoerd in een App Service-exemplaar, maar er zijn praktische grenzen die afhankelijk zijn van wat de code van de toepassing probeert uit te voeren.   
<sup>9</sup>SLA van 99,95% beschikbaar voor implementaties die meerdere exemplaren met Azure Traffic Manager geconfigureerd voor failover gebruiken.  

