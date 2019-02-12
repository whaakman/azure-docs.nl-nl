---
title: Beveiligen met SSL-webservices
titleSuffix: Azure Machine Learning service
description: Meer informatie over het beveiligen van een webservice die is geïmplementeerd met de Azure Machine Learning-service. U kunt de toegang beperken tot webservices en beveiligen van de gegevens die zijn ingediend door clients met behulp van secure socket Layer (SSL) en verificatie op basis van een sleutel.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 160bc0e67b2686d17357241887a207cb4a03002c
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098099"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>SSL gebruiken voor het beveiligen van webservices met Azure Machine Learning-service

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

+ **Implementeren op veld Programmable Gate Arrays (FPGA)**

  Tijdens het implementeren op FPGA, geef waarden op voor de parameters met betrekking tot SSL zoals wordt weergegeven in het volgende codefragment:

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>Werkt u uw DNS

Vervolgens moet u uw DNS om te verwijzen naar de webservice bijwerken.

+ **Voor ACI**:

  Gebruik de hulpprogramma's van uw domeinnaamregistrar om bij te werken van de DNS-record voor uw domeinnaam. De record moet verwijzen naar het IP-adres van de service.

  Afhankelijk van de registrar, en de tijd naar live (TTL) die is geconfigureerd voor de domeinnaam, duurt het enkele minuten tot enkele uren voordat clients de domeinnaam kunnen omzetten.

+ **Voor AKS**:

  Werk de DNS-server op het tabblad "Configuratie" van het 'openbare IP-adres' van het AKS-cluster, zoals weergegeven in de afbeelding. U vindt het openbare IP-adres als een van de resourcetypen die is gemaakt op basis van de resourcegroep met de AKS-knooppunten van de agent en andere netwerkresources.

  ![Azure Machine Learning-service: Beveiligen met SSL-webservices](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Volgende stappen
Leer hoe u het volgende doet:
+ [Een machine learning-model is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
+ [Experimenten en inferentietaken in een Azure-netwerk veilig uitvoeren](how-to-enable-virtual-network.md)
