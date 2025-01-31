---
title: Azure Service Fabric Mesh-toepassingsgeheimen beheren | Microsoft Docs
description: Beheer toepassingsgeheimen zodat u veilig een Service Fabric Mesh-toepassing kunt maken en implementeren.
services: service-fabric-mesh
keywords: geheimen
author: aljo-microsoft
ms.author: aljo
ms.date: 4/2/2019
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chackdan
ms.openlocfilehash: c2548ea3cf892ebe1a56cbb0909bfa5d5e805acf
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503310"
---
# <a name="manage-service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-toepassingsgeheimen beheren
Service Fabric Mesh biedt ondersteuning voor geheimen als Azure-resources. Een Service Fabric Mesh-geheim kan bestaan uit gevoelige informatie in tekstvorm, zoals opslagverbindingsreeksen, wachtwoorden of andere waarden die veilig moeten worden opgeslagen en verzonden. In dit artikel ziet u hoe de Service Fabric Secure Store-service wordt gebruikt om geheimen te implementeren en onderhouden.

Een Mesh-toepassingsgeheim bestaat uit:
* Een **Geheimen**-resource. Dit is een container die geheimen in tekstvorm bevat. Geheimen die zijn opgenomen in de **Geheimen**-resource, worden veilig opgeslagen en verzonden.
* Een of meer **Geheimen/waarden**-resources die zijn opgeslagen in de **Geheimen**-resourcecontainer. Elke **Geheimen/waarden**-resource wordt gekenmerkt door een uniek versienummer. U kunt een versie van een **Geheimen/waarden**-resource niet wijzigen. U kunt alleen een nieuwe versie toevoegen.

Het beheren van Geheimen bestaat uit de volgende stappen:
1. Declareer een NET **geheimen** resource in een Azure Resource Model YAML- of JSON-bestand met behulp van de soort inlinedValue en SecretsStoreRef contentType definities.
2. Declareer Mesh **geheimen/waarden** resources in een Azure Resource Model YAML- of JSON-bestand dat wordt opgeslagen in de **geheimen** -resource (in stap 1).
3. Wijzig Mesh-toepassing om te verwijzen naar Mesh Geheimen-waarden.
4. Implementeer of gebruik een rolling upgrade voor de Mesh-toepassing om Geheimen-waarden te gebruiken.
5. Gebruik Azure 'az' CLI-opdrachten om de levenscyclus van de Secure Store-service te beheren.

## <a name="declare-a-mesh-secrets-resource"></a>Een Mesh Geheimen-resource declareren
Een resource NET geheimen is in een Azure Resource Model-JSON of YAML-bestand met behulp van de definitie van het soort inlinedValue gedeclareerd. De Mesh Geheimen-resource biedt ondersteuning voor geheimen afkomstig uit de Secure Store-service. 
>
Hier volgt een voorbeeld van hoe Mesh Geheimen-resources in een JSON-bestand worden gedeclareerd:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    }
  ]
}
```
Hier volgt een voorbeeld van hoe Mesh Geheimen-resources in een YAML-bestand worden gedeclareerd:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="declare-mesh-secretsvalues-resources"></a>Mesh Geheimen/waarden-resources declareren
Mesh Geheimen/waarden-resources hebben een afhankelijkheid van de Mesh Geheimen-resources die in de vorige stap zijn gedefinieerd.

Met betrekking tot de relatie tussen de 'resources'-sectie 'waarde' en 'naam:'-velden: het tweede gedeelte van de 'naam:'-tekenreeks gescheiden door een dubbele punt is het versienummer dat wordt gebruikt voor een geheim, en de naam vóór de dubbele punt moet overeenkomen met de Mesh Geheimen-waarde waarvoor het een afhankelijkheid heeft. Voor element ```name: mysecret:1.0``` is het versienummer bijvoorbeeld 1.0 en moet de naam van ```mysecret``` overeenkomen met de eerder gedefinieerde ```"value": "mysecret"```.

>
Hier volgt een voorbeeld van hoe Mesh Geheimen/waarden-resources in een JSON-bestand worden gedeclareerd:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx/2019.02.28",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/sfbpHttpsCertificate.pfx"
      ],
      "properties": {
        "value": "[parameters('sfbpHttpsCertificate')]"
      }
    }
  ],
}
```
Hier volgt een voorbeeld van hoe Mesh Geheimen/waarden-resources in een YAML-bestand worden gedeclareerd:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          Secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
            - name: mysecret:1.0
            description: My Mesh Application Secret Value
            secret_type: value
            content_type: text/plain
            value: "P@ssw0rd#1234"
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="modify-mesh-application-to-reference-mesh-secret-values"></a>Mesh-toepassing wijzigen om te verwijzen naar Mesh Geheimen-waarden
Service Fabric Mesh-toepassingen moeten rekening houden met de volgende twee tekenreeksen om Geheimen-waarden voor de Secure Store-service te kunnen gebruiken:
1. Microsoft.ServiceFabricMesh/Secrets.name bevat de naam van het bestand en bevat tevens de Geheimen-waarde als tekst zonder opmaak.
2. De Windows- of Linux-omgevingsvariabele 'Fabric_SettingPath' bevat het pad naar de map waar bestanden met Geheimen-waarden voor de Secure Store-service toegankelijk zijn. Dat pad is 'C:\Settings' voor Mesh-toepassingen op Windows en '/var/settings' voor Mesh-toepassingen op Linux.

## <a name="deploy-or-use-a-rolling-upgrade-for-mesh-application-to-consume-secret-values"></a>Implementeer of gebruik een rolling upgrade voor de Mesh-toepassing om Geheimen-waarden te gebruiken
Het maken van Geheimen en/of versies van Geheimen/waarden is beperkt tot implementaties die zijn gedeclareerd met een resourcemodel. De enige manier om deze resources te maken is door als volgt een resourcemodel-JSON- of YAML-bestand door te geven met de opdracht **az mesh deployment**:

```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```

## <a name="azure-cli-commands-for-secure-store-service-lifecycle-management"></a>Azure CLI-opdrachten om de levenscyclus van de Secure Store-service te beheren

### <a name="create-a-new-secrets-resource"></a>Een nieuwe Geheimen-resource maken
```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```
Geef **template-file** of **template-uri** door (maar niet beide).

Bijvoorbeeld:
- az mesh deployment create --c:\MyMeshTemplates\SecretTemplate1.txt
- AZ mesh-implementatie maken - https:\//www.fabrikam.com/MyMeshTemplates/SecretTemplate1.txt

### <a name="show-a-secret"></a>Een geheim weergeven
Retourneert de beschrijving van het geheim (maar niet de waarde).
```azurecli-interactive
az mesh secret show --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="delete-a-secret"></a>Een geheim verwijderen

- Een geheim kan niet worden verwijderd terwijl ernaar wordt verwezen door een Mesh-toepassing.
- Als u een Geheimen-resource verwijdert, worden alle versies van Geheimen-resources-versies verwijderd.
  ```azurecli-interactive
  az mesh secret delete --Resource-group <myResourceGroup> --secret-name <mySecret>
  ```

### <a name="list-secrets-in-subscription"></a>Geheimen in abonnement weergeven
```azurecli-interactive
az mesh secret list
```
### <a name="list-secrets-in-resource-group"></a>Geheimen in resourcegroep weergeven
```azurecli-interactive
az mesh secret list -g <myResourceGroup>
```
### <a name="list-all-versions-of-a-secret"></a>Alle versies van een geheim weergeven
```azurecli-interactive
az mesh secretvalue list --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="show-secret-version-value"></a>Geheime versiewaarde weergeven
```azurecli-interactive
az mesh secretvalue show --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

### <a name="delete-secret-version-value"></a>Geheime versiewaarde verwijderen
```azurecli-interactive
az mesh secretvalue delete --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

## <a name="next-steps"></a>Volgende stappen 
Lees het overzicht voor meer informatie over Service Fabric Mesh:
- [Service Fabric Mesh - overzicht](service-fabric-mesh-overview.md)
