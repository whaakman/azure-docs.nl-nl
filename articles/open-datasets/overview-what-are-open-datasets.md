---
title: Wat zijn open gegevenssets? Samengestelde openbare gegevenssets
titleSuffix: Azure Open Datasets (preview)
description: Meer informatie over Azure Open gegevenssets (preview), gecureerde gegevenssets uit het publieke domein die klaar zijn voor gebruik in machine learning en analyse-oplossingen. Gegevenssets omvatten openbare gegevens zoals weer, telling, feestdagen en locatie kunt u voorspellende oplossingen te verrijken.
ms.topic: overview
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: 439c25363d4c3b24b391b49811d3806c98171034
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026947"
---
# <a name="what-are-azure-open-datasets-preview-and-how-can-you-use-them"></a>Wat zijn Azure Open-gegevenssets (preview) en hoe u ze kunt gebruiken?

[Azure Open gegevenssets](https://azure.microsoft.com/services/open-datasets/) zijn samengestelde openbare gegevenssets die u extra scenario-specifieke functies toegevoegd aan machine learning-oplossingen voor nauwkeurigere modellen kunt gebruiken. Open gegevenssets zijn in de cloud op Microsoft Azure en zijn direct beschikbaar voor Azure Databricks, Machine Learning-service en Machine Learning Studio. U kunt ook toegang tot de gegevenssets via API's en deze gebruiken in andere producten, zoals Power BI en Azure Data Factory.

Gegevenssets omvatten openbaredomein gegevens voor de weersomstandigheden, telling, feestdagen, openbare veiligheid en locatie die u helpen bij machine learning-modellen trainen en voorspellende oplossingen te verrijken. U kunt ook uw openbare gegevenssets voor Azure Open gegevenssets delen. 

![Azure Open gegevenssets-onderdelen](./media/overview-what-are-open-datasets/open-datasets-components.png)

## <a name="curated-prepared-datasets"></a>Samengestelde, voorbereid gegevenssets
Samengestelde open openbare gegevenssets in Azure opent gegevenssets zijn geoptimaliseerd voor gebruik in machine learning-werkstromen. 

Gegevenswetenschappers merendeel vaak het van de tijd opschonen en voorbereiden van gegevens voor geavanceerde analyses. Open gegevenssets zijn gekopieerd naar de Azure-cloud en voorverwerkte u bespaart u tijd. Met regelmatige tussenpozen gegevens opgehaald uit de bronnen, zoals geparseerd door een FTP-verbinding met de National Oceanic and Atmospheric Administration (NOAA), in een gestructureerde indeling en vervolgens worden uitgebreid zo nodig met functies zoals zip-code of de locatie van de dichtstbijzijnde weerstation.

Gegevenssets zijn cohosted met cloud computing in Azure die de toegang en manipulatie eenvoudiger.  

Hieronder vindt u voorbeelden van beschikbare gegevenssets. 

### <a name="weather-data"></a>Weergegevens
 
|Gegevensset         | Notebooks     | Description                                    |
|----------------|---------------|------------------------------------------------|
|[Surface-gegevens van NOAA geïntegreerd (RBD)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureDatabricks) | Over de hele wereld per uur weergegevens van NOAA met de beste ruimtelijke dekking in Noord-Amerika, Europa, Australië en delen van Azië. Dagelijks bijgewerkt. |
|[Globale NOAA prognose-systeem (algemene)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureDatabricks) | per uur weer van 15 dagen VS prognose van de gegevens van NOAA. Dagelijks bijgewerkt. |

### <a name="calendar-data"></a>Agendagegevens

|Gegevensset         | Notebooks     | Description                                    |
|----------------|---------------|------------------------------------------------|
|[Feestdagen](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureDatabricks) | Gegevens over de hele wereld feestdag, die betrekking hebben op 41 landen of regio's vanaf 1970 naar 2099. Land/regio en of de meeste mensen tijd uitgeschakeld hebben betaald bevat. |

## <a name="access-to-datasets"></a>Toegang tot gegevenssets  
U kunt met een Azure-account toegang tot open gegevenssets met behulp van code of via de Azure service-interface. De gegevens zijn geplaatst met Azure-cloud-rekenresources voor gebruik in uw machine learning-oplossing.  

Open gegevenssets biedt Azure-notitieblokken en Azure Databricks notebooks, die u gebruiken kunt om gegevens met Azure Machine Learning-service en Azure Databricks. Gegevenssets kunnen ook worden geopend via een Python-SDK. 

U kunt een Azure-account voor toegang tot Open gegevenssets; echter niet nodig ze zijn op te vragen in een Python-omgeving zonder of zonder Spark.

## <a name="request-or-contribute-datasets"></a>Vragen of bij te dragen gegevenssets

Als u de gegevens niet vinden u wilt, een e-mail verzenden naar [aanvragen van een gegevensset](mailto:aod@microsoft.com?Subject=Contribute%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A) of [bijdragen van een gegevensset](mailto:aod@microsoft.com?Subject=Request%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A). 

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeld-notebook](samples.md)
* [Zelfstudie: Regressie met NY taxi gegevens modelleren](tutorial-opendatasets-automl.md)
* [Python-SDK voor gegevenssets die worden geopend](https://aka.ms/open-datasets-api)
