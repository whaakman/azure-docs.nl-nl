---
title: Azure Batch-API's en -hulpprogramma's gebruiken voor het ontwikkelen van grootschalige parallelle verwerkingsoplossingen | Microsoft Docs
description: Meer informatie over de API's en hulpprogramma's voor het ontwikkelen van oplossingen met de Azure Batch-service.
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.openlocfilehash: 9a5bbb1ecd3886a1453986c2deadb7b35e54b67b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>Overzicht van Batch-API's en -hulpprogramma's

Het verwerken van parallelle workloads met Azure Batch gebeurt gewoonlijk op programmatische wijze met behulp van een van de [Batch-API's](#batch-development-apis). Uw clienttoepassing of -service kan de Batch-API's gebruiken om met de Batch-service te communiceren. Met de Batch-API's kunt u pools van rekenknooppunten maken en beheren, ofwel virtuele machines ofwel cloudservices. Vervolgens kunt u jobs en taken plannen voor uitvoering op deze knooppunten. 

U kunt voor uw organisatie efficiënt grootschalige workloads verwerken of uw klanten een front-endservice aanbieden, zodat zij (op aanvraag of gepland) jobs en taken kunnen uitvoeren op één knooppunt of honderden of zelfs duizenden knooppunten. U kunt Azure Batch ook gebruiken als onderdeel van een grotere werkstroom, beheerd door hulpprogramma's zoals [Azure Data Factory](../data-factory/v1/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Wanneer u gereed bent om met de Batch-API aan de slag te gaan en u te verdiepen in de mogelijkheden ervan, leest u het [Overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Azure-accounts voor Batch-ontwikkeling
Wanneer u Batch-oplossingen ontwikkelt, gebruikt u de volgende accounts in Microsoft Azure.

* **Azure-account en -abonnement**: als u nog geen Azure-abonnement hebt, kunt u uw [voordelen als MSDN-abonnee][msdn_benefits] activeren of u aanmelden voor een [gratis Azure-account][free_account]. Wanneer u een account maakt, wordt voor u een standaardabonnement gemaakt.
* **Batch-account**: Batch-resources zoals pools, rekenknooppunten, jobs en taken worden aan een Azure Batch-account gekoppeld. Als uw toepassing een aanvraag indient voor de Batch-service, verifieert deze de aanvraag met de Azure Batch-accountnaam, de URL van het account en een toegangssleutel. U kunt een [Batch-account maken](batch-account-create-portal.md) in Azure Portal.
* **Storage-account**: Batch bevat ingebouwde ondersteuning voor het werken met bestanden in [Azure Storage][azure_storage]. Vrijwel elk Batch-scenario gebruikt Azure Blob-opslag voor het faseren van de programma's die door de taken worden uitgevoerd en de gegevens die ze verwerken, en voor de opslag van uitvoergegevens die ze genereren. Zie [Over Azure-opslagaccounts](../storage/common/storage-create-storage-account.md) voor het maken van een opslagaccount.

## <a name="batch-service-apis"></a>API’s voor Batch-service

Uw toepassingen en services kunnen direct REST-API-aanroepen verstrekken of een of meer van de volgende clientbibliotheken gebruiken voor het uitvoeren en beheren van uw Azure Batch-workloads.

| API | API-verwijzing | Download | Zelfstudie | Codevoorbeelden | Meer informatie |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |N.v.t. |- |- | [Ondersteunde versies](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Zelfstudie](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Releaseopmerkingen](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Zelfstudie](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Leesmij](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Leesmij](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Leesmij][api_sample_java] | [Leesmij](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>API’s voor Batch Management

De Azure Resource Manager-API's voor Batch bieden programmatisch toegang tot Batch-accounts. Met deze API's kunt u programmatisch Batch-accounts, quota en toepassingspakketten beheren.  

| API | API-verwijzing | Download | Zelfstudie | Codevoorbeelden |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |N.v.t. |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Zelfstudie](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Batch-opdrachtregelprogramma's

Deze opdrachtregelprogramma's bieden dezelfde functionaliteit als de API’s voor de Batch-service en Batch Management: 

* [PowerShell-cmdlets voor Batch][batch_ps]: met de Azure Batch-cmdlets in de [Azure PowerShell](/powershell/azure/overview)-module kunt u Batch-resources beheren met PowerShell.
* [Azure CLI](/cli/azure/overview): de Azure Command-Line Interface (Azure CLI) is een platformoverschrijdende hulpmiddelenset die shellopdrachten biedt voor interactie met vele Azure-services, waaronder de Batch-service en Batch Management-service. Zie [Batch-resources beheren met de Azure CLI](batch-cli-get-started.md) voor meer informatie over het gebruik van de Azure CLI met Batch.

## <a name="other-tools-for-application-development"></a>Andere hulpmiddelen voor toepassingsontwikkeling

Hier volgen enkele extra hulpprogramma's die mogelijk nuttig zijn voor het bouwen van uw Batch-toepassingen en -services en het opsporen van fouten daarin:

* [Azure Portal][portal]: u kunt Batch-pools, -taken en -opdrachten in de Batch-blades van Azure Portal maken, controleren en verwijderen. U kunt de statusinformatie voor deze en andere resources bekijken terwijl u taken uitvoert en zelfs taken downloadt van de rekenknooppunten in uw pools. U kunt bijvoorbeeld de `stderr.txt` van een taak downloaden bij het oplossen van problemen. U kunt ook Remote Desktop (RDP)-bestanden downloaden die u kunt gebruiken om aan te melden om knooppunten te berekenen.
* [Azure Batch Explorer][batch_explorer]: Batch Explorer biedt een vergelijkbare functionaliteit voor Batch resource management als de Azure Portal, maar in een standalone clienttoepassing Windows Presentation Foundation (WPF). U kunt een van de Batch .NET-voorbeeldtoepassingen beschikbaar op [GitHub][github_samples] bouwen met Visual Studio 2015 of hoger en gebruiken voor het bladeren door en het beheren van de resources in uw Batch-account terwijl u uw Batch-oplossingen ontwikkelt en debugt. Bekijk job-, pool- en opdrachtdetails, download bestanden van rekenknooppunten en maak op afstand verbinding met knooppunten met behulp van Remote Desktop (RDP)-bestanden die u kunt downloaden met de Batch Explorer-interface.
* [Microsoft Azure Storage Explorer][storage_explorer]: hoewel dit strikt genomen geen Azure Batch-hulpprogramma is, is Opslagverkenner wel een waardevol hulpmiddel voor de ontwikkeling en foutopsporing van uw Batch-oplossingen.

## <a name="additional-resources"></a>Aanvullende bronnen

- Zie [Logboekgebeurtenissen voor diagnostische evaluatie en bewaking van de Batch-oplossingen](batch-diagnostics.md) voor meer informatie over het vastleggen van gebeurtenissen van uw Batch-toepassing. Zie [Batch-analyses](batch-analytics.md) voor informatie over gebeurtenissen die worden gegenereerd door de Batch-service.
- Zie [Omgevingsvariabelen van Azure Batch-rekenknooppunten](batch-compute-node-environment-variables.md) voor meer informatie over omgevingsvariabelen voor rekenknooppunten.

## <a name="next-steps"></a>Volgende stappen

* Bekijk het [overzicht met Batch-functies voor ontwikkelaars](batch-api-basics.md), essentiële informatie voor iedereen die Batch wil gaan gebruiken. Het artikel bevat meer gedetailleerde informatie over de Batch-serviceresources zoals groepen, knooppunten, jobs en taken, en de vele API-functies die u tijdens het maken van de Batch-toepassing kunt gebruiken.
* Lees [Aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md) voor informatie over het gebruik van C# en de Batch .NET-bibliotheek om een eenvoudige workload uit te voeren met een gebruikelijke Batch-werkstroom. Als u de Batch-service wilt leren gebruiken, is dit artikel een van de eerste artikelen die u zeker moet lezen. Er is ook een [Python-versie](batch-python-tutorial.md) van de zelfstudie.
* Download de [codevoorbeelden op GitHub][github_samples] om te zien hoe C# en Python kunnen samenwerken met Batch om voorbeeldworkloads te plannen en te verwerken.
* Bekijk het [Batch-leertraject][learning_path] voor een beter beeld van de resources die beschikbaar zijn terwijl u met Batch leert werken.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
