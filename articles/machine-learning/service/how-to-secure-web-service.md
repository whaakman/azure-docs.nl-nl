---
title: Beveiligen met SSL-webservices
titleSuffix: Azure Machine Learning service
description: Meer informatie over het beveiligen van een webservice die is geïmplementeerd met de service Azure Machine Learning met inschakeling van HTTPS. HTTPS beveiligt de gegevens die zijn ingediend door clients die gebruikmaken van transport layer security (TLS), een vervanging voor secure socket Layer (SSL). Dit wordt ook gebruikt door clients om te controleren of de identiteit van de webservice.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 527f16e34e0f21d435fbd166328235566687bc88
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65852011"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>SSL gebruiken voor het beveiligen van webservices met Azure Machine Learning-service

In dit artikel leert u over het beveiligen van een webservice die is geïmplementeerd met de Azure Machine Learning-service. U kunt de toegang beperken tot webservices en beveiligen van de gegevens die zijn ingediend door clients met behulp van [Hypertext Transfer Protocol Secure (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS wordt gebruikt voor het beveiligen van communicatie tussen een client en de webservice door het versleutelen van communicatie tussen de twee. Versleuteling wordt verwerkt met behulp van [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS is nog steeds aangeduid als SSL Secure Sockets Layer (), dat de voorloper van TLS is.

> [!TIP]
> De SDK van Azure Machine Learning wordt de term 'SSL' voor eigenschappen die betrekking hebben op het inschakelen van beveiligde communicatie. Dit betekent niet dat TLS wordt niet gebruikt door de webservice, alleen dat SSL is het beter herkenbaar term voor veel lezers.

TLS en SSL beide zijn afhankelijk van __digitale certificaten__, die worden gebruikt om uit te voeren versleuteling en identiteit verifiëren. Zie voor meer informatie over hoe digitale certificaten werk de Wikipedia-vermelding op [openbare-sleutelinfrastructuur (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Als u niet inschakelen en het gebruik van HTTPS voor uw webservice, is het mogelijk dat gegevens die worden verzonden naar en van de service weergegeven bij andere gebruikers op het internet.
>
> HTTPS kan ook de client om te controleren of de authenticiteit van de server waarmee deze verbinding met maakt. Dit beschermt clients op basis van [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) aanvallen.

Het algemene proces van het beveiligen van een nieuwe webservice of een bestaand is als volgt:

1. Een domeinnaam krijgen.

2. Een digitaal certificaat ophalen.

3. Implementeren of bijwerken van de web-service met de SSL-instelling ingeschakeld.

4. Werkt u uw DNS om te verwijzen naar de webservice.

> [!IMPORTANT]
> Als u naar Azure Kubernetes Service (AKS) implementeert, kunt u uw eigen certificaat of gebruik een certificaat dat is geleverd door Microsoft. Als u het certificaat van Microsoft gebruikt, hoeft u niet om een domeinnaam of het SSL-certificaat te krijgen. Zie voor meer informatie de [SSL inschakelt en implementeert](#enable) sectie.

Er zijn kleine verschillen bij het beveiligen van webservices tussen de [implementatiedoelen](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Een domeinnaam krijgen

Als u een domeinnaam niet al hebt kan, kunt u kopen van een __domeinnaamregistrar__. Tussen registrars, verschilt het proces als de kosten. De registrar biedt u ook met hulpprogramma's voor het beheren van de domeinnaam. Deze hulpprogramma's worden gebruikt om toe te wijzen een volledig gekwalificeerde domeinnaam (zoals www\.contoso.com) naar de IP-adres die als host fungeert voor de webservice.

## <a name="get-an-ssl-certificate"></a>Een SSL-certificaat ophalen

Er zijn veel manieren om op te halen van een SSL-certificaat (digitale certificaat). De meest voorkomende reden is het kopen van een __certificeringsinstantie__ (CA). Ongeacht waar u het certificaat hebt verkregen, moet u de volgende bestanden:

* Een __certificaat__. Het certificaat moet de volledige certificaatketen bevatten en moet PEM gecodeerd.
* Een __sleutel__. De sleutel moet PEM gecodeerd.

Wanneer u een certificaat aanvraagt, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het adres dat u wilt gebruiken voor de webservice opgeven. Bijvoorbeeld, www\.contoso.com. Adres van de tijdstempel in het certificaat en het adres dat wordt gebruikt door de clients worden vergeleken bij het valideren van de identiteit van de webservice. Als de adressen niet overeenkomen, ontvangt de clients een fout.

> [!TIP]
> Als de certificeringsinstantie kan niet het certificaat en sleutel als PEM-gecodeerde bestanden opgeeft, kunt u een hulpprogramma zoals [OpenSSL](https://www.openssl.org/) om de opmaak te wijzigen.

> [!WARNING]
> Zelfondertekende certificaten moeten worden gebruikt alleen voor ontwikkeling. Ze moeten niet worden gebruikt in de productieomgeving. Zelfondertekende certificaten kunnen leiden tot problemen in uw client toepassingen. Zie voor meer informatie de documentatie voor de netwerkbibliotheken die wordt gebruikt in uw clienttoepassing.

## <a id="enable"></a> SSL inschakelen en implementeren

Als u wilt implementeren (of opnieuw implementeren) op de service met SSL is ingeschakeld, stel de `ssl_enabled` parameter `True`, gebruikt u waar van toepassing. Stelt de `ssl_certificate` parameter met de waarde van de __certificaat__ bestand en de `ssl_key` aan de waarde van de __sleutel__ bestand.

+ **Implementeren op Azure Kubernetes Service (AKS) en FPGA**

  > [!NOTE]
  > De informatie in deze sectie geldt ook bij het implementeren van een beveiligde web-service voor de visuele interface. Als u niet bekend bent met behulp van de Python-SDK, raadpleegt u de [overzicht van Azure Machine Learning Python SDK.](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

  Bij het implementeren naar AKS, kunt u een nieuw AKS-cluster maken of koppelen van een bestaande resourcegroep. Het maken van een nieuw cluster maakt gebruik van [AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-) tijdens het koppelen van een bestaand cluster wordt gebruikt [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-). Retourneren beide een configuratieobject dat heeft een `enable_ssl` methode.

  De `enable_ssl` methode kunt ofwel een certificaat dat is geleverd door Microsoft of een die u opgeeft.

  * Wanneer u een certificaat __geleverd door Microsoft__, moet u de `leaf_domain_label` parameter. Met deze parameter wordt de service maken met behulp van een certificaat dat is geleverd door Microsoft. De `leaf_domain_label` wordt gebruikt voor het genereren van de DNS-naam voor de service. Bijvoorbeeld: een waarde van `myservice` maakt u een domeinnaam van `myservice<6-random-characters>.<azureregion>.cloudapp.azure.com`, waarbij `<azureregion>` is de regio waarin de service. Desgewenst kunt u de `overwrite_existing_domain` parameter voor het overschrijven van de bestaande naam van de leaf-domein.

    Als u wilt implementeren (of opnieuw implementeren) op de service met SSL is ingeschakeld, stel de `ssl_enabled` parameter `True`, gebruikt u waar van toepassing. Stelt de `ssl_certificate` parameter met de waarde van de __certificaat__ bestand en de `ssl_key` aan de waarde van de __sleutel__ bestand.

    > [!IMPORTANT]
    > Wanneer u een certificaat dat is geleverd door Microsoft, hoeft u niet de naam van uw eigen certificaat of het domein kopen.

    Het volgende voorbeeld ziet u hoe u configuraties die een SSL-certificaat dat is gemaakt door Microsoft in staat stellen maken:

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

  * Bij het gebruik van __een certificaat dat u hebt aangeschaft__, gebruikt u de `ssl_cert_pem_file`, `ssl_key_pem_file`, en `ssl_cname` parameters. Het volgende voorbeeld ziet u hoe u het maken van de configuraties die gebruikmaken van een SSL-certificaat dat u opgeeft met behulp van `.pem` bestanden:

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

  Voor meer informatie over `enable_ssl`, Zie [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) en [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

+ **Op Azure Container Instances (ACI) implementeren**

  Tijdens de implementatie naar ACI, geef waarden op voor parameters met betrekking tot SSL zoals wordt weergegeven in het volgende codefragment:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

  Zie voor meer informatie, [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Werkt u uw DNS

Vervolgens moet u uw DNS om te verwijzen naar de webservice bijwerken.

+ **Voor ACI**:

  Gebruik de hulpprogramma's van uw domeinnaamregistrar om bij te werken van de DNS-record voor uw domeinnaam. De record moet verwijzen naar het IP-adres van de service.

  Afhankelijk van de registrar, en de tijd naar live (TTL) die is geconfigureerd voor de domeinnaam, duurt het enkele minuten tot enkele uren voordat clients de domeinnaam kunnen omzetten.

+ **Voor AKS**:

  > [!WARNING]
  > Als u gebruikt de `leaf_domain_label` voor het maken van de service met een certificaat dat is geleverd door Microsoft, niet handmatig bijwerken DNS-waarde voor het cluster. De waarde moet automatisch worden ingesteld.

  Werk de DNS-server op het tabblad "Configuratie" van het 'openbare IP-adres' van het AKS-cluster, zoals weergegeven in de afbeelding. U vindt het openbare IP-adres als een van de resourcetypen die is gemaakt op basis van de resourcegroep met de AKS-knooppunten van de agent en andere netwerkresources.

  ![Azure Machine Learning-service: Beveiligen met SSL-webservices](./media/how-to-secure-web-service/aks-public-ip-address.png)



## <a name="next-steps"></a>Volgende stappen
Leer hoe u het volgende doet:
+ [Een machine learning-model is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
+ [Veilig kunt uitvoeren van experimenten en Deductie in een Azure-netwerk](how-to-enable-virtual-network.md)

