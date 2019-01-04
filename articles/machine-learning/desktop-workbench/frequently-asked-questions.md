---
title: Azure Machine Learning-2017 Preview Veelgestelde vragen over | Microsoft Docs
description: In dit artikel bevat veelgestelde vragen en antwoorden voor preview-functies van Azure Machine Learning
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ROBOTS: NOINDEX
ms.openlocfilehash: f573cda9f06837bf44ee39a680b207af1ba0fe07
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53973227"
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>Veelgestelde vragen over Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Azure Machine Learning is een volledig beheerde Azure-service waarmee u kunt maken, testen, beheren en implementeren van machine learning en AI-modellen. Onze services en de downloadbare toepassing bieden een code op de eerste benadering die gebruikmaakt van de cloud, on-premises en edge te bieden van de trein, implementeren, beheren en bewaken van modellen met energiebeheer, de snelheid en flexibiliteit. Azure Machine Learning Studio biedt ook een browser gebaseerde, visuele slepen-en-neerzetten ontwerpomgeving waar geen codering vereist is. 

## <a name="general-product-questions"></a>Algemene vragen

**Welke andere Azure-services zijn vereist?**

Azure Blob Storage en Azure Container Registry worden gebruikt door Azure Machine Learning. Bovendien moet u rekenresources, zoals een Data Science VM of HDInsight-cluster inrichten. Rekenbewerkingen en hosting zijn ook vereist bij het implementeren van uw webservices, zoals [Azure Container Service](https://docs.microsoft.com/azure/aks).

**Hoe Azure Machine Learning in verband met Microsoft Machine Learning-Services in SQL Server 2017?**   

Machine Learning-Services in SQL Server 2017 is een uitbreidbaar en schaalbaar platform voor het integreren van machine learning-taken in de database-werkstromen. Het is een bij uitstek geschikt voor scenario's waarin een on-premises oplossing vereist, bijvoorbeeld waarbij verplaatsing van gegevens is duur en untenable. Cloud of hybride werkbelastingen zijn daarentegen uitstek geschikt voor onze nieuwe Azure-services. 

**Hoe Azure Machine Learning in verband met Microsoft Machine Learning voor Spark?**

MMLSpark biedt deep learning en data science-tools voor Apache Spark, met nadruk op de productiviteit, eenvoudig te experimenteren en daarvan van geavanceerde algoritmen. MMLSpark biedt integratie van de Spark Machine Learning-pijplijnen met de Microsoft Cognitive Toolkit en OpenCV. U kunt de krachtige, zeer schaalbare voorspelbare en analytische modellen voor afbeeldingen en tekst gegevens maken. MMLSpark bevindt zich onder een open-sourcelicentie en als een set van verbruikbare modellen en algoritmes in AML-Workbench is opgenomen. Ga naar onze productdocumentatie voor meer informatie over MMLSpark. 

**Welke versies van Spark worden ondersteund door de nieuwe hulpprogramma's en -services?**

Workbench wordt momenteel bevat en MMLSpark versie 0,8, ondersteunt die compatibel is met Apache Spark 2.1. U hebt ook een optie voor het gebruik van met GPU Docker-installatiekopie van MMLSpark 0,8 op virtuele Linux-machines.

## <a name="experimentation-service"></a>Experimenten-Service

**Wat is de Azure Machine Learning experimenten-Service?**

De experimenten-Service is een beheerde Azure-service die naar een hoger niveau met machine learning experimenten. Experimenten kunnen worden gebouwd, lokaal of in de cloud. Snel schalen prototype op een bureaublad, klikt u vervolgens op virtuele machines of Spark-clusters. Azure VM's met de nieuwste GPU-technologie kunnen u via een deep learning snel en efficiënt. We hebben ook hechte integratie met Git opgenomen, zodat u eenvoudig kunt aansluiten op de bestaande werkstromen voor het bijhouden van code, configuratie en samenwerking. 

**Hoe moet ik betalen voor de experimenten-Service?**

De eerste twee gebruikers die zijn gekoppeld aan uw Azure Machine Learning experimenten-Service zijn gratis. Extra gebruikers wordt in rekening gebracht tegen het tarief openbare Preview van $50/maand. Ga naar onze pagina met prijsdetails voor meer informatie over prijzen en facturering.

**Moet ik betalen op basis van het aantal experimenten dat ik uitvoer?**

Nee, kunnen de experimenten-Service net zoveel experimenten uitvoeren als u behoefte en kosten zijn uitsluitend gebaseerd op het aantal gebruikers. Rekenbronnen voor experimenten worden afzonderlijk in rekening gebracht. We raden u aan het uitvoeren van meerdere experimenten, zodat u kunt het beste aansluit op model voor uw oplossing vinden.

**Welke specifieke soorten reken- en opslagresources kan ik gebruiken?**

De experimenten-service kan uw experimenten uitvoeren op lokale machines (direct of op basis van Docker), [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), en [HDInsight](https://azure.microsoft.com/services/hdinsight/). De service ook toegang heeft tot een [Azure Storage](https://azure.microsoft.com/services/storage/) -account voor het opslaan van de uitvoer van uitvoerbewerkingen en kunnen gebruikmaken van een [Azure DevOps](https://azure.microsoft.com/services/devops/) account voor versiebeheer en Git-opslag. Houd er rekening mee dat u betaalt afzonderlijke voor gebruikte reken- en opslagbronnen kosten, op basis van de afzonderlijke prijzen.

## <a name="model-management"></a>Modelbeheer

**Wat is Azure Machine Learning Modelbeheer?**

Azure Machine Learning Modelbeheer is een beheerde Azure-service waarmee data scientists en dev ops teams betrouwbaar voorspellende modellen implementeren in een groot aantal omgevingen. GIT-opslagplaatsen en Docker-containers bieden traceability en herhaalbaarheid. Modellen kunnen op betrouwbare wijze worden geïmplementeerd in de cloud, on-premises of edge. U kunt één keer in de productieomgeving, beheer de modelprestaties vervolgens proactief opnieuw trainen als vermindert de prestaties. U kunt modellen op lokale machines te implementeren [virtuele Azure-machines](https://azure.microsoft.com/services/virtual-machines/), Spark op [HDInsight](https://azure.microsoft.com/services/hdinsight/) of Kubernetes georganiseerd [Azure Container Service](https://azure.microsoft.com/services/container-service/) clusters.

**Wat is een 'model'?**

Een model is voor het beheren van de uitvoer van een experimenten uitvoeren die is gepromoveerd. Een model dat is geregistreerd in de hosting-account wordt van uw abonnement, met inbegrip van modellen die worden bijgewerkt via opnieuw trainen of versie iteratie geteld.

**Wat is een 'beheerde model"?**

Een model is de uitvoer van een trainingsproces en is de toepassing van een algoritme voor Machine Learning voor het trainen van gegevens. Model Management kunt u modellen als webservices implementeren, verschillende versies van uw modellen beheren en bewaken van de prestaties en metrische gegevens. 'Beheerd' modellen zijn geregistreerd met een Azure Machine Learning Modelbeheer-account. Een voorbeeld: u hebt een scenario waarin u een voorspelling wilt doen van de verkoop. Tijdens de experimentatiefase genereert u meerdere modellen met behulp van verschillende gegevenssets of algoritmes. U hebt bijvoorbeeld vier modellen met verschillende nauwkeurigheden gegenereerd maar kies alleen het model te registreren met de hoogste nauwkeurigheid. Het model dat is geregistreerd, wordt het model van uw eerste beheerde.
 
**Wat is een 'implementatie?'**

Model Management kunt u modellen implementeren als verpakte webservicecontainers in Azure. Deze webservices kunnen worden aangeroepen met behulp van REST-API's. Elke webservice wordt geteld als één implementatie en het totale aantal actieve implementaties worden geteld onderdeel van uw abonnement. Gebruiken we het voorbeeld, wanneer u uw beste prestatiemodel implementeert, wordt uw abonnement met één implementatie verhoogd. Als u vervolgens opnieuw trainen en implementeren van een andere versie, hebt u twee implementaties. Als u dat vaststelt het nieuwere model beter is en verwijder de oorspronkelijke, het aantal implementaties is met één verlaagd.

**Welke specifieke rekenbronnen worden beschikbaar voor mijn implementaties?** 

Modelbeheer kan uw implementaties uitvoeren als Docker-containers die zijn geregistreerd bij [Azure Container Service](https://azure.microsoft.com/services/container-service/), als [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), of op lokale machines. Aanvullende implementatiedoelen wordt binnenkort toegevoegd. Houd er rekening mee dat u betaalt afzonderlijke voor alle gebruikte rekenbronnen op basis kosten van de afzonderlijke prijzen.

**Kan ik de Azure Machine Learning-Modelbeheer gebruiken voor het implementeren van modellen die zijn gebouwd met behulp van hulpprogramma's dan de experimenten-Service?**

U krijgt de beste resultaten bij het implementeren van modellen die zijn gemaakt met behulp van de experimenten-Service. U kunt echter modellen die zijn gebouwd met behulp van andere frameworks en programma's implementeren. We bieden ondersteuning voor tal van modellen, met inbegrip van MMLSpark, TensorFlow, Microsoft Cognitive Toolkit, scikit-learn, Keras, enzovoort. 

**Kan ik mijn eigen Azure-resources gebruiken?**

Ja, de experimenten-Service en Modelbeheer werken in combinatie met meerdere Azure gegevensarchieven, compute-workloads en andere services. Raadpleeg onze technische documentatie voor meer informatie over de vereiste Azure-services.

**Ondersteuning voor zowel on-premises en u scenario's voor implementatie in de cloud?**

Ja. We ondersteuning voor on-premises en cloud-implementatiescenario's via Docker-containers. Lokale uitvoeringsdoelen bevatten: één knooppunt Docker-implementaties, [Microsoft SQL Server met ML-Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop en Spark. We bieden ook ondersteuning voor cloud-implementaties via Docker, met inbegrip van: geclusterde implementaties via Azure Container Service en Kubernetes, HDInsight en Spark-clusters. Edge-scenario's worden ondersteund via Docker-containers en Azure IOT Edge. 

**Kan ik een Docker-installatiekopie die is gemaakt met de Azure Machine Learning CLI op een andere host uitvoeren?**

Ja. Zolang de host heeft onvoldoende rekenresources voor het hosten van de docker-installatiekopie, kunt u de afbeelding als een webservice op een docker-host.

**Opnieuw trainen van geïmplementeerde modellen ondersteunen**

Ja, kunt u meerdere versies van hetzelfde model implementeren. Modelbeheer biedt ondersteuning voor service-updates voor alle bijgewerkte modellen en afbeeldingen.

## <a name="workbench"></a>Workbench

**Wat is Azure Machine Learning Workbench?**

De Azure Machine Learning Workbench is een companion-toepassing die is gebouwd voor professionele gegevenswetenschappers. Beschikbaar voor Windows en Mac, de Machine Learning Workbench biedt overzicht, beheer en controle voor machine learning-oplossingen. De Machine Learning Workbench omvat toegang aan hypermoderne AI-frameworks van zowel Microsoft als open source-community. We hebben de meest populaire toolkits voor data science, waaronder TensorFlow, Microsoft Cognitive Toolkit, Spark ML en scikit opgenomen-meer en meer. We hebben ook integratie met populaire gegevenswetenschap IDE's zoals Jupyter notebooks, PyCharm en Visual Studio Code ingeschakeld. De Machine Learning Workbench heeft ingebouwde voorbereiding mogelijkheden voor het snel voorbeeld, begrijpen en voorbereiden van gegevens, gestructureerd of ongestructureerd. Onze nieuwe hulpprogramma voor gegevensvoorbereiding, met de naam [PROSE](https://microsoft.github.io/prose/), is gebouwd op hypermoderne technologie van Microsoft Research.

**Workbench een IDE is?**

Nee. De Machine Learning Workbench is ontworpen als een aanvulling op de populaire IDE's zoals Jupyter Notebooks, Visual Studio Code en PyCharm, maar het is niet een volledig functionele IDE. De Machine Learning Workbench biedt enkele basistekst bewerkingsmogelijkheden, maar het opsporen van fouten, de intellisense en andere veel gebruikte IDE mogelijkheden worden niet ondersteund. Raden wij aan dat u uw favoriete IDE voor het ontwikkelen van code, bewerken en het opsporen van fouten. U kunt ook meer om te proberen [Visual Studio Code-hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

**Is er een toeslag voor het gebruik van de Azure Machine Learning Workbench?**

Nee. Azure Machine Learning Workbench is een gratis toepassing. U kunt deze toepassing downloaden op net zoveel machines en voor net zoveel gebruikers als u wilt. Voor het gebruik van Azure Machine Learning Workbench hebt u een Experimenten-account nodig.

**Opdrachtregel mogelijkheden ondersteund?**

Ja, Azure Machine Learning biedt een volledige CLI-interface. De Machine Learning CLI is standaard geïnstalleerd met de Azure Machine Learning Workbench. Het is ook beschikbaar als onderdeel van de Linux Data Science virtual machine in Azure en wordt geïntegreerd in de [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)


**Kan ik Jupyter-Notebooks gebruiken met Workbench?**

Ja. U kunt Jupyter-notebooks uitvoeren in Workbench met Workbench als de toepassing van de client-hosting, net zoals u een browser als een client gebruiken zou. 

**Welke kernels voor Jupyter-Notebook worden ondersteund?**

De huidige versie van Jupyter opgenomen in Workbench wordt gestart, een Python 3-kernel en een extra kernel voor elk bestand 'runconfig' in de map aml_config. Ondersteunde configuraties zijn onder andere:
- Lokale Python
- Python in lokale of externe Docker

## <a name="data-formats-and-capabilities"></a>Opmaak van gegevens en mogelijkheden

**Welke indelingen worden momenteel ondersteund voor gegevensopname in Workbench?**

De hulpmiddelen voor het voorbereiden van gegevens in Workbench momenteel ondersteuning voor opname van de volgende indelingen: 
- Bestanden met scheidingstekens, zoals CSV, TSV, enzovoort.
- Bestanden met vaste breedte
- Bestanden met tekst zonder opmaak
- Excel (xls/xlsx)
- JSON-bestanden
- Parquet-bestanden 
- Aangepaste bestanden (scripts) als uw oplossing is vereist voor opname van gegevens uit andere bronnen, Python-code kan worden gebruikt om... 

**Welke opslaglocaties gegevens worden momenteel ondersteund?**

Voor de openbare preview ondersteunt Workbench opname van gegevens mogelijk: 
- Lokale harde schijf of toegewezen netwerk opslaglocatie
- Azure BLOB of Azure Storage (een Azure-abonnement vereist)
- Azure SQL Server
- Microsoft SQL Server


**Wat voor soort data wrangling, voorbereiding en transformaties zijn beschikbaar?**

Voor openbare preview ondersteunt Workbench "Afgeleid kolom door bijvoorbeeld" 'Kolom splitsen door voorbeeld', 'Tekst Clustering', 'Verwerken ontbrekende Values' en nog veel meer. Workbench biedt ook ondersteuning voor gegevensconversie van het type, de gegevens worden verzameld (aantal, gemiddelde, afwijking, enzovoort) en complexe joins. Ga naar onze productdocumentatie voor een volledige lijst met ondersteunde mogelijkheden. 

**Zijn er maximale grootte van gegevens door Azure Machine Learning Workbench, experimenten en Modelbeheer-afgedwongen?**

De nieuwe services leggen Nee, geen gegevens beperkingen. Er zijn echter beperkingen die door de omgeving waarin u uw gegevens voor te bereiden, modeltraining, experimenten of implementatie wilt uitvoeren. Bijvoorbeeld, als u voor een lokale omgeving voor training ontwikkelt, gelden er limieten voor de beschikbare ruimte op de harde schijf. U kunt ook als u HDInsight gebruiken, u worden beperkt door de grootte van alle bijbehorende of compute-beperkingen. 

## <a name="algorithms-and-libraries"></a>Algoritmen en bibliotheken

**Welke algoritmen worden ondersteund in Azure Machine Learning Workbench?**

Onze preview-producten en services omvatten het beste van de open source-community. We bieden ondersteuning voor een breed scala aan algoritmen en -bibliotheken, waaronder TensorFlow, scikit-informatie, Apache Spark en de Microsoft Cognitive Toolkit. De Azure Machine Learning Workbench pakketten ook de [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy) pakket.

**Hoe Azure Machine Learning in verband met de Microsoft Cognitive Toolkit?**

De [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/) is een van de vele frameworks die worden ondersteund door onze nieuwe hulpprogramma's en -services. De Cognitive Toolkit is een uniforme deep learning-werkset waarmee u kunt gebruiken en het combineren van populaire machine learning-modellen, met inbegrip van Feed-Forward Deep Neural Networks, Convolutionele netwerken volgorde op volgorde en recurrente netwerken. Voor meer informatie over Microsoft Cognitive Toolkit, gaat u naar onze [productdocumentatie](https://docs.microsoft.com/cognitive-toolkit/). 
