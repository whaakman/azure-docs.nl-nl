| Resource | Gratis | Gedeeld (preview) | Basic | Standard | Premium (Preview)</th> |
| --- | --- | --- | --- | --- | --- |
| [Web-, mobiele of API-apps](https://azure.microsoft.com/services/app-service/) per [App Service-abonnement](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |
| [Logische apps](https://azure.microsoft.com/services/app-service/logic/) per [App Service-abonnement](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 per core |20 per core |
| [App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 per regio |10 per resourcegroep |100 per resourcegroep |100 per resourcegroep |100 per resourcegroep |
| Rekenuren voor type sjabloonexemplaar |Gedeeld |Gedeeld |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup></p> |
| [Scale-Out](../articles/app-service/web-sites-scale.md) (max. exemplaren) |1 gedeeld |1 gedeeld |3-specifieke<sup>3</sup> |10 toegewezen<sup>3</sup> |20 (50 in as-omgeving) toegewezen<sup>3,4</sup> |
| Opslag<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| CPU-tijd (5 min)<sup>6</sup> |3 minuten |3 minuten |Onbeperkte, betaalde op standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaardtarieven |Onbeperkt, betalen tegen standaardtarieven |
| CPU-tijd (dagen)<sup>6</sup> |60 minuten |240 minuten |Onbeperkte, betaalde op standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaardtarieven |Onbeperkt, betalen tegen standaardtarieven |
| Geheugen (1 uur) |1024 MB per App Service-abonnement |1024 MB per app |N.v.t. |N.v.t. |N.v.t. |
| Bandbreedte |165 MB |Onbeperkte, [tarieven voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkt, gegevensoverdracht tarieven toepassen |Onbeperkt, gegevensoverdracht tarieven toepassen |Onbeperkt, gegevensoverdracht tarieven toepassen |
| Toepassingsarchitectuur |32-bits |32-bits |32-bits/64-bits |32-bits/64-bits |32-bits/64-bits |
| Web-Sockets per exemplaar<sup>7</sup> |5 |35 |350 |Onbeperkt |Onbeperkt |
| Gelijktijdige [verbindingen het foutopsporingsprogramma](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) per toepassing |1 |1 |1 |5 |5 |
| [azurewebsites.NET subdomein met FTP-/ S en SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Aangepast domein](../articles/app-service/app-service-web-tutorial-custom-domain.md) ondersteunen | |X |X |X |X |
| Aangepast domein [SSL-ondersteuning](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Onbeperkte SNI-SSL-verbindingen |Onbeperkte SNI SSL en 1 opgenomen IP SSL-verbindingen |Onbeperkte SNI SSL en 1 opgenomen IP SSL-verbindingen |
| Geïntegreerde belastingverdeler | |X |X |X |X |
| [AlwaysOn](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Geplande back-ups](../articles/app-service/web-sites-backup.md) | | | | Geplande back-ups elke 2 uur, maximaal 12 back-ups per dag (handmatige + geplande) | Geplande back-ups elk uur een maximum van 50 back-ups per dag (handmatige + geplande) |
| [Automatisch schalen](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [WebJobs](../articles/app-service/web-sites-create-web-jobs.md)<sup>8</sup> |X |X |X |X |X |
| [Azure Scheduler](https://azure.microsoft.com/services/scheduler/) ondersteunen | |X |X |X |X |
| [Eindpuntbewaking](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Faseringssleuven](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Aangepaste domeinen per app</a> | |500 |500 |500 |500 |
| SLA | |<p> |99,9% |99.95%<sup>10</sup> |99.95%<sup>9</sup> |

<sup>1</sup>apps en opslagquota zijn per App Service-abonnement tenzij anders vermeld wordt.  
<sup>2</sup>het werkelijke aantal apps die u op deze machines hosten kunt is afhankelijk van de activiteit van de apps, de grootte van de machine-exemplaren en de bijbehorende bronnen beter worden benut.  
<sup>3</sup>specifieke exemplaren van verschillende grootte kunnen zijn. Zie [prijzen van App Service](https://azure.microsoft.com/pricing/details/app-service/) voor meer informatie.  
<sup>4</sup>premium-laag kan maximaal 50 exemplaren (afhankelijk van beschikbaarheid) en 500 GB aan schijfruimte berekent bij gebruik van App Service-omgevingen en 20 compute-exemplaren en 250 GB opslagruimte anders.  
<sup>5</sup>de opslaglimiet is de totale grootte van de inhoud voor alle apps in hetzelfde App Service-plan. Meer opties voor opslag zijn beschikbaar in [App Service-omgeving](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>deze resources worden beperkt door fysieke resources op de specifieke exemplaren (de exemplaargrootte en het aantal exemplaren van).  
<sup>7</sup>als u een app in de basisstaffel naar twee exemplaren schalen, hebt u 350 gelijktijdige verbindingen voor elk van de twee exemplaren.  
<sup>8</sup>aangepaste uitvoerbare bestanden en/of scripts uitvoeren op aanvraag volgens een schema of continu als achtergrondtaak binnen uw App Service-exemplaar. Altijd beschikbaar is vereist voor de continue uitvoering van WebJobs. Azure Scheduler Gratis of Standaard is vereist voor geplande WebJobs. Er is geen vooraf gedefinieerde limiet voor het aantal WebJobs die kunnen worden uitgevoerd in een App Service-exemplaar, maar er zijn praktische grenzen die afhankelijk zijn van wat de toepassingscode probeert te doen.   
<sup>9</sup>SLA van 99,95% die zijn opgegeven voor implementaties met meerdere exemplaren met Azure Traffic Manager is geconfigureerd voor failover.  

