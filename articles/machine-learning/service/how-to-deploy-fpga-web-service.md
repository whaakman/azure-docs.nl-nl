---
title: Het implementeren van een model als een webservice op een FPGA met Azure Machine Learning
description: Informatie over het implementeren van een webservice met een uitgevoerd op een FPGA met Azure Machine Learning-model.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: f3237980a1ad1969b5cf8d42d547ddf96608dd97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33789395"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Een model als een webservice op een FPGA met Azure Machine Learning implementeren

In dit document, leert u hoe uw werkstation-omgeving instellen en implementeren van een model als een webservice op [programmeerbare gate matrices (FPGA) veld](concept-accelerate-with-fpgas.md). De webservice gebruikt Project Brainwave voor het uitvoeren van het model op FPGA.

Met behulp van FPGAs biedt zeer lage latentie inferencing, zelfs met een enkele batchgrootte.

## <a name="create-an-azure-machine-learning-model-management-account"></a>Een Azure Machine Learning-Model Management-account maken

1. Ga naar de pagina voor het Model-Management-Account maken op de [Azure Portal](https://aka.ms/aml-create-mma).

2. In de portal maken van een Model-Management-Account in de **VS-Oost 2** regio.

   ![Afbeelding van het scherm Model Management-Account maken](media/how-to-deploy-fpga-web-service/azure-portal-create-mma.PNG)

3. Geef een naam op voor uw Account Model-Management, kies een abonnement en een resourcegroep kiezen.

   >[!IMPORTANT]
   >Voor de locatie, moet u kiezen **VS-Oost 2** als regio.  Er zijn geen andere regio's zijn momenteel beschikbaar.

4. Kies een prijscategorie (S1 voldoende is, maar ook werken S2 en S3).  De laag DevTest wordt niet ondersteund.  

5. Klik op **Selecteer** om te bevestigen dat de prijscategorie.

6. Klik op **maken** aan de linkerkant op het ML-Model.

## <a name="get-model-management-account-information"></a>Model Management accountgegevens ophalen

Voor informatie over uw Model Management Account (MMA), klikt u op de __Model-Account van beheerserver__ in de Azure portal.

Kopieer de waarden van de volgende items:

+ Model Management accountnaam (in op de linkerbovenhoek)
+ Resourcegroepnaam
+ Abonnements-id
+ Locatie (gebruik ' eastus2')

![Model Management accountgegevens](media/how-to-deploy-fpga-web-service/azure-portal-mma-info.PNG)

## <a name="set-up-your-machine"></a>Instellen van uw computer

Als u uw werkstation voor FPGA implementatie instelt, volg deze stappen:

1. Download en installeer de nieuwste versie van [Git](https://git-scm.com/downloads).

2. Installeer [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

3. Gebruik de volgende opdracht vanaf een opdrachtprompt Git voor het downloaden van de Anaconda-omgeving:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```

4. Voor het maken van de omgeving, opent u een **Anaconda Prompt** (niet een Azure Machine Learning Workbench vragen) en voer de volgende opdracht:

    > [!IMPORTANT]
    > De `environment.yml` bestand bevindt zich in de git-opslagplaats die u in de vorige stap hebt gekloond. Wijzig het pad naar behoefte om te verwijzen naar het bestand op uw werkstation.

    ```
    conda env create -f environment.yml
    ```

5. Gebruik de volgende opdracht voor het activeren van de omgeving:

    ```
    conda activate amlrealtimeai
    ```

6. Gebruik de volgende opdracht voor het starten van de Jupyter-Notebook-server:

    ```
    jupyter notebook
    ```

    De uitvoer van deze opdracht is vergelijkbaar met de volgende tekst:

    ```text
    Copy/paste this URL into your browser when you connect for the first time, to login with a token:
        http://localhost:8888/?token=bb2ce89cc8ae931f5df50f96e3a6badfc826ff4100e78075
    ```

    > [!TIP]
    > U krijgt een ander token telkens wanneer die u de opdracht uitvoert.

    Als uw browser niet automatisch naar de Jupyter-notebook wordt geopend, gebruikt u de HTTP-URL die is geretourneerd door de vorige opdracht om de pagina te openen.

    ![Afbeelding van de webpagina Jupyter-Notebook](./media/how-to-deploy-fpga-web-service/jupyter-notebook.png)

## <a name="deploy-your-model"></a>Implementeer uw model

Open in de Jupyter-Notebook de `00_QuickStart.ipynb` notebook uit de `notebooks/resnet50` directory. Volg de instructies in de notebook naar:

* Definieer de service
* Het model implementeren
* Het geïmplementeerde model gebruiken
* Er zijn geïmplementeerde services verwijderen

> [!IMPORTANT]
> Om te optimaliseren latentie en doorvoer, moet uw werkstation in dezelfde Azure-regio als het eindpunt.  Op dit moment worden de API's gemaakt in de Oost ons Azure-regio.

## <a name="ssltls-and-authentication"></a>SSL/TLS- en verificatie

Azure Machine Learning biedt ondersteuning voor SSL en verificatie op basis van sleutels. Hiermee kunt u toegang tot uw service en beveiligd door clients verzonden gegevens te beperken.

> [!NOTE]
> De stappen in deze sectie zijn alleen van toepassing op Azure Machine Learning Hardware versnelde-modellen. Zie voor standaardservices Azure Machine Learning de [het instellen van SSL op Azure Machine Learning Compute](https://docs.microsoft.com/azure/machine-learning/preview/how-to-setup-ssl-on-mlc) document.

> [!IMPORTANT]
> Verificatie is alleen ingeschakeld voor services die u hebt opgegeven een SSL-certificaat en de sleutel. 
>
> Als u SSL niet inschakelt, kunnen alle gebruikers op internet aanroepen naar de service is mogelijk.
>
> Als u SSL inschakelen en verificatiesleutel vereist is bij het openen van de service.

SSL versleutelt de gegevens die worden verzonden tussen de client en de service. Het wordt ook gebruikt door de client de identiteit van de server.

U kunt een service implementeren met SSL is ingeschakeld, of een reeds geïmplementeerde service zodat deze bijwerken. De stappen zijn hetzelfde:

1. Verkrijgen van een domeinnaam.

2. Een SSL-certificaat verkrijgen.

3. Implementeren of bijwerken van de service met SSL is ingeschakeld.

4. Werk uw DNS om te verwijzen naar de service.

### <a name="acquire-a-domain-name"></a>Verkrijgen van een domeinnaam

Als u een domeinnaam geen al eigenaar, u kunt kopen bij een __domeinnaamregistrar__. Tussen registrars, verschilt het proces als de kosten. De registrar ook biedt u hulpprogramma's voor het beheren van de domeinnaam. Deze hulpprogramma's worden gebruikt voor het toewijzen van een volledig gekwalificeerde domeinnaam (zoals www.contoso.com) het IP-adres dat uw service wordt gehost op.

### <a name="acquire-an-ssl-certificate"></a>Een SSL-certificaat verkrijgen

Er zijn veel manieren om een SSL-certificaat te verkrijgen. De meest voorkomende is om te kopen bij een __certificeringsinstantie__ (CA). Ongeacht waar u het certificaat hebt verkregen, moet u de volgende bestanden:

* Een __certificaat__. Het certificaat moet de volledige certificaatketen bevatten en moet het PEM-gecodeerd zijn.
* Een __sleutel__. De sleutel moet PEM-codering.

> [!TIP]
> Als de certificeringsinstantie niet het certificaat en de sleutel als PEM-gecodeerde bestanden verstrekken, kunt u een hulpprogramma zoals [OpenSSL](https://www.openssl.org/) de indeling wijzigen.

> [!IMPORTANT]
> Zelfondertekende certificaten moeten alleen worden gebruikt voor ontwikkeling. Ze moeten niet worden gebruikt in productie.
>
> Als u een zelfondertekend certificaat gebruikt, raadpleegt u de [gebruikmaakt van de services met zelfondertekende certificaten](#self-signed) sectie voor meer specifieke informatie.

> [!WARNING]
> Wanneer u een certificaat aanvraagt, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het adres dat u wilt gebruiken voor de service opgeven. Bijvoorbeeld: www.contoso.com. Het adres dat is geautoriseerd in het certificaat en het adres dat wordt gebruikt door de clients worden vergeleken bij het valideren van de identiteit van de service.
>
> Als de adressen niet overeenkomen, ontvangt de clients een foutmelding. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Implementeren of bijwerken van de service met SSL is ingeschakeld

Voor het implementeren van de service met SSL is ingeschakeld, stel de `ssl_enabled` -parameter voor `True`. Stel de `ssl_certificate` parameter met de waarde van de __certificaat__ bestand en de `ssl_key` aan de waarde van de __sleutel__ bestand. Het volgende voorbeeld ziet u een service implementeren met SSL is ingeschakeld:

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

Het antwoord bevat ook een __primaire sleutel__ en __secundaire sleutel__ die worden gebruikt voor het verbruik van de service.

### <a name="update-your-dns-to-point-to-the-service"></a>Uw DNS om te verwijzen naar de service bijwerken

Gebruik de hulpprogramma's van uw domeinnaamregistrar bijwerken van de DNS-record voor de domeinnaam. De record moet verwijzen naar het IP-adres van de service.

> [!NOTE]
> Afhankelijk van de registrar, en de tijd te live (TTL) is geconfigureerd voor de domeinnaam, dit kan enkele minuten tot enkele uren duren voordat clients de domeinnaam kunnen oplossen.

### <a name="consuming-authenticated-services"></a>Geverifieerde services gebruiken

De volgende voorbeelden laten zien hoe u een geverifieerde service met behulp van Python en C# gebruiken:

> [!NOTE]
> Vervang `authkey` met de primaire of secundaire sleutel geretourneerd bij het maken van de service.

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

Andere clients gRPC kunnen aanvragen verifiëren door een autorisatie-header. Het algemeen de aanpak is het maken van een `ChannelCredentials` object die combineert `SslCredentials` met `CallCredentials`. Dit is toegevoegd aan de autorisatie-header van de aanvraag. Zie voor meer informatie over het implementeren van ondersteuning voor uw specifieke headers [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

De volgende voorbeelden laten zien hoe u het instellen van de header in C# en gaat u naar:

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

Met behulp van beide methoden zorgt ervoor dat de gRPC het certificaat te gebruiken als het certificaat van de hoofdmap.

> [!IMPORTANT]
> gRPC accepteert geen niet-vertrouwde certificaten. Met behulp van een niet-vertrouwd certificaat mislukt met een `Unavailable` statuscode. De details van de fout bevatten `Connection Failed`.
