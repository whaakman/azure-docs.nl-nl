---
title: Naslaginformatie over Azure Service Fabric NET Maven | Microsoft Docs
description: De verwijzing voor het gebruik van de Maven-invoegtoepassing voor Service Fabric-NET bevat
services: service-fabric-mesh
keywords: maven, java, cli
author: suhuruli
ms.author: suhuruli
ms.date: 11/26/2018
ms.topic: reference
ms.service: service-fabric-mesh
manager: subramar
ms.openlocfilehash: 08e842f5b91bd0ca5f8e8b2a7866f3f9a689ac28
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999000"
---
# <a name="maven-plugin-for-service-fabric-mesh"></a>Maven-invoegtoepassing voor Service Fabric Mesh

## <a name="prerequisites"></a>Vereisten

- Java-SDK
- Maven
- Azure CLI met mesh-extensie
- Service Fabric-CLI

## <a name="goals"></a>Doelstellingen

### `azure-sfmesh:init`
- Hiermee maakt u een `servicefabric` map met een `appresources` map met de `application.yaml` bestand. 

### `azure-sfmesh:addservice`
- Hiermee maakt u een map in `servicefabric` map met de naam van de service en wordt gemaakt van de service YAML-bestand. 

### `azure-sfmesh:addnetwork`
- Genereert een `network` YAML met de naam van het opgegeven netwerk in de `appresources` map 

### `azure-sfmesh:addgateway`
- Genereert een `gateway` YAML met de gatewaynaam van de opgegeven in de `appresources` map 

### `azure-sfmesh:addsecret`
- Genereert een `secret` YAML met de naam van de opgegeven geheim in de `appresources` map 

### `azure-sfmesh:addsecretvalue`
- Genereert een `secretvalue` YAML met de naam van de opgegeven geheim en de geheime waarde in de `appresources` map 

### `azure-sfmesh:deploy`
- Voegt de yamls uit de `servicefabric` map en maakt u een Azure Resource Manager-sjabloon JSON in de huidige map.
- Implementeert alle resources in de omgeving net van Azure Service Fabric 

### `azure-sfmesh:deploytocluster`
- Hiermee maakt u een map (`meshDeploy`) die de implementatie betreffende JSON's gegenereerd op basis van yamls die van toepassing zijn voor Service Fabric-clusters bevat
- Alle resources implementeert in de Service Fabric-cluster
 

## <a name="usage"></a>Gebruik

Voor het gebruik van de Maven-invoegtoepassing in uw Maven-Java-app, moet u het volgende codefragment toevoegen aan het pom.xml-bestand:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
      </plugin>
    </plugins>
  </build>
</project>
```

## <a name="common-configuration"></a>Algemene configuratie

De Maven-invoegtoepassing op dit moment biedt geen ondersteuning voor algemene configuraties van Maven-invoegtoepassingen voor Azure.

## <a name="how-to"></a>Instructies

### <a name="initialize-maven-project-for-azure-service-fabric-mesh"></a>Maven-project voor Azure Service Fabric NET initialiseren
Voer de volgende opdracht om de toepassing resource YAML-bestand te maken.

```cmd
mvn azure-sfmesh:init -DapplicationName=helloworldserver
```

- Hiermee maakt u een map met de naam `servicefabric->appresources` in de hoofdmap waarin een toepassing YAML met de naam `app_helloworldserver`

### <a name="add-resource-to-your-application"></a>Resource toevoegen aan uw toepassing

#### <a name="add-a-new-network-to-your-application"></a>Een nieuw netwerk toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een netwerk resource yaml. 

```cmd
mvn azure-sfmesh:addnetwork -DnetworkName=helloworldservicenetwork -DnetworkAddressPrefix=10.0.0.4/22
```

- Hiermee maakt u een YAML-netwerk in de map `servicefabric->appresources` met de naam `network_helloworldservicenetwork`

#### <a name="add-a-new-service-to-your-application"></a>Een nieuwe service toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een service-yaml. 

```cmd
mvn azure-sfmesh:addservice -DapplicationName=helloworldserver -DserviceName=helloworldservice -DimageName=helloworldserver:latest -DlistenerPort=8080 -DnetworkRef=helloworldservicenetwork
```

- Hiermee maakt u een YAML-service in de map `servicefabric->helloworldservice` met de naam `service_helloworldservice` die verwijst naar de `helloworldservicenetwork` & de `helloworldserver` app
- De service wilt luisteren op poort 8080
- De service zou doen met ***helloworldserver:latest*** zoals deze wordt containerinstallatiekopie.

#### <a name="add-a-new-gateway-resource-to-your-application"></a>Een nieuwe gateway-resource toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een gateway resource yaml. 

```cmd
mvn azure-sfmesh:addgateway -DapplicationName=helloworldserver -DdestinationNetwork=helloworldservicenetwork -DgatewayName=helloworldgateway -DlistenerName=helloworldserviceListener -DserviceName=helloworldservice -DsourceNetwork=open -DtcpPort=80
```

- Hiermee maakt u een nieuwe gateway YAML in map `servicefabric->appresources` met de naam `gateway_helloworldgateway`
- Verwijzingen `helloworldservicelistener` als de service-listener naar aanroepen vanaf deze gateway luistert. Ook wordt verwezen naar de `helloworldservice` als de service, `helloworldservicenetwork` als het netwerk en `helloworldserver` als de toepassing. 
- Luistert naar aanvragen op poort 80

#### <a name="add-a-new-volume-to-your-application"></a>Een nieuw volume toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een volume resource yaml. 

```cmd
mvn azure-sfmesh:addvolume -DvolumeAccountKey=key -DvolumeAccountName=name -DvolumeName=vol1 -DvolumeShareName=share
```

- Hiermee maakt u een YAML-volume in de map `servicefabric->appresources` met de naam `volume_vol1`
- Hiermee stelt u eigenschappen voor vereiste parameters `volumeAccountKey`, en `volumeShareName` zoals hierboven
- Voor meer informatie over hoe u om te verwijzen naar dit volume gemaakt, gaat u naar de volgende [-App implementeren met behulp van Azure-bestanden Volume](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)

#### <a name="add-a-new-secret-resource-to-your-application"></a>Een nieuwe geheime resource toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een geheime resource yaml. 

```cmd
mvn azure-sfmesh:addsecret -DsecretName=secret1
```

- Hiermee maakt u een geheim YAML in map `servicefabric->appresources` met de naam `secret_secret1`
- Voor meer informatie over hoe u om te verwijzen naar dit geheim gemaakt, gaat u naar de volgende [geheimen beheren](service-fabric-mesh-howto-manage-secrets.md)

#### <a name="add-a-new-secretvalue-resource-to-your-application"></a>Een nieuwe secretvalue resource toevoegen aan uw toepassing
Voer de volgende opdracht om te maken van een resource secretvalue yaml. 

```cmd
mvn azure-sfmesh:addsecretvalue -DsecretValue=someVal -DsecretValueName=secret1/v1
```

- Maak een secretvalue YAML in map `servicefabric->appresources` met de naam `secretvalue_secret1_v1`

### <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren

Met behulp van doel `azure-sfmesh:deploytocluster`, kunt u de toepassing lokaal via de onderstaande opdracht uitvoeren:

```cmd
mvn azure-sfmesh:deploytocluster
```

Dit doel wordt standaard geïmplementeerd van resources met het lokale cluster. Als u met het lokale cluster implementeert, wordt aangenomen dat u hebt een lokaal Service Fabric-cluster ingesteld en geactiveerd. Lokale Service Fabric-cluster voor resources is momenteel alleen ondersteund voor [Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

- Betreffende JSON's maakt van yamls die van toepassing zijn voor Service Fabric-clusters
- Vervolgens implementeert op het clustereindpunt

### <a name="deploy-application-to-azure-service-fabric-mesh"></a>Toepassing implementeren in Azure Service Fabric NET

Met behulp van doel `azure-sfmesh:deploy`, u kunt implementeren op Service Fabric NET-omgeving met de onderstaande opdracht:

```cmd
mvn azure-sfmesh:deploy -DresourceGroup=todoapprg -Dlocation=eastus
```

- Hiermee maakt u een resourcegroep met de naam `todoapprg` als deze niet bestaat.
- Hiermee maakt u een Azure Resource Manager-sjabloon JSON door de YAMLs samen te voegen. 
- Implementeert de JSON naar de Azure Service Fabric NET-omgeving.