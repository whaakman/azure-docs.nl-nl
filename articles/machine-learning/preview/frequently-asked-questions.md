---
title: Azure Machine Learning-2017 Preview Veelgestelde vragen | Microsoft Docs
description: In dit artikel bevat veelgestelde vragen en antwoorden voor Azure Machine Learning preview-functies
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ms.openlocfilehash: 94c6576f92b5a004e0fd509c1dc6e091c294658a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>Azure Machine Learning Veelgestelde vragen

## <a name="general-product-questions"></a>Algemene vragen over

**Wat is Azure Machine Learning?**

Azure Machine Learning is een volledig beheerde Azure-service waarmee u kunt maken, testen, beheren en implementeren van machine learning en AI-modellen. Onze nieuwe services en de downloadbare toepassing bieden een code first-aanpak die gebruikmaakt van de cloud, on-premises en rand bieden de trein, implementeren, beheren en bewaken van modellen met energiebeheer, de snelheid en flexibiliteit. Azure Machine Learning Studio biedt ook een browser, visuele slepen en neerzetten ontwerpomgeving waar geen codering vereist is. 

**Hoe ga ik aan de slag met de preview?**

De eenvoudigste manier om te beginnen is aan te melden voor de Service experimenteren in Azure. Vanuit de portal kunt u ook onze gratis toepassing van de Workbench downloaden of zich aanmeldt voor een Model-Management-account. U kunt bovendien Ga naar onze documentatiesite voor artikelen, video's, demo code en meer. 

**Ik heb geen een Azure-abonnement. Kan ik de nieuwe services nog steeds proberen?**

Onze nieuwe services vereist als onderdeel van de portfolio van Microsoft Azure, een Azure-abonnement. Daarnaast moet er voldoende machtigingen voor het maken van de activa zoals resourcegroepen, virtuele Machines, enzovoort. 

**In welke regio's zijn dat de nieuwe services beschikbaar zijn?**

Ondersteunde Azure-regio's voor experimenteren en het model-management-service onder Azure Machine Learning zoekt op de [Azure producten per regio](https://azure.microsoft.com/regions/services/) pagina.

Meer regio's worden toegevoegd wanneer we het product ontwikkelen.  U kunt helpen ons prioriteren welke regio's om te implementeren op on onze [Azure Machine Learning Feedback](https://feedback.azure.com/forums/257792-machine-learning) site. 

**Welke andere Azure-services zijn vereist?**

Om volledig gebruikmaken van alle mogelijkheden van Azure Machine Learning, Azure Machine Learnign Visual Studio VSTS account, Azure Blob Storage, Azure Container register, gegevens wetenschappelijke VM of HDInsight compute en Azure Container Service gebruikt.

**Hoe Azure Machine Learning verband tussen Microsoft-Services Machine Learning in SQL Server 2017?**   

Machine Learning-Services in SQL Server 2017 is een uitbreidbaar, schaalbare platform voor het integreren van machine learning-taken in de database-werkstromen. Het is een bij uitstek geschikt voor scenario's waarin een on-premises oplossing vereist, bijvoorbeeld waarbij gegevensverplaatsing dure of untenable is. Cloud- of hybride werkbelastingen zijn daarentegen een geweldige geschikt is voor de nieuwe Azure-services. 

**Ondersteund zowel Python en R? Hoe zit het met andere programmeertalen zoals C++**

Wordt ondersteund Python alleen. We werken R-integratie en is deze beschikbaar binnenkort verwacht. 

**Hoe Azure Machine Learning hebben betrekking op Microsoft Machine Learning voor Spark?**

A: MMLSpark biedt grondige learning en data wetenschappelijke tools voor Apache Spark, met nadruk op de productiviteit, eenvoudig te experimenteren en van geavanceerde algoritmen. MMLSpark biedt integratie van Spark Machine Learning pijplijnen met de Microsoft cognitieve Toolkit en OpenCV. U kunt krachtige, zeer schaalbare voorspellende en analytische modellen voor tekst en de installatiekopie van gegevens maken. MMLSpark is beschikbaar voor een open source-licentie en is opgenomen in de Workbench AML als een set van verbruikbare modellen en algoritmen. Ga naar onze productdocumentatie voor meer informatie over MMLSpark. 

**Welke versies van Spark worden ondersteund door de nieuwe hulpprogramma's en services? Bovenste gedeelte**

Workbench bevat momenteel en MMLSpark versie 0,8, ondersteunt die compatibel is met Apache Spark 2.1. Hebt u ook een optie voor het gebruik van GPU ingeschakeld Docker-afbeelding van MMLSpark 0,8 op Linux virtuele machines.

## <a name="experimentation-service"></a>Experimenteren Service

**Wat is Azure Machine Learning experimenteren Service?**

De Service experimenteren is een beheerde Azure-service die machine learning-experimenten naar het volgende niveau gaat. Experimenten kunnen worden opgebouwd, lokaal of in de cloud. Snel prototype op het bureaublad, klikt u vervolgens kan worden uitgebreid naar virtuele machines of Spark-clusters. Virtuele machines in Azure met de nieuwste GPU-technologie kunnen u benaderen grondige weten snel en efficiënt. We hebben ook nauwe integratie met Git opgenomen zodat u eenvoudig kunt aansluiten op bestaande werkstromen voor het bijhouden van code, configuratie en samenwerking. 

**Hoe ik gefactureerd voor de Service experimenteren?**

De eerste twee gebruikers die zijn gekoppeld aan uw Azure Machine Learning-experimenten Service zijn gratis. Extra gebruikers worden met een snelheid van de openbare Preview van 50 USD/maand berekend. Ga naar onze pagina met prijzen voor meer informatie over prijzen en facturering.

**Ik gefactureerd op basis van hoeveel experimenten ik uitvoeren?**

Nee, kan de Service experimenteren zoveel experimenten als u nodig en kosten alleen op basis van het aantal gebruikers. Rekenbronnen voor experimenten worden afzonderlijk in rekening gebracht. We raden u aan het uitvoeren van meerdere experimenten, zodat u kunt het beste past model voor uw oplossing vinden.   

**Welke specifieke soorten berekenings-en opslagbronnen kan ik gebruiken?**

De service experimenteren uw experimenten kunt uitvoeren op lokale computers (direct of op basis van Docker), [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), en [HDInsight](https://azure.microsoft.com/services/hdinsight/). De service ook toegang heeft tot een [Azure Storage](https://azure.microsoft.com/services/storage/) account voor het opslaan van de uitvoer van de uitvoering en kunnen gebruikmaken van een [Visual Studio Team Service](https://azure.microsoft.com/services/visual-studio-team-services/) account voor versiebeheer en Git-opslag. Houd er rekening mee dat u wordt gefactureerd afzonderlijk voor elk verbruikte rekenkracht en opslagbronnen, op basis van hun afzonderlijke prijzen.  


## <a name="model-management"></a>Modelbeheer

**Wat is Azure Machine Learning-Model Management?**

Azure Machine Learning-Model-Management is een beheerde Azure-service waarmee gegevens verzameld en dev ops teams voorspellende modellen betrouwbaar implementeren in een groot aantal verschillende omgevingen. GIT-opslagplaatsen en Docker-containers bieden traceerbaarheid en gebruiken van herhaalbaarheid. Modellen kunnen op betrouwbare wijze worden geïmplementeerd in de cloud, on-premises of edge. U kunt één keer in productie, model prestaties beheren vervolgens retrain proactief als de prestaties afnemen. U kunt modellen op lokale computers te implementeren [Azure Virtual machines](https://azure.microsoft.com/services/virtual-machines/), Spark op [HDInsight](https://azure.microsoft.com/services/hdinsight/) of Kubernetes gedirigeerd [Azure Container Service](https://azure.microsoft.com/services/container-service/) clusters.  

**Wat is een 'model'?**

Een model zich in de uitvoer van een experimenteren uitvoeren die voor het Modelbeheer van het is gepromoveerd. Een model dat is geregistreerd in de hosting-account wordt op basis van uw abonnement, met inbegrip van modellen die worden bijgewerkt via retraining of versie herhaling geteld.

**Wat is er een 'beheerde model'?**

Een model is de uitvoer van een trainingsproces en is de toepassing van een algoritme voor Machine Learning voor het trainen van gegevens. Model Management kunt u modellen als webservices implementeren, beheren van verschillende versies van uw modellen en de prestaties en metrische gegevens controleren. 'Beheerde' modellen zijn geregistreerd met een account voor Azure Machine Learning-Model Management. Een voorbeeld: u hebt een scenario waarin u een voorspelling wilt doen van de verkoop. Tijdens de fase experimenteren kunt u veel modellen genereren met behulp van verschillende gegevenssets of algoritmen. U hebt gegenereerd vier modellen met verschillende accuratesse maar alleen het model met de hoogste nauwkeurigheid registreert. Het model dat is geregistreerd, wordt uw eerste beheerde model.
 
**Wat is er een 'implementatie'?**

Model Management kunt u modellen implementeren als verpakte web servicecontainers in Azure. Deze webservices kunnen worden aangeroepen met REST API's. Elke webservice wordt beschouwd als een enkele implementatie en het totale aantal actieve implementaties worden geteld voor uw abonnement. Met behulp van de prognose bijvoorbeeld wanneer u de best presterende van model implementeert, wordt uw abonnement met één implementatie verhoogd. Als u vervolgens opnieuw trainen en implementeren van een andere versie, hebt u twee implementaties. Als u dat vaststelt het nieuwe model is beter en verwijderen de oorspronkelijke, het aantal implementaties is met één verlaagd.  

**Welke specifieke compute-bronnen beschikbaar zijn voor mijn implementaties?** 

Model Management uw implementaties kunt uitvoeren, zoals Docker-containers die zijn geregistreerd bij [Azure Container Service](https://azure.microsoft.com/services/container-service/), als [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), of op lokale computers. Implementatie van aanvullende doelen wordt binnenkort toegevoegd. Houd er rekening mee dat u de factuur onafhankelijk voor alle resources verbruikt compute, op basis van hun afzonderlijke prijzen.     

**Kan ik Azure Machine Learning-Model Management gebruiken voor het implementeren van modellen gebouwd met behulp van hulpprogramma's dan de Service experimenteren?**

U krijgt de beste ervaring wanneer het implementeren van modellen die zijn gemaakt met behulp van de Service experimenteren. U kunt echter modellen gebouwd met behulp van andere frameworks en hulpprogramma's implementeren. We bieden ondersteuning voor tal van modellen inclusief MMLSpark, TensorFlow, Microsoft cognitieve Toolkit scikit-meer, Keras, enzovoort. 

**Kan ik mijn eigen Azure-resources gebruiken?**

Ja, de experimenteren Service en de Model-Management worden gebruikt in combinatie met meerdere Azure gegevensarchieven, compute werkbelastingen en andere services. Raadpleeg onze technische documentatie voor meer informatie over Azure-services vereist.

**Ondersteuning voor zowel on-premises en u cloud implementatiescenario's?**

Ja. We ondersteuning voor lokale en cloud implementatiescenario's via de Docker-containers. Lokale uitvoering doelen bevatten: één knooppunt Docker-implementaties, [Microsoft SQL Server met ML Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop of Spark. We bieden ook ondersteuning voor cloud implementaties via Docker, met inbegrip van: geclusterde implementaties via Azure Container Service en Kubernetes, HDInsight of Spark-clusters. Edge-scenario's worden ondersteund via de Docker-containers en Azure IOT rand. 

**Kan ik een Docker-installatiekopie die is gemaakt met de Azure Machine Learning CLI op een andere host uitvoeren?**

Ja. Zolang de host heeft onvoldoende rekenresources voor het hosten van de docker-installatiekopie, kunt u de afbeelding als een webservice op een willekeurige host docker.

**Ondersteund retraining van geïmplementeerde modellen?**

Ja, kunt u meerdere versies van hetzelfde model implementeren. Model Management wordt ondersteuning voor service-updates voor alle bijgewerkte modellen en afbeeldingen.

## <a name="workbench"></a>Workbench

**Wat is de Workbench van Azure Machine Learning?**

De Azure Machine Learning-Workbench is een companion-toepassing voor professionele gegevenswetenschappers gebouwd. Beschikbaar voor Windows en Mac, de Machine Learning-Workbench biedt overzicht, beheer en controle voor machine learning-oplossingen. De Machine Learning-Workbench omvat toegang voor geavanceerde AI frameworks van Microsoft en de open-source-community. Vindt u de meest populaire gegevens wetenschappelijke toolkits, met inbegrip van TensorFlow, Microsoft cognitieve Toolkit, Spark ML, scikit-meer en meer. We hebben ook integratie met populaire gegevenswetenschap IDE zoals Jupyter-notebooks PyCharm en Visual Studio Code ingeschakeld. De Machine Learning-Workbench bevat ingebouwde gegevens voorbereiding mogelijkheden voor het snel steekproef, te begrijpen en voorbereiden van gegevens, of gestructureerde of ongestructureerde. Onze nieuwe Gegevenshulpprogramma voor het voorbereiden aangeroepen [PROSE](https://microsoft.github.io/prose/), is gebaseerd op geavanceerde technologie van Microsoft Research.  

**Workbench een IDE is?**

Nee. De Machine Learning-Workbench is ontworpen als aanvulling op de populaire IDE zoals Jupyter-Notebooks en Visual Studio Code PyCharm maar is niet een volledig functionele IDE. De Machine Learning-Workbench biedt een aantal basistekst mogelijkheden bewerken, maar foutopsporing, de intellisense en andere vaak gebruikte IDE-mogelijkheden worden niet ondersteund. Het is raadzaam dat u uw favoriete IDE voor ontwikkeling van de code bewerken en het opsporen van fouten. 

**Is er kosten met zich mee voor het gebruik van de Workbench van Azure Machine Learning?**

Nee. Azure Machine Learning-Workbench is een gratis toepassing. U kunt deze toepassing downloaden op net zoveel machines en voor net zoveel gebruikers als u wilt. Voor het gebruik van Azure Machine Learning Workbench hebt u een Experimenten-account nodig. .  

**Opdrachtregelprogramma mogelijkheden ondersteund?**

A: Ja, biedt Azure Machine Learning een volledige CLI-interface. De Machine Learning CLI is standaard geïnstalleerd met de Azure Machine Learning-Workbench. Het is ook beschikbaar als onderdeel van de Gegevenswetenschap Linux virtuele machine in Azure en zal worden geïntegreerd in de [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)


**Kan ik Jupyter-Notebooks gebruiken met Workbench?**

Ja. U kunt Jupyter-notebooks uitvoeren in de Workbench met Workbench de clienttoepassing hosting, net zoals u een browser als een client gebruiken zou. 

**Welke kernels voor Jupyter-Notebook worden ondersteund?**

A: de huidige versie van Jupyter opgenomen in de Workbench wordt gestart van een Python-3-kernel en een extra kernel voor elk bestand 'runconfig' in de map aml_config. Ondersteunde configuraties zijn onder andere:
- Lokale Python
- Python in lokale of externe Docker

## <a name="data-formats-and-capabilities"></a>Gegevensopmaak en mogelijkheden

**Welke bestandsindelingen momenteel worden ondersteund voor gegevensopname in Workbench?**

Momenteel ondersteund in de hulpmiddelen voor het voorbereiden van gegevens in de Workbench opname van de volgende indelingen: 
- Bestanden met scheidingstekens zoals CSV, TSV, enz.  
- Vaste breedte-bestanden
- Bestanden met tekst zonder opmaak
- Excel (.xls/xlsx)
- JSON-bestanden
- Parketvloeren bestanden 
- Aangepaste bestanden (scripts) als uw oplossing gegevensopname van aanvullende bronnen vereist Python-code kan worden gebruikt om... 

**De opslaglocaties voor welke gegevens worden momenteel ondersteund?**

Voor de openbare preview ondersteunt Workbench gegevensopname uit: 
- Lokale vaste schijf of een toegewezen netwerkstation opslaglocatie
- Azure BLOB- of Azure Storage (een Azure-abonnement vereist)
- Azure SQL Server
- Microsoft SQL Server


**Welke soorten gegevens worsteling en de voorbereiding en transformaties zijn beschikbaar?**

Voor de openbare preview ondersteunt Workbench "Afgeleid kolom door bijvoorbeeld" 'Kolom splitsen door voorbeeld', 'Tekst Clustering', 'Verwerken ontbreken waarden voor' en vele andere.  Workbench biedt ook ondersteuning voor conversie van type, het samenvoegen van gegevens (aantal, gemiddelde, VARIANTIE, enzovoort) en complexe joins. Ga naar onze productdocumentatie voor een volledige lijst met ondersteunde mogelijkheden. 

**Zijn er maximale grootte van gegevens door Azure Machine Learning Workbench, experimenteren of Model Management afgedwongen?**

A: de nieuwe services leggen Nee, geen gegevens beperkingen. Er zijn echter beperkingen die zijn geïntroduceerd in de omgeving waarin u uw gegevens voorbereiden, model training, experimenteren of implementatie wilt uitvoeren. Bijvoorbeeld, als u voor een lokale omgeving voor training ontwikkelt, u beperkt door de beschikbare ruimte op de harde schijf. U kunt ook als u voor HDInsight ontwikkelt, u worden beperkt door een gekoppelde grootte of compute-beperkingen. 

## <a name="algorithms-and-libraries"></a>Algoritmen en bibliotheken

**Welke algoritmen worden ondersteund in Azure Machine Learning Workbench?**

Onze preview producten en services omvatten het beste van de open-source-community. We bieden ondersteuning voor een breed scala aan algoritmen en bibliotheken met inbegrip van TensorFlow, scikit-informatie over Apache Spark en de Microsoft cognitieve Toolkit. De Azure Machine Learning-Workbench pakketten ook de [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy) pakket.

**Hoe Azure Machine Learning gerelateerd aan de Microsoft cognitieve Toolkit?**

De [Microsoft cognitieve Toolkit](https://www.microsoft.com/cognitive-toolkit/) is een veel frameworks die worden ondersteund door onze nieuwe hulpprogramma's en services. De cognitieve Toolkit is een uniform deep-learning-toolkit waarmee u kunt gebruiken en het combineren van populaire machine learning-modellen Feed doorsturen Deep Neural Networks, Convolutional netten, inclusief Sequence naar Sequence en terugkerende netwerken. Voor meer informatie over Microsoft cognitieve Toolkit, gaat u naar onze [productdocumentatie](https://docs.microsoft.com/cognitive-toolkit/). 

## <a name="pricing-and-billing"></a>Prijzen en facturering

**Bent u in rekening gebracht voor Azure Machine Learning tijdens de preview?** 

A: het Azure Machine Learning Workbench toepassing beschikbaar is gratis Azure-abonnees. De Service experimenteren en het Modelbeheer bieden gratis lagen, naast betaald lagen, die beschikbaar zijn op een korting tijdens de openbare Preview.

**Ik gefactureerd op basis van hoeveel experimenten ik uitvoeren?**

Nee, u kunt met de Experimenten-service net zoveel experimenten uitvoeren als u wilt. De kosten zijn uitsluitend gebaseerd op het aantal gebruikers. Rekenbronnen voor experimenten worden afzonderlijk in rekening gebracht.  We raden u aan het uitvoeren van meerdere experimenten voor het beste model voor uw oplossing aankomen. 

**Ik in rekening gebracht op basis van hoe vaak mijn web-services wordt heet?**

Nee. Web-services kunnen zo vaak als nodig zijn, zonder gevolgen facturering Model Management worden aangeroepen. U hebt de volledige controle om uw implementaties te schalen om te voldoen aan de vereisten van uw toepassingen.

**Hoe kan ik het aantal eenheden dat ik hebt aangeschaft in de Azure Machine Learning-Model Management schalen?**

Kunt u het aantal eenheden, omhoog of omlaag, met behulp van de Azure-portal of de CLI. 

**Hoe wordt mijn factuur eruit?**

Facturen worden dagelijks geproduceerd. Voor facturatiedoeleinden begint een dag om middernacht (UTC). Facturen worden maandelijks gegenereerd. Afzonderlijke kosten verbonden zijn voor Azure-services die worden gebruikt in combinatie met Azure Machine Learning. Kosten kunnen bevatten, maar zijn niet beperkt tot: 
- COMPUTE-kosten
- HDInsight
- Azure Container Service
- Azure Container Registry 
- Azure Blob Storage
- Application Insights
- Azure Key Vault
- Visual Studio Team Services
- Azure Event Hub
- Azure Stream Analytics voor meer informatie of als een rekening voorbeeld wilt bekijken, gaat u naar onze pagina met prijzen. 

## <a name="support-and-training"></a>Ondersteuning en training

**Waar kan ik training krijgen voor Azure Machine Learning?**

Het [Azure Machine Learning-documentatiecentrum](./overview-what-is-azure-ml.md) bevat video’s voor zelfstudie en handleidingen. Deze stapsgewijze instructies vindt de services en de levenscyclus van de gegevens wetenschappelijke toegelicht. We voegen regelmatig nieuw materiaal toe aan Machine Learning Center. U kunt aanvragen versturen voor extra materiaal op het forum met feedback van gebruikers.

**Waar kan ik ondersteuning krijgen voor Azure Machine Learning?**

Voor technische ondersteuning, Ga voor de ondersteuning van Azure en selecteer van Machine Learning. Azure Machine Learning heeft ook een actieve community op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-machine-learning) gemarkeerd met "Azure-Machine-Learning' wordt bewaakt door het team.  Suggesties en functie-aanvragen kunnen worden ingediend bij de [Azure Machine Learning Feedback](https://feedback.azure.com/forums/257792-machine-learning) site. 
