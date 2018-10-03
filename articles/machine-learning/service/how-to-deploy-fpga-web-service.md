---
title: Een model implementeren als een webservice op een FPGA met Azure Machine Learning
description: Leer hoe u een webservice implementeren met een model uitvoeren op een FPGA met Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 10/01/2018
ms.openlocfilehash: df6637f1a52b679ba9ad0a49fb37d4e4b72f35e4
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237820"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Een model implementeren als een webservice op een FPGA met Azure Machine Learning

U kunt een model implementeren als een webservice op [programmable gate arrays (FPGA's) veld](concept-accelerate-with-fpgas.md).  Met behulp van FPGA's biedt zeer lage latentie inferentietaken, zelfs met een enkele batchgrootte.   

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- U moet aanvragen en worden goedgekeurd voor FPGA quotum. Vul het aanvraagformulier voor de quota voor het aanvragen van toegang: https://aka.ms/aml-real-time-ai

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.
 
  - Uw werkruimte moet zich in de *VS-Oost 2* regio.

  - Installeer de contrib extra's:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Uw model maakt en implementeert
Een pijplijn maken voor de invoer parametriseer-installatiekopie met behulp van ResNet 50 op een FPGA voorverwerken, en voer vervolgens de functies via een classifer getraind op de ImageNet-gegevensset.

Volg de instructies voor:

* De pijplijn model definiëren
* Het model implementeren
* De geïmplementeerde model gebruiken
* Geïmplementeerde services verwijderen

> [!IMPORTANT]
> Voor het optimaliseren van latentie en doorvoer, moet uw client zich in dezelfde Azure-regio als het eindpunt.  Op dit moment worden de API's gemaakt in de oostelijke VS Azure-regio.



### <a name="preprocess-image"></a>Afbeelding voorverwerken
De eerste fase van de pijplijn is met de voorverwerking van de installatiekopieën.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Featurizer toevoegen
Initialiseren van het model en een TensorFlow-controlepunt van de quantized versie van ResNet50 moet worden gebruikt als een featurizer downloaden.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Classificatie toevoegen
Deze classificatie is getraind op de ImageNet-gegevensset.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>De servicedefinitie van de maken
Nu dat u gedefinieerd, hebt de voorverwerking van de installatiekopie, featurizer en classificatie die wordt uitgevoerd op de service, kunt u de servicedefinitie van een kunt maken. De servicedefinitie van de is een set bestanden die zijn gegenereerd op basis van het model dat is geïmplementeerd naar de FPGA-service. Definitie van de service bestaat uit een pijplijn. De pijplijn is een reeks fasen die in volgorde worden uitgevoerd.  TensorFlow-fasen, Keras-fasen en BrainWave fasen worden ondersteund.  De fasen worden uitgevoerd in volgorde van de service, met de uitvoer van de invoer van elke fase in de volgende fase.

Maken van een fase TensorFlow, een sessie met de grafiek (in dit geval standaard grafiek wordt gebruikt) en de invoer en uitvoer tensors naar deze fase.  Deze informatie wordt gebruikt om op te slaan van de grafiek, zodat deze kan worden uitgevoerd op de service.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Model implementeren
Een service maken vanuit definitie van de service.  Uw werkruimte moet zich in de locatie VS-Oost 2.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>De service testen
Een afbeelding verzenden naar de API en testen van het antwoord, een toewijzing van de uitvoer klasse-ID aan de naam van de klasse ImageNet toevoegen.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Uw service aanroepen en de naam van het bestand 'uw image.jpg' hieronder vervangen door een installatiekopie vanaf uw computer. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Opschonen van de service
De service verwijderen.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Beveiligde FPGA-webservices

Azure Machine Learning-modellen die worden uitgevoerd op FPGA's bieden ondersteuning voor SSL en verificatie op basis van een sleutel. Hiermee kunt u toegang tot uw service en beveiligd door clients verzonden gegevens te beperken.

> [!IMPORTANT]
> Verificatie is alleen ingeschakeld voor services die een SSL-certificaat en de sleutel hebt opgegeven. 
>
> Als u SSL niet inschakelt, kunnen elke gebruiker op het internet voor aanroepen naar de service wordt mogelijk.
>
> Als u SSL inschakelt en de verificatiesleutel vereist is bij het openen van de service.

SSL versleutelt de gegevens die tussen de client en de service worden verzonden. Het wordt ook gebruikt door de client om te controleren of de identiteit van de server.

U kunt een service implementeren met SSL is ingeschakeld of bijwerken van een reeds geïmplementeerde service als u wilt inschakelen. De stappen zijn hetzelfde:

1. De naam van een domein aan te schaffen.

2. Een SSL-certificaat aan te schaffen.

3. Implementeren of bijwerken van de service met SSL is ingeschakeld.

4. Hiermee werkt u uw DNS-server om te verwijzen naar de service.

### <a name="acquire-a-domain-name"></a>De naam van een domein verkrijgen

Als u een domeinnaam niet al hebt kan, kunt u kopen van een __domeinnaamregistrar__. Tussen registrars, verschilt het proces als de kosten. De registrar biedt u ook met hulpprogramma's voor het beheren van de domeinnaam. Deze hulpprogramma's worden gebruikt om een volledig gekwalificeerde domeinnaam (zoals www.contoso.com) toewijzen aan de IP-adres dat uw service wordt gehost op.

### <a name="acquire-an-ssl-certificate"></a>Een SSL-certificaat verkrijgen

Er zijn veel manieren om op te halen van een SSL-certificaat. De meest voorkomende reden is het kopen van een __certificeringsinstantie__ (CA). Ongeacht waar u het certificaat hebt verkregen, moet u de volgende bestanden:

* Een __certificaat__. Het certificaat moet de volledige certificaatketen bevatten en moet PEM gecodeerd.
* Een __sleutel__. De sleutel moet PEM gecodeerd.

> [!TIP]
> Als de certificeringsinstantie kan niet het certificaat en sleutel als PEM-gecodeerde bestanden opgeeft, kunt u een hulpprogramma zoals [OpenSSL](https://www.openssl.org/) om de opmaak te wijzigen.

> [!IMPORTANT]
> Zelfondertekende certificaten moeten worden gebruikt alleen voor ontwikkeling. Ze moeten niet worden gebruikt in de productieomgeving.
>
> Als u een zelfondertekend certificaat gebruikt, raadpleegt u de [gebruikmaakt van de services met zelfondertekende certificaten](#self-signed) sectie voor specifieke instructies.

> [!WARNING]
> Wanneer u een certificaat aanvraagt, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het adres dat u van plan bent te gebruiken voor de service opgeven. Bijvoorbeeld: www.contoso.com. Adres van de tijdstempel in het certificaat en het adres dat wordt gebruikt door de clients worden vergeleken bij het valideren van de identiteit van de service.
>
> Als de adressen niet overeenkomen, ontvangt de clients een fout. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Implementeren of bijwerken van de service met SSL ingeschakeld

Voor het implementeren van de service met SSL is ingeschakeld, stel de `ssl_enabled` parameter `True`. Stelt de `ssl_certificate` parameter met de waarde van de __certificaat__ bestand en de `ssl_key` aan de waarde van de __sleutel__ bestand. Het volgende voorbeeld ziet u een service implementeren met SSL is ingeschakeld:

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

Het antwoord van de `create_service` bewerking bevat het IP-adres van de service. Het IP-adres wordt gebruikt bij het toewijzen van de DNS-naam naar het IP-adres van de service.

Het antwoord bevat ook een __primaire sleutel__ en __secundaire sleutel__ die worden gebruikt voor het gebruik van de service.

### <a name="update-your-dns-to-point-to-the-service"></a>Werkt u uw DNS om te verwijzen naar de service

Gebruik de hulpprogramma's van uw domeinnaamregistrar om bij te werken van de DNS-record voor uw domeinnaam. De record moet verwijzen naar het IP-adres van de service.

> [!NOTE]
> Afhankelijk van de registrar, en de tijd naar live (TTL) die is geconfigureerd voor de domeinnaam, duurt het enkele minuten tot enkele uren voordat clients de domeinnaam kunnen omzetten.

### <a name="consume-authenticated-services"></a>Geverifieerde services gebruiken

De volgende voorbeelden ziet u hoe u gebruik van een geverifieerde service met behulp van Python en C#:

> [!NOTE]
> Vervang `authkey` geretourneerd met de primaire of secundaire sleutel bij het maken van de service.

```python
from amlrealtimeai import PredictionClient
client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
image_file = R'C:\path_to_file\image.jpg'
results = client.score_image(image_file)
```

```csharp
var client = new ScoringClient(host, 50051, useSSL, "authKey");
float[,] result;
using (var content = File.OpenRead(image))
    {
        IScoringRequest request = new ImageRequest(content);
        result = client.Score<float[,]>(request);
    }
```

Andere clients gRPC kunnen aanvragen verifiëren door in te stellen van een autorisatie-header. De algemene aanpak is het maken van een `ChannelCredentials` -object dat combineert `SslCredentials` met `CallCredentials`. Dit wordt toegevoegd aan de autorisatie-header van de aanvraag. Zie voor meer informatie over het implementeren van ondersteuning voor uw specifieke headers [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

De volgende voorbeelden ziet u hoe het instellen van de koptekst in C# en gaat u naar:

```csharp
creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                      async (context, metadata) =>
                      {
                          metadata.Add(new Metadata.Entry("authorization", "authKey"));
                          await Task.CompletedTask;
                      }));

```

```go
conn, err := grpc.Dial(serverAddr, 
    grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
    grpc.WithPerRPCCredentials(&authCreds{
    Key: "authKey"}))

type authCreds struct {
    Key string
}

func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
    return map[string]string{
        "authorization": c.Key,
    }, nil
}

func (c *authCreds) RequireTransportSecurity() bool {
    return true
}
```

### <a id="self-signed"></a>Gebruikmaakt van de services met zelfondertekende certificaten

Er zijn twee manieren om te zorgen dat de client om te verifiëren met een server die is beveiligd met een zelfondertekend certificaat:

* Stel op het clientsysteem de `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` omgevingsvariabele op het clientsysteem om te verwijzen naar het certificaatbestand.

* Bij het maken van een `SslCredentials` object, de inhoud van het certificaatbestand doorgeven aan de constructor.

Met behulp van een van beide methoden zorgt ervoor dat de gRPC gebruik van het certificaat als het basiscertificaat.

> [!IMPORTANT]
> gRPC accepteert geen niet-vertrouwde certificaten. Met behulp van een niet-vertrouwd certificaat mislukken met een `Unavailable` statuscode. De details van de fout bevatten `Connection Failed`.

## <a name="sample-notebook"></a>Voorbeeld-notebook

Concepten in dit artikel wordt uitgelegd in de `project-brainwave/project-brainwave-quickstart.ipynb` notebook.

Dit notitieblok ophalen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
