---
title: Azure Service Fabric integreren met API Management | Microsoft Docs
description: Ontdek hoe u snel aan de slag met Azure API Management en Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/13/2017
ms.author: ryanwi
ms.openlocfilehash: 8ff8c425189efdd7ea21984528bf7ea765e17955
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>API Management met Service Fabric implementeren
Deze zelfstudie maakt deel uit twee van een serie. Deze zelfstudie ziet u het instellen van [Azure API Management](../api-management/api-management-key-concepts.md) met Service Fabric om verkeer te leiden naar een back-end-service in Service Fabric.  Wanneer u klaar bent, hebt u API Management geïmplementeerd op een VNET, een API-bewerking voor het verzenden van verkeer naar de back-end-stateless services geconfigureerd. Zie voor meer informatie over Azure API Management-scenario's met Service Fabric, de [overzicht](service-fabric-api-management-overview.md) artikel.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Implementeren van API Management
> * API Management configureren
> * Een API-bewerking maken
> * Een back-end-beleid configureren
> * De API toevoegen aan een product

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure met behulp van een sjabloon
> * API Management met Service Fabric implementeren

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) of [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure
- Als u een Windows-cluster implementeert, kunt u een Windows-ontwikkelomgeving instellen. Installeer [Visual Studio 2017](http://www.visualstudio.com) en de **ontwikkelen van Azure**, **ASP.NET en web ontwikkeling**, en **.NET Core platformoverschrijdende ontwikkeling**werkbelastingen.  Stel een [.NET ontwikkelomgeving](service-fabric-get-started.md).
- Als u een Linux-cluster implementeert, stelt u een Java-ontwikkelomgeving op [Linux](service-fabric-get-started-linux.md) of [Mac OS](service-fabric-get-started-mac.md).  Installeer de [Service Fabric CLI](service-fabric-cli.md). 

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Aanmelden bij Azure en uw abonnement te selecteren
Aanmelden bij uw Azure-account, selecteer uw abonnement voordat u Azure-opdrachten uitvoeren.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-api-management"></a>Implementeren van API Management
Cloud-toepassingen moeten doorgaans een front-gateway voor een potentieel inkomend voor gebruikers, apparaten of andere toepassingen. Een gateway kan in Service Fabric worden alle stateless services zoals een ASP.NET Core toepassing of een andere service die zijn ontworpen voor inkomend verkeer, zoals Event Hubs, IoT-Hub of Azure API Management. Deze zelfstudie is een inleiding tot het gebruik van Azure API Management als een gateway voor uw Service Fabric-toepassingen. API Management wordt rechtstreeks geïntegreerd met Service Fabric, zodat u voor het publiceren van API's met een groot aantal regels voor het doorsturen naar uw back-end Service Fabric-services. 

Nu dat u een veilige hebt [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) implementeren in Azure API Management op het virtuele netwerk (VNET) in het subnet en het NSG aangewezen voor API Management. Voor deze zelfstudie de API Management-Resource Manager-sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u hebt ingesteld in de vorige [Windows cluster-zelfstudie](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux cluster zelfstudie](service-fabric-tutorial-create-vnet-and-linux-cluster.md). 

De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
 
- [APIM.JSON][apim-arm]
- [APIM.parameters.JSON][apim-parameters-arm]

Vul de lege parameters in de `apim.parameters.json` voor uw implementatie.

Het volgende script gebruiken voor het implementeren van de Resource Manager-sjabloon en de parameter-bestanden voor API Management:

```powershell
$ResourceGroupName = "tutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="tutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="configure-api-management"></a>API Management configureren

Zodra uw API Management en Service Fabric-cluster worden geïmplementeerd, kunt u beveiligingsinstellingen en een Service Fabric-back-end kunt configureren in API Management. Hiermee kunt u het beleid van een back-end die verkeer naar uw Service Fabric-cluster verzendt maken.

### <a name="configure-api-management-security"></a>API Management-beveiliging configureren

Voor het configureren van de Service Fabric-back-end, moet u eerst API Management-beveiligingsinstellingen te configureren. Ga naar uw API Management-service in de Azure portal beveiligingsinstellingen voor configureren.

#### <a name="enable-the-api-management-rest-api"></a>De REST-API van de API-beheer inschakelen

De API Management REST API is momenteel de enige manier om een back-endservice configureren. De eerste stap is het inschakelen van de REST-API van API Management en beveilig deze.

 1. Selecteer in de API Management-service **Management API** onder **beveiliging**.
 2. Controleer de **API Management REST API inschakelen** selectievakje.
 3. Opmerking de **Management API URL**, die we later gebruiken voor het instellen van de Service Fabric-back-end.
 4. Genereren een **Access Token** als u een vervaldatum en een sleutel selecteert, klikt u vervolgens op de **genereren** knop naar de onderkant van de pagina.
 5. Kopieer de **toegangstoken** en op te slaan.  We gebruiken het toegangstoken in de volgende stappen. Houd er rekening mee dat deze verschilt van de primaire en secundaire sleutel.

#### <a name="upload-a-service-fabric-client-certificate"></a>Een Service Fabric-client-certificaat uploaden

API Management moet verifiëren met uw Service Fabric-cluster voor detectie van de service met behulp van een clientcertificaat dat toegang tot het cluster heeft. Voor het gemak, deze zelfstudie gebruikt hetzelfde certificaat eerder hebt opgegeven bij het maken van de [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), die standaard kan worden gebruikt voor toegang tot uw cluster.

 1. Selecteer in de API Management-service **clientcertificaten** onder **beveiliging**.
 2. Klik op de **+ toevoegen** knop.
 2. Selecteer het persoonlijke sleutelbestand (.pfx) van het cluster-certificaat dat u hebt opgegeven bij het maken van uw Service Fabric-cluster, een naam geven en het wachtwoord van de persoonlijke sleutel bevatten.

> [!NOTE]
> Deze zelfstudie gebruikt hetzelfde certificaat voor clientverificatie en clusterbeveiliging knooppunt naar. U kunt een afzonderlijke clientcertificaat als u geconfigureerd hebt voor toegang tot uw Service Fabric-cluster.

### <a name="configure-the-backend"></a>De back-end configureren

Nu dat API Management-beveiliging is geconfigureerd, kunt u de Service Fabric-back-end kunt configureren. Voor Service Fabric-back-ends is de Service Fabric-cluster de back-end, in plaats van een specifieke Service Fabric-service. Hierdoor kan een enkele beleidsregel voor het routeren naar meer dan één service in het cluster.

Deze stap is vereist van het toegangstoken dat u eerder hebt gegenereerd en de vingerafdruk voor uw cluster-certificaat dat u eerder hebt geüpload naar API Management.

> [!NOTE]
> Als u een afzonderlijke clientcertificaat in de vorige stap voor API Management gebruikt, moet u de vingerafdruk voor het clientcertificaat naast de certificaatvingerafdruk cluster in deze stap.

Verzenden van de volgende HTTP PUT-aanvraag naar de URL van API Management API u eerder hebt genoteerd bij het inschakelen van de API Management REST API voor het configureren van de Service Fabric-back-endservice. U ziet een `HTTP 201 Created` antwoord als de opdracht is geslaagd. Zie voor meer informatie over elk veld de API Management [back-end voor API-naslagdocumentatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

HTTP-opdracht en URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Headers voor aanvraag:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Hoofdtekst van de aanvraag:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

De **url** parameter hier is een volledig gekwalificeerde servicenaam van een service in het cluster dat alle aanvragen worden doorgestuurd naar standaard als er geen servicenaam is opgegeven in een back-end-beleid. U kunt de naam van een valse service, zoals "fabric: / valse/service ' als u niet van plan bent om een fallback-service.

Verwijzen naar de API Management [back-end voor API-naslagdocumentatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) voor meer informatie over elk veld.

```python
#import requests library for making REST calls
import requests
import json

#specify url
url = 'https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10'

token = "SharedAccessSignature integration&201710140514&Lqnbei7n2Sot6doiNtxMFPUi/m9LsNa+1ZK/PdxqFl49JFWjXh1wW5Gd99r/tIOeHp6dU8UV5iZUdOPfcrm5hg=="

payload = {
    "description": "My Service Fabric backend",
    "url": "fabric:/ApiApplication/ApiWebService",
    "protocol": "http",
    "resourceId": "https://tutorialcluster.eastus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "https://tutorialcluster.eastus.cloudapp.azure.com:19080" ],
            "clientCertificateThumbprint": "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80",
            "serverCertificateThumbprints": [ "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}

headers = {'Authorization': token, "Content-Type": "application/json"}

#Call REST API
response = requests.put(url, data=json.dumps(payload), headers=headers)

#Print Response
print(response.status_code)
print(response.text)
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Een Service Fabric-back-end-service implementeren

Nu dat u de Fabric-Service is geconfigureerd als een back-end voor API Management hebt, kunt u back-end-beleid schrijven voor uw API's die verkeer aan uw Service Fabric-services verzenden. Maar u moet eerst een service die wordt uitgevoerd in Service Fabric om aanvragen te accepteren.  Als u eerder hebt gemaakt een [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md), een .NET Service Fabric-service implementeren.  Als u eerder hebt gemaakt een [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md), een Java Service Fabric-service implementeren.

### <a name="deploy-a-net-service-fabric-service"></a>Een .NET Service Fabric-service implementeren

Voor deze zelfstudie maken we een eenvoudige staatloze ASP.NET Core betrouwbare Service met de standaardsjabloon voor de Web-API-project. Hiermee maakt u een HTTP-eindpunt voor uw service, die u beschikbaar via Azure API Management:

```
/api/values
```

Start Visual Studio als beheerder en maak een ASP.NET Core-service:

 1. In Visual Studio-Selecteer-Bestand > Nieuw Project.
 2. Selecteer de sjabloon Service Fabric-toepassing in de Cloud en geef deze de naam **'ApiApplication'**.
 3. Selecteer de servicesjabloon ASP.NET Core en noem het project **'WebApiService'**.
 4. Selecteer de projectsjabloon Web API ASP.NET Core 1.1.
 5. Zodra het project is gemaakt, opent u `PackageRoot\ServiceManifest.xml` en verwijder de `Port` kenmerk van de eindpuntconfiguratie resource:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Hierdoor kunnen Service Fabric om op te geven van een poort dynamisch van de toepassing-poortbereik wordt geopend via de Netwerkbeveiligingsgroep in het cluster Resource Manager-sjabloon die het verkeer naar deze stromen van API Management.
 
 6. Druk op F5 in Visual Studio om te controleren of de web-API is lokaal beschikbaar. 

    Open Service Fabric Explorer en inzoomen naar beneden op een specifiek exemplaar van de service ASP.NET Core ziet u het basisadres wordt de service luistert op. Voeg `/api/values` naar de basistabel adres en in een browser te openen. Hiermee wordt de Get-methode op de ValuesController in de Web-API-sjabloon. Deze retourneert de standaardreactie die wordt geleverd door de sjabloon, een JSON-matrix die twee tekenreeksen bevat:

    ```json
    ["value1", "value2"]`
    ```

    Dit is het eindpunt dat u via API Management in Azure gebruiken.

 7. Ten slotte de toepassing aan het cluster in Azure implementeren. [Met Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), met de rechtermuisknop op het toepassingsproject en selecteert u **publiceren**. Geef uw clustereindpunt (bijvoorbeeld `mycluster.southcentralus.cloudapp.azure.com:19000`) om de toepassing aan uw Service Fabric-cluster in Azure te implementeren.

Een stateless service met de naam van ASP.NET Core `fabric:/ApiApplication/WebApiService` moet nu worden uitgevoerd in het Service Fabric-cluster in Azure.

### <a name="create-a-java-service-fabric-service"></a>Een Java Service Fabric-service maken
Voor deze zelfstudie implementeert we een eenvoudige webserver die berichten terug naar de gebruiker een echo. De voorbeeldtoepassing echo-server bevat een HTTP-eindpunt voor uw service, die u beschikbaar via Azure API Management.

1. Kloon de Java ophalen voorbeelden is gestart.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   cd service-fabric-java-getting-started
   ```

2. Bewerken *Services/EchoServer/EchoServer1.0/EchoServerApplication/EchoServerPkg/ServiceManifest.xml*. Het eindpunt bijwerken zodat de service op poort 8081 luistert.

   ```xml
   <Endpoint Name="WebEndpoint" Protocol="http" Port="8081" />
   ```

3. Sla *ServiceManifest.xml*, klikt u vervolgens de toepassing EchoServer1.0 wordt opgebouwd.

   ```bash
   cd Services/EchoServer/EchoServer1.0/
   gradle
   ```

4. Implementeer de toepassing aan het cluster.

   ```bash
   cd Scripts
   sfctl cluster select --endpoint http://mycluster.southcentralus.cloudapp.azure.com:19080
   ./install.sh
   ```

   Een Java staatloze service met de naam `fabric:/EchoServerApplication/EchoServerService` moet nu worden uitgevoerd in het Service Fabric-cluster in Azure.

5. Open een browser en typ in http://mycluster.southcentralus.cloudapp.azure.com:8081/getMessage, ziet u "[versie 1.0] Hello World!" weergegeven.


## <a name="create-an-api-operation"></a>Een API-bewerking maken

Nu we gaan maken van een bewerking in API Management waarmee externe clients communiceren met de ASP.NET Core staatloze service in de Service Fabric-cluster wordt uitgevoerd.

1. Aanmelden bij de Azure-portal en navigeer naar uw API Management-service-implementatie.
2. Selecteer in de blade API Management-service **API's**
3. Een nieuwe API toevoegen door te klikken op **+ API**, klikt u vervolgens **leeg API** vak en invullen van het dialoogvenster:

    - **Webservice-URL**: voor Service Fabric-back-ends, deze URL-waarde niet wordt gebruikt. Hier kunt u een willekeurige waarde plaatsen. Gebruik voor deze zelfstudie: 'http://servicefabric'.
    - **Weergavenaam**: Geef een naam op voor uw API. Voor deze zelfstudie gebruiken 'Service Fabric-App'.
    - **Naam**: Voer 'fabric-service-app'.
    - **URL-schema**: Selecteer **HTTP**, **HTTPS**, of **beide**. Gebruik voor deze zelfstudie **beide**.
    - **Achtervoegsel voor API-URL**: Geef een achtervoegsel voor de API. Voor deze zelfstudie gebruiken 'Mijntoep'.
 
4. Selecteer **Service Fabric-App** uit de lijst met API's en klik op **+ toevoegbewerking** toevoegen van een front-API-bewerking. Vul de parameterwaarden:
    
    - **URL**: Selecteer **ophalen** en geef een URL-pad voor de API. Voor deze zelfstudie '/ api/waarden' te gebruiken.  Standaard worden de URL-pad hier opgegeven wordt het URL-pad naar de back-end Service Fabric-service verzonden. Als u de dezelfde URL-pad hier uw service gebruikt, werkt in dit geval "/ api/waarden" en vervolgens de bewerking zonder verdere aanpassing. U kunt ook een URL-pad hier die verschilt van het URL-pad gebruikt door uw back-end Service Fabric-service opgeven in dat geval moet u ook later een herschrijven pad opgeven in het beleid opnieuw.
    - **Weergavenaam**: Geef een naam op voor de API. Voor deze zelfstudie gebruiken "Waarden".

5. Klik op **Opslaan**.

## <a name="configure-a-backend-policy"></a>Een back-end-beleid configureren

Het back-end-beleid bindt alles bij elkaar. Dit is waar het configureren van de Service Fabric-service van het back-end waarnaar aanvragen worden doorgestuurd. U kunt dit beleid toepassen op alle API-bewerkingen. De [back-endconfiguratie voor Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) biedt de volgende vragen routering besturingselementen: 
 - Service-exemplaar selecteren door op te geven van een naam in Service Fabric-service-exemplaar, ofwel hardcoded (bijvoorbeeld `"fabric:/myapp/myservice"`) of gegenereerd op basis van de HTTP-aanvraag (bijvoorbeeld `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Omzetting van de partitie een partitiesleutel met behulp van een Service Fabric-partitieschema te genereren.
 - De selectie van de replica voor stateful services.
 - Oplossing opnieuw voorwaarden waarmee u kunt de voorwaarden opgeven voor een servicelocatie opnieuw op te lossen en het opnieuw verzenden van een aanvraag.

Voor deze zelfstudie maakt u een back-end-beleid waarmee aanvragen rechtstreeks worden doorgestuurd naar de ASP.NET Core staatloze service eerder hebt geïmplementeerd:

 1. Selecteer en bewerken de **inkomende beleidsregels** voor de bewerking van de waarden door te klikken op het bewerkingspictogram en te selecteren **codeweergave**.
 2. Voeg in de beleidseditor code een `set-backend-service` beleid onder inkomende beleidsregels, zoals hier wordt weergegeven en klik op de **opslaan** knop:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

Voor een volledige set kenmerken voor Service Fabric-back-end-beleid, raadpleegt u de [API Management-documentatie voor back-end](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-the-api-to-a-product"></a>De API toevoegen aan een product 

Voordat u de API aanroepen kunt, moet deze worden toegevoegd aan een product waar u toegang aan gebruikers verlenen. 

 1. Selecteer in de API Management-service **producten**.
 2. Standaard API Management providers twee producten: Starter en onbeperkt. Selecteer het onbeperkte product.
 3. Selecteer **+ API's toevoegen**.
 4. Selecteer de `Service Fabric App` API die u hebt gemaakt in de vorige stappen en klik op de **Selecteer** knop.

### <a name="test-it"></a>Testen

U kunt nu een aanvraag verzenden naar uw back-endservice in Service Fabric via API Management rechtstreeks vanuit de Azure-portal.

 1. Selecteer in de API Management-service **API**.
 2. In de **Service Fabric-App** API die u hebt gemaakt in de vorige stappen, selecteer de **Test** tabblad en vervolgens de **waarden** bewerking.
 3. Klik op de **verzenden** knop een testaanvraag verzenden naar de back-endservice.  U ziet een vergelijkbaar met HTTP-antwoord:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Access-Control-Allow-Origin: https://apimanagement.hosting.portal.azure.net

    Access-Control-Allow-Credentials: true

    Access-Control-Expose-Headers: Transfer-Encoding,Date,Server,Vary,Ocp-Apim-Trace-Location

    Ocp-Apim-Trace-Location: https://apimgmtstuvyx3wa3oqhdbwy.blob.core.windows.net/apiinspectorcontainer/RaVVuJBQ9yxtdyH55BAsjQ2-1?sv=2015-07-08&sr=b&sig=Ab6dPyLpTGAU6TdmlEVu32DMfdCXTiKAASUlwSq3jcY%3D&se=2017-09-15T05%3A49%3A53Z&sp=r&traceId=ed9f1f4332e34883a774c34aa899b832

    Date: Thu, 14 Sep 2017 05:49:56 GMT


    [
    "value1",
    "value2"
    ]
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Een cluster bevat de clusterresource zelf én andere Azure-resources. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen.

Aanmelden bij Azure en selecteer de abonnements-ID die u wilt verwijderen van het cluster.  U kunt uw abonnements-ID vinden door de logboekregistratie in de [Azure-portal](http://portal.azure.com). Verwijderen van de resourcegroep en alle clusterbronnen met behulp van de [cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
$ResourceGroupName = "tutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="tutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="conclusion"></a>Conclusie
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Implementeren van API Management
> * API Management configureren
> * Een API-bewerking maken
> * Een back-end-beleid configureren
> * De API toevoegen aan een product

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json