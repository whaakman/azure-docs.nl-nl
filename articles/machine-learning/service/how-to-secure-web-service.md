---
title: Beveiligde web-services met behulp van SSL
titleSuffix: Azure Machine Learning service
description: Meer informatie over het beveiligen van een webservice die wordt geïmplementeerd via de service Azure Machine Learning met inschakeling van HTTPS. Gegevens uit door clients beveiligt HTTPS met behulp van transport layer security (TLS), een vervanging voor secure socket Layer (SSL). Clients gebruiken HTTPS ook om te controleren of de identiteit van de webservice.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 779a34800057284ce77b6d76e030ddca5fadc25f
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393829"
---
# <a name="use-ssl-to-secure-a-web-service-through-azure-machine-learning"></a>SSL gebruiken voor het beveiligen van een webservice via Azure Machine Learning

In dit artikel wordt beschreven hoe u voor het beveiligen van een webservice die wordt geïmplementeerd via de Azure Machine Learning-service.

U gebruikt [HTTPS](https://en.wikipedia.org/wiki/HTTPS) toegang beperken tot webservices en beveiligen van de gegevens die clients verzenden. Beveiligde communicatie tussen een client en een webservice helpt HTTPS door het versleutelen van communicatie tussen de twee. Maakt gebruik van versleuteling [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS wordt soms ook wel aangeduid als *Secure Sockets Layer* (SSL), dat is de voorloper van TLS.

> [!TIP]
> De SDK van Azure Machine Learning wordt de term 'SSL' voor eigenschappen die zijn gerelateerd aan beveiligde communicatie. Dit betekent niet dat de webservice niet gebruiken *TLS*. SSL is slechts een meer algemeen erkende term.

TLS en SSL beide zijn afhankelijk van *digitale certificaten*, die helpen bij verificatie van versleuteling en identiteit. Zie het onderwerp Wikipedia voor meer informatie over hoe digitale certificaten werk [openbare-sleutelinfrastructuur](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!WARNING]
> Als u geen gebruik van HTTPS voor uw webservice, zijn gegevens die worden verzonden naar en van de service mogelijk zichtbaar voor iedereen op internet.
>
> HTTPS kan ook de client om te controleren of de authenticiteit van de server waarmee deze verbinding wordt maakt. Deze functie beschermt clients op basis van [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) aanvallen.

Dit is het algemene proces voor het beveiligen van een webservice:

1. Een domeinnaam krijgen.

2. Een digitaal certificaat ophalen.

3. Implementeer of bijwerken van de webservice met SSL is ingeschakeld.

4. Werkt u uw DNS om te verwijzen naar de webservice.

> [!IMPORTANT]
> Als u naar Azure Kubernetes Service (AKS) implementeert, kunt u uw eigen certificaat hebt gekocht of gebruik van een certificaat dat wordt geleverd door Microsoft. Als u een certificaat van Microsoft gebruikt, moet u niet om een domeinnaam of het SSL-certificaat te krijgen. Zie voor meer informatie de [SSL inschakelt en implementeert](#enable) sectie van dit artikel.

Er zijn kleine verschillen wanneer u webservices tussen beveiligde [implementatiedoelen](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Een domeinnaam krijgen

Als u niet al een domeinnaam hebt, kopen bij een *domeinnaamregistrar*. Het proces en de prijs verschilt de registrars. De registrar biedt hulpprogramma's voor het beheren van de domeinnaam. U deze hulpprogramma's om toe te wijzen volledig gekwalificeerde domeinnaam (FQDN) gebruiken (zoals www\.contoso.com) naar de IP-adres dat als host fungeert voor de webservice.

## <a name="get-an-ssl-certificate"></a>Een SSL-certificaat ophalen

Er zijn veel manieren om op te halen van een SSL-certificaat (digitale certificaat). De meest voorkomende reden is het kopen van een *certificeringsinstantie* (CA). Ongeacht waar u het certificaat krijgen, moet u de volgende bestanden:

* Een **certificaat**. Het certificaat moet de volledige certificaatketen bevatten en moet "PEM gecodeerd."
* Een **sleutel**. De sleutel moet ook PEM gecodeerd.

Wanneer u een certificaat aanvraagt, moet u de FQDN-naam van het adres dat u wilt gebruiken voor de webservice opgeven (bijvoorbeeld, www\.contoso.com). Het adres dat wordt vermeld in het certificaat en het adres dat de clients gebruiken worden om te controleren of de identiteit van de webservice vergeleken. Als de adressen die niet overeenkomen, haalt de client een foutbericht weergegeven.

> [!TIP]
> Als de certificeringsinstantie kan niet het certificaat en sleutel als PEM-gecodeerde bestanden opgeeft, kunt u een hulpprogramma zoals [OpenSSL](https://www.openssl.org/) om de opmaak te wijzigen.

> [!WARNING]
> Gebruik *zelfondertekend* certificaten alleen voor ontwikkeling. Gebruik geen ze in productie-omgevingen. Zelfondertekende certificaten kunnen leiden tot problemen in uw client toepassingen. Zie voor meer informatie de documentatie voor de netwerkbibliotheken die gebruikmaakt van de clienttoepassing.

## <a id="enable"></a> SSL inschakelen en implementeren

Als u wilt implementeren (of opnieuw implementeren) op de service met SSL is ingeschakeld, stel de *ssl_enabled* parameter in op 'True' wanneer deze van toepassing is. Stel de *ssl_certificate* parameter met de waarde van de *certificaat* bestand. Stel de *ssl_key* aan de waarde van de *sleutel* bestand.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>Implementeren in AKS en veld-programmable gate array (FPGA)

  > [!NOTE]
  > De informatie in deze sectie geldt ook wanneer u een beveiligde web-service voor de visuele interface implementeert. Als u niet bekend bent met het gebruik van de Python-SDK, Zie [wat is de Azure Machine Learning-SDK voor Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

Wanneer u met AKS implementeert, kunt u een nieuw AKS-cluster maken of koppelen van een bestaande resourcegroep.
  
-  Als u een nieuw cluster maakt, gebruikt u  **[AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-)** .
- Als u een bestaand cluster koppelt, kunt u gebruiken  **[AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-)** . Retourneren beide een configuratieobject dat heeft een **enable_ssl** methode.

De **enable_ssl** methode kunt gebruiken een certificaat dat wordt geleverd door Microsoft of een certificaat dat u aanschaft.

  * Wanneer u een certificaat van Microsoft gebruikt, moet u de *leaf_domain_label* parameter. Deze parameter genereert de DNS-naam voor de service. Een waarde van 'myservice' maakt bijvoorbeeld een domeinnaam van ' myservice\<zes willekeurige tekens >.\< azureregio >. cloudapp.azure.com ", waarbij \<azureregio > is de regio waarin de service. Desgewenst kunt u de *overwrite_existing_domain* parameter overschrijf de bestaande *leaf_domain_label*.

    Als u wilt implementeren (of opnieuw implementeren) op de service met SSL is ingeschakeld, stel de *ssl_enabled* parameter in op 'True' wanneer deze van toepassing is. Stel de *ssl_certificate* parameter met de waarde van de *certificaat* bestand. Stel de *ssl_key* aan de waarde van de *sleutel* bestand.

    > [!IMPORTANT]
    > Wanneer u een certificaat van Microsoft gebruikt, moet u niet de naam van uw eigen certificaat of het domein kopen.

    Het volgende voorbeeld ziet u hoe u een configuratie waarmee een SSL-certificaat van Microsoft maakt:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * Bij het gebruik *een certificaat dat u hebt aangeschaft*, gebruikt u de *ssl_cert_pem_file*, *ssl_key_pem_file*, en *ssl_cname* de parameters. Het volgende voorbeeld ziet u hoe u *.pem* bestanden te maken van een configuratie die gebruikmaakt van een SSL-certificaat dat u hebt gekocht:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Voor meer informatie over *enable_ssl*, Zie [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) en [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Implementeren op Azure Container Instances

Wanneer u in Azure Container Instances implementeert, opgeven u waarden voor SSL-gerelateerde parameters, zoals in de volgende code codefragment wordt weergegeven:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Zie voor meer informatie, [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Werkt u uw DNS

Vervolgens moet u uw DNS om te verwijzen naar de webservice bijwerken.

+ **Voor Container Instances:**

  Gebruik de hulpprogramma's van uw domeinnaamregistrar om bij te werken van de DNS-record voor uw domeinnaam. De record moet verwijzen naar het IP-adres van de service.

  Er is een vertraging van minuten of uren voordat clients de domeinnaam, afhankelijk van de registrar en 'time to live' (TTL) die geconfigureerd voor de domeinnaam kunnen omzetten.

+ **Voor AKS:**

  > [!WARNING]
  > Als u hebt gebruikt *leaf_domain_label* voor het maken van de service met behulp van een certificaat van Microsoft, niet handmatig bijwerken van de DNS-waarde voor het cluster. De waarde moet automatisch worden ingesteld.

  Bijwerken van de DNS-server op de **configuratie** tabblad van het openbare IP-adres van het AKS-cluster. (Zie de volgende afbeelding.) Het openbare IP-adres is een resourcetype dat wordt gemaakt onder de resourcegroep met de AKS-knooppunten van de agent en andere netwerkresources.

  ![Azure Machine Learning-service: Beveiligen met SSL-webservices](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Volgende stappen
Leer hoe u het volgende doet:
+ [Een machine learning-model is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
+ [Veilig kunt uitvoeren van experimenten en Deductie in een Azure-netwerk](how-to-enable-virtual-network.md)
