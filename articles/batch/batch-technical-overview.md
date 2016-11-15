---
title: Basisbeginselen van Azure Batch-service | Microsoft Docs
description: Meer informatie over het gebruik van de Azure Batch-service voor grootschalige parallelle en HPC-workloads
services: batch
documentationcenter: 
author: mmacy
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/22/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2559aae752f319805c2c9f87a2e2a81d9b37e4b2


---
# <a name="basics-of-azure-batch"></a>Basisbeginselen van Azure Batch
In Azure Batch kunt u grootschalige parallelle en High Performance Computing (HPC)-toepassingen efficiënt in de cloud uitvoeren. Dit is een platformservice die de uitvoering van rekenintensief werk op een beheerde verzameling van virtuele machines plant en automatisch het aantal rekenresources kan aanpassen aan de behoeften van uw jobs.

Met de Batch-service definieert u Azure-rekenresources om uw toepassingen parallel en op de juiste schaal uit te voeren. U kunt taken op aanvraag uitvoeren of geplande taken uitvoeren en u hoeft niet handmatig een HPC-cluster, individuele virtuele machines, virtuele netwerken of een complexe job- en taakplanningsinfrastructuur te maken, configureren en beheren.

## <a name="use-cases-for-batch"></a>Gebruiksvoorbeelden voor Batch
Batch is een beheerde Azure-service die wordt gebruikt voor *batchverwerking* of *batchberekeningen*, waarbij een groot aantal vergelijkbare taken wordt uitgevoerd om een bepaald gewenst resultaat te verkrijgen. Batchverwerking wordt doorgaans gebruikt door organisaties die regelmatig grote hoeveelheden gegevens verwerken, transformeren en analyseren.

Batch is bijzonder geschikt voor intrinsiek parallelle (ook wel bekend als 'perfect parallelle') toepassingen en workloads. Intrinsiek parallelle workloads worden eenvoudig onderverdeeld in meerdere taken die op meerdere computers tegelijk werk verrichten.

![Parallelle taken][1]<br/>

Dit zijn enkele voorbeelden van workloads die vaak met deze techniek worden verwerkt:

* Modellering van financiële risico's
* Hydrologische en klimaatgegevensanalyse
* Rendering, analyse en verwerking van beelden
* Mediacodering en -transcodering
* Genetische sequentieanalyse
* Technische spanningsanalyse
* Softwaretests

Batch kan ook parallelle berekeningen met aan het einde een verkleiningsstap, evenals complexere HPC-workloads, uitvoeren zoals [Message Passing Interface (MPI)](batch-mpi.md)-toepassingen.

Zie [Batch and HPC solutions](batch-hpc-solutions.md) (Batch- en HPC-oplossingen) voor een vergelijking tussen Batch en andere opties voor HPC-oplossingen in Azure.

## <a name="developing-with-batch"></a>Ontwikkelen met Batch
Het verwerken van parallelle workloads met Batch gebeurt gewoonlijk via ene programma met één van de [Batch API’s](#batch-development-apis). Met de Batch-API's maakt en beheert u pools van rekenknooppunten (virtuele machines) en plant u jobs en taken voor uitvoering op die knooppunten. Een clienttoepassing of -service die u schrijft, gebruikt de Batch-API's om met de Batch-service te communiceren.

U kunt voor uw organisatie efficiënt grootschalige workloads verwerken of uw klanten een front-endservice aanbieden, zodat zij (op aanvraag of gepland) jobs en taken kunnen uitvoeren op één knooppunt of honderden of zelfs duizenden knooppunten. U kunt Batch ook gebruiken als onderdeel van een grotere werkstroom, beheerd door hulpprogramma's zoals [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [!TIP]
> Wanneer u gereed bent om met de Batch-API aan de slag te gaan en u te verdiepen in de mogelijkheden ervan, leest u het [Overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md).
> 
> 

### <a name="azure-accounts-youll-need"></a>Azure-accounts die u nodig hebt
Wanneer u Batch-oplossingen ontwikkelt, gebruikt u de volgende accounts in Microsoft Azure.

* **Azure-account en -abonnement** - Als u nog geen Azure-abonnement hebt, kunt u uw [voordelen als MSDN-abonnee][msdn_benefits] activeren of u aanmelden voor een [gratis Azure-account][free_account]. Wanneer u een account maakt, wordt voor u een standaardabonnement gemaakt.
* **Batch-account** - Wanneer uw toepassingen met de Batch-service communiceren, worden de accountnaam, de URL van het account en een toegangssleutel als referenties gebruikt. Al uw Batch-resources zoals pools, rekenknooppunten, jobs en taken worden aan een Batch-account gekoppeld. U kunt een [Batch-account maken](batch-account-create-portal.md) in Azure Portal.
* **Storage-account** - Batch bevat ingebouwde ondersteuning voor het werken met bestanden in [Azure Storage][azure_storage]. Vrijwel elk Batch-scenario gebruikt Azure Storage; voor het faseren van de programma's die door de taken worden uitgevoerd en de gegevens die zij verwerken, en voor de opslag van uitvoergegevens die zij genereren. Zie [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md) voor het maken van een opslagaccount.

### <a name="batch-development-apis"></a>Batch-ontwikkelings-API's
De toepassingen en services kunnen rechtstreekse REST API-aanroepen uitgeven, een of meer van de volgende clientbibliotheken gebruiken of een combinatie van beide voor het beheren van rekenresources en het uitvoeren van parallelle workloads op de geschikte schaal met behulp van de Batch-service.

| API | API-verwijzing | Download | Codevoorbeelden |
| --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |N.v.t. |[MSDN][batch_rest] |
| **Batch .NET** |[MSDN][api_net] |[NuGet ][api_net_nuget] |[GitHub][api_sample_net] |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |
| **Batch Java** (in voorbereiding) |[github.io][api_java] |[Maven][api_java_jar] |[GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Batch-resourcebeheer
Naast de client-API's kunt u ook het volgende gebruiken om resources in uw Batch-account te beheren.

* [PowerShell-cmdlets voor Batch][batch_ps]: met de Azure Batch-cmdlets in de [Azure PowerShell](../powershell-install-configure.md)-module kunt u Batch-resources beheren met PowerShell.
* [Azure CLI](../xplat-cli-install.md): de Azure Command-Line Interface (Azure CLI) is een platformoverschrijdende hulpmiddelenset die shellopdrachten biedt voor interactie met vele Azure-services, waaronder Batch.
* [Batch Management .NET](batch-management-dotnet.md)-clientbibliotheek: Ook beschikbaar via [NuGet][api_net_mgmt_nuget]. U kunt de Batch Management .NET-clientbibliotheek gebruiken om Batch-accounts, -quota en -toepassingspakketten programmatisch te beheren. Verwijzing voor de beheerbibliotheek bevindt zich op [MSDN][api_net_mgmt].

### <a name="batch-tools"></a>Batch-hulpprogramma's
Hoewel ze niet zijn vereist voor het bouwen van oplossingen met behulp van Batch, zijn er waardevolle hulpprogramma’s voor de ontwikkeling en foutopsporing van uw Batch-toepassingen en -services.

* [Azure-portal][portal]: U kunt Batch pools, taken en opdrachten in de batch blades van de Azure-portal maken, controleren en verwijderen. U kunt de statusinformatie voor deze en andere resources bekijken terwijl u taken uitvoert en zelfs taken downloadt van de Compute-knooppunten in uw pools (download bijvoorbeeld een `stderr.txt` van een mislukte taak bij probleemoplossing). U kunt ook Remote Desktop (RDP)-bestanden downloaden die u kunt gebruiken om aan te melden om knooppunten te berekenen.
* [Azure Batch Explorer][batch_explorer]: Batch Explorer biedt een vergelijkbare functionaliteit voor Batch resource management als de Azure-portal, maar in standalone Windows Presentation Foundation (WPF) clienttoepassing. Als een van de Batch .NET-voorbeeldtoepassingen beschikbaar op [GitHub][github_samples], kunt u het bouwen met Visual Studio 2015 of hoger en gebruiken om te bladeren door en voor het beheren van de resources in uw Batch-account terwijl u uw Batch-oplossingen ontwikkelt en debugt. Bekijk job-, pool- en opdrachtdetails, download bestanden van rekenknooppunten en maak op afstand verbinding met knooppunten met behulp van Remote Desktop (RDP)-bestanden die u kunt downloaden met de Batch Explorer-interface.
* [Microsoft Azure Storage Explorer][storage_explorer]: hoewel dit strikt genomen geen Azure Batch-hulpprogramma is, is Storage Explorer een ander waardevol hulpmiddel voor gebruik bij de ontwikkeling en foutopsporing van uw Batch-oplossingen.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenario: een parallelle workload uitschalen
Een gangbare oplossing die gebruikmaakt van de Batch-API's om met de Batch-service te communiceren, bestaat erin intrinsiek parallel werk (zoals de rendering van beelden voor 3D-scènes) uit te schalen op een pool van rekenknooppunten. Deze pool van rekenknooppunten kan uw 'render farm' zijn die bijvoorbeeld tientallen, honderden of zelfs duizenden kernen aan uw renderingjob beschikbaar stelt.

Het volgende diagram toont een algemene Batch-werkstroom, met een clienttoepassing of gehoste service die gebruikmaakt van Batch om een parallelle workload uit te voeren.

![Werkstroom van Batch-oplossing][2]

In dit veelvoorkomende scenario verwerkt uw toepassing of service een rekenworkload in Azure Batch door de volgende stappen uit te voeren:

1. Upload de **invoerbestanden** en de **toepassing** die deze bestanden zal verwerken naar uw Azure-opslagaccount. De invoerbestanden kunnen alle gegevens zijn die uw toepassing zal verwerken, zoals de modellering van financiële gegevens of te transcoderen videobestanden. De toepassingsbestanden kunnen elke toepassing zijn die wordt gebruikt voor het verwerken van de gegevens, zoals een 3D-renderingtoepassing of mediatranscoder.
2. Maak een Batch-**pool** van rekenknooppunten in uw Batch-account. Deze knooppunten zijn de virtuele machines die uw taken zullen uitvoeren. U geeft eigenschappen op, zoals de [knooppuntgrootte](../cloud-services/cloud-services-sizes-specs.md), het bijbehorende besturingssysteem en de locatie in Azure Storage van de toepassing die moet worden geïnstalleerd wanneer de knooppunten aan de pool worden toegevoegd (de toepassing die u in stap 1 hebt geüpload). U kunt ook de pool zodanig configureren, dat deze [automatisch wordt geschaald vergroot/verkleind](batch-automatic-scaling.md) (waarbij het aantal rekenknooppunten in de pool dynamisch wordt aangepast) als reactie op de workload die door uw taken wordt gegenereerd.
3. Maak een Batch-**job** om de workload uit te voeren op de pool van rekenknooppunten. Wanneer u een taak maakt, koppelt u deze aan een Batch-pool.
4. Voeg **taken** toe aan de job. Wanneer u taken aan een taak toevoegt, plant de Batch-service automatisch de taken voor uitvoering op de rekenknooppunten in de pool. Elke taak maakt gebruik van de toepassing die u hebt geüpload om de invoerbestanden te verwerken.
   
   * 4a. Voordat een taak wordt uitgevoerd, kan deze de gegevens (de invoerbestanden) die deze moet verwerken, downloaden naar het rekenknooppunt waaraan de taak is toegewezen. Als de toepassing nog niet op het knooppunt is geïnstalleerd (zie stap 2), kan deze in plaats daarvan hier worden gedownload. Nadat de downloads zijn voltooid, worden de taken uitgevoerd op hun toegewezen knooppunten.
5. Wanneer de taken worden uitgevoerd, kunt u in Batch een query uitvoeren om de voortgang van de job en de bijbehorende taken te controleren. Uw clienttoepassing of service communiceert met de Batch-service via HTTPS. Omdat u mogelijk duizenden taken controleert die op duizenden rekenknooppunten worden uitgevoerd, moet u [in de Batch-service een efficiënte query uitvoeren](batch-efficient-list-queries.md).
6. Nadat de taken zijn voltooid, kunnen ze hun resultaatgegevens uploaden naar Azure Storage. U kunt ook bestanden rechtstreeks uit rekenknooppunten ophalen.
7. Wanneer u bij uw controle detecteert dat de taken in uw job zijn voltooid, kan uw clienttoepassing of service de uitvoergegevens downloaden voor verdere verwerking of evaluatie.

Houd er rekening mee dat dit slechts één manier is om Batch te gebruiken en dat in dit scenario slechts enkele van de beschikbare functies worden beschreven. U kunt bijvoorbeeld [meerdere taken parallel](batch-parallel-node-tasks.md) uitvoeren op elk rekenknooppunt en u kunt [jobvoorbereidingstaken en jobvrijgevingstaken](batch-job-prep-release.md) gebruiken om de knooppunten voor te bereiden op uw taken, en ze achteraf op te schonen.

## <a name="next-steps"></a>Volgende stappen
Nu u een gedetailleerd overzicht van de Batch-service hebt gezien, is het tijd om dieper in te gaan op hoe u dit kunt gebruiken om uw rekenintensieve parallelle workloads te verwerken.

* Bekijk het [overzicht met Batch-functies voor ontwikkelaars](batch-api-basics.md), essentiële informatie voor iedereen die Batch wil gaan gebruiken. Het artikel bevat meer gedetailleerde informatie over de Batch-serviceresources zoals groepen, knooppunten, jobs en taken, en de vele API-functies die u tijdens het maken van de Batch-toepassing kunt gebruiken.
* Lees [Aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md) voor informatie over het gebruik van C# en de Batch .NET-bibliotheek om een eenvoudige workload uit te voeren met een gebruikelijke Batch-werkstroom. Als u de Batch-service wilt leren gebruiken, is dit artikel een van de eerste artikelen die u zeker moet lezen. Er is ook een [Python-versie](batch-python-tutorial.md) van de zelfstudie.
* Download de [codevoorbeelden op GitHub][github_samples] om te zien hoe C# en Python kunnen samenwerken met Batch om voorbeeldworkloads te plannen en te verwerken.
* Bekijk het [leertraject van Batch][learning_path] voor een beter beeld van de resources die beschikbaar zijn als u met Batch leert werken.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png



<!--HONumber=Nov16_HO2-->


