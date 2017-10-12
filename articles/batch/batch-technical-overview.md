---
title: Met Azure Batch worden oplossingen voor grootschalige parallelle rekenbewerkingen uitgevoerd in de cloud | Microsoft Docs
description: Meer informatie over het gebruik van de Azure Batch-service voor grootschalige parallelle en HPC-workloads
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a99f96db0c1e8bcd0cf29c564e5badf0eb728e56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Intrinsiek parallelle workloads uitvoeren met Batch

Azure Batch is een platformservice voor het efficiënt uitvoeren van grootschalige parallelle en HPC-toepassingen (High Performance Computing) in de cloud. Azure Batch plant de uitvoering van rekenintensief werk op een beheerde verzameling van virtuele machines en kan automatisch het aantal rekenresources aanpassen aan de behoeften van uw jobs.

Met Azure Batch kunt u gemakkelijk Azure-rekenresources definiëren om uw toepassingen parallel en op de juiste schaal uit te voeren. U hoeft niet handmatig een HPC-cluster, individuele virtuele machines, virtuele netwerken of een complexe job- en taakplanningsinfrastructuur te maken, te configureren en te beheren. Azure Batch automatiseert of vereenvoudigt deze taken voor u.

## <a name="use-cases-for-batch"></a>Gebruiksvoorbeelden voor Batch
Batch is een beheerde Azure-service die wordt gebruikt voor *batchverwerking* of *batchberekeningen*, waarbij een groot aantal vergelijkbare taken wordt uitgevoerd om een bepaald gewenst resultaat te verkrijgen. Batchverwerking wordt doorgaans gebruikt door organisaties die regelmatig grote hoeveelheden gegevens verwerken, transformeren en analyseren.

Batch is bijzonder geschikt voor intrinsiek parallelle (ook wel bekend als 'perfect parallelle') toepassingen en workloads. Intrinsiek parallelle workloads zijn workloads die eenvoudig kunnen worden onderverdeeld in meerdere taken die op meerdere computers tegelijk werk verrichten.

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

Zie [HPC-, Batch- en Big Compute-oplossingen](../virtual-machines/linux/high-performance-computing.md) voor een vergelijking tussen Batch en andere HPC-oplossingsopties in Azure.

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenario: een parallelle workload uitschalen
Een gangbare oplossing die gebruikmaakt van de Batch-API's om met de Batch-service te communiceren, bestaat erin intrinsiek parallel werk (zoals de rendering van beelden voor 3D-scènes) uit te schalen op een pool van rekenknooppunten. Deze pool van rekenknooppunten kan uw 'render farm' zijn die bijvoorbeeld tientallen, honderden of zelfs duizenden kernen aan uw renderingjob beschikbaar stelt.

Het volgende diagram toont een algemene Batch-werkstroom, met een clienttoepassing of gehoste service die gebruikmaakt van Batch om een parallelle workload uit te voeren.

![Werkstroom van Batch-oplossing][2]

In dit veelvoorkomende scenario verwerkt uw toepassing of service een rekenworkload in Azure Batch door de volgende stappen uit te voeren:

1. Upload de **invoerbestanden** en de **toepassing** die deze bestanden zal verwerken naar uw Azure-opslagaccount. De invoerbestanden kunnen alle gegevens zijn die uw toepassing zal verwerken, zoals de modellering van financiële gegevens of te transcoderen videobestanden. De toepassingsbestanden kunnen elke toepassing zijn die wordt gebruikt voor het verwerken van de gegevens, zoals een 3D-renderingtoepassing of mediatranscoder.
2. Maak een Batch-**pool** van rekenknooppunten in uw Batch-account. Deze knooppunten zijn de virtuele machines die uw taken zullen uitvoeren. U geeft eigenschappen op, zoals de [knooppuntgrootte](../cloud-services/cloud-services-sizes-specs.md), het bijbehorende besturingssysteem en de locatie in Azure Storage van de toepassing die moet worden geïnstalleerd wanneer de knooppunten aan de pool worden toegevoegd (de toepassing die u in stap 1 hebt geüpload). U kunt ook de pool zodanig configureren dat deze [automatisch wordt geschaald](batch-automatic-scaling.md) als reactie op de workload die door uw taken wordt gegenereerd. Door automatisch te schalen, wordt het aantal rekenknooppunten in de pool automatisch aangepast.
3. Maak een Batch-**job** om de workload uit te voeren op de pool van rekenknooppunten. Wanneer u een taak maakt, koppelt u deze aan een Batch-pool.
4. Voeg **taken** toe aan de job. Wanneer u taken aan een taak toevoegt, plant de Batch-service automatisch de taken voor uitvoering op de rekenknooppunten in de pool. Elke taak maakt gebruik van de toepassing die u hebt geüpload om de invoerbestanden te verwerken.
   
   * 4a. Voordat een taak wordt uitgevoerd, kan deze de gegevens (de invoerbestanden) die deze moet verwerken, downloaden naar het rekenknooppunt waaraan de taak is toegewezen. Als de toepassing nog niet op het knooppunt is geïnstalleerd (zie stap 2), kan deze in plaats daarvan hier worden gedownload. Nadat de downloads zijn voltooid, worden de taken uitgevoerd op hun toegewezen knooppunten.
5. Wanneer de taken worden uitgevoerd, kunt u in Batch een query uitvoeren om de voortgang van de job en de bijbehorende taken te controleren. Uw clienttoepassing of -service communiceert met de Batch-service via HTTPS. Omdat u mogelijk duizenden taken bewaakt die worden uitgevoerd op duizenden rekenknooppunten, moet u ervoor zorgen dat u [een query op de Batch-service op efficiënte wijze uitvoert](batch-efficient-list-queries.md).
6. Nadat de taken zijn voltooid, kunnen ze hun resultaatgegevens uploaden naar Azure Storage. U kunt ook bestanden rechtstreeks ophalen van het bestandssysteem op een rekenknooppunt.
7. Wanneer u bij uw controle detecteert dat de taken in uw job zijn voltooid, kan uw clienttoepassing of service de uitvoergegevens downloaden voor verdere verwerking of evaluatie.

Houd er rekening mee dat dit slechts één manier is om Batch te gebruiken en dat in dit scenario slechts enkele van de beschikbare functies worden beschreven. U kunt bijvoorbeeld [meerdere taken parallel](batch-parallel-node-tasks.md) uitvoeren op elk rekenknooppunt en u kunt [jobvoorbereidingstaken en jobvrijgevingstaken](batch-job-prep-release.md) gebruiken om de knooppunten voor te bereiden op uw taken, en ze achteraf op te schonen.

## <a name="next-steps"></a>Volgende stappen
Nu u een gedetailleerd overzicht van de Batch-service hebt gezien, is het tijd om dieper in te gaan op hoe u dit kunt gebruiken om uw rekenintensieve parallelle workloads te verwerken.

* Bekijk het [overzicht met Batch-functies voor ontwikkelaars](batch-api-basics.md), essentiële informatie voor iedereen die Batch wil gaan gebruiken. Het artikel bevat meer gedetailleerde informatie over de Batch-serviceresources zoals groepen, knooppunten, jobs en taken, en de vele API-functies die u tijdens het maken van de Batch-toepassing kunt gebruiken.
* Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
* Lees [Aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md) voor informatie over het gebruik van C# en de Batch .NET-bibliotheek om een eenvoudige workload uit te voeren met een gebruikelijke Batch-werkstroom. Als u de Batch-service wilt leren gebruiken, is dit artikel een van de eerste artikelen die u zeker moet lezen. Er is ook een [Python-versie](batch-python-tutorial.md) van de zelfstudie.
* Download de [codevoorbeelden op GitHub][github_samples] om te zien hoe C# en Python kunnen samenwerken met Batch om voorbeeldworkloads te plannen en te verwerken.
* Bekijk het [Batch-leertraject][learning_path] voor een beter beeld van de resources die beschikbaar zijn terwijl u met Batch leert werken.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
