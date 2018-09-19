---
title: 'Azure AD Connect sync: Scheduler | Microsoft Docs'
description: Dit onderwerp beschrijft de functie ingebouwde scheduler in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d8deb03d03446c1452d73a7c08df4cf14ffcd5b5
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46311022"
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect sync: Scheduler
In dit onderwerp beschrijft de ingebouwde scheduler in Azure AD Connect-synchronisatie (ook wel) synchronisatie-engine).

Deze functie is ingevoerd in de build 1.1.105.0 (uitgebracht februari 2016).

## <a name="overview"></a>Overzicht
Azure AD Connect-synchronisatie wijzigingen optreden in uw on-premises directory met behulp van een scheduler synchroniseren. Er zijn twee manieren laden scheduler, een voor Wachtwoordsynchronisatie en andere voor object of kenmerk synchronisatie en het onderhoud taken. In dit onderwerp bevat informatie over de laatste.

In eerdere versies is de planner voor objecten en kenmerken buiten de synchronisatie-engine. Het Windows Taakplanner of een afzonderlijke Windows-service gebruikt voor het activeren van het proces van synchronisatie. De scheduler wordt met de ingebouwde 1.1 versies voor de synchronisatie-engine en sta aangepast. De nieuwe standaard Synchronisatiefrequentie is 30 minuten.

De scheduler is verantwoordelijk voor twee taken:

* **Synchronisatiecyclus**. Het proces om te importeren en exporteren van wijzigingen synchroniseren.
* **Onderhoudstaken**. Vernieuwen van sleutels en certificaten voor wachtwoord opnieuw instellen en Device Registration Service (DRS). Oude vermeldingen in de operations-logboek opschonen.

De scheduler zelf altijd wordt uitgevoerd, maar deze kan worden geconfigureerd voor één of geen van deze taken alleen worden uitgevoerd. Bijvoorbeeld, als u nodig hebt om uw eigen cyclus synchronisatieproces, kunt u uitschakelen van deze taak in de scheduler maar nog steeds de onderhoudstaak worden uitgevoerd.

## <a name="scheduler-configuration"></a>Scheduler-configuratie
Als u wilt zien van uw huidige configuratie-instellingen, gaat u naar PowerShell en voer `Get-ADSyncScheduler`. U ziet er ongeveer als deze afbeelding:

![GetSyncScheduler](./media/how-to-connect-sync-feature-scheduler/getsynccyclesettings2016.png)

Als u ziet **de synchronisatieopdracht of de cmdlet is niet beschikbaar** wanneer u deze cmdlet uitvoert, klikt u vervolgens de PowerShell-module is niet geladen. Dit probleem kan optreden als u Azure AD Connect wordt uitgevoerd op een domeincontroller of op een server met een hogere mate van PowerShell beperking dan de standaardinstellingen. Als u deze fout ziet, voert u `Import-Module ADSync` de cmdlet om beschikbaar te maken.

* **AllowedSyncCycleInterval**. Het kortste tijdsinterval tussen synchronisatie cycli toegestaan door Azure AD. U vaker dan deze instelling kan niet worden gesynchroniseerd en nog steeds worden ondersteund.
* **CurrentlyEffectiveSyncCycleInterval**. De planning momenteel van kracht. Deze heeft dezelfde waarde als CustomizedSyncInterval (indien ingesteld) als deze niet hoger zijn dan AllowedSyncInterval. Als u een build voordat 1.1.281 gebruiken en u CustomizedSyncCycleInterval wijzigt, wordt deze wijziging van kracht na de volgende synchronisatiecyclus. Van build 1.1.281 wordt de wijziging direct van kracht.
* **CustomizedSyncCycleInterval**. Als u wilt dat de scheduler om uit te voeren op een andere frequentie dan de standaardwaarde van 30 minuten, configureert u deze instelling. In de bovenstaande afbeelding is de planner is ingesteld op in plaats daarvan wordt elk uur uitgevoerd. Als u deze instelling op een waarde lager dan AllowedSyncInterval instelt, wordt deze gebruikt.
* **NextSyncCyclePolicyType**. Delta- of eerste. Hiermee definieert u of de volgende uitvoering alleen proces nog deltawijzigingen moet, of de volgende uitvoering moet doen als een volledige importeren en synchroniseren. De laatste zou ook een nieuwe of gewijzigde regels verwerken.
* **NextSyncCycleStartTimeInUTC**. Volgende keer dat de scheduler begint de volgende synchronisatiecyclus.
* **PurgeRunHistoryInterval**. De tijd logboeken voor bewerkingen moeten worden opgeslagen. Deze logboeken kunnen worden gecontroleerd in synchronization servicemanager. De standaardwaarde is dat deze logboeken 7 dagen.
* **SyncCycleEnabled**. Geeft aan of de scheduler de import, synchronisatie en processen voor exporteren wordt uitgevoerd als onderdeel van de werking ervan.
* **MaintenanceEnabled**. Geeft aan of het onderhoudsproces voor is ingeschakeld. Het bijwerken van de certificaten/sleutels en Hiermee verwijdert u de operations-logboek.
* **StagingModeEnabled**. Laat zien als [faseringsmodus](how-to-connect-sync-operations.md#staging-mode) is ingeschakeld. Als deze instelling is ingeschakeld, klikt u vervolgens het onderdrukt de uitvoer die worden uitgevoerd, maar nog steeds import en synchronisatie worden uitgevoerd.
* **SchedulerSuspended**. Ingesteld door Connect tijdens een upgrade voor het tijdelijk blokkeren de scheduler wordt uitgevoerd.

U kunt sommige van deze instellingen met wijzigen `Set-ADSyncScheduler`. De volgende parameters kunnen worden gewijzigd:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

In eerdere versies van Azure AD Connect, **isStagingModeEnabled** in Set-ADSyncScheduler is blootgesteld. Het is **niet-ondersteunde** deze eigenschap in te stellen. De eigenschap **SchedulerSuspended** moet alleen worden gewijzigd door Connect. Het is **niet-ondersteunde** rechtstreeks instellen met PowerShell.

De configuratie van de scheduler wordt opgeslagen in Azure AD. Als u een tijdelijke server hebt, zijn geen wijzigingen in de primaire server ook van invloed op de staging-server (met uitzondering van IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxis: `Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dagen, uu - uren, mm - minuten, ss - seconden

Voorbeeld: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Hiermee wijzigt u de scheduler elke drie uur uitgevoerd.

Voorbeeld: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Wijzigingen wijzigen met de scheduler om dagelijks uitgevoerd te.

### <a name="disable-the-scheduler"></a>De scheduler uitschakelen  
Als u configuratiewijzigingen aanbrengen wilt, klikt u vervolgens wilt u uitschakelen met de scheduler. Bijvoorbeeld, wanneer u [filtering configureren](how-to-connect-sync-configure-filtering.md) of [wijzigingen aanbrengen in synchronisatieregels](how-to-connect-sync-change-the-configuration.md).

Schakel de scheduler uitvoeren `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![De scheduler uitschakelen](./media/how-to-connect-sync-feature-scheduler/schedulerdisable.png)

Wanneer u uw wijzigingen hebt aangebracht, vergeet niet om in te schakelen van de scheduler opnieuw met `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-the-scheduler"></a>De scheduler starten
De scheduler wordt standaard elke 30 minuten uitgevoerd. In sommige gevallen wilt u mogelijk een synchronisatiecyclus tussen de geplande cycli uitvoeren of u wilt uitvoeren van een ander type.

**Delta-synchronisatiecyclus**  
Een delta-synchronisatiecyclus bevat de volgende stappen uit:

* Delta-import op alle Connectors
* Deltasynchronisatie op alle Connectors
* Op alle Connectors exporteren

Kan het zijn dat u hebt een urgent wijzigen die onmiddellijk moet worden gesynchroniseerd daarom moet u een cyclus handmatig uitvoeren. Als u nodig hebt om uit te voeren een cyclus handmatig vervolgens vanuit PowerShell uitvoeren `Start-ADSyncSyncCycle -PolicyType Delta`.

**Volledige synchronisatiecyclus**  
Als u een van de volgende configuratiewijzigingen hebt aangebracht, moet u uit te voeren een volledige synchronisatie (ook wel) Eerste):

* Meer objecten of kenmerken moeten worden geïmporteerd uit een bronmap toegevoegd
* Wijzigingen aangebracht in de synchronisatieregels
* Gewijzigd [filteren](how-to-connect-sync-configure-filtering.md) , zodat een ander aantal objecten opgenomen worden moet

Als u een van deze wijzigingen hebt aangebracht, moet u een volledige synchronisatiecyclus uitvoeren, zodat de synchronisatie-engine de mogelijkheid heeft om het opnieuw samenvoegen van de connector-ruimten. Een volledige synchronisatiecyclus bevat de volgende stappen uit:

* Volledige Import op alle Connectors
* Volledige synchronisatie van alle Connectors
* Op alle Connectors exporteren

Voor het starten van de cyclus van een volledige synchronisatie uitvoeren `Start-ADSyncSyncCycle -PolicyType Initial` vanuit een PowerShell-prompt. Met deze opdracht start een volledige synchronisatiecyclus.

## <a name="stop-the-scheduler"></a>De scheduler stoppen
Als een synchronisatiecyclus wordt momenteel door de scheduler worden uitgevoerd, moet u mogelijk om deze te stoppen. Bijvoorbeeld als u de installatiewizard te starten en u deze foutmelding krijgt:

![SyncCycleRunningError](./media/how-to-connect-sync-feature-scheduler/synccyclerunningerror.png)

Wanneer een synchronisatiecyclus wordt uitgevoerd, kunt u wijzigingen in de configuratie kan niet maken. U kunt wachten totdat de scheduler het proces is voltooid, maar u ook dat deze voorkomen kunt, zodat u onmiddellijk uw wijzigingen kunt aanbrengen. De huidige cyclus stoppen, is geen schadelijke en wijzigingen in behandeling worden verwerkt door de volgende keer wordt uitgevoerd.

1. Gestart door de scheduler om te stoppen van de huidige cyclus met de PowerShell-cmdlet te vertellen `Stop-ADSyncSyncCycle`.
2. Als u een build voordat 1.1.281, stopt klikt u vervolgens het stoppen van de scheduler niet de huidige Connector van de huidige taak. Als u wilt afdwingen dat de Connector om te stoppen, de volgende acties uitvoeren: ![StopAConnector](./media/how-to-connect-sync-feature-scheduler/stopaconnector.png)
   * Start **Synchronization Service** vanuit het startmenu. Ga naar **Connectors**, markeert u de Connector met de status **met**, en selecteer **stoppen** uit de acties.

De scheduler nog steeds actief is en start opnieuw op de eerstvolgende gelegenheid installeren.

## <a name="custom-scheduler"></a>Aangepaste scheduler
De cmdlets die zijn beschreven in deze sectie zijn alleen beschikbaar in de build [1.1.130.0](reference-connect-version-history.md#111300) en hoger.

Als de ingebouwde scheduler niet voldoet aan uw vereisten, kunt u de Connectors die met behulp van PowerShell kunt plannen.

### <a name="invoke-adsyncrunprofile"></a>Invoke-ADSyncRunProfile
Voor een Connector op deze manier kunt u een profiel starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

De namen te gebruiken voor [Connector namen](how-to-connect-sync-service-manager-ui-connectors.md) en [profielnamen uitvoeren](how-to-connect-sync-service-manager-ui-connectors.md#configure-run-profiles) kunt u vinden in de [Synchronization Service Manager UI](how-to-connect-sync-service-manager-ui.md).

![Uitvoeringsprofiel aanroepen](./media/how-to-connect-sync-feature-scheduler/invokerunprofile.png)  

De `Invoke-ADSyncRunProfile` cmdlet synchroon is, dat wil zeggen, deze niet terug besturingselement totdat de Connector de bewerking is voltooid, met of zonder een fout.

Wanneer u uw Connectors plant, is de aanbeveling om ze in de volgende volgorde:

1. (Volledige/Delta) Importeren uit on-premises adreslijsten, zoals Active Directory
2. (Volledige/Delta) Importeren uit Azure AD
3. (Volledige/Delta) Synchronisatie van on-premises adreslijsten, zoals Active Directory
4. (Volledige/Delta) Synchronisatie van Azure AD
5. Exporteren naar Azure AD
6. Exporteren naar on-premises adreslijsten, zoals Active Directory

Deze volgorde is hoe de Connectors in de ingebouwde scheduler wordt uitgevoerd.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
U kunt ook de synchronisatie-engine om te controleren of het is bezet of niet-actieve bewaken. Deze cmdlet retourneert een lege verzameling resultaten als de synchronisatie-engine niet actief is en niet een Connector wordt uitgevoerd. Als een Connector wordt uitgevoerd, retourneert deze de naam van de Connector.

```
Get-ADSyncConnectorRunStatus
```

![Uitvoeringsstatus van de connector](./media/how-to-connect-sync-feature-scheduler/getconnectorrunstatus.png)  
In de bovenstaande afbeelding is de eerste regel van een status waarbij de synchronisatie-engine niet actief is. De tweede regel uit als de Azure AD-Connector wordt uitgevoerd.

## <a name="scheduler-and-installation-wizard"></a>Wizard Scheduler en installatie
Als u de installatiewizard wordt gestart, klikt u vervolgens de planning tijdelijk onderbroken. Dit gedrag is omdat deze aangenomen dat wordt u configuratiewijzigingen aanbrengt en deze instellingen kunnen niet worden toegepast als de synchronisatie-engine actief wordt uitgevoerd. Om deze reden niet laat de installatiewizard openen omdat deze de synchronisatie-engine stopt van het uitvoeren van acties die synchronisatie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
