---
title: Webservices beveiligen met SSL
titleSuffix: Azure Machine Learning service
description: Meer informatie over het beveiligen van een webservice die is geïmplementeerd met de service Azure Machine Learning met inschakeling van HTTPS. HTTPS beveiligt de gegevens die zijn ingediend door clients die gebruikmaken van transport layer security (TLS), een vervanging voor secure socket Layer (SSL). Dit wordt ook gebruikt door clients om te controleren of de identiteit van de webservice.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 1a6aa75f3d25cd88cd1edb9b2cdcfabc3b4ec8f9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58103890"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>SSL gebruiken voor het beveiligen van webservices met Azure Machine Learning-service

In dit artikel leert u over het beveiligen van een webservice die is geïmplementeerd met de Azure Machine Learning-service. U kunt de toegang beperken tot webservices en beveiligen van de gegevens die zijn ingediend door clients met behulp van [Hypertext Transfer Protocol Secure (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS wordt gebruikt voor het beveiligen van communicatie tussen een client en de webservice door het versleutelen van communicatie tussen de twee. Versleuteling wordt verwerkt met behulp van [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Dit is nog steeds aangeduid als SSL Secure Sockets Layer (), dat de voorloper van TLS is.

> [!TIP]
> De SDK van Azure Machine Learning wordt de term 'SSL' voor eigenschappen die betrekking hebben op het inschakelen van beveiligde communicatie. Dit betekent niet dat TLS wordt niet gebruikt door de webservice, alleen dat SSL is het beter herkenbaar term voor veel lezers.

TLS en SSL beide zijn afhankelijk van __digitale certificaten__, die worden gebruikt om uit te voeren versleuteling en identiteit verifiëren. Zie voor meer informatie over hoe digitale certificaten werk de Wikipedia-vermelding op [openbare-sleutelinfrastructuur (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Als u niet inschakelen en het gebruik van HTTPS voor uw webservice, is het mogelijk dat gegevens die worden verzonden naar en van de service weergegeven bij andere gebruikers op het internet.
>
> HTTPS kan ook de client om te controleren of de authenticiteit van de server waarmee deze verbinding met maakt. Dit beschermt clients op basis van [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) aanvallen.

Het proces van het beveiligen van een nieuwe webservice of een bestaand is als volgt:

1. Een domeinnaam krijgen.

2. Een digitaal certificaat ophalen.

3. Implementeren of bijwerken van de web-service met de SSL-instelling ingeschakeld.

4. Werkt u uw DNS om te verwijzen naar de webservice.

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
