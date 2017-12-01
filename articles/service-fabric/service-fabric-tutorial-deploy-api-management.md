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
ms.date: 11/10/2017
ms.author: ryanwi
ms.openlocfilehash: da1f2c3170aba9dc13d77a8729a98e7b655edea8
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>API Management met Service Fabric implementeren
Deze zelfstudie is een vierde deel van een reeks.  Implementatie van Azure API Management met Service Fabric is een geavanceerd scenario.  API Management is handig als u nodig hebt voor het publiceren van API's met een groot aantal routeringsregels voor uw back-end Service Fabric-services. Cloud-toepassingen moeten doorgaans een front-gateway voor een potentieel inkomend voor gebruikers, apparaten of andere toepassingen. Een gateway kan alle stateless services die zijn bestemd voor inkomend verkeer zoals een APP.NET Core toepassing, Event Hubs, IoT-Hub of Azure API Management zijn in Service Fabric. 

Deze zelfstudie ziet u het instellen van [Azure API Management](../api-management/api-management-key-concepts.md) met Service Fabric om verkeer te leiden naar een back-end-service in Service Fabric.  Wanneer u klaar bent, hebt u API Management geïmplementeerd op een VNET, een API-bewerking voor het verzenden van verkeer naar de back-end-stateless services geconfigureerd. Zie voor meer informatie over Azure API Management-scenario's met Service Fabric, de [overzicht](service-fabric-api-management-overview.md) artikel.

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
> * [Een in- of -cluster schalen](/service-fabric-tutorial-scale-cluster.md)
> * [Upgrade de runtime van een cluster](service-fabric-tutorial-upgrade-cluster.md)
> * API Management met Service Fabric implementeren

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) of [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure
- Als u een Windows-cluster implementeert, kunt u een Windows-ontwikkelomgeving instellen. Installeer [Visual Studio 2017](http://www.visualstudio.com) en de **ontwikkelen van Azure**, **ASP.NET en web ontwikkeling**, en **.NET Core platformoverschrijdende ontwikkeling**werkbelastingen.  Stel een [.NET ontwikkelomgeving](service-fabric-get-started.md).
- Als u een Linux-cluster implementeert, stelt u een Java-ontwikkelomgeving op [Linux](service-fabric-get-started-linux.md) of [Mac OS](service-fabric-get-started-mac.md).  Installeer de [Service Fabric CLI](service-fabric-cli.md). 

## <a name="network-topology"></a>Netwerktopologie
Nu dat u een veilige hebt [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) implementeren in Azure API Management op het virtuele netwerk (VNET) in het subnet en het NSG aangewezen voor API Management. Voor deze zelfstudie de API Management-Resource Manager-sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u hebt ingesteld in de vorige [Windows cluster-zelfstudie](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux cluster zelfstudie](service-fabric-tutorial-create-vnet-and-linux-cluster.md). Deze zelfstudie implementeert u de volgende topologie naar Azure waarin API Management en Service Fabric in de subnetten van hetzelfde virtuele netwerk zijn:

 ![Een bijschrift][sf-apim-topology-overview]

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

## <a name="deploy-a-service-fabric-back-end-service"></a>Een Service Fabric-back-end-service implementeren

Voordat u API Management om verkeer te routeren naar een Service Fabric-back-end-service configureert, moet u eerst een actieve service aanvragen worden geaccepteerd.  Als u eerder hebt gemaakt een [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md), een .NET Service Fabric-service implementeren.  Als u eerder hebt gemaakt een [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md), een Java Service Fabric-service implementeren.

### <a name="deploy-a-net-service-fabric-service"></a>Een .NET Service Fabric-service implementeren

Voor deze zelfstudie een basic staatloze ASP.NET Core betrouwbare Service maken met behulp van de standaardsjabloon voor de Web-API-project. Hiermee maakt u een HTTP-eindpunt voor uw service, die u beschikbaar via Azure API Management.

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

    Verwijderen van de poort kan Service Fabric om op te geven van een poort dynamisch van het toepassingspoortbereik geopend via de Netwerkbeveiligingsgroep in het cluster Resource Manager-sjabloon die het verkeer naar deze stromen van API Management.
 
 6. Druk op F5 in Visual Studio om te controleren of de web-API is lokaal beschikbaar. 

    Open Service Fabric Explorer en inzoomen naar beneden op een specifiek exemplaar van de service ASP.NET Core ziet u het basisadres wordt de service luistert op. Voeg `/api/values` naar de basistabel adres en geopend in een browser, die de Get-methode op de ValuesController in de sjabloon voor de Web-API aanroept. Deze retourneert de standaardreactie die wordt geleverd door de sjabloon, een JSON-matrix die twee tekenreeksen bevat:

    ```json
    ["value1", "value2"]`
    ```

    Dit is het eindpunt dat u via API Management in Azure gebruiken.

 7. Ten slotte de toepassing aan het cluster in Azure implementeren. In Visual Studio met de rechtermuisknop op het toepassingsproject en selecteer **publiceren**. Geef uw clustereindpunt (bijvoorbeeld `mycluster.southcentralus.cloudapp.azure.com:19000`) om de toepassing aan uw Service Fabric-cluster in Azure te implementeren.

Een stateless service met de naam van ASP.NET Core `fabric:/ApiApplication/WebApiService` moet nu worden uitgevoerd in het Service Fabric-cluster in Azure.

### <a name="create-a-java-service-fabric-service"></a>Een Java Service Fabric-service maken
Voor deze zelfstudie implementeert een eenvoudige webserver, een echo back-berichten naar de gebruiker. De voorbeeldtoepassing echo-server bevat een HTTP-eindpunt voor uw service, die u beschikbaar via Azure API Management.

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

## <a name="download-and-understand-the-resource-manager-template"></a>De Resource Manager-sjabloon downloaden en begrijpen
Download en sla de volgende Resource Manager-sjabloon en de parameters-bestand:
 
- [APIM.JSON][apim-arm]
- [APIM.parameters.JSON][apim-parameters-arm]

De volgende secties worden de resources die worden gedefinieerd als de *apim.json* sjabloon. Volg de koppelingen naar de sjabloon-naslagdocumentatie in elke sectie voor meer informatie. De configureerbare gedefinieerde parameters in de *apim.parameters.json* parameterbestand verderop in dit artikel zijn ingesteld.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) beschrijft het service-exemplaar van API Management: naam, SKU of laag locatie voor resourcegroep, informatie over de uitgever en virtueel netwerk.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates
[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) API Management-beveiliging configureert. API Management moet verifiëren met uw Service Fabric-cluster voor detectie van de service met behulp van een clientcertificaat dat toegang tot het cluster heeft. Deze zelfstudie wordt gebruikgemaakt van hetzelfde certificaat eerder hebt opgegeven bij het maken van de [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), die standaard kan worden gebruikt voor toegang tot uw cluster. 

Deze zelfstudie gebruikt hetzelfde certificaat voor clientverificatie en clusterbeveiliging knooppunt naar. U kunt een afzonderlijke clientcertificaat als u geconfigureerd hebt voor toegang tot uw Service Fabric-cluster. Geef de **naam**, **wachtwoord**, en **gegevens** (base-64 gecodeerde tekenreeks) van het persoonlijke sleutelbestand (.pfx) van het cluster-certificaat dat u hebt opgegeven bij het maken van uw Service Fabric-cluster.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends
[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) beschrijving van de back-endservice verkeer wordt doorgestuurd naar. 

Voor Service Fabric-back-ends is de Service Fabric-cluster de back-end in plaats van een specifieke Service Fabric-service. Hierdoor kan een enkele beleidsregel voor het routeren naar meer dan één service in het cluster. De **url** veld hier is een volledig gekwalificeerde servicenaam van een service in het cluster dat alle aanvragen worden doorgestuurd naar standaard als er geen servicenaam is opgegeven in een back-end-beleid. U kunt de naam van een valse service, zoals "fabric: / valse/service ' als u niet van plan bent om een fallback-service. **resourceId** Hiermee geeft u het cluster management-eindpunt.  **clientCertificateThumbprint** en **serverCertificateThumbprints** certificaten gebruikt voor verificatie met het cluster te identificeren.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products
[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) maakt u een product. In Azure API Management zijn een product bevat een of meer API's, evenals een gebruiksquotum en de gebruiksvoorwaarden. Zodra een product is gepubliceerd, kunnen ontwikkelaars abonneren op het product en begint met het gebruik van API's van het product. 

Voer een beschrijvende **displayName** en **beschrijving** voor het product. Een abonnement is vereist maar abonnement goedkeuring door een beheerder is niet in deze zelfstudie.  Dit product **status** is 'gepubliceerd' en zichtbaar is voor abonnees. 

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis
[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) maakt u een API. Een API Management-API vertegenwoordigt een reeks bewerkingen die kunnen worden aangeroepen door clienttoepassingen. Nadat de bewerkingen zijn toegevoegd, wordt de API is toegevoegd aan een product en kan worden gepubliceerd. Als een API is gepubliceerd, kunt u het abonnement op en gebruikt door ontwikkelaars.

- **displayName** mag een naam voor uw API. Voor deze zelfstudie gebruiken 'Service Fabric-App'.
- **naam** biedt een unieke en beschrijvende naam voor de API, zoals 'fabric-service-app'. Deze wordt weergegeven in de portals ontwikkelaars en uitgever. 
- **serviceUrl** verwijst naar de HTTP-service voor het implementeren van de API. API management verzendt aanvragen naar dit adres. De waarde van deze URL wordt niet gebruikt voor Service Fabric-back-ends. Hier kunt u een willekeurige waarde plaatsen. Voor deze zelfstudie, bijvoorbeeld 'http://servicefabric'. 
- **pad** wordt toegevoegd aan de basis-URL voor de API management-service. De basis-URL is gemeenschappelijk voor alle API's die worden gehost door een exemplaar van API Management-service. API Management API's onderscheidt door hun achtervoegsel en daarom het achtervoegsel moet uniek zijn voor elke API voor een opgegeven uitgever. 
- **protocollen** bepalen welke protocollen kunnen worden gebruikt voor toegang tot de API. Voor deze zelfstudie lijst **http** en **https**.
- **pad** is een achtervoegsel voor de API. Voor deze zelfstudie gebruiken 'Mijntoep'.

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations
[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) voordat een API in API Management kunnen worden gebruikt, bewerkingen moeten worden toegevoegd aan de API.  Externe clients gebruiken een bewerking om te communiceren met de ASP.NET Core staatloze service in de Service Fabric-cluster wordt uitgevoerd.

Een front-API als bewerking wilt toevoegen, vult u de waarden:

- **displayName** en **beschrijving** beschrijven. Voor deze zelfstudie gebruiken "Waarden".
- **methode** Hiermee geeft u de HTTP-term.  Geef voor deze zelfstudie **ophalen**.
- **urlTemplate** wordt toegevoegd aan de basis-URL van de API en identificeert één HTTP-bewerking.  Gebruik voor deze zelfstudie `/api/values` als u de .NET-back-endservice toegevoegd of `getMessage` als u de back-endservice Java hebt toegevoegd.  Standaard worden de URL-pad hier opgegeven wordt het URL-pad naar de back-end Service Fabric-service verzonden. Als u de dezelfde URL-pad hier dat uw service gebruikt, zoals '/ api/waarden voor' gebruikt vervolgens werkt de bewerking zonder verdere aanpassing. U kunt ook een URL-pad hier die verschilt van het URL-pad gebruikt door uw back-end Service Fabric-service opgeven in dat geval moet u ook later een herschrijven pad opgeven in het beleid opnieuw.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies
[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) maakt een back-end-beleid, dat alles bij elkaar verbindt. Dit is waar het configureren van de Service Fabric-service van het back-end waarnaar aanvragen worden doorgestuurd. U kunt dit beleid toepassen op alle API-bewerkingen.  Zie voor meer informatie [overzicht beleidsregels](/azure/api-management/api-management-howto-policies). 

De [back-endconfiguratie voor Service Fabric](/azure/api-management/api-management-transformation-policies#SetBackendService) biedt de volgende vragen routering besturingselementen: 
 - Service-exemplaar selecteren door op te geven van een naam in Service Fabric-service-exemplaar, ofwel hardcoded (bijvoorbeeld `"fabric:/myapp/myservice"`) of gegenereerd op basis van de HTTP-aanvraag (bijvoorbeeld `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Omzetting van de partitie een partitiesleutel met behulp van een Service Fabric-partitieschema te genereren.
 - De selectie van de replica voor stateful services.
 - Oplossing opnieuw voorwaarden waarmee u kunt de voorwaarden opgeven voor een servicelocatie opnieuw op te lossen en het opnieuw verzenden van een aanvraag.

**policyContent** is de Json escape-teken XML-inhoud van het beleid.  Maak een back-end-beleid op route-aanvragen rechtstreeks naar de .NET- of Java staatloze service eerder hebt geïmplementeerd voor deze zelfstudie. Voeg een `set-backend-service` beleid onder inkomende beleidsregels.  "Service-name" vervangen door `fabric:/ApiApplication/WebApiService` als u de .NET-back-end-service hebt geïmplementeerd of `fabric:/EchoServerApplication/EchoServerService` als u de Java-service hebt geïmplementeerd.
    
```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

## <a name="set-parameters-and-deploy-api-management"></a>Parameters instellen en implementeren van API Management
Vul in de volgende lege parameters in de *apim.parameters.json* voor uw implementatie. 

|Parameter|Waarde|
|---|---|
|apimInstanceName|SF apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Developer|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|CertificatePassword|q6D7nN %6ck@6| 
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;Base-64 gecodeerde tekenreeks&gt;|
|url_path|/ api/waarden|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.Azure.com:19080|
|inbound_policy|&lt;XML-tekenreeks&gt;|

*certificatePassword* en *serviceFabricCertificateThumbprint* moet overeenkomen met het cluster-certificaat gebruikt voor het instellen van het cluster.  

*serviceFabricCertificate* is van het certificaat als een Base64-gecodeerde tekenreeks, die kan worden gegenereerd door het volgende script:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

In *inbound_policy*, vervangen door 'servicenaam' `fabric:/ApiApplication/WebApiService` als u de .NET-back-end-service hebt geïmplementeerd of `fabric:/EchoServerApplication/EchoServerService` als u de Java-service hebt geïmplementeerd.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

Het volgende script gebruiken voor het implementeren van de Resource Manager-sjabloon en de parameter-bestanden voor API Management:

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="test-it"></a>Testen

U kunt nu een aanvraag verzenden naar uw back-endservice in Service Fabric via API Management rechtstreeks vanuit de [Azure-portal](https://portal.azure.com).

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
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen
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

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
