Wanneer u gegevensschijven toevoegt aan een Linux-VM, kan er fouten optreden als een schijf niet op LUN 0 bestaat. Als u een schijf met een handmatig toevoegt de `azure vm disk attach-new` opdracht en geeft u een LUN (`--lun`) in plaats van de Azure-platform om te bepalen van de juiste LUN toe te staan, zorgt die een schijf al bestaat / wordt aangelegd op LUN 0. 

Bekijk het volgende voorbeeld met een fragment van de uitvoer van `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

De twee gegevensschijven bestaat op LUN 0 en 1 LUN (de eerste kolom in de `lsscsi` uitvoer details `[host:channel:target:lun]`). Beide schijven moeten accessbile uit vanuit de virtuele machine. Als u had handmatig de eerste schijf moet worden toegevoegd op LUN 1 en de tweede schijf bij LUN 2 hebt opgegeven, ziet u mogelijk niet de schijven goed van binnen uw virtuele machine.

> [!NOTE]
> De Azure `host` waarde 5 in deze voorbeelden, maar dit kan variëren, afhankelijk van het type opslag dat u selecteert.
> 
> 

Dit gedrag schijf is niet een probleem met de Azure, maar het geval waarin de Linux-kernel volgen de SCSI-specificaties. Als de Linux-kernel de SCSI-bus op gekoppelde apparaten scant, moet u een apparaat gevonden voor LUN 0 in om het systeem om door te gaan met het scannen van extra apparaten. Als zodanig:

* Controleer de uitvoer van `lsscsi` na het toevoegen van een gegevensschijf om te controleren of er een schijf voor LUN 0.
* Als de schijf niet correct binnen uw virtuele machine weergegeven wordt, controleert u of dat een schijf op LUN 0 bestaat.

