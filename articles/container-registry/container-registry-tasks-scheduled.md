---
title: Azure Container Registry taken plannen
description: Timers een Azure Container Registry-taak uitvoeren op een ingesteld schema instellen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/27/2019
ms.author: danlep
ms.openlocfilehash: a1123a30025f9be6e994e69703f5ee1aa05d1b49
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509701"
---
# <a name="run-an-acr-task-on-a-defined-schedule"></a>Een ACR-taak uitvoeren op een ingesteld schema

Dit artikel laat u het uitvoeren van een [ACR taak](container-registry-tasks-overview.md) volgens een schema. Een taak door het instellen van een of meer *timeractiveringen*. 

Plannen van een taak is handig voor scenario's als volgt uit:

* Een container-workload voor gepland onderhoudsbewerkingen uitvoeren. Voer bijvoorbeeld een container geplaatste app als u wilt verwijderen van overbodige installatiekopieën uit het register.
* Een set van tests uitvoeren op een productie-installatiekopie tijdens de werkdag als onderdeel van de bewaking van uw live-site.

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om uit te voeren in de voorbeelden in dit artikel. Als u wilt gebruiken op het bestand lokaal op, versie 2.0.68 of hoger vereist is. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.


## <a name="about-scheduling-a-task"></a>Over het plannen van een taak

* **Trigger met de cron-expressie** -de timertrigger voor een taak maakt gebruik van een *cron-expressie*. De expressie is een tekenreeks met vijf velden op te geven de minuut, uur, dag, maand en dag van week voor het activeren van de taak. Frequenties van maximaal één keer per minuut worden ondersteund. 

  Bijvoorbeeld, de expressie `"0 12 * * Mon-Fri"` activeert een taak op twaalf uur 's middags UTC op elke weekdag. Zie [details](#cron-expressions) verderop in dit artikel.
* **Meerdere timeractiveringen** -meerdere timers toe te voegen aan een taak is toegestaan, zolang de schema's verschillen. 
    * Meerdere timeractiveringen opgeven wanneer u de taak maakt, of deze later toevoegen.
    * Naam (optioneel) de triggers voor eenvoudiger beheer, of ACR taken Triggernamen die standaard biedt.
    * Als de timer planningen elkaar tegelijk overlappen, ACR taken wordt de taak op het geplande tijdstip geactiveerd voor elke timer. 
* **Andere taaktriggers** -In een taak timer wordt geactiveerd, kunt u ook triggers op basis van inschakelen [bron doorvoercode](container-registry-tutorial-build-task.md) of [baseren updates van installatiekopieën van](container-registry-tutorial-base-image-update.md). Net als andere taken ACR, kunt u ook [handmatig activeren][az-acr-task-run] een geplande taak.

## <a name="create-a-task-with-a-timer-trigger"></a>Een taak maken met een trigger voor timer

Wanneer u een taak met maakt de [az acr-taak maken][az-acr-task-create] opdracht, kunt u optioneel een timertrigger toevoegen. Voeg de `--schedule` parameter en geeft u een cron-expressie voor de timer. 

Als een eenvoudig voorbeeld de volgende opdracht triggers met de `hello-world` installatiekopie van Docker Hub elke dag om 21:00 UTC. De taak wordt uitgevoerd zonder de context van een bron-code.

```azurecli
az acr task create \
  --name mytask \
  --registry myregistry \
  --context /dev/null \
  --cmd hello-world \
  --schedule "0 21 * * *"
```

Voer de [az acr taak weergeven][az-acr-task-show] opdracht om te zien dat de timertrigger is geconfigureerd. Standaard wordt de basisinstallatiekopie update-trigger ook ingeschakeld.

```console
$ az acr task show --name mytask --registry registry --output table
NAME      PLATFORM    STATUS    SOURCE REPOSITORY       TRIGGERS
--------  ----------  --------  -------------------     -----------------
mytask    linux       Enabled                           BASE_IMAGE, TIMER
```

Activeren van de taak handmatig met [az acr-taak uitvoeren][az-acr-task-run] om ervoor te zorgen dat deze juist is ingesteld:

```azurecli
az acr task run --name mytask --registry myregistry
```

Als de container wordt uitgevoerd, wordt de uitvoer is vergelijkbaar met het volgende:

```console
Queued a run with ID: cf2a
Waiting for an agent...
2019/06/28 21:03:36 Using acb_vol_2ca23c46-a9ac-4224-b0c6-9fde44eb42d2 as the home volume
2019/06/28 21:03:36 Creating Docker network: acb_default_network, driver: 'bridge'
[...]
2019/06/28 21:03:38 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

Na het geplande tijdstip, voert de [az acr list-wordt uitgevoerd][az-acr-task-list-runs] opdracht uit om te controleren dat de timer voor de taak geactiveerd zoals verwacht:

```azurecli
az acr task list runs --name mytask --registry myregistry --output table
``` 

Wanneer de timer geslaagd is, is uitvoer vergelijkbaar met het volgende:

```console
RUN ID    TASK     PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  -------- ----------  ---------  ---------  --------------------  ----------
[...]
cf2b      mytask   linux       Succeeded  Timer      2019-06-28T21:00:23Z  00:00:06
cf2a      mytask   linux       Succeeded  Manual     2019-06-28T20:53:23Z  00:00:06
```
            
## <a name="manage-timer-triggers"></a>Timer triggers beheren

Gebruik de [az acr taak timer][az-acr-task-timer] opdrachten voor het beheren van de timer triggers voor een ACR-taak.

### <a name="add-or-update-a-timer-trigger"></a>Toevoegen of bijwerken van een timertrigger

Nadat een taak is gemaakt, moet u eventueel een timertrigger toevoegen via de [az acr taak timer toevoegen][az-acr-task-timer-add] opdracht. Het volgende voorbeeld wordt de naam van een timer-trigger *timer2* naar *mytask* eerder hebt gemaakt. Deze timer is geactiveerd de taak elke dag om 10:30 UTC.

```azurecli
az acr task timer add \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 10 * * *"
```

De planning van een bestaande trigger bijwerken of wijzigen van de status ervan, met behulp van de [az acr taakupdate timer][az-acr-task-timer-update] opdracht. Bijvoorbeeld, werken de trigger met de naam *timer2* voor het activeren van de taak om 11:30 UTC:

```azurecli
az acr task timer update \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 11 * * *"
```

### <a name="list-timer-triggers"></a>Lijst met timer-triggers

De [az acr-takenlijst timer][az-acr-task-timer-list] opdracht geeft u de timer triggers voor een taak instellen:

```azurecli
az acr task timer list --name mytask --registry myregistry
```

Voorbeelduitvoer:

```JSON
[
  {
    "name": "timer2",
    "schedule": "30 11 * * *",
    "status": "Enabled"
  },
  {
    "name": "t1",
    "schedule": "0 21 * * *",
    "status": "Enabled"
  }
]
```

### <a name="remove-a-timer-trigger"></a>Verwijderen van een timertrigger 

Gebruik de [az acr taak timer verwijderen][az-acr-task-timer-remove] opdracht een timertrigger verwijderen uit een taak. Het volgende voorbeeld verwijdert u de *timer2* activeren van *mytask*:

```azurecli
az acr task timer remove \
  --name mytask \
  --registry myregistry \
  --timer-name timer2
```

## <a name="cron-expressions"></a>Cron-expressies

ACR-taken gebruikt de [NCronTab](https://github.com/atifaziz/NCrontab) bibliotheek met het interpreteren van de cron-expressies. Ondersteunde expressies in ACR taken heeft vijf vereiste velden van elkaar gescheiden door spaties:

`{minute} {hour} {day} {month} {day-of-week}`

De tijdzone die wordt gebruikt in combinatie met de cron-expressies is Coordinated Universal Time (UTC). Uren zijn in 24-uurs notatie.

> [!NOTE]
> ACR taken biedt geen ondersteuning voor de `{second}` of `{year}` veld cron-expressies. Als u een cron-expressie gebruikt in een ander systeem kopieert, moet u deze velden verwijderen als ze worden gebruikt.

Elk veld kan een van de volgende typen waarden hebben:

|Type  |Voorbeeld  |Wanneer ze worden geactiveerd  |
|---------|---------|---------|
|Een specifieke waarde |<nobr>"5 * * * *"</nobr>|elk uur op 5 minuten na het uur|
|Alle waarden (`*`)|<nobr>"* 5 * * *"</nobr>|elke minuut van het uur begin 5:00 UTC (60 keer per dag)|
|Een bereik (`-` operator)|<nobr>"0 1-3 * * *"</nobr>|3 keer per dag om 1:00 uur, 2:00 en 3:00 UTC|  
|Een set waarden (`,` operator)|<nobr>"20,30,40 * * * *"</nobr>|3 keer per uur, op 20 minuten, 30 minuten en 40 minuten na het uur|
|Een waarde voor interval (`/` operator)|<nobr>"*/10 * * * *"</nobr>|6 keer per uur, op 10 minuten, 20 minuten, enzovoort, na het uur

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="cron-examples"></a>Cron-voorbeelden

|Voorbeeld|Wanneer ze worden geactiveerd  |
|---------|---------|
|`"*/5 * * * *"`|om de vijf minuten|
|`"0 * * * *"`|één keer aan de bovenkant van elk uur|
|`"0 */2 * * *"`|elke twee uur|
|`"0 9-17 * * *"`|één keer per uur van 9:00-17:00 UTC|
|`"30 9 * * *"`|om 9:30 UTC elke dag|
|`"30 9 * * 1-5"`|om 9:30 UTC elke weekdag|
|`"30 9 * Jan Mon"`|om 9:30 UTC elke maandag in januari|


## <a name="next-steps"></a>Volgende stappen

Voor voorbeelden van taken is geactiveerd door doorvoeracties voor source code of een basisinstallatiekopie updates, bekijk de [ACR taken reeks](container-registry-tutorial-quick-task.md).



<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks


<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-timer]: /cli/azure/acr/task/timer
[az-acr-task-timer-add]: /cli/azure/acr/task/timer#az-acr-task-timer-add
[az-acr-task-timer-remove]: /cli/azure/acr/task/timer#az-acr-task-timer-remove
[az-acr-task-timer-list]: /cli/azure/acr/task/timer#az-acr-task-timer-list
[az-acr-task-timer-update]: /cli/azure/acr/task/timer#az-acr-task-timer-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task]: /cli/azure/acr/task
[azure-cli-install]: /cli/azure/install-azure-cli