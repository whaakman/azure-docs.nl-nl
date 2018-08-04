---
title: Azure Service Fabric reverse proxy-beveiligde communicatie | Microsoft Docs
description: Omgekeerde proxy om in te schakelen veilige end-to-end-communicatie configureren.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: d8a11a3289037602535d1b5727d041e376012bd8
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502437"
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Verbinding maken met een beveiligd service met de omgekeerde proxy

In dit artikel wordt uitgelegd hoe u beveiligde verbinding tussen de reverse proxy- en -services, waardoor u een beveiligd kanaal voor end-to-end. Zie voor meer informatie over omgekeerde proxy's, [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md)

Verbinding maken met veilige services wordt alleen ondersteund als omgekeerde proxy is geconfigureerd om te luisteren op HTTPS. In dit artikel wordt ervan uitgegaan dat dit het geval is.
Raadpleeg [omgekeerde proxy van de instellingen in Azure Service Fabric](service-fabric-reverseproxy-setup.md) de omgekeerde proxy configureren in Service Fabric.

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Beveiligde verbinding tot stand brengen tussen de reverse proxy- en -services 

### <a name="reverse-proxy-authenticating-to-services"></a>Omgekeerde proxy verificatie bij services:
De omgekeerde proxy met services met behulp van het certificaat wordt aangeduid. Voor Azure-clusters die het certificaat wordt opgegeven met ***reverseProxyCertificate*** eigenschap in de [ **Microsoft.ServiceFabric/clusters** ](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) [Resource Typ sectie](../azure-resource-manager/resource-group-authoring-templates.md) van de Resource Manager-sjabloon. Voor zelfstandige clusters, het certificaat is opgegeven met ofwel de ***ReverseProxyCertificate*** of de ***ReverseProxyCertificateCommonNames*** eigenschap in de **Security**sectie van ClusterConfig.json. Zie voor meer informatie, [omgekeerde proxy inschakelen op zelfstandige clusters](service-fabric-reverseproxy-setup.md#enable-reverse-proxy-on-standalone-clusters). 

Services kunnen de logica om te controleren of het certificaat dat door de omgekeerde proxy implementeren. De services kunnen u de details van het goedgekeurde client-certificaat opgeven als configuratie-instellingen in het configuratiepakket. Dit kan worden gelezen tijdens runtime en gebruikt voor het valideren van het certificaat dat door de omgekeerde proxy. Raadpleeg [beheren toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) om toe te voegen van de configuratie-instellingen. 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Omgekeerde proxy van de service-identiteit via het certificaat dat door de service te controleren:
Omgekeerde proxy biedt ondersteuning voor de volgende beleidsregels voor het uitvoeren van validatie van het servercertificaat van de certificaten die door services: None, ServiceCommonNameAndIssuer en ServiceCertificateThumbprints.
Schakel het beleid voor omgekeerde proxy te gebruiken, geef de **ApplicationCertificateValidationPolicy** in de **ApplicationGateway/Http** sectie onder [instelling fabricSettings](service-fabric-cluster-fabric-settings.md).

De volgende sectie bevat informatie over de configuratie voor elk van deze opties.

### <a name="service-certificate-validation-options"></a>Validatieopties voor service-certificaat 

- **Geen**: Reverse proxy-verificatie van het certificaat voor via proxy overslaan en de beveiligde verbinding tot stand brengt. Dit is de standaardinstelling.
Geef de **ApplicationCertificateValidationPolicy** met waarde **geen** in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "None"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCommonNameAndIssuer**: omgekeerde proxy controleert of het certificaat dat door de service op basis van de algemene naam van het certificaat en de vingerafdruk van de directe verlener: Geef de **ApplicationCertificateValidationPolicy** met waarde **ServiceCommonNameAndIssuer** in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCommonNameAndIssuer"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Als u wilt instellen van de lijst met algemene naam van service en de vingerafdrukken van de uitgever van een [ **ApplicationGateway/Http/ServiceCommonNameAndIssuer** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttpservicecommonnameandissuer) sectie onder **instelling fabricSettings**, zoals hieronder wordt weergegeven. Meerdere certificaat algemene en -verlenersleutel vingerafdruk paren kunnen worden toegevoegd de **parameters** matrix. 

   Als de omgekeerde proxy eindpunt maakt verbinding met geeft een certificaat die gebruikt wordt en -verlenersleutel vingerafdruk overeenkomt met een van de waarden die u hier opgeeft, SSL-kanaal tot stand is gebracht. 
   Omgekeerde proxy mislukt als de mislukt zodat deze overeenkomen met de details van het certificaat van de client-aanvraag met een 502 (ongeldige Gateway)-statuscode. De regel van de HTTP-status bevat ook de zin "Ongeldige SSL-certificaat." 

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
               "parameters": [
                 {
                   "name": "WinFabric-Test-Certificate-CN1",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
                 },
                 {
                   "name": "WinFabric-Test-Certificate-CN2",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCertificateThumbprints**: omgekeerde proxy controleert of het via proxy op basis van de vingerafdruk van certificaat. U kunt kiezen waarmee u deze route wanneer de services die zijn geconfigureerd met de zelf-ondertekende certificaten: Geef de **ApplicationCertificateValidationPolicy** met waarde **ServiceCertificateThumbprints** in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCertificateThumbprints"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Geef ook de vingerafdrukken met een **ServiceCertificateThumbprints** vermelding in de **ApplicationGateway/Http** sectie. Meerdere vingerafdrukken kunnen worden opgegeven als een door komma's gescheiden lijst in het waardeveld, zoals hieronder wordt weergegeven:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                   ...
                 {
                   "name": "ServiceCertificateThumbprints",
                   "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Als de vingerafdruk van het servercertificaat wordt weergegeven in dit configuratie-item, slaagt de omgekeerde proxy de SSL-verbinding. Anders wordt de verbinding beëindigd en mislukt de aanvraag van de client met een 502 (ongeldige Gateway). De regel van de HTTP-status bevat ook de zin "Ongeldige SSL-certificaat."

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Eindpunt selectie logica als services stellen zowel beveiligd als niet-beveiligde eindpunten beschikbaar
Service fabric ondersteunt het configureren van meerdere eindpunten voor een service. Zie voor meer informatie, [bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md).

Omgekeerde proxy selecteert u een van de eindpunten voor het doorsturen van de aanvraag op basis van de **ListenerName** queryparameter in de [service-URI](./service-fabric-reverseproxy.md#uri-format-for-addressing-services-by-using-the-reverse-proxy). Als de **ListenerName** parameter niet wordt opgegeven, omgekeerde proxy kan een willekeurig eindpunt uit de lijst met eindpunten kiezen. Afhankelijk van de eindpunten zijn geconfigureerd voor de service, kan het eindpunt dat is geselecteerd een eindpunt HTTP of HTTPS zijn. Mogelijk zijn er scenario's of vereisten waar u de omgekeerde proxy voor het werken in een 'alleen beveiligde modus'; dat wil zeggen, wilt u niet dat de beveiligde omgekeerde proxy voor het doorsturen van aanvragen voor niet-beveiligde eindpunten. Omgekeerde proxy instellen op de modus alleen beveiligen te geven de **SecureOnlyMode** configuratie-item met de waarde **waar** in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> [!NOTE]
> Als het wordt uitgevoerd **SecureOnlyMode**, als een client heeft aangegeven dat een **ListenerName** omgekeerde proxy overeenkomt met een eindpunt HTTP(unsecured), mislukt de aanvraag met een 404 (niet gevonden) HTTP-statuscode.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>Instellen van verificatie van clientcertificaten via de omgekeerde proxy
SSL-beëindiging doet zich voor in de omgekeerde proxy en alle gegevens van de client-certificaat is verbroken. Voor de services uit te voeren verificatie van clientcertificaten, geeft u de **ForwardClientCertificate** instellen in de [ **ApplicationGateway/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) sectie.

1. Wanneer **ForwardClientCertificate** is ingesteld op **false**, omgekeerde proxy wordt niet certificaat van de client tijdens de SSL-handshake aanvraag met de client.
Dit is de standaardinstelling.

2. Wanneer **ForwardClientCertificate** is ingesteld op **waar**, omgekeerde proxy ondersteuningsaanvragen certificaat van de client tijdens de SSL-handshake voor de client.
Het doorsturen aan de client vervolgens de gegevens in een aangepaste HTTP-header met de naam van certificaat **X-clientcertificaat**. De headerwaarde is de tekenreeks met base64 gecodeerde PEM-indeling van het certificaat van de client. De service kan slagen/mislukt de aanvraag met de juiste statuscode na het inspecteren van gegevens van het certificaat.
Als de client geen TLV-een certificaat, stuurt een lege header reverse proxy- en kan de service verwerkt de aanvraag.

> [!NOTE]
> Omgekeerde proxy is een louter doorstuurserver. Een validatie van het certificaat van de client wordt niet uitgevoerd.


## <a name="next-steps"></a>Volgende stappen
* [Instellen en configureren van omgekeerde proxy op een cluster](service-fabric-reverseproxy-setup.md).
* Raadpleeg [de omgekeerde proxy configureren voor het verbinding maken met veilige services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) voor Azure Resource Manager voorbeeldsjablonen configureren beveiligen omgekeerde proxy met het certificaat voor verschillende validatieopties voor.
* Bekijk een voorbeeld van HTTP-communicatie tussen services in een [voorbeeldproject op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Externe procedureaanroepen met externe communicatie Reliable Services](service-fabric-reliable-services-communication-remoting.md)
* [Web-API die gebruikmaakt van OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)
