---
title: 'Azure AD Connect-synchronisatie: Scheduler | Microsoft Docs'
description: Dit onderwerp beschrijft de functie ingebouwde scheduler in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 63f69756b3933fecdec75cc677e1098447e5b94e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect-synchronisatie: Scheduler
Dit onderwerp beschrijft de ingebouwde scheduler in Azure AD Connect-synchronisatie (ook synchronisatie-engine).

Deze functie is geïntroduceerd met build 1.1.105.0 (uitgebracht februari 2016).

## <a name="overview"></a>Overzicht
Azure AD Connect-synchronisatie synchroniseren veranderingen in uw lokale directory met behulp van een scheduler. Er zijn twee scheduler processen, één voor Wachtwoordsynchronisatie en één voor de synchronisatie en het onderhoud taken/het kenmerk object. Dit onderwerp worden de laatste.

In eerdere versies is de planner voor objecten en kenmerken extern voor de synchronisatie-engine. Deze Windows Taakplanner of een afzonderlijke Windows-service gebruikt voor het activeren van het synchronisatieproces. De planner is met de ingebouwde versies 1.1 op de synchronisatie-engine en enige aanpassing staan. De nieuwe standaard Synchronisatiefrequentie is 30 minuten.

De planner is verantwoordelijk voor twee taken:

* **Synchronisatiecyclus**. Het proces voor het importeren en exporteren van de wijzigingen synchroniseren.
* **Onderhoudstaken**. Vernieuwen van sleutels en certificaten voor wachtwoord opnieuw instellen en Device Registration Service (DRS). Oude vermeldingen in de operations-logboek opschonen.

De planner zelf altijd wordt uitgevoerd, maar deze kan worden geconfigureerd voor een of geen van deze taken alleen uitvoeren. Bijvoorbeeld als u uw eigen cyclus synchronisatieproces hebt moet, kunt u uitschakelen van deze taak in de planner maar de onderhoudstaak nog steeds worden uitgevoerd.

## <a name="scheduler-configuration"></a>Scheduler-configuratie
Uw huidige configuratie-instellingen, Ga naar PowerShell en voer `Get-ADSyncScheduler`. U ziet dat lijkt op deze afbeelding:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Als u ziet **de synchronisatieopdracht of de cmdlet is niet beschikbaar** wanneer u deze cmdlet uitvoert, klikt u vervolgens de PowerShell-module is niet geladen. Dit probleem kan optreden als u Azure AD Connect op een domeincontroller of op een server met hogere niveaus van PowerShell beperking dan de standaardinstellingen uitgevoerd. Als u deze fout ziet, voert u `Import-Module ADSync` beschikbaar maken van de cmdlet.

* **AllowedSyncCycleInterval**. Het kortste tijdsinterval tussen synchronisatie cycli toegestaan door Azure AD. U vaker dan deze instelling kan niet worden gesynchroniseerd en nog steeds worden ondersteund.
* **CurrentlyEffectiveSyncCycleInterval**. De planning op dat moment van kracht. Deze heeft dezelfde waarde als CustomizedSyncInterval (indien ingesteld) als dit niet vaker dan AllowedSyncInterval. Als u een build voordat 1.1.281 gebruiken en u CustomizedSyncCycleInterval wijzigt, wordt deze wijziging van kracht na de volgende synchronisatiecyclus. Vanaf build 1.1.281 wordt de wijziging direct van kracht.
* **CustomizedSyncCycleInterval**. Als u wilt dat de planner 30 minuten wordt uitgevoerd op een andere frequentie dan de standaardwaarde, configureert u deze instelling. In de bovenstaande afbeelding is de planner ingesteld op in plaats daarvan elk uur uitgevoerd. Als u deze instelling in een waarde die lager is dan AllowedSyncInterval instelt, wordt deze gebruikt.
* **NextSyncCyclePolicyType**. Delta of initiële. Definieert als u de volgende keer uitvoeren moet alleen proces nog deltawijzigingen, of de volgende keer uitvoeren moet doen als een volledige te importeren en synchroniseren. Nieuwe of gewijzigde regels zou ook verwerk deze opnieuw.
* **NextSyncCycleStartTimeInUTC**. Zodra de scheduler begint de volgende synchronisatiecyclus.
* **PurgeRunHistoryInterval**. De tijd bewerkingslogboeken moeten worden bewaard. Deze logboeken kunnen worden gecontroleerd in synchronization servicemanager. De standaardwaarde is dat deze logboeken 7 dagen.
* **SyncCycleEnabled**. Hiermee wordt aangegeven of de planner de import, synchronisatie en export processen wordt uitgevoerd als onderdeel van de werking ervan.
* **MaintenanceEnabled**. Hiermee wordt weergegeven als het onderhoudsproces is ingeschakeld. De certificaten/sleutels bijgewerkt en verwijdert de operations-logboek.
* **StagingModeEnabled**. Toont als [faseringsmodus](active-directory-aadconnectsync-operations.md#staging-mode) is ingeschakeld. Als deze instelling is ingeschakeld, wordt vervolgens het onderdrukt de uitvoer wordt uitgevoerd, maar nog steeds import en synchronisatie uitvoeren.
* **SchedulerSuspended**. Ingesteld door Connect tijdens een upgrade naar tijdelijk blok de scheduler wordt uitgevoerd.

U kunt sommige van deze instellingen met wijzigen `Set-ADSyncScheduler`. De volgende parameters kunnen worden gewijzigd:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

In eerdere versies van Azure AD Connect **isStagingModeEnabled** is weergegeven in de Set ADSyncScheduler. Het is **niet-ondersteunde** stelt deze eigenschap. De eigenschap **SchedulerSuspended** moet alleen worden gewijzigd door te verbinden. Het is **niet-ondersteunde** rechtstreeks instellen met PowerShell.

De scheduler-configuratie wordt opgeslagen in Azure AD. Als er een testserver is, elke wijziging in de primaire server ook van invloed op de testserver (met uitzondering van IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxis:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dagen, uu - uren, mm - minuten en ss - seconden

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Hiermee wijzigt u de planner voor elke drie uur uitgevoerd.

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Wijzigingen wijzigen de planner om dagelijks uitgevoerd te.

### <a name="disable-the-scheduler"></a>De planner uitschakelen  
Als u configuratiewijzigingen aanbrengen wilt, vervolgens wilt u de planner uitschakelen. Bijvoorbeeld, wanneer u [configureert filtering](active-directory-aadconnectsync-configure-filtering.md) of [wijzigingen aanbrengen in synchronisatieregels](active-directory-aadconnectsync-change-the-configuration.md).

Voer hiervoor de planner `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![De planner uitschakelen](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

Wanneer u uw wijzigingen hebt aangebracht, vergeet niet om in te schakelen van de planner opnieuw met `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-the-scheduler"></a>Start de planner
De planner is standaard elke 30 minuten wordt uitgevoerd. In sommige gevallen kunt u een synchronisatiecyclus tussen de geplande cycli worden uitgevoerd of moet u een ander type uitvoeren.

**Delta-synchronisatiecyclus**  
Een delta-synchronisatiecyclus bevat de volgende stappen uit:

* Delta-import op alle Connectors
* Deltasynchronisatie op alle Connectors
* Op alle Connectors exporteren

Kan het zijn dat u een urgente wijziging die onmiddellijk moet worden gesynchroniseerd dat is hebt waarom moet u handmatig een cyclus uitvoeren. Als u moet handmatig een cyclus, klikt u vervolgens vanuit PowerShell uitvoeren uitvoeren `Start-ADSyncSyncCycle -PolicyType Delta`.

**Volledige synchronisatiecyclus**  
Als u een van de volgende configuratiewijzigingen hebt aangebracht, moet u een volledige synchronisatiecyclus (ook uitvoeren Initiële):

* Meer objecten of -kenmerken moeten worden geïmporteerd uit een bronmap toegevoegd
* Wijzigingen aangebracht in de synchronisatieregels
* Gewijzigd [filteren](active-directory-aadconnectsync-configure-filtering.md) zodat een verschillend aantal objecten opgenomen worden moet

Als u een van deze wijzigingen hebt aangebracht, moet u de cyclus van een volledige synchronisatie uitvoeren, zodat de synchronisatie-engine de mogelijkheid heeft voor het opnieuw samenvoegen van de connector spaties. Een volledige synchronisatiecyclus bevat de volgende stappen uit:

* Volledige Import op alle Connectors
* Volledige synchronisatie van alle Connectors
* Op alle Connectors exporteren

Voor het initiëren van de cyclus van een volledige synchronisatie uitvoeren `Start-ADSyncSyncCycle -PolicyType Initial` vanuit een PowerShell-prompt. Deze opdracht start een volledige synchronisatie-cyclus.

## <a name="stop-the-scheduler"></a>Stoppen van de planner
Als de planner is een synchronisatiecyclus wordt uitgevoerd, is het mogelijk om deze te stoppen. Als u de installatiewizard te starten en u beschikt over deze fout bijvoorbeeld:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Wanneer een synchronisatiecyclus wordt uitgevoerd, kunt u kunt geen configuratiewijzigingen aanbrengen. U kan wachten tot de planner het proces is voltooid, maar u deze ook stoppen kunt zodat u onmiddellijk uw wijzigingen kunt aanbrengen. De huidige cyclus wordt gestopt, is geen schadelijke en wijzigingen in behandeling worden verwerkt door de volgende keer wordt uitgevoerd.

1. Gestart door de planner stoppen van de huidige cyclus met de PowerShell-cmdlet melding `Stop-ADSyncSyncCycle`.
2. Als u een build voordat 1.1.281 gebruikt, verhindert klikt u vervolgens het stoppen van de planner niet dat de huidige Connector van de huidige taak. Als u wilt dat de Connector te stoppen, de volgende acties uitvoeren: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
   * Start **synchronisatieservice** vanuit het startmenu. Ga naar **Connectors**, markeer de Connector met de status **met**, en selecteer **stoppen** uit de acties.

De planner nog steeds actief is en wordt opnieuw gestart op de eerstvolgende gelegenheid installeren.

## <a name="custom-scheduler"></a>Aangepaste scheduler
De cmdlets die zijn beschreven in deze sectie zijn alleen beschikbaar in de build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) en hoger.

Als de ingebouwde scheduler voldoet niet aan uw vereisten, kunt u de Connectors met behulp van PowerShell plannen.

### <a name="invoke-adsyncrunprofile"></a>Aanroepen ADSyncRunProfile
Voor een verbindingslijn op deze manier kunt u een profiel starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

De namen te gebruiken voor [Connector namen](active-directory-aadconnectsync-service-manager-ui-connectors.md) en [groepnamen van profielen uitvoeren](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) vindt u in de [Synchronization Service Manager UI](active-directory-aadconnectsync-service-manager-ui.md).

![Uitvoeringsprofiel aanroepen](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

De `Invoke-ADSyncRunProfile` cmdlet synchroon is, dat wil zeggen, deze geen retourneert besturingselement totdat de Connector is de bewerking voltooid is of met een fout.

Wanneer u uw Connectors plant, is de aanbeveling te plannen in de volgende volgorde:

1. (Volledige/Delta) Importeren uit een on-premises adreslijsten, zoals Active Directory
2. (Volledige/Delta) Importeren uit Azure AD
3. (Volledige/Delta) Synchronisatie van on-premises adreslijsten, zoals Active Directory
4. (Volledige/Delta) Synchronisatie van Azure AD
5. Exporteren naar Azure AD
6. Exporteren naar on-premises adreslijsten, zoals Active Directory

Deze volgorde is hoe de Connectors in de ingebouwde scheduler wordt uitgevoerd.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
U kunt ook de synchronisatie-engine om te controleren of deze bezet of niet-actieve is bewaken. Deze cmdlet retourneert een leeg resultaat als de synchronisatie-engine niet actief is en niet een Connector wordt uitgevoerd. Als u een Connector wordt uitgevoerd, wordt de naam van de Connector.

```
Get-ADSyncConnectorRunStatus
```

![Uitvoeringsstatus van de connector](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
In de bovenstaande afbeelding is de eerste regel van een status waarbij de synchronisatie-engine niet actief is. De tweede regel uit als de Azure AD-Connector wordt uitgevoerd.

## <a name="scheduler-and-installation-wizard"></a>Wizard Scheduler en installatie
Als u de installatiewizard wordt gestart, klikt u vervolgens de planner is tijdelijk niet beschikbaar. Dit gedrag is omdat ervan wordt uitgegaan configuratiewijzigingen die u maakt en deze instellingen kunnen niet worden toegepast als de synchronisatie-engine actief wordt uitgevoerd. Om deze reden niet laten de installatiewizard openen omdat stopt de synchronisatie-engine geen synchronisatie acties kan uitvoeren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
