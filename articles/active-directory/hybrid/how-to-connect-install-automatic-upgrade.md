---
title: 'Azure AD Connect: Automatische upgrade | Microsoft Docs'
description: Dit onderwerp beschrijft de ingebouwde automatische upgrade functie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfd61b78ca3027ade1f2f48dec33e0a8ed508d3d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58003913"
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automatische upgrade
Deze functie is ingevoerd met build [1.1.105.0 (uitgebracht februari 2016)](reference-connect-version-history.md#111050).  Deze functie is bijgewerkt in [1.1.561 bouwen](reference-connect-version-history.md#115610) en biedt nu ondersteuning voor aanvullende scenario's die niet eerder werden ondersteund.

## <a name="overview"></a>Overzicht
Zorg dat uw Azure AD Connect-installatie is altijd up-to-date is eenvoudiger dan ooit met de **Automatische upgrade** functie. Deze functie is standaard ingeschakeld voor snelle installatie en upgrade van DirSync. Wanneer een nieuwe versie wordt uitgebracht, wordt de installatie wordt automatisch bijgewerkt.
Automatische upgrade is standaard ingeschakeld voor het volgende:

* Snelle installatie van de instellingen en DirSync-upgrades.
* Met behulp van SQL Express LocalDB, dit is wat snelle instellingen altijd gebruikt. DirSync gebruiken met SQL Express gebruiken ook LocalDB.
* Het AD-account is het standaardaccount MSOL_ gemaakt door snelle instellingen en DirSync.
* Minder dan 100.000 objecten hebt in de metaverse.

De huidige status van de automatische upgrade kan worden weergegeven met de PowerShell-cmdlet `Get-ADSyncAutoUpgrade`. De volgende statussen heeft:

| Status | Opmerking |
| --- | --- |
| Ingeschakeld |Automatisch bijwerken is ingeschakeld. |
| Uitgesteld |Door het systeem alleen ingesteld. Het systeem is **momenteel niet** in aanmerking voor automatische upgrades. |
| Uitgeschakeld |Automatisch bijwerken is uitgeschakeld. |

U kunt wisselen tussen **ingeschakeld** en **uitgeschakelde** met `Set-ADSyncAutoUpgrade`. Alleen het systeem moet de status ingesteld **onderbroken**.  Voorafgaand aan 1.1.750.0 zou de cmdlet Set-ADSyncAutoUpgrade Autoupgrade geblokkeerd als de status van de automatische clientupdate is ingesteld op uitgesteld. Deze functionaliteit is nu gewijzigd zodat AutoUpgrade niet worden geblokkeerd.

Automatische upgrade wordt met behulp van Azure AD Connect Health voor de upgrade-infrastructuur. Voor automatisch bijwerken om te werken, moet u de URL's hebt geopend in uw proxyserver voor **Azure AD Connect Health** zoals beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).


Als de **Synchronization Service Manager** gebruikersinterface op de server wordt uitgevoerd, en vervolgens de upgrade wordt onderbroken totdat de gebruikersinterface is gesloten.

## <a name="troubleshooting"></a>Problemen oplossen
Als de Connect-installatie wordt niet zelf omgezet in zoals verwacht, voert u deze stappen om erachter te komen wat is mogelijk onjuist.

U moet eerst niet verwacht dat de automatische upgrade naar de eerste dag die een nieuwe versie is uitgebracht worden geprobeerd. Er is een opzettelijke aanvraaggrootte voordat u een upgrade wordt geprobeerd Schrik niet als de installatie direct wordt niet bijgewerkt.

Als u denkt iets niet goed is dat, klikt u vervolgens voert u eerst `Get-ADSyncAutoUpgrade` om ervoor te zorgen automatisch bijwerken is ingeschakeld.

Controleer vervolgens of dat u hebt de vereiste URL's in uw proxy of firewall geopend. Automatische updates is Azure AD Connect Health gebruiken zoals wordt beschreven in de [overzicht](#overview). Als u een proxy gebruikt, zorg ervoor dat de gezondheid van is geconfigureerd voor het gebruik van een [proxyserver](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ook test de [status connectiviteit](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) naar Azure AD.

Met de verbinding met Azure AD zijn geverifieerd, is het tijd om te zoeken in de gebeurtenislogboeken. De event viewer starten en kijk in de **toepassing** gebeurtenislogboek. Een gebeurtenislogboek-filter voor de bron toevoegen **Azure AD Connect upgraden** en het bereik van gebeurtenis-id **300-399**.  
![Filter voor automatisch bijwerken van het gebeurtenislogboek](./media/how-to-connect-install-automatic-upgrade/eventlogfilter.png)  

U ziet nu de gebeurtenislogboeken die zijn gekoppeld aan de status voor automatisch bijwerken.  
![Filter voor automatisch bijwerken van het gebeurtenislogboek](./media/how-to-connect-install-automatic-upgrade/eventlogresult.png)  

De resultaatcode heeft een voorvoegsel met een overzicht van de status.

| Het voorvoegsel van de resultaat | Description |
| --- | --- |
| Geslaagd |De installatie is bijgewerkt. |
| UpgradeAborted |Een tijdelijke situatie de upgrade gestopt. Het wordt opnieuw geprobeerd en de verwachting is dat deze later slaagt. |
| UpgradeNotSupported |Het systeem heeft een configuratie die wordt geblokkeerd door het systeem automatisch wordt bijgewerkt. Er wordt opnieuw geprobeerd om te zien als de status wordt gewijzigd, maar de verwachting is dat het systeem handmatig moet worden bijgewerkt. |

Hier volgt een lijst van de meest voorkomende berichten die u wilt zoeken. Deze lijst bevat niet alle, maar het bericht met het resultaat moet wissen met wat het probleem is.

| Bericht met het resultaat | Description |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Kan niet schrijven naar het register. |
| UpgradeAbortedInsufficientDatabasePermissions |De ingebouwde groep administrators heeft geen machtigingen voor de database. Handmatig een upgrade naar de nieuwste versie van Azure AD Connect om dit probleem te verhelpen. |
| UpgradeAbortedInsufficientDiskSpace |Er is onvoldoende schijfruimte voor de ondersteuning van een upgrade. |
| UpgradeAbortedSecurityGroupsNotPresent |Kan niet zoeken en oplossen van alle beveiligingsgroepen die worden gebruikt door de synchronisatie-engine. |
| UpgradeAbortedServiceCanNotBeStarted |De NT-Service **Microsoft Azure AD Sync** kan niet worden gestart. |
| UpgradeAbortedServiceCanNotBeStopped |De NT-Service **Microsoft Azure AD Sync** kan niet stoppen. |
| UpgradeAbortedServiceIsNotRunning |De NT-Service **Microsoft Azure AD Sync** wordt niet uitgevoerd. |
| UpgradeAbortedSyncCycleDisabled |De optie SyncCycle in de [scheduler](how-to-connect-sync-feature-scheduler.md) is uitgeschakeld. |
| UpgradeAbortedSyncExeInUse |De [synchronization servicemanager UI](how-to-connect-sync-service-manager-ui.md) is geopend op de server. |
| UpgradeAbortedSyncOrConfigurationInProgress |De installatiewizard wordt uitgevoerd of een synchronisatie buiten de planner is gepland. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedAdfsSignInMethod | U kunt AD FS hebt geselecteerd als de methode aanmelden. |
| UpgradeNotSupportedCustomizedSyncRules |U kunt uw eigen aangepaste regels hebt toegevoegd aan de configuratie. |
| UpgradeNotSupportedDeviceWritebackEnabled |U hebt ingeschakeld de [Write-back van apparaat](how-to-connect-device-writeback.md) functie. |
| UpgradeNotSupportedGroupWritebackEnabled |U hebt ingeschakeld de [Write-back van groep](how-to-connect-preview.md#group-writeback) functie. |
| UpgradeNotSupportedInvalidPersistedState |De installatie is niet een Express-instellingen of een upgrade van DirSync. |
| UpgradeNotSupportedMetaverseSizeExceeeded |U hebt meer dan 100.000 objecten in de metaverse. |
| UpgradeNotSupportedMultiForestSetup |U verbinding maakt met meer dan één forest. Snelle installatie maakt alleen verbinding met één forest. |
| UpgradeNotSupportedNonLocalDbInstall |U maakt geen gebruik van een SQL Server Express LocalDB-database. |
| UpgradeNotSupportedNonMsolAccount |De [Connector voor AD DS-account](reference-connect-accounts-permissions.md#ad-ds-connector-account) is niet het standaardaccount voor MSOL_ meer. |
| UpgradeNotSupportedNotConfiguredSignInMethod | Bij het instellen van AAD Connect, u hebt gekozen *niet configureert* bij het selecteren van de methode voor eenmalige aanmelding. |
| UpgradeNotSupportedPtaSignInMethod | U kunt Pass through-verificatie hebt geselecteerd als de methode aanmelden. |
| UpgradeNotSupportedStagingModeEnabled |De server is ingesteld in [faseringsmodus](how-to-connect-sync-staging-server.md). |
| UpgradeNotSupportedUserWritebackEnabled |U hebt ingeschakeld de [Write-back van gebruiker](how-to-connect-preview.md#user-writeback) functie. |

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
