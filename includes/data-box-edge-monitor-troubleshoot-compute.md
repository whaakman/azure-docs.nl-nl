---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 7058d7f46373f8adaacbcbf90e5ea591a15f8f37
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58404072"
---
Opdrachten zijn beschikbaar om te controleren of het oplossen van modules op een Data Box-Edge-apparaat dat de compute-functie die is geconfigureerd, heeft een subset van docker. Voor een overzicht van de beschikbare opdrachten [verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface) en gebruik de `dkrdbe` functie.

```powershell
[10.100.10.10]: PS>dkrdbe -?
Usage: dkrdbe COMMAND

Commands:
   image [prune]
   images
   inspect
   login
   logout
   logs
   port
   ps
   pull
   start
   stats
   stop
   system [df]
   top

[10.100.10.10]: PS>
```
De volgende tabel bevat een korte beschrijving van de opdrachten die beschikbaar zijn voor `dkrdbe`:

|command  |Description |
|---------|---------|
|`image`     | Installatiekopieën beheren       |
|`images`     | Installatiekopieën vermelden         |
|`inspect`     | Gegevens op laag niveau op Docker objecten retourneren         |
|`login`     | Aanmelden bij een Docker-register         |
|`logout`     | Meld u af van een Docker-register         |
|`logs`     | De logboeken van een container ophalen        |
|`port`     | Poorttoewijzingen voor lijst of een specifieke toewijzing voor de container        |
|`ps`     | Containers weergeven        |
|`pull`     | Een installatiekopie of een opslagplaats van een register ophalen         |
|`start`     | Een of meer van de gestopte containers starten         |
|`stats`     | Een live stream van gebruiksstatistieken voor resource (s) weergegeven         |
|`stop`     | Een of meer actieve containers te stoppen        |
|`system`     | Docker beheren         |
|`top`     | De actieve processen van een container weergeven         |

Als u Help-informatie voor alle beschikbare opdrachten, gebruikt `dkrdbe <command-name> --help`.

Bijvoorbeeld, om te begrijpen van het gebruik van de `port` typen:

```powershell
[10.100.10.10]: P> dkrdbe port --help

Usage:  dkr port CONTAINER [PRIVATE_PORT[/PROTO]]

List port mappings or a specific mapping for the container
[10.100.10.10]: P> dkrdbe login --help

Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
[10.100.10.10]: PS>
```

De beschikbare opdrachten voor de `dkrdbe` functie dezelfde parameters gebruiken als de die worden gebruikt voor de normale docker-opdrachten. Voor de opties en parameters die worden gebruikt met de opdracht docker, gaat u naar [gebruikt u de Docker-opdrachtregel](https://docs.docker.com/engine/reference/commandline/docker/).

### <a name="to-check-if-the-module-deployed-successfully"></a>Om te controleren of de module is geïmplementeerd

COMPUTE-modules zijn containers die een zakelijke logica geïmplementeerd. Uitvoeren om te zien als een compute-module is geïmplementeerd, de `ps` beheren en controleren of de container (overeenkomt met de compute-module) wordt uitgevoerd.

Aan de lijst met alle containers (met inbegrip van degene die zijn onderbroken), voert u de `ps -a` opdracht.

```powershell
[10.100.10.10]: P> dkrdbe ps -a
CONTAINER ID        IMAGE                                                COMMAND                   CREATED             STATUS              PORTS                                                                  NAMES
d99e2f91d9a8        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  movefile
0a06f6d605e9        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  filemove
2f8a36e629db        mcr.microsoft.com/azureiotedge-hub:1.0               "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days           0.0.0.0:443->443/tcp, 0.0.0.0:5671->5671/tcp, 0.0.0.0:8883->8883/tcp   edgeHub
acce59f70d60        mcr.microsoft.com/azureiotedge-agent:1.0             "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days                                                                                  edgeAgent
[10.100.10.10]: PS>
```

Als er een fout opgetreden bij het maken van de containerinstallatiekopie of is tijdens het ophalen van de installatiekopie, voert u `logs edgeAgent`.  `EdgeAgent` de IoT Edge-runtime-container die verantwoordelijk is voor het inrichten van andere containers is.

Omdat `logs edgeAgent` dumpen van alle logboeken, een goede manier om te zien van de recente fouten is het gebruik van de optie `--tail 20`.


```powershell
[10.100.10.10]: PS>dkrdbe logs edgeAgent --tail 20
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Util.Uds.HttpUdsMessageHandler] - Connected socket /var/run/iotedge/mgmt.sock
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Util.Uds.HttpUdsMessageHandler] - Sending request http://mgmt.sock/modules?api-version=2018-06-28
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Agent] - Getting edge agent config...
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Agent] - Obtained edge agent config
2019-02-28 23:38:23.469 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Edgelet.ModuleManagementHttpClient] - Received a valid Http response from unix:///var/run/iotedge/mgmt.soc
k for List modules
--------------------CUT---------------------
--------------------CUT---------------------
08:28.1007774+00:00","restartCount":0,"lastRestartTimeUtc":"2019-02-26T20:08:28.1007774+00:00","runtimeStatus":"running","version":"1.0","status":"running","restartPolicy":"always
","type":"docker","settings":{"image":"edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64","imageHash":"sha256:47778be0602fb077d7bc2aaae9b0760fbfc7c058bf4df192f207ad6cbb96f7cc","c
reateOptions":"{\"HostConfig\":{\"Binds\":[\"/home/hcsshares/share4-dl460:/home/input\",\"/home/hcsshares/share4-iot:/home/output\"]}}"},"env":{}}
2019-02-28 23:38:28.480 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Planners.HealthRestartPlanner] - HealthRestartPlanner created Plan, with 0 command(s).
```

### <a name="to-get-container-logs"></a>Containerlogbestanden ophalen

Logboeken voor een specifieke container ophalen, lijst eerst met de container en vervolgens de logboeken ophalen voor de container waarin u geïnteresseerd bent.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Als u de lijst met actieve containers, voer de `ps` opdracht.

    ```powershell
    [10.100.10.10]: P> dkrdbe ps
    CONTAINER ID        IMAGE                                                COMMAND                   CREATED             STATUS              PORTS                                                                  NAMES
    d99e2f91d9a8        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  movefile
    0a06f6d605e9        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  filemove
    2f8a36e629db        mcr.microsoft.com/azureiotedge-hub:1.0               "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days           0.0.0.0:443->443/tcp, 0.0.0.0:5671->5671/tcp, 0.0.0.0:8883->8883/tcp   edgeHub
    acce59f70d60        mcr.microsoft.com/azureiotedge-agent:1.0             "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days                                                                                  edgeAgent
    ```

3. Maak een notitie van de container-ID voor de container die u moet de logboeken voor.

4. Als u de logboeken voor een specifieke container, voer de `logs` opdracht geven de container-ID.

    ```powershell
    [10.100.10.10]: PS>dkrdbe logs d99e2f91d9a8
    02/26/2019 18:21:45: Info: Opening module client connection.
    02/26/2019 18:21:46: Info: Initializing with input: /home/input, output: /home/output.
    02/26/2019 18:21:46: Info: IoT Hub module client initialized.
    02/26/2019 18:22:24: Info: Received message: 1, SequenceNumber: 0 CorrelationId: , MessageId: 081886a07e694c4c8f245a80b96a252a Body: [{"ChangeType":"Created","ShareRelativeFilePath":"\\__Microsoft Data Box Edge__\\Upload\\Errors.xml","ShareName":"share4-dl460"}]
    02/26/2019 18:22:24: Info: Moving input file: /home/input/__Microsoft Data Box Edge__/Upload/Errors.xml to /home/output/__Microsoft Data Box Edge__/Upload/Errors.xml
    02/26/2019 18:22:24: Info: Processed event.
    02/26/2019 18:23:38: Info: Received message: 2, SequenceNumber: 0 CorrelationId: , MessageId: 30714d005eb048e7a4e7e3c22048cf20 Body: [{"ChangeType":"Created","ShareRelativeFilePath":"\\f [10]","ShareName":"share4-dl460"}]
    02/26/2019 18:23:38: Info: Moving input file: /home/input/f [10] to /home/output/f [10]
    02/26/2019 18:23:38: Info: Processed event.
    ```

### <a name="to-monitor-the-usage-statistics-of-the-device"></a>Voor het bewaken van de gebruiksstatistieken van het apparaat

U kunt het geheugen, CPU-gebruik en i/o-controleren op het apparaat met de `stats` opdracht.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Voer de `stats` opdracht om de live stream uitschakelen en alleen het eerste resultaat ophalen.

   ```powershell
   dkrdbe stats --no-stream
   ```

   Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

    ```
    [10.100.10.10]: P> dkrdbe stats --no-stream
    CONTAINER ID        NAME          CPU %         MEM USAGE / LIMIT     MEM %         NET I/O             BLOCK I/O           PIDS
    d99e2f91d9a8        movefile      0.0           24.4MiB / 62.89GiB    0.04%         751kB / 497kB       299kB / 0B          14
    0a06f6d605e9        filemove      0.00%         24.11MiB / 62.89GiB   0.04%         679kB / 481kB       49.5MB / 0B         14
    2f8a36e629db        edgeHub       0.18%         173.8MiB / 62.89GiB   0.27%         4.58MB / 5.49MB     25.7MB / 2.19MB     241
    acce59f70d60        edgeAgent     0.00%         35.55MiB / 62.89GiB   0.06%         2.23MB / 2.31MB     55.7MB / 332kB      14
    [10.100.10.10]: PS>
    ```

