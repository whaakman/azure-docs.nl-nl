---
title: Azure Service Fabric reverse proxy veilige communicatie | Microsoft Docs
description: Configureer reverse proxy zodat veilige end-to-end communicatie.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: 568f9638c59282bcd7d3fae058a1588a889c22dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Verbinding maken met een beveiligde service met de omgekeerde proxy

In dit artikel wordt uitgelegd hoe beveiligde verbinding tussen de omgekeerde proxy en services, waardoor u een beveiligd kanaal voor end-to-end.

Verbinding maken met veilige services wordt alleen ondersteund als omgekeerde proxy is geconfigureerd om te luisteren op HTTPS. De rest van het document wordt ervan uitgegaan dat dit het geval is.
Raadpleeg [omgekeerde proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) voor het configureren van de omgekeerde proxy in Service Fabric.

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Beveiligde verbinding tot stand brengen tussen de omgekeerde proxy en services 

### <a name="reverse-proxy-authenticating-to-services"></a>Omgekeerde proxy verificatie bij services:
De omgekeerde proxy identificeert zichzelf op services met behulp van een certificaat opgegeven met ***reverseProxyCertificate*** eigenschap in de **Cluster** [resourcesectie type](../azure-resource-manager/resource-group-authoring-templates.md). De methode om te controleren of het certificaat voor de omgekeerde proxy kunnen implementeren met Services. De services kunnen u de certificaatgegevens geaccepteerde client opgeven als configuratie-instellingen in het configuratiepakket. Dit kan worden gelezen tijdens runtime en gebruikt voor het valideren van het certificaat voor de omgekeerde proxy. Raadpleeg [beheren toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) de configuratieinstellingen toe te voegen. 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Omgekeerde proxy van de service-identiteit via het certificaat dat is doorgegeven door de service te controleren:
Als u de validatie van het servercertificaat van de certificaten die worden aangeboden door de services, omgekeerde proxy biedt ondersteuning voor een van de volgende opties: None, ServiceCommonNameAndIssuer en ServiceCertificateThumbprints.
U kunt een van deze drie opties selecteren de **ApplicationCertificateValidationPolicy** in het gedeelte parameters van ApplicationGateway/Http-element onder [fabricSettings](service-fabric-cluster-fabric-settings.md).

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

Raadpleeg de volgende sectie voor meer informatie over aanvullende configuratie voor elk van deze opties.

### <a name="service-certificate-validation-options"></a>Validatie van opties voor service-certificaat 

- **Geen**: omgekeerde proxy overslaat verificatie van het certificaat via Proxy-service en de beveiligde verbinding tot stand brengt. Dit is de standaardinstelling.
Geef de **ApplicationCertificateValidationPolicy** met waarde **geen** in het gedeelte parameters van ApplicationGateway/HTTP-element.

- **ServiceCommonNameAndIssuer**: omgekeerde proxy controleert of het certificaat dat is doorgegeven door de service op basis van de algemene naam van het certificaat en de directe verlener vingerafdruk: Geef de **ApplicationCertificateValidationPolicy** met waarde **ServiceCommonNameAndIssuer** in het gedeelte parameters van ApplicationGateway/HTTP-element.

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

Als u wilt instellen van de lijst met algemene naam van de service en de vingerafdrukken van de verlener een **Http-ApplicationGateway/ServiceCommonNameAndIssuer** element onder fabricSettings, zoals hieronder wordt weergegeven. Meerdere algemene naam van certificaat en verlener vingerafdruk paren kunnen worden toegevoegd in het element van de matrix parameters. 

Als de omgekeerde proxy eindpunt maakt verbinding met geeft een certificaat die algemene naam en de verlener vingerafdruk overeenkomt met een van de waarden die u hier opgeeft, SSL-kanaal tot stand is gebracht. Omgekeerde proxy mislukt bij een fout aan de certificaatgegevens van de client-aanvraag met statuscode 502 (ongeldige Gateway). De HTTP-statusregel bevat ook de woorden "Ongeldig SSL-certificaat." 

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


- **ServiceCertificateThumbprints**: omgekeerde proxy controleert of de via proxy servicecertificaat op basis van de vingerafdruk. U kunt deze route voor wanneer de services worden geconfigureerd met een zelf-ondertekende certificaten gaat: Geef de **ApplicationCertificateValidationPolicy** met waarde **ServiceCertificateThumbprints** in het gedeelte parameters van ApplicationGateway/HTTP-element.

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

Geef ook de vingerafdrukken met een **ServiceCertificateThumbprints** item onder sectie opdrachtregelparameters van ApplicationGateway/HTTP-element. Meerdere vingerafdrukken kunnen worden opgegeven als een door komma's gescheiden lijst in het waardeveld, zoals hieronder wordt weergegeven:

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

Als de vingerafdruk van het servercertificaat wordt vermeld in dit configuratie-item, lukt de omgekeerde proxy de SSL-verbinding. Anders wordt de verbinding verbroken en mislukt de aanvraag van de client met een 502 (ongeldige Gateway). De HTTP-statusregel bevat ook de woorden "Ongeldig SSL-certificaat."

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Eindpunt selectie logica wanneer services beveiligd, evenals een onbeveiligde eindpunten weergeven
Service fabric ondersteunt meerdere eindpunten voor een service te configureren. Zie [Specify resources in a service manifest](service-fabric-service-manifest-resources.md) (Bronnen opgeven in een servicemanifest).

Omgekeerde proxy selecteert één van de eindpunten voor het doorsturen van de aanvraag op basis van de **ListenerName** queryparameter. Als dit niet is opgegeven, kan het kiezen van een willekeurig eindpunt in de lijst met eindpunten. Dit is nu een HTTP of HTTPS-eindpunt. Er zijn mogelijk scenario's / vereisten waar u de omgekeerde proxy werkt met een 'beveiligde modus alleen', Internet Explorer u wilt niet dat de beveiligde omgekeerde proxy voor het doorsturen van aanvragen voor niet-beveiligde eindpunten. Dit kan worden bereikt door op te geven de **SecureOnlyMode** configuratie-item met de waarde **true** in het gedeelte parameters van ApplicationGateway/HTTP-element.   

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

> 
> Als in **SecureOnlyMode**, als client heeft aangegeven dat een **ListenerName** omgekeerde proxy overeenkomt met een eindpunt HTTP(unsecured), mislukt de aanvraag met statuscode 404 HTTP (niet gevonden).

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>Instellen van de verificatie van clientcertificaten via de omgekeerde proxy
SSL-beëindiging vindt plaats op de omgekeerde proxy en alle gegevens van de client-certificaat is verloren gegaan. Voor de services voor het uitvoeren van de verificatie van clientcertificaten, stelt u de **ForwardClientCertificate** instellen in de sectie opdrachtregelparameters van ApplicationGateway/HTTP-element.

1. Wanneer **ForwardClientCertificate** is ingesteld op **false**, reverse proxy wordt geen aanvragen voor het clientcertificaat tijdens de SSL-handshake met de client.
Dit is de standaardinstelling.

2. Wanneer **ForwardClientCertificate** is ingesteld op **true**, reverse Proxyaanvragen voor het certificaat van de client tijdens de SSL-handshake met de client.
Deze vervolgens stuurt de client gegevens van het certificaat in een aangepaste HTTP-header met de naam **X-clientcertificaat**. De headerwaarde is de tekenreeks base64-gecodeerd PEM-indeling van het certificaat van de client. De service kan slagen/mislukken de aanvraag met de juiste statuscode nadat u de gegevens van het certificaat hebt.
Als de client heeft een certificaat niet aanwezig is omgekeerde proxy stuurt de header van een leeg en kan de service die de aanvraag te verwerken.

> Omgekeerde proxy is een alleen-doorstuurserver. Een validatie van het certificaat van de client wordt niet uitgevoerd.


## <a name="next-steps"></a>Volgende stappen
* Raadpleeg [configureren omgekeerde proxy voor het verbinding maken met veilige services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) voor Azure Resource Manager voorbeelden van de sjabloon voor het configureren van secure reverse proxy gebruikt met het certificaat voor verschillende validatieopties voor.
* Een voorbeeld bekijken van HTTP-communicatie tussen services in een [voorbeeldproject op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Externe procedureaanroepen weer dat met Reliable Services voor externe toegang](service-fabric-reliable-services-communication-remoting.md)
* [Web-API die gebruikmaakt van OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)
