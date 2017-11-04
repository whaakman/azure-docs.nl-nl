---
title: Azure Service Fabric met API Management snel aan de slag | Microsoft Docs
description: Deze handleiding wordt beschreven hoe u snel aan de slag met Azure API Management en Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 2969834713fc7c2f1a2e281a6c988158d803dc45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric met Azure API Management snel starten

Deze handleiding wordt beschreven hoe u Azure API Management met Service Fabric instellen en configureren van uw eerste API-bewerking voor het verzenden van verkeer naar de back-endservices in Service Fabric. Zie voor meer informatie over Azure API Management-scenario's met Service Fabric, de [overzicht](service-fabric-api-management-overview.md) artikel. 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a>API Management en Service Fabric implementeren in Azure

De eerste stap is het implementeren van API Management en een Service Fabric-cluster in Azure in een gedeelde virtuele netwerk. Hiermee kunt API Management voor directe communicatie met Service Fabric zodat het detectie-service, service partitie resolutie en voorwaarts-verkeer rechtstreeks naar een back-endservice in Service Fabric kunt uitvoeren.

### <a name="topology"></a>Topologie

Deze handleiding implementeert u de volgende topologie naar Azure waarin API Management en Service Fabric in de subnetten van hetzelfde virtuele netwerk zijn:

 ![Een bijschrift][sf-apim-topology-overview]

Om snel aan de slag, zijn voor elke stap van de implementatie van Resource Manager-sjablonen beschikbaar:

 - Netwerktopologie:
    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]
 - Service Fabric-cluster:
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]
 - API Management:
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

### <a name="sign-in-to-azure-and-select-your-subscription"></a>Aanmelden bij Azure en uw abonnement te selecteren

Maakt gebruik van deze handleiding [Azure PowerShell][azure-powershell]. Wanneer u een nieuwe PowerShell-sessie start, zich aanmelden bij uw Azure-account en uw abonnement te selecteren voordat u Azure-opdrachten uitvoeren.
 
Meld u bij uw Azure-account:

```powershell
Login-AzureRmAccount
```

Selecteer uw abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een nieuwe resourcegroep voor uw implementatie. Geef deze een naam en een locatie.

```powershell
New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a>De netwerktopologie implementeren

De eerste stap is het instellen van de netwerktopologie die API Management en de Service Fabric-cluster wordt geïmplementeerd. De [network.json] [ network-arm] Resource Manager-sjabloon is geconfigureerd voor het maken van een virtueel netwerk (VNET) met twee subnetten en twee Netwerkbeveiligingsgroep groepen (NSG). 

De [network.parameters.json] [ network-parameters-arm] parameterbestand bevat de namen van de subnetten en nsg's die API Management en Service Fabric om te worden geïmplementeerd. Voor deze handleiding hoeft de parameterwaarden die niet worden gewijzigd. De sjablonen API Management en Service Fabric-Resource Manager gebruikt deze waarden, zodat ze hier zijn gewijzigd, moeten ze worden gewijzigd in de Resource Manager-sjablonen dienovereenkomstig. 

 1. De volgende Resource Manager-sjabloon en parameters bestand te downloaden:

    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]

 2. Gebruik de volgende PowerShell-opdracht de Resource Manager-sjabloon en de parameterbestanden bestanden voor de netwerkinstallatie van de te implementeren:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a>De Service Fabric-cluster implementeren

Zodra de netwerkbronnen hebt implementeren, wordt de volgende stap is een Service Fabric-cluster implementeren voor het VNET in het subnet en NSG aangewezen voor de Service Fabric-cluster. Voor deze zelfstudie zijn de Service Fabric-Resource Manager-sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u in de vorige stap hebt ingesteld. 

De Service Fabric-cluster Resource Manager-sjabloon is geconfigureerd voor het maken van een beveiligde cluster met Certificaatbeveiliging. Het certificaat wordt gebruikt voor het beveiligen van communicatie van knooppunt naar voor uw cluster en voor het beheren van de gebruikerstoegang tot uw Service Fabric-cluster. API Management gebruikt dit certificaat voor toegang tot de Service Fabric Naming Service voor de servicedetectie van de.

Deze stap is vereist dat u een certificaat in de Sleutelkluis voor clusterbeveiliging. Zie voor meer informatie over het instellen van een beveiligde cluster met Sleutelkluis [in deze handleiding over het maken van een cluster in Azure Resource Manager gebruiken](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> U kunt Azure Active Directory-verificatie naast het certificaat dat wordt gebruikt voor toegang tot cluster toevoegen. Azure Active Directory is de aanbevolen manier om de gebruikerstoegang beheren voor uw Service Fabric-cluster, maar is niet nodig om deze zelfstudie te voltooien. Een certificaat is vereist in beide gevallen voor cluster knooppunt naar beveiliging en Azure API Management-verificatie op dit moment biedt geen ondersteuning voor verificatie met Azure Active Directory voor een Service Fabric-back-end.

 1. De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]

 2. Vul de lege parameters in de `cluster.parameters.json` bestand voor uw implementatie, inclusief de [Sleutelkluis informatie](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) voor uw cluster-certificaat.

 3. Gebruik de volgende PowerShell-opdracht voor het implementeren van de Resource Manager-sjabloon en de parameterbestanden bestanden om de Service Fabric-cluster te maken:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Implementeren van API Management

Ten slotte API Management te implementeren naar het VNET in het subnet en NSG die is aangewezen voor API Management. U hoeft niet te wachten op de implementatie van de Service Fabric-cluster te voltooien voordat u implementeert API Management. 

Voor deze zelfstudie zijn de API Management-Resource Manager-sjabloon is vooraf geconfigureerd voor het gebruik van de namen van de VNET, subnet en NSG die u in de vorige stap hebt ingesteld. 

 1. De volgende Resource Manager-sjabloon en parameters bestand te downloaden:
 
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

 2. Vul de lege parameters in de `apim.parameters.json` voor uw implementatie.

 3. Gebruik de volgende PowerShell-opdracht voor het implementeren van de Resource Manager-sjabloon en de parameter-bestanden voor API Management:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>API Management configureren

Zodra uw API Management en Service Fabric-cluster worden geïmplementeerd, kunt u een Service Fabric-back-end kunt configureren in API Management. Hiermee kunt u het beleid van een back-end die verkeer naar uw Service Fabric-cluster verzendt maken.

### <a name="api-management-security"></a>Beveiliging van API Management

Voor het configureren van de Service Fabric-back-end, moet u eerst API Management-beveiligingsinstellingen te configureren. Ga naar uw API Management-service in de Azure portal beveiligingsinstellingen voor configureren.

#### <a name="enable-the-api-management-rest-api"></a>De REST-API van de API-beheer inschakelen

De API Management REST API is momenteel de enige manier om een back-endservice configureren. De eerste stap is het inschakelen van de REST-API van API Management en beveilig deze.

 1. Selecteer in de API Management-service **Management-API - voorbeeld** onder **beveiliging**.
 2. Controleer de **API Management REST API inschakelen** selectievakje.
 3. De URL van de API Management Opmerking: dit is de URL die we later gebruiken voor het instellen van de Service Fabric-back-end
 4. Genereren een **toegangstoken** als u een vervaldatum en een sleutel selecteert, klikt u vervolgens op de **genereren** knop naar de onderkant van de pagina.
 5. Kopieer de **toegangstoken** en sla deze - kun je in de volgende stappen. Houd er rekening mee dat deze verschilt van de primaire en secundaire sleutel.

#### <a name="upload-a-service-fabric-client-certificate"></a>Een Service Fabric-client-certificaat uploaden

API Management moet verifiëren met uw Service Fabric-cluster voor detectie van de service met behulp van een clientcertificaat dat toegang tot het cluster heeft. In deze zelfstudie gebruikt voor het gemak, hetzelfde certificaat opgegeven bij het maken van de Service Fabric-cluster, die standaard kan worden gebruikt voor toegang tot uw cluster.

 1. Selecteer in de API Management-service **clientcertificaten - PREVIEW** onder **beveiliging**.
 2. Klik op de **+ toevoegen** knop
 2. Selecteer het persoonlijke sleutelbestand (.pfx) van het cluster-certificaat dat u hebt opgegeven bij het maken van uw Service Fabric-cluster, een naam geven en het wachtwoord van de persoonlijke sleutel bevatten.

> [!NOTE]
> Deze zelfstudie gebruikt hetzelfde certificaat voor clientverificatie en clusterbeveiliging knooppunt naar. U kunt een afzonderlijke clientcertificaat als u geconfigureerd hebt voor toegang tot uw Service Fabric-cluster.

### <a name="configure-the-backend"></a>De back-end configureren

Nu dat API Management-beveiliging is geconfigureerd, kunt u de Service Fabric-back-end kunt configureren. Voor Service Fabric-back-ends is de Service Fabric-cluster de back-end, in plaats van een specifieke Service Fabric-service. Hierdoor kan een enkele beleidsregel voor het routeren naar meer dan één service in het cluster.

Deze stap is vereist van het toegangstoken dat u eerder hebt gegenereerd en de vingerafdruk voor uw cluster-certificaat dat u hebt geüpload naar de API Management in de vorige stap.

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

De **url** parameter hier is een volledig gekwalificeerde servicenaam van een service in het cluster dat alle aanvragen worden doorgestuurd naar standaard als er geen servicenaam is opgegeven in een back-end-beleid. U kunt de naam van een valse service, zoals "fabric: / valse/service ' als u niet van plan bent om een fallback-service. Rekening mee de **url** moet de indeling "fabric: / app/service ' zelfs als deze een valse terugval service.

Verwijzen naar de API Management [back-end voor API-naslagdocumentatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) voor meer informatie over elk veld.

#### <a name="example"></a>Voorbeeld

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Een Service Fabric-back-end-service implementeren

Nu dat u de Fabric-Service is geconfigureerd als een back-end voor API Management hebt, kunt u back-end-beleid schrijven voor uw API's die verkeer aan uw Service Fabric-services verzenden. Maar u moet eerst een service die wordt uitgevoerd in Service Fabric om aanvragen te accepteren.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Een Service Fabric-service met een HTTP-eindpunt maken

Voor deze zelfstudie maken we een eenvoudige staatloze ASP.NET Core betrouwbare Service met de standaardsjabloon voor de Web-API-project. Hiermee maakt u een HTTP-eindpunt voor uw service, die u via Azure API Management gebruiken zult:

```
/api/values
```

Start [instellen van uw ontwikkelomgeving voor het ontwikkelen van ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Zodra uw ontwikkelomgeving is ingesteld, start Visual Studio als beheerder en maak een ASP.NET Core-service:

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

    Dit is het eindpunt dat u via API Management in Azure gebruiken zult.

 7. Ten slotte de toepassing aan het cluster in Azure implementeren. [Met Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), met de rechtermuisknop op het toepassingsproject en selecteert u **publiceren**. Geef uw clustereindpunt (bijvoorbeeld `mycluster.westus.cloudapp.azure.com:19000`) om de toepassing aan uw Service Fabric-cluster in Azure te implementeren.

Een stateless service met de naam van ASP.NET Core `fabric:/ApiApplication/WebApiService` moet nu worden uitgevoerd in het Service Fabric-cluster in Azure.

## <a name="create-an-api-operation"></a>Een API-bewerking maken

Nu we gaan maken van een bewerking in API Management waarmee externe clients communiceren met de ASP.NET Core staatloze service in de Service Fabric-cluster wordt uitgevoerd.

 1. Aanmelden bij de Azure-portal en navigeer naar uw API Management-service-implementatie.
 2. Selecteer in de blade API Management-service **-API's - Preview**
 3. Een nieuwe API toevoegen door te klikken op de **leeg API** vak en invullen van het dialoogvenster:

     - **Webservice-URL**: voor Service Fabric-back-ends, deze URL-waarde niet wordt gebruikt. Hier kunt u een willekeurige waarde plaatsen. Gebruik voor deze zelfstudie: `http://servicefabric`.
     - **Naam**: Geef een naam op voor uw API. Gebruik voor deze zelfstudie `Service Fabric App`.
     - **URL-schema**: Selecteer HTTP, HTTPS of beide. Gebruik voor deze zelfstudie `both`.
     - **Achtervoegsel voor API-URL**: Geef een achtervoegsel voor de API. Gebruik voor deze zelfstudie `myapp`.
 
 4. Zodra de API is gemaakt, klikt u op **+ toevoegbewerking** toevoegen van een front-API-bewerking. Vul de parameterwaarden:
    
     - **URL**: Selecteer `GET` en geef een URL-pad voor de API. Gebruik voor deze zelfstudie `/api/values`.
     
       Standaard worden de URL-pad hier opgegeven wordt het URL-pad naar de back-end Service Fabric-service verzonden. Als u de dezelfde URL-pad hier uw service in dit geval gebruikt `/api/values`, klikt u vervolgens de bewerking werkt zonder verdere aanpassing. U kunt ook een URL-pad hier die verschilt van het URL-pad gebruikt door uw back-end Service Fabric-service opgeven, in welk geval u moet ook later een herschrijven pad opgeven in het beleid opnieuw.
     - **Weergavenaam**: Geef een naam op voor de API. Gebruik voor deze zelfstudie `Values`.

## <a name="configure-a-backend-policy"></a>Een back-end-beleid configureren

Het back-end-beleid bindt alles bij elkaar. Dit is waar het configureren van de Service Fabric-service van het back-end waarnaar aanvragen worden doorgestuurd. U kunt dit beleid toepassen op alle API-bewerkingen. De [back-endconfiguratie voor Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) biedt de volgende vragen routering besturingselementen: 
 - Service-exemplaar selecteren door op te geven van een naam in Service Fabric-service-exemplaar, ofwel hardcoded (bijvoorbeeld `"fabric:/myapp/myservice"`) of gegenereerd op basis van de HTTP-aanvraag (bijvoorbeeld `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Omzetting van de partitie een partitiesleutel met behulp van een Service Fabric-partitieschema te genereren.
 - De selectie van de replica voor stateful services.
 - Oplossing opnieuw voorwaarden waarmee u kunt de voorwaarden opgeven voor een servicelocatie opnieuw op te lossen en het opnieuw verzenden van een aanvraag.

Voor deze zelfstudie maakt u een back-end-beleid waarmee aanvragen rechtstreeks worden doorgestuurd naar de ASP.NET Core staatloze service eerder hebt geïmplementeerd:

 1. Selecteren en te bewerken de **inkomende beleidsregels** voor de `Values` bewerking door te klikken op het bewerkingspictogram en vervolgens te klikken op **codeweergave**.
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

### <a name="add-the-api-to-a-product"></a>De API toevoegen aan een product. 

Voordat u de API aanroepen kunt, moet deze worden toegevoegd aan een product waar u toegang aan gebruikers verlenen. 

 1. Selecteer in de API Management-service **producten - PREVIEW**.
 2. Standaard API Management providers twee producten: Starter en onbeperkt. Selecteer het onbeperkte product.
 3. Selecteer de API's.
 4. Klik op de **+ toevoegen** knop.
 5. Selecteer de `Service Fabric App` API die u hebt gemaakt in de vorige stappen en klik op de **Selecteer** knop.

### <a name="test-it"></a>Testen

U kunt nu een aanvraag verzenden naar uw back-endservice in Service Fabric via API Management rechtstreeks vanuit de Azure-portal.

 1. Selecteer in de API Management-service **API - voorbeeld**.
 2. In de `Service Fabric App` API die u hebt gemaakt in de vorige stappen, selecteer de **Test** tabblad.
 3. Klik op de **verzenden** knop een testaanvraag verzenden naar de back-endservice.

## <a name="next-steps"></a>Volgende stappen

U hebt op dit moment een basisinstellingen met Service Fabric en API Management.

Deze zelfstudie maakt gebruik van verificatie op basis van certificaten basisgebruiker voor uw Service Fabric-cluster weer om u snel instellen. Meer geavanceerde gebruikersverificatie voor uw Service Fabric-cluster heeft de voorkeur boven met [Azure Active Directory-verificatie](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Vervolgens [maken en configureren van geavanceerde productinstellingen in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) voorbereiden van uw toepassing op concrete verkeer.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
