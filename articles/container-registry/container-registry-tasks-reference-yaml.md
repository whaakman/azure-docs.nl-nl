---
title: Azure Container Registry taken referentie - YAML
description: Verwijzing voor het definiëren van taken in YAML voor ACR-taken, waaronder taakeigenschappen, stap typen stapeigenschappen en ingebouwde variabelen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: b2398e7db7ed91dee8d85c0c50058bb15b9f4c7e
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894129"
---
# <a name="acr-tasks-reference-yaml"></a>ACR-taken naar te verwijzen: YAML

WebTest met meerdere stappen taakdefinitie in ACR taken biedt een container-georiënteerde compute primitieve nemen die is gericht op te bouwen, testen en patchen van containers. In dit artikel bevat informatie over de opdrachten, parameters, eigenschappen en syntaxis voor de YAML-bestanden die uw taken meerdere stappen definiëren.

In dit artikel bevat een verwijzing voor het maken van meerdere stappen taak YAML-bestanden voor ACR-taken. Als u een inleiding tot het ACR-taken wilt, raadpleegt u de [ACR taken overzicht](container-registry-tasks-overview.md).

## <a name="acr-taskyaml-file-format"></a>ACR-task.yaml-bestandsindeling

ACR taken ondersteunt meerdere stappen taak declaratie in standaard YAML-syntaxis. U definieert de stappen van een taak in een YAML-bestand. U kunt de taak vervolgens handmatig uitvoeren door door te geven van het bestand naar de [az acr uitvoeren] [ az-acr-run] opdracht. Of het bestand gebruiken voor het maken van een taak met [az acr-taak maken] [ az-acr-task-create] die automatisch wordt geactiveerd op een Git-doorvoeren of base installatiekopie-update. Hoewel dit artikel wordt verwezen naar `acr-task.yaml` als het bestand met de stappen, ACR taken biedt ondersteuning voor een geldige bestandsnaam met een [extensie ondersteund](#supported-task-filename-extensions).

Het hoogste niveau `acr-task.yaml` primitieven zijn **taak eigenschappen**, **stap typen**, en **stap eigenschappen**:

* [Eigenschappen van de taak](#task-properties) zijn van toepassing op alle stappen in de uitvoering van de taak. Er zijn verschillende algemene taakeigenschappen, met inbegrip van:
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* [Taak stap typen](#task-step-types) vertegenwoordigen de typen van de acties die kunnen worden uitgevoerd in een taak. Er zijn drie typen voor stap:
  * `build`
  * `push`
  * `cmd`
* [Taak stapeigenschappen](#task-step-properties) parameters die betrekking hebben op een afzonderlijke stap. Er zijn verschillende stapeigenschappen, met inbegrip van:
  * `startDelay`
  * `timeout`
  * `when`
  * .. .en is nog veel meer.

De algemene indeling van een `acr-task.yaml` bestand, met inbegrip van sommige algemene stapeigenschappen volgt. Tijdens het niet een volledige weergave van alle beschikbare stapeigenschappen of het gebruik van het type stap biedt het een kort overzicht van de basic-indeling.

```yml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
steps: # A collection of image or container actions.
  - build: # Equivalent to "docker build," but in a multi-tenant environment
  - push: # Push a newly built or retagged image to a registry.
    when: # Step property that defines either parallel or dependent step execution.
  - cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
    startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Ondersteunde taak bestandsnaamextensies

ACR taken bevat verschillende bestandsnaamextensies, met inbegrip van gereserveerde `.yaml`, die als een taakbestand wordt verwerkt. Elke uitbreiding *niet* in de volgende lijst wordt beschouwd als ACR taken van een docker-bestand: yaml, .yml, .toml, .json, .sh, .bash, .zsh, .ps1, PS, .cmd, .bat, TS, .js, .php, .py, .rb, .lua

YAML is de enige bestandsindeling die momenteel worden ondersteund door de ACR-taken. De andere bestandsextensies zijn gereserveerd voor mogelijke toekomstige ondersteuning.

## <a name="run-the-sample-tasks"></a>De voorbeeldtaken uitvoeren

Er zijn verschillende taak voorbeeldbestanden waarnaar wordt verwezen in de volgende secties van dit artikel. De voorbeeldtaken in een openbare GitHub-opslagplaats zijn [Azure-Samples/acr-taken][acr-tasks]. U kunt ze uitvoeren met de Azure CLI-opdracht [az acr uitvoeren][az-acr-run]. De voorbeeldopdrachten zijn vergelijkbaar met:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

De opmaak van de voorbeeldopdrachten wordt ervan uitgegaan dat van u kunt een standaard-register hebt geconfigureerd in de Azure CLI, zodat ze laat de `--registry` parameter. Gebruik voor het configureren van een register standaard de [az configureren] [ az-configure] opdracht met de `--defaults` parameter accepteert een `acr=REGISTRY_NAME` waarde.

Bijvoorbeeld, het configureren van de Azure CLI met een standaard-register met de naam 'myregistry':

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Eigenschappen van taak

Taakeigenschappen is doorgaans worden weergegeven aan de bovenkant van een `acr-task.yaml` -bestand en algemene eigenschappen die gedurende de volledige uitvoering van de stappen van de van toepassing zijn. Sommige van deze algemene eigenschappen kunnen worden overschreven in een afzonderlijke stap.

| Eigenschap | Type | Optioneel | Description | Onderdrukking ondersteund | Standaardwaarde |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | string | Ja | De versie van de `acr-task.yaml` bestand zoals geparseerd door de taken van de ACR-service. Terwijl ACR taken streeft ernaar om te behouden voor achterwaartse compatibiliteit, kunt deze waarde ACR-taken voor compatibiliteit binnen een opgegeven versie. Als u niets opgeeft, kunt u standaard naar de nieuwste versie. | Nee | Geen |
| `stepTimeout` | Int (seconden) | Ja | Het maximum aantal seconden dat een stap kunt uitvoeren. Als de eigenschap voor een taak is opgegeven, wordt de standaardwaarde ingesteld `timeout` eigenschap van de stappen uit. Als de `timeout` eigenschap is opgegeven voor een stap dit vervangt de eigenschap die is opgegeven door de taak. | Ja | 600 (10 minuten) |
| `workingDirectory` | string | Ja | De werkmap van de container tijdens runtime. Als de eigenschap voor een taak is opgegeven, wordt de standaardwaarde ingesteld `workingDirectory` eigenschap van de stappen uit. Als op een stap hebt opgegeven, wordt de eigenschap die is opgegeven door de taak overschreven. | Ja | `$HOME` |
| `env` | [string, string, ...] | Ja |  Matrix van tekenreeksen in `key=value` indeling die de omgevingsvariabelen voor de taak te definiëren. Als de eigenschap voor een taak is opgegeven, wordt de standaardwaarde ingesteld `env` eigenschap van de stappen uit. Als op een stap hebt opgegeven, heeft deze voorrang omgevingsvariabelen overgenomen van de taak. | Geen |
| `secrets` | [secret, secret, ...] | Ja | Matrix van [geheim](#secret) objecten. | Geen |
| `networks` | [netwerk, netwerk,...] | Ja | Matrix van [netwerk](#network) objecten. | Geen |

### <a name="secret"></a>geheim

Het geheime object heeft de volgende eigenschappen.

| Eigenschap | Type | Optioneel | Beschrijving | Standaardwaarde |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | string | Nee | De id van de geheime sleutel. | Geen |
| `akv` | string | Ja | De geheime URL van de Azure Key Vault (AKV). | Geen |
| `clientID` | string | Ja | De client-ID van de gebruiker toegewezen identiteit voor Azure-resources worden beheerd. | Geen |

### <a name="network"></a>netwerk

Het object network heeft de volgende eigenschappen.

| Eigenschap | Type | Optioneel | Beschrijving | Standaardwaarde |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | string | Nee | De naam van het netwerk. | Geen |
| `driver` | string | Ja | Het stuurprogramma voor het beheren van het netwerk. | Geen |
| `ipv6` | bool | Ja | Of IPv6-netwerken is ingeschakeld. | `false` |
| `skipCreation` | bool | Ja | Of u wilt maken van netwerk overslaan. | `false` |
| `isDefault` | bool | Ja | Of het netwerk is een standaard-netwerk dat is opgegeven met Azure Container Registry | `false` |

## <a name="task-step-types"></a>Stap taaktypen

ACR taken ondersteunt drie typen van de stap. Elk staptype ondersteunt verschillende eigenschappen, zoals uiteengezet in de sectie voor elk staptype.

| Staptype | Beschrijving |
| --------- | ----------- |
| [`build`](#build) | Maakt een containerinstallatiekopie met de vertrouwde `docker build` syntaxis. |
| [`push`](#push) | Voert een `docker push` van de zojuist gemaakte of retagged installatiekopieën naar een containerregister. Azure Container Registry, andere persoonlijke registers en de openbare Docker Hub worden ondersteund. |
| [`cmd`](#cmd) | Uitvoeringen van een container als een opdracht, en de parameters aan van de container doorgegeven `[ENTRYPOINT]`. De `cmd` staptype biedt ondersteuning voor parameters, zoals `env`, `detach`, en andere vertrouwde `docker run` opdrachtopties inschakelen eenheid en functionele testen met gelijktijdige container kan worden uitgevoerd. |

## <a name="build"></a>Build

Een containerinstallatiekopie maken. De `build` staptype vertegenwoordigt een multitenant en veilige manier van het uitvoeren `docker build` in de cloud als een eersteklas primitieve nemen.

### <a name="syntax-build"></a>Syntaxis: bouwen

```yml
version: v1.0.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

De `build` staptype biedt ondersteuning voor de parameters in de volgende tabel. De `build` staptype biedt ook ondersteuning voor alle opties voor opbouwen van de [docker build](https://docs.docker.com/engine/reference/commandline/build/) opdracht, zoals `--build-arg` om in te stellen variabelen voor de build-tijd.

| Parameter | Description | Optioneel |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Definieert de volledig gekwalificeerde `image:tag` van de gemaakte installatiekopie.<br /><br />Installatiekopieën kunnen worden gebruikt voor interne taak validaties, zoals functionele tests, niet alle installatiekopieën vereisen `push` naar een register. De installatiekopie van het echter voor het exemplaar van een installatiekopie binnen een uitvoering van de taak, moet een naam om te verwijzen naar.<br /><br />In tegenstelling tot `az acr build`, actieve taken ACR biedt geen push standaardgedrag. Met ACR-taken, de standaard-scenario wordt ervan uitgegaan dat de mogelijkheid om te bouwen, te valideren en vervolgens een installatiekopie pushen. Zie [push](#push) voor informatie over het eventueel push ingebouwde installatiekopieën. | Ja |
| `-f` &#124; `--file` | Hiermee geeft u het Dockerfile dat is doorgegeven aan `docker build`. Indien niet opgegeven, wordt de standaardinstelling docker-bestand in de hoofdmap van de context gebruikt. Als u een docker-bestand, geeft u de bestandsnaam ten opzichte van de hoofdmap van de context. | Ja |
| `context` | De hoofdmap wordt doorgegeven aan `docker build`. De hoofdmap van elke taak is ingesteld op een gedeelde [workingDirectory](#task-step-properties), en bevat de hoofdmap van de bijbehorende gekloonde Git-map. | Nee |

### <a name="properties-build"></a>Eigenschappen: bouwen

De `build` staptype ondersteunt de volgende eigenschappen. Informatie van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

| | | |
| -------- | ---- | -------- |
| `detach` | bool | Optioneel |
| `disableWorkingDirectoryOverride` | bool | Optioneel |
| `entryPoint` | string | Optioneel |
| `env` | [string, string, ...] | Optioneel |
| `expose` | [string, string, ...] | Optioneel |
| `id` | string | Optioneel |
| `ignoreErrors` | bool | Optioneel |
| `isolation` | string | Optioneel |
| `keep` | bool | Optioneel |
| `network` | object | Optioneel |
| `ports` | [string, string, ...] | Optioneel |
| `pull` | bool | Optioneel |
| `repeat` | int | Optioneel |
| `retries` | int | Optioneel |
| `retryDelay` | Int (seconden) | Optioneel |
| `secret` | object | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [string, string, ...] | Optioneel |
| `workingDirectory` | string | Optioneel |

### <a name="examples-build"></a>Voorbeelden: bouwen

#### <a name="build-image---context-in-root"></a>Bouw installatiekopie - context in de hoofdmap

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Installatiekopie - context in de submap maken

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>Push

Push een of meer ingebouwd of retagged installatiekopieën naar een containerregister. Biedt ondersteuning voor persoonlijke registers zoals Azure Container Registry of de openbare Docker Hub pushen.

### <a name="syntax-push"></a>Syntaxis: push

De `push` staptype biedt ondersteuning voor een verzameling van afbeeldingen. Syntaxis van de verzameling YAML ondersteund inline en geneste indelingen. Een één installatiekopie pushen wordt doorgaans weergegeven met behulp van inlinesyntaxis:

```yml
version: v1.0.0
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Gebruik voor een betere leesbaarheid geneste syntaxis als u meerdere installatiekopieën kunt pushen:

```yml
version: v1.0.0
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Eigenschappen: push

De `push` staptype ondersteunt de volgende eigenschappen. Informatie van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

| | | |
| -------- | ---- | -------- |
| `env` | [string, string, ...] | Optioneel |
| `id` | string | Optioneel |
| `ignoreErrors` | bool | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [string, string, ...] | Optioneel |

### <a name="examples-push"></a>Voorbeelden: push

#### <a name="push-multiple-images"></a>Meerdere installatiekopieën pushen

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Bouwen, te pushen en uitvoeren

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

De `cmd` staptype van een container wordt uitgevoerd.

### <a name="syntax-cmd"></a>Syntaxis: cmd

```yml
version: v1.0.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Eigenschappen: cmd

De `cmd` staptype ondersteunt de volgende eigenschappen:

| | | |
| -------- | ---- | -------- |
| `detach` | bool | Optioneel |
| `disableWorkingDirectoryOverride` | bool | Optioneel |
| `entryPoint` | string | Optioneel |
| `env` | [string, string, ...] | Optioneel |
| `expose` | [string, string, ...] | Optioneel |
| `id` | string | Optioneel |
| `ignoreErrors` | bool | Optioneel |
| `isolation` | string | Optioneel |
| `keep` | bool | Optioneel |
| `network` | object | Optioneel |
| `ports` | [string, string, ...] | Optioneel |
| `pull` | bool | Optioneel |
| `repeat` | int | Optioneel |
| `retries` | int | Optioneel |
| `retryDelay` | Int (seconden) | Optioneel |
| `secret` | object | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [string, string, ...] | Optioneel |
| `workingDirectory` | string | Optioneel |

U vindt de details van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

### <a name="examples-cmd"></a>Voorbeelden: cmd

#### <a name="run-hello-world-image"></a>Hallo wereld-installatiekopie uitvoeren

Met deze opdracht wordt uitgevoerd de `hello-world.yaml` taakbestand, die verwijst naar de [hello-world](https://hub.docker.com/_/hello-world/) installatiekopie op Docker Hub.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Bash-installatiekopie en echo "Hallo wereld" uitvoeren

Met deze opdracht wordt uitgevoerd de `bash-echo.yaml` taakbestand, die verwijst naar de [bash](https://hub.docker.com/_/bash/) installatiekopie op Docker Hub.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Specifieke bash afbeeldingscode uitvoeren

Als u wilt uitvoeren van de versie van een specifieke installatiekopie, geef de code in de `cmd`.

Met deze opdracht wordt uitgevoerd de `bash-echo-3.yaml` taakbestand, die verwijst naar de [bash: 3.0](https://hub.docker.com/_/bash/) installatiekopie op Docker Hub.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Uitvoeren van aangepaste installatiekopieën

De `cmd` staptype verwijst naar afbeeldingen met behulp van de standaard `docker run` indeling. Afbeeldingen niet voorafgegaan door een register wordt aangenomen dat afkomstig zijn van docker.io. Het vorige voorbeeld kan evenredig worden weergegeven als:

```yml
version: v1.0.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

Met behulp van de standaard `docker run` image reference overeenkomst, `cmd` afbeeldingen uit een persoonlijk register of de openbare Docker Hub kan worden uitgevoerd. Als u van afbeeldingen in het hetzelfde register waarin ACR-taak wordt uitgevoerd gebruikgemaakt, moet u niet een registerreferenties opgeven.

* Uitvoeren van een installatiekopie die afkomstig is van een Azure container registry

    Vervang `[myregistry]` met de naam van het register:

    ```yml
    version: v1.0.0
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Generaliseren van de Register-referentie met een variabele uitvoeren

    In plaats van hard-coding de registernaam van uw in een `acr-task.yaml` -bestand, kunt u deze draagbaarder met behulp van een [uitvoeren variabele](#run-variables). De `Run.Registry` variabele breidt tijdens runtime aan de naam van het register waarin de taak wordt uitgevoerd.

    Om de vorige taak zodat deze in alle Azure-containerregister werkt, verwijzen naar de [Run.Registry](#runregistry) variabele in de naam van de installatiekopie:

    ```yml
    version: v1.0.0
    steps:
      - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Stap taakeigenschappen

Elk staptype biedt ondersteuning voor diverse eigenschappen die geschikt zijn voor het betreffende type. De volgende tabel bevat alle beschikbare stapeigenschappen. Niet alle stap typen ondersteuning voor alle eigenschappen. Als u wilt zien welke van deze eigenschappen zijn beschikbaar voor elk staptype, Zie de [cmd](#cmd), [bouwen](#build), en [push](#push) stap type in de secties.

| Eigenschap | Type | Optioneel | Beschrijving | Standaardwaarde |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | bool | Ja | Of de container moet worden losgekoppeld wanneer wordt uitgevoerd. | `false` |
| `disableWorkingDirectoryOverride` | bool | Ja | Of u wilt uitschakelen `workingDirectory` functionaliteit overschrijven. Gebruik deze in combinatie met `workingDirectory` hebben volledige controle over de werkmap van de container. | `false` |
| `entryPoint` | string | Ja | Onderdrukt de `[ENTRYPOINT]` van een stap van de container. | Geen |
| `env` | [string, string, ...] | Ja | Matrix van tekenreeksen in `key=value` indeling die de omgevingsvariabelen voor de stap definiëren. | Geen |
| `expose` | [string, string, ...] | Ja | Matrix van de poorten die beschikbaar worden gesteld van de container. |  Geen |
| [`id`](#example-id) | string | Ja | Unieke identificatie van de stap in de taak. Overige stappen in de taak kunnen verwijzen naar van een stap `id`, zoals voor afhankelijkheden controleren met `when`.<br /><br />De `id` is ook de naam van de container die wordt uitgevoerd. Processen die worden uitgevoerd in andere containers in de taak kunnen verwijzen naar de `id` als de DNS-hostnaam of voor het voorbeeld met docker-Logboeken [id] te openen. | `acb_step_%d`, waarbij `%d` is de index 0 op basis van de stap boven naar beneden in het YAML-bestand |
| `ignoreErrors` | bool | Ja | Of u wilt markeren de stap als geslaagd, ongeacht of is een fout tijdens het uitvoeren van de container opgetreden. | `false` |
| `isolation` | string | Ja | Het isolatieniveau van de container. | `default` |
| `keep` | bool | Ja | Of de container van de stap moet worden opgeslagen na de uitvoering. | `false` |
| `network` | object | Ja | Hiermee geeft u een netwerk waarin de container wordt uitgevoerd. | Geen |
| `ports` | [string, string, ...] | Ja | Matrix van de poorten die worden gepubliceerd vanuit de container op de host. |  Geen |
| `pull` | bool | Ja | Of u wilt afdwingen dat een pull van de container voordat deze om te voorkomen dat een cachegedrag wordt uitgevoerd. | `false` |
| `privileged` | bool | Ja | Of u wilt uitvoeren van de container in beschermde modus. | `false` |
| `repeat` | int | Ja | Het aantal nieuwe pogingen te herhalen van de uitvoering van een container. | 0 |
| `retries` | int | Ja | Het aantal nieuwe pogingen om te proberen als een container, de uitvoering ervan mislukt. Een nieuwe poging wordt alleen uitgevoerd als de afsluitcode van de container dan nul is. | 0 |
| `retryDelay` | Int (seconden) | Ja | De vertraging in seconden tussen nieuwe pogingen van de uitvoering van de container. | 0 |
| `secret` | object | Ja | Hiermee geeft u een Azure Key Vault-geheim of een beheerde identiteit voor Azure-resources. | Geen |
| `startDelay` | Int (seconden) | Ja | Het aantal seconden vertraging van de container kan worden uitgevoerd. | 0 |
| `timeout` | Int (seconden) | Ja | Maximaal aantal seconden dat een stap kan worden uitgevoerd voordat de wordt beëindigd. | 600 |
| [`when`](#example-when) | [string, string, ...] | Ja | Hiermee configureert u de afhankelijkheid van een stap op een of meer andere stappen binnen de taak. | Geen |
| `user` | string | Ja | De gebruikersnaam of het gebruikers-id van een container | Geen |
| `workingDirectory` | string | Ja | Hiermee stelt u de werkmap voor een stap. ACR-taken maakt standaard een hoofdmap als de werkmap. Echter, als uw build verschillende stappen heeft, eerdere stappen kunnen delen artefacten met latere stappen door de dezelfde werkmap op te geven. | `$HOME` |

### <a name="examples-task-step-properties"></a>Voorbeelden: Stap taakeigenschappen

#### <a name="example-id"></a>Voorbeeld: id

Maak twee installatiekopieën, instanties van de installatiekopie van een functionele test. Elke stap wordt geïdentificeerd door een unieke `id` waarin andere stappen in de taak naar hun `when` eigenschap.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Voorbeeld: als

De `when` eigenschap geeft u de afhankelijkheid van een stap voor de andere stappen binnen de taak. Ondersteunt twee parameterwaarden:

* `when: ["-"]` -Geeft aan dat er geen afhankelijkheden van andere stappen. Het opgeven van een stap `when: ["-"]` , worden onmiddellijk worden uitgevoerd, en kunt u gelijktijdige stap worden uitgevoerd.
* `when: ["id1", "id2"]` -Hiermee geeft u de stap is afhankelijk van de stappen met `id` 'id1' en `id` 'id2'. Deze stap wordt niet worden uitgevoerd tot zowel "id1" en "id2" stappen worden voltooid.

Als `when` is niet opgegeven in een stap die stap is afhankelijk van de voltooiing van de vorige stap in de `acr-task.yaml` bestand.

Stap kan worden uitgevoerd zonder `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Stap kan worden uitgevoerd met `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Parallelle afbeeldingen bouwen:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Parallelle image build en afhankelijke testen:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Variabelen voor de uitvoeren

ACR taken bevat een reeks variabelen die beschikbaar voor taakstappen zijn wanneer ze worden uitgevoerd. Deze variabelen zijn toegankelijk via de indeling `{{.Run.VariableName}}`, waarbij `VariableName` is een van de volgende:

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Voer&#46;ID

Elk uitgevoerd via `az acr run`, of op basis van de uitvoering van taken die zijn gemaakt via trigger `az acr task create` een unieke id hebben. De ID vertegenwoordigt de uitvoering die momenteel wordt uitgevoerd.

Doorgaans gebruikt voor een unieke labels een installatiekopie:

```yml
version: v1.0.0
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

De volledig gekwalificeerde servernaam van het register. Doorgaans gebruikt voor het algemeen verwijzen naar het register waar de taak wordt uitgevoerd.

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

De huidige UTC-tijd de uitvoering is begonnen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van taken meerdere stappen, de [WebTest met meerdere stappen bouwen, testen en patch-taken uitvoeren in ACR taken](container-registry-tasks-multi-step.md).

Stap voor stap wordt gemaakt, vindt de [ACR taken overzicht](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure
