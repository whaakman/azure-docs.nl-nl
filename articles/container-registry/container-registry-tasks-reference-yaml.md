---
title: Azure Container Registry taken referentie - YAML
description: Verwijzing voor het definiëren van taken in YAML voor ACR-taken, waaronder taakeigenschappen, stap typen stapeigenschappen en ingebouwde variabelen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 11/13/2018
ms.author: danlep
ms.openlocfilehash: e91b4e881c0f39304e3042d556f111db2089f7de
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334479"
---
# <a name="acr-tasks-reference-yaml"></a>Taken van de ACR-verwijzing: YAML

WebTest met meerdere stappen taakdefinitie in ACR taken biedt een container-georiënteerde compute primitieve nemen die is gericht op te bouwen, testen en patchen van containers. In dit artikel bevat informatie over de opdrachten, parameters, eigenschappen en syntaxis voor de YAML-bestanden die uw taken meerdere stappen definiëren.

In dit artikel bevat een verwijzing voor het maken van meerdere stappen taak YAML-bestanden voor ACR-taken. Als u een inleiding tot het ACR-taken wilt, raadpleegt u de [ACR taken overzicht](container-registry-tasks-overview.md).

> [!IMPORTANT]
> De functie taken meerdere stappen van de ACR-taken is momenteel in preview. Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="acr-taskyaml-file-format"></a>ACR-task.yaml-bestandsindeling

ACR taken ondersteunt meerdere stappen taak declaratie in standaard YAML-syntaxis. U definieert van een taak stappen in een YAML-bestand dat u kunt vervolgens handmatig uitvoeren of automatisch op Git doorvoeren of base installatiekopie update hebt geactiveerd. Hoewel dit artikel wordt verwezen naar `acr-task.yaml` als het bestand met de stappen, ACR taken biedt ondersteuning voor een geldige bestandsnaam met een [extensie ondersteund](#supported-task-filename-extensions).

Het hoogste niveau `acr-task.yaml` primitieven zijn **taak eigenschappen**, **stap typen**, en **stap eigenschappen**:

* [Eigenschappen van de taak](#task-properties) zijn van toepassing op alle stappen in de uitvoering van de taak. Er zijn drie algemene eigenschappen:
  * versie
  * stepTimeout
  * totalTimeout
* [Taak stap typen](#task-step-types) vertegenwoordigen de typen van de acties die kunnen worden uitgevoerd in een taak. Er zijn drie typen voor stap:
  * Build
  * Push
  * cmd
* [Taak stapeigenschappen](#task-step-properties) parameters die betrekking hebben op een afzonderlijke stap. Er zijn verschillende stapeigenschappen, met inbegrip van:
  * startDelay
  * timeout
  * wanneer
  * .. .en is nog veel meer.

De algemene indeling van een `acr-task.yaml` bestand, met inbegrip van sommige algemene stapeigenschappen volgt. Tijdens het niet een volledige weergave van alle beschikbare stapeigenschappen of het gebruik van het type stap biedt het een kort overzicht van de basic-indeling.

```yaml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
totalTimeout: # Total seconds allowed for all steps to complete.
steps: # A collection of image or container actions.
    build: # Equivalent to "docker build," but in a multi-tenant environment
    push: # Push a newly built or retagged image to a registry.
      when: # Step property that defines either parallel or dependent step execution.
    cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
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

Taakeigenschappen is doorgaans worden weergegeven aan de bovenkant van een `acr-task.yaml` -bestand en algemene eigenschappen die van toepassing gedurende de volledige uitvoering van de taak zijn zijn. Sommige van deze algemene eigenschappen kunnen worden overschreven in een afzonderlijke stap.

| Eigenschap | Type | Optioneel | Beschrijving | Onderdrukking ondersteund | Standaardwaarde |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | tekenreeks | Nee | De versie van de `acr-task.yaml` bestand zoals geparseerd door de taken van de ACR-service. Terwijl ACR taken streeft ernaar om te behouden voor achterwaartse compatibiliteit, kunt deze waarde ACR-taken voor compatibiliteit binnen een opgegeven versie. | Nee | Geen |
| `stepTimeout` | Int (seconden) | Ja | Het maximum aantal seconden dat een stap kunt uitvoeren. Deze eigenschap kan worden genegeerd in een stap door het instellen van de stap [time-out](#timeout) eigenschap. | Ja | 600 (10 minuten) |
| `totalTimeout` | Int (seconden) | Ja | Het maximale aantal seconden dat een taak kan worden uitgevoerd. Een "uitvoeren" bevat de uitvoering en voltooiing van alle stappen in de taak, of geslaagd of mislukt. Ook is afdrukken taak uitvoer, zoals afhankelijkheden van gedetecteerde installatiekopie en de taakstatus kan worden uitgevoerd. | Nee | 3600 (1 uur) |

## <a name="task-step-types"></a>Stap taaktypen

ACR taken ondersteunt drie typen van de stap. Elk staptype ondersteunt verschillende eigenschappen, zoals uiteengezet in de sectie voor elk staptype.

| Staptype | Beschrijving |
| --------- | ----------- |
| [`build`](#build) | Maakt een containerinstallatiekopie met de vertrouwde `docker build` syntaxis. |
| [`push`](#push) | Voert een `docker push` van de zojuist gemaakte of retagged installatiekopieën naar een containerregister. Azure Container Registry, andere persoonlijke registers en de openbare Docker Hub worden ondersteund.
| [`cmd`](#cmd) | Uitvoeringen van een container als een opdracht, en de parameters aan van de container doorgegeven `[ENTRYPOINT]`. De `cmd` staptype biedt ondersteuning voor parameters, zoals het env, loskoppelen, en andere vertrouwde `docker run` opdrachtopties inschakelen eenheid en functionele testen met gelijktijdige container kan worden uitgevoerd. |

## <a name="build"></a>Build

Een containerinstallatiekopie maken. De `build` staptype vertegenwoordigt een multitenant en veilige manier van het uitvoeren `docker build` in de cloud als een eersteklas primitieve nemen.

### <a name="syntax-build"></a>Syntaxis: bouwen

```yaml
version: 1.0-preview-1
steps:
    - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
      [property]: [value]
```

De `build` staptype biedt ondersteuning voor de parameters in de volgende tabel. De `build` staptype biedt ook ondersteuning voor alle opties voor opbouwen van de [docker build](https://docs.docker.com/engine/reference/commandline/build/) opdracht, zoals `--build-arg` om in te stellen variabelen voor de build-tijd.

| Parameter | Beschrijving | Optioneel |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Definieert de volledig gekwalificeerde `image:tag` van de gemaakte installatiekopie.<br /><br />Installatiekopieën kunnen worden gebruikt voor interne taak validaties, zoals functionele tests, niet alle installatiekopieën vereisen `push` naar een register. De installatiekopie van het echter voor het exemplaar van een installatiekopie binnen een uitvoering van de taak, moet een naam om te verwijzen naar.<br /><br />In tegenstelling tot `az acr build`, actieve taken ACR biedt geen push standaardgedrag. Met ACR-taken, de standaard-scenario wordt ervan uitgegaan dat de mogelijkheid om te bouwen, te valideren en vervolgens een installatiekopie pushen. Zie [push](#push) voor informatie over het eventueel push ingebouwde installatiekopieën. | Ja |
| `-f` &#124; `--file` | Hiermee geeft u het Dockerfile dat is doorgegeven aan `docker build`. Indien niet opgegeven, wordt de standaardinstelling docker-bestand in de hoofdmap van de context gebruikt. Als u een alternatief Dockerfile, slagen voor de bestandsnaam ten opzichte van de hoofdmap van de context. | Ja |
| `context` | De hoofdmap wordt doorgegeven aan `docker build`. De hoofdmap van elke taak is ingesteld op een gedeelde [workingDirectory](#task-step-properties), en bevat de hoofdmap van de bijbehorende gekloonde Git-map. | Nee |

### <a name="properties-build"></a>Eigenschappen: bouwen

De `build` staptype ondersteunt de volgende eigenschappen. U vindt de details van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

| | | |
| -------- | ---- | -------- |
| `detach` | BOOL | Optioneel |
| `entryPoint` | tekenreeks | Optioneel |
| `env` | [tekenreeks, tekenreeks,...] | Optioneel |
| `id` | tekenreeks | Optioneel |
| `ignoreErrors` | BOOL | Optioneel |
| `keep` | BOOL | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [tekenreeks, tekenreeks,...] | Optioneel |
| `workingDirectory` | tekenreeks | Optioneel |

### <a name="examples-build"></a>Voorbeelden: bouwen

#### <a name="build-image---context-in-root"></a>Bouw installatiekopie - context in de hoofdmap

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Installatiekopie - context in de submap maken

```yaml
version: 1.0-preview-1
steps:
- build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>Push

Push een of meer ingebouwd of retagged installatiekopieën naar een containerregister. Biedt ondersteuning voor persoonlijke registers zoals Azure Container Registry of de openbare Docker Hub pushen.

### <a name="syntax-push"></a>Syntaxis: push

De `push` staptype biedt ondersteuning voor een verzameling van afbeeldingen. Syntaxis van de verzameling YAML ondersteund inline en geneste indelingen. Een één installatiekopie pushen wordt doorgaans weergegeven met behulp van inlinesyntaxis:

```yaml
version: 1.0-preview-1
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Gebruik voor een betere leesbaarheid geneste syntaxis als u meerdere installatiekopieën kunt pushen:

```yaml
version: 1.0-preview-1
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Eigenschappen: push

De `push` staptype ondersteunt de volgende eigenschappen. U vindt de details van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

| | | |
| -------- | ---- | -------- |
| `env` | [tekenreeks, tekenreeks,...] | Optioneel |
| `id` | tekenreeks | Optioneel |
| `ignoreErrors` | BOOL | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [tekenreeks, tekenreeks,...] | Optioneel |

### <a name="examples-push"></a>Voorbeelden: push

#### <a name="push-multiple-images"></a>Meerdere installatiekopieën pushen

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Bouwen, te pushen en uitvoeren

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

De `cmd` staptype van een container wordt uitgevoerd.

### <a name="syntax-cmd"></a>Syntaxis: cmd

```yaml
version: 1.0-preview-1
steps:
    - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Eigenschappen: cmd

De `cmd` staptype ondersteunt de volgende eigenschappen:

| | | |
| -------- | ---- | -------- |
| `detach` | BOOL | Optioneel |
| `entryPoint` | tekenreeks | Optioneel |
| `env` | [tekenreeks, tekenreeks,...] | Optioneel |
| `id` | tekenreeks | Optioneel |
| `ignoreErrors` | BOOL | Optioneel |
| `keep` | BOOL | Optioneel |
| `startDelay` | Int (seconden) | Optioneel |
| `timeout` | Int (seconden) | Optioneel |
| `when` | [tekenreeks, tekenreeks,...] | Optioneel |
| `workingDirectory` | tekenreeks | Optioneel |

U vindt de details van deze eigenschappen in de [stap taakeigenschappen](#task-step-properties) sectie van dit artikel.

### <a name="examples-cmd"></a>Voorbeelden: cmd

#### <a name="run-hello-world-image"></a>Hallo wereld-installatiekopie uitvoeren

Met deze opdracht wordt uitgevoerd de `hello-world.yaml` taakbestand, die verwijst naar de [hello-world](https://hub.docker.com/_/hello-world/) installatiekopie op Docker Hub.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml --> [!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Bash-installatiekopie en echo "Hallo wereld" uitvoeren

Met deze opdracht wordt uitgevoerd de `bash-echo.yaml` taakbestand, die verwijst naar de [bash](https://hub.docker.com/_/bash/) installatiekopie op Docker Hub.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml --> [!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Specifieke bash afbeeldingscode uitvoeren

Als u wilt uitvoeren van de versie van een specifieke installatiekopie, geef de code in de `cmd`.

Met deze opdracht wordt uitgevoerd de `bash-echo-3.yaml` taakbestand, die verwijst naar de [bash: 3.0](https://hub.docker.com/_/bash/) installatiekopie op Docker Hub.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml --> [!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Uitvoeren van aangepaste installatiekopieën

De `cmd` staptype verwijst naar afbeeldingen met behulp van de standaard `docker run` indeling. Afbeeldingen niet voorafgegaan door een register wordt aangenomen dat afkomstig zijn van docker.io. Het vorige voorbeeld kan evenredig worden weergegeven als:

```yaml
version: 1.0-preview-1
steps:
    - cmd: docker.io/bash:3.0 echo hello world
```

Met behulp van de standaard `docker run` image reference overeenkomst, `cmd` afbeeldingen die zich bevinden in een persoonlijk register of de openbare Docker Hub kan worden uitgevoerd. Als u van afbeeldingen in het hetzelfde register waarin ACR-taak wordt uitgevoerd gebruikgemaakt, moet u niet een registerreferenties opgeven.

* Een installatiekopie die zich bevinden in een Azure container registry uitvoeren

    Vervang `[myregistry]` met de naam van het register:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Generaliseren van de Register-referentie met een variabele uitvoeren

    In plaats van hard-coding de registernaam van uw in een `acr-task.yaml` -bestand, kunt u deze draagbaarder met behulp van een [uitvoeren variabele](#run-variables). De `Run.Registry` variabele breidt tijdens runtime aan de naam van het register waarin de taak wordt uitgevoerd.

    Om de vorige taak zodat deze in alle Azure-containerregister werkt, verwijzen naar de [Run.Registry](#runregistry) variabele in de naam van de installatiekopie:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Stap taakeigenschappen

Elk staptype biedt ondersteuning voor diverse eigenschappen die geschikt zijn voor het betreffende type. De volgende tabel bevat alle beschikbare stapeigenschappen. Niet alle stap typen ondersteuning voor alle eigenschappen. Als u wilt zien welke van deze eigenschappen zijn beschikbaar voor elk staptype, Zie de [cmd](#cmd), [bouwen](#build), en [push](#push) stap type in de secties.

| Eigenschap | Type | Optioneel | Beschrijving |
| -------- | ---- | -------- | ----------- |
| `detach` | BOOL | Ja | Of de container moet worden losgekoppeld wanneer wordt uitgevoerd. |
| `entryPoint` | tekenreeks | Ja | Onderdrukt de `[ENTRYPOINT]` van een stap van de container. |
| `env` | [tekenreeks, tekenreeks,...] | Ja | Matrix van tekenreeksen in `key=value` indeling die de omgevingsvariabelen voor de stap definiëren. |
| [`id`](#example-id) | tekenreeks | Ja | Unieke identificatie van de stap in de taak. Overige stappen in de taak kunnen verwijzen naar van een stap `id`, zoals voor afhankelijkheden controleren met `when`.<br /><br />De `id` is ook de naam van de container die wordt uitgevoerd. Processen die worden uitgevoerd in andere containers in de taak kunnen verwijzen naar de `id` als de DNS-hostnaam of voor het voorbeeld met docker-Logboeken [id] te openen. |
| `ignoreErrors` | BOOL | Ja | Als de waarde `true`, de stap is gemarkeerd als voltooid is, ongeacht of een fout tijdens de uitvoering ervan opgetreden. Standaard: `false`. |
| `keep` | BOOL | Ja | Of de container van de stap moet worden opgeslagen na de uitvoering. |
| `startDelay` | Int (seconden) | Ja | Het aantal seconden vertraging van een stap worden uitgevoerd. |
| `timeout` | Int (seconden) | Ja | Maximaal aantal seconden dat een stap kan worden uitgevoerd voordat de wordt beëindigd. |
| [`when`](#example-when) | [tekenreeks, tekenreeks,...] | Ja | Hiermee configureert u de afhankelijkheid van een stap op een of meer andere stappen binnen de taak. |
| `workingDirectory` | tekenreeks | Ja | Hiermee stelt u de werkmap voor een stap. ACR-taken maakt standaard een hoofdmap als de werkmap. Echter, als uw build verschillende stappen heeft, eerdere stappen kunnen delen artefacten met latere stappen door de dezelfde werkmap op te geven. |

### <a name="examples-task-step-properties"></a>Voorbeelden: Stap taakeigenschappen

#### <a name="example-id"></a>Voorbeeld: id

Maak twee installatiekopieën, instanties van de installatiekopie van een functionele test. Elke stap wordt geïdentificeerd door een unieke `id` waarin andere stappen in de taak naar hun `when` eigenschap.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Voorbeeld: als

De `when` eigenschap geeft u de afhankelijkheid van een stap voor de andere stappen binnen de taak. Ondersteunt twee parameterwaarden:

* `when: ["-"]` -Geeft aan dat er geen afhankelijkheden van andere stappen. Het opgeven van een stap `when: ["-"]` , worden onmiddellijk worden uitgevoerd, en kunt u gelijktijdige stap worden uitgevoerd.
* `when: ["id1", "id2"]` -Hiermee geeft u de stap is afhankelijk van de stappen met `id` 'id1' en `id` 'id2'. Deze stap wordt niet worden uitgevoerd tot zowel "id1" en "id2" stappen worden voltooid.

Als `when` is niet opgegeven in een stap die stap is afhankelijk van de voltooiing van de vorige stap in de `acr-task.yaml` bestand.

Stap kan worden uitgevoerd zonder `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Stap kan worden uitgevoerd met `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Parallelle image build:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml --> [!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Parallelle image build en afhankelijke testen:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Variabelen voor de uitvoeren

ACR taken bevat een reeks variabelen die beschikbaar voor taakstappen zijn wanneer ze worden uitgevoerd. Deze variabelen zijn toegankelijk via de indeling `{{.Run.VariableName}}`, waarbij `VariableName` is een van de volgende:

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Voer&#46;ID

Elk uitgevoerd via `az acr run`, of op basis van de uitvoering van taken die zijn gemaakt via trigger `az acr task create` een unieke id hebben. De ID vertegenwoordigt de uitvoering die momenteel wordt uitgevoerd.

Doorgaans gebruikt voor een unieke labels een installatiekopie:

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

De volledig gekwalificeerde servernaam van het register. Doorgaans gebruikt voor het algemeen verwijzen naar het register waar de taak wordt uitgevoerd.

```yaml
version: 1.0-preview-1
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
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-configure]: /cli/azure/reference-index#az-configure