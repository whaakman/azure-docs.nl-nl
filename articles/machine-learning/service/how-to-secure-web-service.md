---
title: Azure Machine Learning-webservices met SSL beveiligde
description: Meer informatie over het beveiligen van een webservice die is geïmplementeerd met de Azure Machine Learning-service. U kunt de toegang beperken tot webservices en beveiligen van de gegevens die zijn ingediend door clients met behulp van secure socket Layer (SSL) en verificatie op basis van een sleutel.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: a9dee2d88c986ed292567a18c29cd325833b5e6f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832867"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Azure Machine Learning-webservices met SSL beveiligde

In dit artikel leert u over het beveiligen van een webservice die is geïmplementeerd met de Azure Machine Learning-service. U kunt de toegang beperken tot webservices en beveiligen van de gegevens die zijn ingediend door clients met behulp van secure socket Layer (SSL) en verificatie op basis van een sleutel.

> [!Warning]
> Als u SSL niet inschakelt, kunnen elke gebruiker op het internet voor aanroepen met de webservice wordt mogelijk.

SSL versleuteld worden verzonden tussen de client en de webservice. Het wordt ook gebruikt door de client om te controleren of de identiteit van de server. Verificatie is alleen ingeschakeld voor services die een SSL-certificaat en de sleutel hebt opgegeven.  Als u SSL inschakelt, is een verificatiesleutel vereist bij het openen van de webservice.

Of u een webservice die met SSL ingeschakeld implementeert of u SSL voor bestaande geïmplementeerde webservice inschakelen, zijn de stappen hetzelfde:

1. Een domeinnaam krijgen.

2. Een SSL-certificaat ophalen.

3. Implementeren of bijwerken van de web-service met de SSL-instelling ingeschakeld.

4. Werkt u uw DNS om te verwijzen naar de webservice.

Er zijn kleine verschillen bij het beveiligen van webservices tussen de [implementatiedoelen](how-to-deploy-and-where.md). 

## <a name="get-a-domain-name"></a>Een domeinnaam krijgen

Als u een domeinnaam niet al hebt kan, kunt u kopen van een __domeinnaamregistrar__. Tussen registrars, verschilt het proces als de kosten. De registrar biedt u ook met hulpprogramma's voor het beheren van de domeinnaam. Deze hulpprogramma's worden gebruikt om een volledig gekwalificeerde domeinnaam (zoals www.contoso.com) toewijzen aan het IP-adres die als host fungeert voor de webservice.

## <a name="get-an-ssl-certificate"></a>Een SSL-certificaat ophalen

Er zijn veel manieren om op te halen van een SSL-certificaat. De meest voorkomende reden is het kopen van een __certificeringsinstantie__ (CA). Ongeacht waar u het certificaat hebt verkregen, moet u de volgende bestanden:

* Een __certificaat__. Het certificaat moet de volledige certificaatketen bevatten en moet PEM gecodeerd.
* Een __sleutel__. De sleutel moet PEM gecodeerd.

Wanneer u een certificaat aanvraagt, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het adres dat u wilt gebruiken voor de webservice opgeven. Bijvoorbeeld: www.contoso.com. Adres van de tijdstempel in het certificaat en het adres dat wordt gebruikt door de clients worden vergeleken bij het valideren van de identiteit van de webservice. Als de adressen niet overeenkomen, ontvangt de clients een fout. 

> [!TIP]
> Als de certificeringsinstantie kan niet het certificaat en sleutel als PEM-gecodeerde bestanden opgeeft, kunt u een hulpprogramma zoals [OpenSSL](https://www.openssl.org/) om de opmaak te wijzigen.

> [!WARNING]
> Zelfondertekende certificaten moeten worden gebruikt alleen voor ontwikkeling. Ze moeten niet worden gebruikt in de productieomgeving. Zelfondertekende certificaten kunnen leiden tot problemen in uw client toepassingen. Zie voor meer informatie de documentatie voor de netwerkbibliotheken die wordt gebruikt in uw clienttoepassing.

## <a name="enable-ssl-and-deploy"></a>SSL inschakelen en implementeren

Als u wilt implementeren (of opnieuw implementeren) op de service met SSL is ingeschakeld, stel de `ssl_enabled` parameter `True`, gebruikt u waar van toepassing. Stelt de `ssl_certificate` parameter met de waarde van de __certificaat__ bestand en de `ssl_key` aan de waarde van de __sleutel__ bestand. 

+ **Implementeren op Azure Kubernetes Service (AKS)**
  
  Tijdens het inrichten van het AKS-cluster, geef waarden op voor parameters met betrekking tot SSL zoals wordt weergegeven in het volgende codefragment:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Op Azure Container Instances (ACI) implementeren**
 
  Tijdens de implementatie naar ACI, geef waarden op voor parameters met betrekking tot SSL zoals wordt weergegeven in het volgende codefragment:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementeren op veld Programmable Gate Arrays (FPGA's)**

  Het antwoord van de `create_service` bewerking bevat het IP-adres van de service. Het IP-adres wordt gebruikt bij het toewijzen van de DNS-naam naar het IP-adres van de service. Het antwoord bevat ook een __primaire sleutel__ en __secundaire sleutel__ die worden gebruikt voor het gebruik van de service. Geef waarden op voor parameters met betrekking tot SSL zoals wordt weergegeven in het volgende codefragment:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Werkt u uw DNS

Vervolgens moet u uw DNS om te verwijzen naar de webservice bijwerken.

+ **Voor ACI en FPGA**:  

  Gebruik de hulpprogramma's van uw domeinnaamregistrar om bij te werken van de DNS-record voor uw domeinnaam. De record moet verwijzen naar het IP-adres van de service.  

  Afhankelijk van de registrar, en de tijd naar live (TTL) die is geconfigureerd voor de domeinnaam, duurt het enkele minuten tot enkele uren voordat clients de domeinnaam kunnen omzetten.

+ **Voor AKS**: 

  Werk de DNS-server op het tabblad "Configuratie" van het 'openbare IP-adres' van het AKS-cluster, zoals weergegeven in de afbeelding. U vindt het openbare IP-adres als een van de resourcetypen die is gemaakt op basis van de resourcegroep met de AKS-knooppunten van de agent en andere netwerkresources.

  ![Azure Machine Learning-service: beveiligen met SSL-webservices](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [verbruiken een ML-Model is geïmplementeerd als een webservice](how-to-consume-web-service.md).