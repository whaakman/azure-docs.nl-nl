---
title: Knooppunt-eenheidacties schalen in Azure Stack | Microsoft Docs
description: Informatie over het knooppuntstatus bekijken en gebruiken van de kracht van power uitschakelen, uitschakelen en hervatten van knooppunt-acties op een geïntegreerde Azure Stack-systeem.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/22/2019
ms.openlocfilehash: 24c500251c8e91b7542c5c9d3e77676205c88c1f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58090964"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Knooppunt-eenheidacties schalen in Azure Stack

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel wordt beschreven hoe u de status van een schaaleenheid bekijken. U vindt de eenheid van de knooppunten. U kunt uitvoeren van knooppunt acties zoals power op power uitschakelen, afsluiten, leegmaken, hervatten en herstellen. Meestal gebruikt u deze acties knooppunt tijdens veld vervanging van onderdelen of voor het herstellen van een knooppunt.

> [!Important]  
> Alle knooppunt-acties die worden beschreven in dit artikel moeten gericht op één knooppunt tegelijk.

## <a name="view-the-node-status"></a>De knooppuntstatus weergeven

U kunt de status van een schaaleenheid en de bijbehorende knooppunten weergeven in de beheerdersportal.

Ga als volgt te werk om de status van een schaaleenheid weer te geven:

1. Op de **regiobeheer** tegel, selecteert u de regio.
2. Aan de linkerkant, onder **beschikken over infrastructurele resources**, selecteer **schaaleenheden**.
3. Selecteer in de resultaten de schaaleenheid.
4. Aan de linkerkant, onder **algemene**, selecteer **knooppunten**.

   Bekijk de volgende informatie:

   - De lijst met afzonderlijke knooppunten
   - Operationele Status (Zie onderstaande lijst)
   - Energiebeheer-Status (actief of gestopt)
   - server-model
   - IP-adres van de BMC (baseboard management controller)
   - Totale aantal kernen
   - totale hoeveelheid geheugen

![status van een schaaleenheid](media/azure-stack-node-actions/multinodeactions.png)

### <a name="node-operational-states"></a>Operationele status van knooppunt

| Status | Description |
|----------------------|-------------------------------------------------------------------|
| In uitvoering | Het knooppunt is actief deelnemer in de schaaleenheid. |
| Gestopt | Het knooppunt is niet beschikbaar. |
| Toevoegen | Het knooppunt wordt actief toegevoegd aan de schaaleenheid. |
| Bezig met het herstellen van | Het knooppunt wordt actief hersteld. |
| Onderhoud | Het knooppunt wordt onderbroken en geen actieve gebruiker-werkbelasting wordt uitgevoerd. |
| Herstel is vereist | Een fout is aangetroffen die vereist dat het knooppunt moet worden hersteld. |

## <a name="scale-unit-node-actions"></a>Schaalacties eenheid knooppunt

Als u informatie over een scale unit-knooppunt bekijken, kunt u ook knooppunt acties uitvoeren zoals:
 - Starten en stoppen (afhankelijk van de huidige status van de power)
 - Uitschakelen en hervat (afhankelijk van de operations-status)
 - Herstellen
 - Afsluiten

De operationele status van het knooppunt bepaalt welke opties beschikbaar zijn.

U moet Azure Stack-PowerShell-modules installeren. Deze cmdlets worden de **Azs.Fabric.Admin** module. Als u wilt installeren of Controleer of de installatie van PowerShell voor Azure Stack, Zie [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md).

## <a name="stop"></a>Stoppen

De **stoppen** actie schakelt het knooppunt. Het is hetzelfde als wanneer u op de knop drukt. Het biedt een afsluitingssignaal verzenden naar het besturingssysteem. Voor geplande stop-bewerkingen, probeer altijd het afsluiten is eerst. 

Deze actie wordt doorgaans gebruikt wanneer een knooppunt vastgelopen is en niet meer op aanvragen reageert.

De stopactie wilt uitvoeren, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

```PowerShell  
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

In het onwaarschijnlijke geval dat de actie bij stoppen niet werkt, probeer het opnieuw en als er een tweede keer mislukt de BMC webinterface gebruiken in plaats daarvan.

Zie voor meer informatie, [Stop-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/stop-azsscaleunitnode).

## <a name="start"></a>Starten

De **start** actie Hiermee schakelt u het knooppunt. Het is hetzelfde als wanneer u op de knop drukt. 
 
Als u wilt uitvoeren met de actie beginnen, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

```PowerShell  
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

In het onwaarschijnlijke geval dat de startactie niet werkt, probeer het opnieuw en als er een tweede keer mislukt de BMC webinterface gebruiken in plaats daarvan.

Zie voor meer informatie, [Start AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/start-azsscaleunitnode).

## <a name="drain"></a>Clusterbesturingssysteem

De **leegmaken** actie verplaatst u alle actieve werkbelastingen naar de resterende knooppunten in die specifieke schaaleenheid.

Deze actie wordt doorgaans gebruikt tijdens het veld vervanging van onderdelen, zoals het vervangen van een heel knooppunt.

> [!Important]
> Zorg ervoor dat u een verwerkingsstop-bewerking op een knooppunt tijdens gepland onderhoud, waar gebruikers zijn aangemeld. Onder bepaalde omstandigheden kunnen actieve werklasten onderbrekingen optreden.

De actie van het clusterbesturingssysteem wilt uitvoeren, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

```PowerShell  
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Zie voor meer informatie, [uitschakelen AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/disable-azsscaleunitnode).

## <a name="resume"></a>Hervatten

De **hervatten** actie wordt een uitgeschakelde knooppunt hervat en wordt er aangegeven actief is voor plaatsing van werkbelastingen. Eerdere workloads die op het knooppunt werden uitgevoerd mislukken niet terug. (Als u een verwerkingsstop-bewerking op een knooppunt moet uitschakelen. Wanneer u het knooppunt opnieuw op, deze niet is gemarkeerd als actief is voor plaatsing van werkbelastingen. Wanneer u klaar bent, moet u de actie doorgaan naar het knooppunt als actief gemarkeerd.)

Als u wilt uitvoeren met de actie hervatten, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

```PowerShell  
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Zie voor meer informatie, [inschakelen AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/enable-azsscaleunitnode).

## <a name="repair"></a>Herstellen

De **herstellen** actie herstelt een knooppunt. Deze alleen gebruikt voor een van de volgende scenario's:
 - Volledige knooppunt vervanging (met of zonder nieuwe gegevensschijven)
 - Na onderdeelfouten hardware- en vervangingsdimensie (als op de hoogte in de documentatie van het veld (FRU) replaceable unit).

> [!Important]  
> Raadpleeg uw OEM-hardwareleverancier FRU-documentatie voor de exacte stappen wanneer u nodig hebt om een knooppunt of afzonderlijke hardware-onderdelen te vervangen. De documentatie FRU wordt aangegeven of u de herstelactie moet na het vervangen van een hardware-onderdeel uitvoeren. 

Wanneer u de herstelactie uitvoert, moet u het BMC IP-adres opgeven. 

Als u wilt de herstelactie uitvoeren, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

  ```PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPv4Address <BMCIPv4Address>
  ```

## <a name="shutdown"></a>Afsluiten

De **afsluiten** actie eerste verplaatst alle actieve werkbelastingen naar de resterende knooppunten in de dezelfde schaaleenheid. Vervolgens de actie zonder problemen wordt afgesloten het scale unit-knooppunt.

Nadat u een knooppunt dat afgesloten is begint, moet u uitvoeren de [hervatten](#resume) actie. Eerdere workloads die op het knooppunt werden uitgevoerd mislukken niet terug.

Als het afsluiten is mislukt, probeert de [leegmaken](#drain) bewerking gevolgd door het afsluiten is.

De afsluitactie wilt uitvoeren, open een verhoogde PowerShell-prompt en voer de volgende cmdlet uit:

  ```PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -Shutdown
  ```



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure Stack-infrastructuur beheerder-module, [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.6.0).
