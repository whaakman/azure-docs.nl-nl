---
title: Beveiliging voor bedrijven
titleSuffix: Azure Machine Learning service
description: 'Veilig gebruik van Azure Machine Learning-service: verificatie, autorisatie, beveiliging van het netwerk, gegevensversleuteling en bewaking.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/10/2019
ms.openlocfilehash: b950e7d38235d089c6236c76136d8ec2fc7a1f74
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731330"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Bedrijfsbeveiliging voor Azure Machine Learning-service

In dit artikel leert u over beveiligingsfuncties die beschikbaar zijn met de Azure Machine learning-webservice.

Wanneer u een service in de cloud gebruikt, is het aanbevolen om te beperken van toegang tot de gebruikers die deze nodig hebben. Hiermee start u inzicht hebt in de verificatie en autorisatie model gebruikt door de service. U kunt ook wilt beperken van toegang tot het netwerk of veilig worden toegevoegd aan resources in uw on-premises netwerk met die in de cloud. Gegevensversleuteling is ook essentieel in rust en terwijl de gegevens worden verplaatst tussen services. Tot slot moet u mogelijk om te controleren van de service en een logboek van alle activiteiten te produceren.

## <a name="authentication"></a>Authentication
Multi-factor authentication wordt ondersteund als Azure Active Directory (Azure AD) is geconfigureerd voor dezelfde.
* Client meldt zich aan bij Azure AD en Azure Resource Manager-token opgehaald.  Gebruikers- en Service-Principals worden volledig ondersteund.
* Client geeft naar Azure Resource Manager-token en alle Azure Machine Learning-services
* Azure Machine Learning-service biedt een Azure Machine Learning-token voor het berekenen van de gebruiker. Bijvoorbeeld, Machine Learning-Computing. Dit token wordt gebruikt door de gebruiker van Azure Machine Learning compute om aan te roepen terug in Azure Machine Learning-service (beperkt bereik aan werkruimte) na de uitvoering is voltooid.

![Schermopname die laat zien hoe verificatie werkt in Azure Machine Learning-service](./media/enterprise-readiness/authentication.png)

### <a name="authentication-keys-for-web-service-deployment"></a>Verificatie-codes voor implementatie van de webservice

Als u verificatie voor een implementatie inschakelt, maakt u automatisch verificatiesleutels.

* Verificatie is standaard ingeschakeld wanneer u in Azure Kubernetes Service implementeert.
* Verificatie is standaard uitgeschakeld wanneer u in Azure Container Instances implementeert.

Voor het beheren van verificatie, gebruikt u de `auth_enabled` parameter wanneer u het maken of bijwerken van een implementatie.

Als verificatie is ingeschakeld, kunt u de `get_keys` methode voor het ophalen van een primaire en secundaire verificatiesleutel:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Als u een sleutel opnieuw genereren wilt, gebruikt u [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)


## <a name="authorization"></a>Autorisatie

U kunt meerdere werkruimten maken en elke werkruimte kan worden gedeeld door meerdere personen. Wanneer u een werkruimte deelt, kunt u toegang tot deze kunt beheren door de volgende rollen toewijzen aan gebruikers:
* Eigenaar
* Inzender
* Lezer
    
De volgende tabel bevat enkele van de primaire bewerkingen voor Azure Machine Learning-service en de rollen die ze kunnen uitvoeren:

| Azure Machine Learning-service opnieuw | Eigenaar | Inzender | Lezer |
| ---- |:----:|:----:|:----:|
| Werkruimte maken | ✓ | ✓ | |
| Werkruimte voor delen | ✓ | |  |
| Compute maken | ✓ | ✓ | |
| Compute toevoegen | ✓ | ✓ | |
| Gegevensopslag koppelen | ✓ | ✓ | |
| Een Experiment uitvoeren | ✓ | ✓ | |
| Uitvoeringen/metrische gegevens weergeven | ✓ | ✓ | ✓ |
| Model registreren | ✓ | ✓ | |
| Installatiekopie maken | ✓ | ✓ | |
| -Webservice implementeren | ✓ | ✓ | |
| Modellen/afbeeldingen weergeven | ✓ | ✓ | ✓ |
| Webservice aanroepen | ✓ | ✓ | ✓ |

Als de ingebouwde rollen onvoldoende is voor uw behoeften zijn, kunt u ook aangepaste rollen maken. Houd er rekening mee dat het alleen aangepaste rollen die wordt ondersteund voor bewerkingen in de werkruimte en Machine Learning-Computing. De aangepaste rollen hebben mogelijk lezen, schrijven of verwijderen van machtigingen voor de werkruimte en de rekenresource in deze werkruimte. U kunt de rol beschikbaar maken op het niveau van een specifieke werkruimte, het niveau van een specifieke resourcegroep of een bepaald abonnement. Zie voor meer informatie, [beheren van gebruikers en rollen in een Azure Machine Learning-werkruimte](how-to-assign-roles.md)

### <a name="securing-compute-and-data"></a>Compute en -gegevens beveiligen
Eigenaren en bijdragers kunnen alle compute-doelen en gegevensarchieven die zijn gekoppeld aan de werkruimte gebruiken.  
Elke werkruimte heeft ook een gekoppeld door het systeem toegewezen beheerde identiteit (met dezelfde naam als de werkruimte) met de volgende machtigingen voor de gekoppelde resources die worden gebruikt in de werkruimte:

Zie voor meer informatie over beheerde identiteiten [beheerde identiteiten voor een Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

| Resource | Machtigingen |
| ----- | ----- |
| Werkruimte | Inzender | 
| Opslagaccount | Gegevensbijdrager voor Blob | 
| Key Vault | Toegang tot alle sleutels en geheimen,-certificaten | 
| Azure Container Registry | Inzender | 
| Resourcegroep met de werkruimte | Inzender | 
| Resourcegroep met de Key Vault (indien dit is anders dan de constructor met de werkruimte) | Inzender | 

Het verdient aanbeveling dat beheerders Trek de toegang van de beheerde identiteit tot de resources die hierboven worden vermeld. Toegang kan worden hersteld met de bewerking sleutels opnieuw synchroniseren.

Azure Machine Learning-service maakt u een extra toepassing (de naam begint met aml-) met het niveau toegangsrechten van een bijdrager in uw abonnement voor elke regio werkruimte. Voor bijvoorbeeld. Als u een werkruimte in VS-Oost en een andere werkruimte in Noord-Europa in hetzelfde abonnement ziet u 2 dergelijke toepassingen. Dit is nodig zodat die rekenresources van Azure Machine Learning-service kan helpen beheren.


## <a name="network-security"></a>Netwerkbeveiliging

De Azure Machine Learning-service is afhankelijk van andere Azure-services voor compute-resources. COMPUTE-resources (compute-doelen) worden gebruikt om te trainen en modellen te implementeren. Deze compute-doelen kunnen worden gemaakt binnen een virtueel netwerk. Bijvoorbeeld, kunt u de Microsoft Data Science Virtual Machine naar een model te trainen en implementeer vervolgens het model naar Azure Kubernetes Service (AKS).  

Zie voor meer informatie, [experimenten en inferentietaken uitvoeren in een virtueel netwerk](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Gegevensversleuteling

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'
#### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Machine Learning-service slaat momentopnamen, uitvoer en logboeken op in de Azure Blob Storage-account die is gekoppeld aan de werkruimte van de Azure Machine Learning-service en die zich bevinden in het abonnement van de gebruiker. Alle gegevens die zijn opgeslagen in Azure Blob-opslag is in rust versleuteld met behulp van Microsoft-Managed sleutels.

Zie voor meer informatie over hoe u uw eigen sleutels voor de gegevens die zijn opgeslagen in Azure Blob Storage brengt [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Traininggegevens worden meestal ook opgeslagen in Azure Blob-opslag zodat deze toegankelijk is voor de training compute. Deze opslag wordt niet beheerd door Azure Machine Learning, maar gekoppeld voor compute als een extern bestandssysteem.

#### <a name="cosmos-db"></a>Cosmos DB
Azure Machine Learning-service slaat metrische gegevens en metagegevens om de Cosmos-DB die zich in een abonnement dat Microsoft wordt beheerd door Azure Machine Learning-service. Alle gegevens die zijn opgeslagen in Cosmos DB worden versleuteld in rust door Microsoft beheerde sleutels.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)
Alle containerinstallatiekopieën in het register (ACR) worden in rust versleuteld. Azure automatisch een installatiekopie van een versleuteld voordat deze worden opgeslagen en ontsleutelt deze tijdens het als Azure Machine Learning-service wordt de afbeelding opgehaald.

#### <a name="machine-learning-compute"></a>Machine Learning Compute
De besturingssysteemschijf is voor elk knooppunt is opgeslagen in Azure Storage versleuteld met Microsoft beheerde sleutels in Azure Machine Learning-service storage-accounts. Deze berekening is tijdelijk en clusters doorgaans omlaag worden geschaald wanneer er geen uitvoeringen zijn in de wachtrij. De onderliggende virtuele machine is verlopen en de schijf met besturingssysteem verwijderd. Azure disk encryption wordt niet ondersteund voor de besturingssysteemschijf.
Elke virtuele machine heeft ook een lokale tijdelijke schijf voor OS-bewerkingen. Deze schijf kan ook optioneel worden gebruikt om gegevens te Faseren training. Deze schijf is niet versleuteld. Zie voor meer informatie over de werking van versleuteling-at-rest in Azure [Azure Data Encryption-at-Rest](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest). 

### <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Zowel interne communicatie tussen verschillende micro Azure Machine Learning-services en externe communicatie van het aanroepen van het scoring-eindpunt worden ondersteund met behulp van SSL. Alle voor toegang tot de Azure-opslag is ook via een beveiligd kanaal. Zie voor meer informatie, [Secure Azure Machine Learning-webservices met behulp van SSL](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Met behulp van Azure Key Vault
Key Vault-instantie die is gekoppeld aan de werkruimte wordt gebruikt door Azure Machine Learning-service voor het opslaan van referenties van verschillende typen:
* De verbindingsreeks van de gekoppelde storage-account
* Wachtwoorden van opslagplaats voor Azure Container instances
* Verbinding tekenreeksen met gegevens opslaat. 

SSH-wachtwoorden en compute-doelen zoals HDI HDInsight en de VM-sleutels worden opgeslagen in een afzonderlijke Sleutelkluis die is gekoppeld aan Microsoft-abonnement. Azure Machine Learning-service slaat de wachtwoorden of sleutels opgegeven door de gebruiker in plaats daarvan het genereert, wordt geautoriseerd en een eigen SSH-sleutels worden opgeslagen om verbinding met virtuele machine/HDInsight om uit te voeren van experimenten. Elke werkruimte heeft een bijbehorende door het systeem toegewezen beheerde identiteit (met dezelfde naam als de werkruimte) die toegang tot alle sleutels, geheimen en certificaten in de Key Vault heeft.

 
## <a name="monitoring"></a>Bewaking

Gebruikers kunnen zien van het activiteitenlogboek onder de werkruimte om te zien van verschillende bewerkingen die worden uitgevoerd in de werkruimte en het ophalen van de algemene informatie, zoals de naam van de bewerking, gebeurtenis gestart door, timestamp, enzovoort.

De volgende schermafbeelding ziet u het activiteitenlogboek voor een werkruimte:

![Schermopname die laat zien activiteitenlogboek onder een werkruimte](./media/enterprise-readiness/workspace-activity-log.png)


Scoring details van de aanvraag worden opgeslagen in de Application Insights, die in het abonnement van de gebruiker tijdens het maken van de werkruimte is gemaakt. Hieronder vallen velden als HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, duur, enzovoort.


## <a name="data-flow-diagram"></a>Stroomdiagram van gegevens

### <a name="create-workspace"></a>Werkruimte maken
Het volgende diagram toont de werkstroom van de werkruimte maken.
Gebruiker meldt zich aan bij Azure AD in elk van de ondersteunde clients Azure Machine Learning-service (Azure CLI, Python-SDK-portal) en het juiste Azure Resource Manager-token aanvragen.  Gebruiker roept vervolgens Azure Resource Manager voor het maken van de werkruimte.  Azure Resource Manager-contactpersonen de Azure Machine Learning-service-Resourceprovider voor het inrichten van de werkruimte.  Aanvullende bronnen worden gemaakt in het abonnement van de klant tijdens het maken van de werkruimte:
* Key Vault (voor het opslaan van geheimen)
* Een Azure Storage-account (met inbegrip van de Blob & bestandsshare)
* Azure Container Registry (voor het opslaan van docker-installatiekopieën voor inferentietaken en experimenten)
* Application Insights (voor het opslaan van telemetrie)

Andere berekeningen die is gekoppeld aan een werkruimte (Azure Kubernetes Service, virtuele machine enzovoort) kunnen ook worden ingericht door klanten zo nodig. 

![Schermafbeelding van de maken werkruimte werkstroom](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Broncode (trainingsscripts) opslaan
Het volgende diagram toont de werkstroom van de momentopname van code.
Dat is gekoppeld aan een Azure Machine Learning werkruimte-service zijn mappen (experimenten), waarin de broncode (trainingsscripts).  Deze worden opgeslagen op de lokale computer van de klant en in de cloud (in de Azure Blob-opslag van de klant-abonnement). Deze code-momentopnamen worden gebruikt voor uitvoering of controle voor historische controle.

![Schermafbeelding van de maken werkruimte werkstroom](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Training
Het volgende diagram toont de training-werkstroom.
* Azure Machine Learning-service wordt aangeroepen met de momentopname-ID voor de momentopname van de code hebt opgeslagen
* Azure Machine Learning-service maakt uitvoert (optioneel)-ID & token uit Azure Machine Learning-service, die later wordt gebruikt door de compute-doelen, zoals Machine Learning Compute/VM kunnen praten met Azure Machine Learning-service
* U kunt een beheerde compute (ex.) Machine Learning-Computing) of niet-beheerde compute (ex.) Virtuele machine) uw trainingstaken uit te voeren. De gegevensstroom wordt uitgelegd voor zowel de volgende scenario's:
* (Virtuele machine/HDInsight/lokaal – geopend met behulp van SSH-referenties in Key Vault in Microsoft-abonnement) Azure Machine Learning-service wordt beheer code uitgevoerd op de compute-doel dat:
    1.  De omgeving voorbereid (Opmerking: Docker is ook een optie voor virtuele machine/lokale. Zie de stappen voor het Machine Learning-Computing hieronder om te begrijpen hoe actieve experiment op docker-container werkt)
    2.  De code downloaden
    3.  Stelt u de omgeving variabelen/configuraties
    4.  Gebruikersscript (code momentopname hierboven genoemde) wordt uitgevoerd
* (Machine Learning-Computing – geopend met behulp van de werkruimte beheerde identiteit) Houd er rekening mee dat omdat de Machine Learning-Computing is een beheerde compute dat wil zeggen, deze wordt beheerd door Microsoft, als gevolg hiervan deze wordt uitgevoerd onder het Microsoft-abonnement.
    1.  Externe Docker-constructie wordt gestart, indien nodig
    2.  Beheer van code schrijft naar gebruiker Azure-bestandsshare
    3.  Container wordt gestart met de eerste opdracht dat wil zeggen, beheer van code in de bovenstaande stap


#### <a name="querying-runs--metrics"></a>Uitvoeren van query's wordt uitgevoerd en metrische gegevens
Deze stap wordt weergegeven in de stroom waar training schrijfbewerkingen compute de *uitvoeren metrische gegevens* terug naar de Azure Machine Learning-service van waar deze wordt opgeslagen in de Cosmos DB. Clients kunnen aanroepen van Azure Machine Learning-service die op zijn beurt metrische gegevens ophalen uit de Cosmos DB en deze terug naar de client geretourneerd.

![Schermafbeelding van de maken werkruimte werkstroom](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Het maken van webservices
Het volgende diagram toont de inferentietaken werkstroom waarin model is geïmplementeerd als een webservice.
Zie de informatie hieronder:
* Gebruiker zich registreert voor een model met behulp van een client, zoals Azure ML-SDK
* Gebruiker maakt een installatiekopie met behulp van model, score bestands- en andere afhankelijkheden model
* De Docker-installatiekopie is gemaakt en opgeslagen in de ACR
* De webservice wordt geïmplementeerd voor de compute-doel (ACI/AKS) met behulp van de hierboven gemaakte
* Scoring details van de aanvraag worden opgeslagen in de Application Insights, die zich in abonnement van de gebruiker
* Telemetrie wordt ook gepusht naar Microsoft/Azure-abonnement

![Schermafbeelding van de maken werkruimte werkstroom](./media/enterprise-readiness/inferencing.png)

## <a name="next-steps"></a>Volgende stappen

* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Het uitvoeren van voorspellingen van batch](how-to-run-batch-predictions.md)
* [Uw Azure Machine Learning-modellen met Application Insights bewaken](how-to-enable-app-insights.md)
* [Verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md)
* [Azure Machine Learning-service SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning-service gebruiken met Azure Virtual Networks](how-to-enable-virtual-network.md)
* [Aanbevolen procedures voor het bouwen van aanbevelingssystemen](https://github.com/Microsoft/Recommenders)
* [Een realtime aanbeveling API bouwen op Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
