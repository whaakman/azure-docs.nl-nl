Bij het toevoegen van gegevensschijven met een Linux VM, kan er fouten optreden als een schijf niet op LUN 0 bestaat. Als u een schijf handmatig met behulp van toevoegt de `azure vm disk attach-new` opdracht en geeft u een LUN (`--lun`) in plaats van het Azure-platform om te bepalen van de juiste LUN toe te staan, Let erop dat een schijf al bestaat / wordt aangelegd op LUN 0. 

Houd rekening met het volgende voorbeeld met een fragment van de uitvoer van `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

De twee gegevensschijven tegelijk LUN 0 en LUN 1 (de eerste kolom in de `lsscsi` uitvoer details `[host:channel:target:lun]`). Beide schijven moeten accessbile uit vanuit de virtuele machine. Als u had handmatig de eerste schijf moet worden toegevoegd op LUN 1 en de tweede schijf op LUN 2 hebt opgegeven, ziet u mogelijk niet de schijven goed uit vanuit de virtuele machine.

> [!NOTE]
> De Azure `host` waarde is ingesteld op 5 in deze voorbeelden, maar dit kan variëren afhankelijk van het type opslag die u selecteert.
> 
> 

Dit gedrag van de schijf is niet een probleem met Azure, maar de manier waarop de Linux-kernel volgt de SCSI-specificaties. Als de Linux-kernel de SCSI-bus gekoppelde apparaten scant, kan een apparaat moet worden gevonden op LUN 0 in om het systeem om door te gaan met het scannen op aanvullende apparaten. Als zodanig:

* Controleer de uitvoer van `lsscsi` na het toevoegen van een gegevensschijf om te controleren of u een schijf hebt in LUN 0.
* Als de schijf niet wordt weergegeven correct binnen uw virtuele machine, controleert u of dat er bestaat een schijf in LUN 0.

