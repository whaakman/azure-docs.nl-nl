---
title: 'Azure AD Connect: Automatische upgrade | Microsoft Docs'
description: Dit onderwerp beschrijft de ingebouwde automatische upgrade functie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: ddf23193e7b9c2fb7a69045be539841dbb6e6249
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: automatische upgrade
Deze functie is geïntroduceerd met build 1.1.105.0 (uitgebracht februari 2016).

## <a name="overview"></a>Overzicht
Zorg dat de installatie van Azure AD Connect is altijd actueel is nooit zo eenvoudig met de **Automatische upgrade** functie. Deze functie is standaard ingeschakeld voor snelle installatie en upgrade van DirSync. Wanneer een nieuwe versie wordt uitgebracht, wordt de installatie automatisch bijgewerkt.
Automatische upgrade is standaard ingeschakeld voor het volgende:

* Snelle installatie instellingen en DirSync-upgrades.
* Met behulp van SQL Express LocalDB, dit is wat snelle instellingen altijd gebruiken. DirSync met SQL Express ook LocalDB gebruiken.
* Het AD-account is het standaard MSOL_ account gemaakt door snelle instellingen en DirSync.
* Minder dan 100.000 objecten hebben in de metaverse.

De huidige status van de automatische upgrade kan worden bekeken met de PowerShell-cmdlet `Get-ADSyncAutoUpgrade`. De volgende statussen heeft:

| Status | Opmerking |
| --- | --- |
| Ingeschakeld |Automatische upgrade is ingeschakeld. |
| Uitgesteld |Door het systeem alleen ingesteld. Het systeem is niet meer in aanmerking voor automatische upgrades. |
| Uitgeschakeld |Automatische upgrade is uitgeschakeld. |

U kunt schakelen tussen **ingeschakeld** en **uitgeschakelde** met `Set-ADSyncAutoUpgrade`. Alleen het systeem moet de status ingesteld **onderbroken**.

Automatische upgrade maakt gebruik van Azure AD Connect Health voor de upgrade-infrastructuur. Automatische upgrade werkt, controleert u of u de URL's hebt geopend in uw proxyserver voor **Azure AD Connect Health** zoals beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).


Als de **Synchronization Service Manager** gebruikersinterface op de server wordt uitgevoerd en vervolgens de upgrade wordt onderbroken tot de gebruikersinterface is gesloten.

## <a name="troubleshooting"></a>Problemen oplossen
Als de Connect-installatie niet automatisch bijgewerkt wanneer wordt zoals verwacht, voert u deze stappen om erachter te komen wat is mogelijk onjuist.

U moet eerst niet verwacht dat de automatische upgrade naar de eerste dag die een nieuwe versie wordt uitgebracht worden uitgevoerd. Er is een opzettelijke aanvraaggrootte voordat u een upgrade wordt uitgevoerd, zodat normaal als de installatie onmiddellijk wordt niet bijgewerkt.

Als u denkt iets niet goed is dat, klikt u vervolgens voert u eerst `Get-ADSyncAutoUpgrade` om ervoor te zorgen automatische upgrade is ingeschakeld.

Controleer vervolgens of dat u de gewenste URL's in uw proxy of firewall hebt geopend. Automatische updates is Azure AD Connect Health gebruiken zoals beschreven in de [overzicht](#overview). Als u een proxy gebruikt, controleert u of Health is geconfigureerd voor het gebruik van een [proxyserver](../connect-health/active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ook testen de [Health connectiviteit](../connect-health/active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) naar Azure AD.

Met de verbinding met Azure AD zijn geverifieerd, is het tijd om te zoeken in de gebeurtenislogboeken. Start Logboeken en zoekt u in de **toepassing** eventlog. Een eventlog-filter voor de bron toevoegen **Azure AD Connect Upgrade** en het bereik van gebeurtenis-id **300 399**.  
![Eventlog-filter voor de automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

U ziet nu de gebeurtenislogboeken meer die zijn gekoppeld aan de status voor automatische upgrade.  
![Eventlog-filter voor de automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

De resultaatcode heeft een voorvoegsel en met een overzicht van de status.

| Het voorvoegsel van de resultaat | Beschrijving |
| --- | --- |
| Geslaagd |De installatie is bijgewerkt. |
| UpgradeAborted |Een tijdelijke situatie de upgrade gestopt. Deze opnieuw uit te voeren en de verwachting is dat later lukt. |
| UpgradeNotSupported |Het systeem heeft een configuratie die wordt geblokkeerd door het systeem automatisch wordt bijgewerkt. Dit wordt opnieuw geprobeerd om te zien als de status wordt gewijzigd, maar de verwachting is dat het systeem handmatig moet worden bijgewerkt. |

Hier volgt een lijst van de meest voorkomende berichten die u wilt zoeken. Deze lijst bevat niet alle, maar het resultaat bericht moet wissen met wat het probleem is.

| Resultaat bericht | Beschrijving |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Kan niet schrijven naar het register. |
| UpgradeAbortedInsufficientDatabasePermissions |De ingebouwde groep administrators heeft geen machtigingen voor de database. Upgrade handmatig naar de meest recente versie van Azure AD Connect om dit probleem te verhelpen. |
| UpgradeAbortedInsufficientDiskSpace |Er is onvoldoende schijfruimte voor de ondersteuning van een upgrade. |
| UpgradeAbortedSecurityGroupsNotPresent |Kan niet zoeken en los alle beveiligingsgroepen die worden gebruikt door de synchronisatie-engine. |
| UpgradeAbortedServiceCanNotBeStarted |De NT-Service **Microsoft Azure AD Sync** kan niet worden gestart. |
| UpgradeAbortedServiceCanNotBeStopped |De NT-Service **Microsoft Azure AD Sync** kan niet stoppen. |
| UpgradeAbortedServiceIsNotRunning |De NT-Service **Microsoft Azure AD Sync** wordt niet uitgevoerd. |
| UpgradeAbortedSyncCycleDisabled |De optie SyncCycle in de [scheduler](active-directory-aadconnectsync-feature-scheduler.md) is uitgeschakeld. |
| UpgradeAbortedSyncExeInUse |De [service Synchronisatiebeheer UI](active-directory-aadconnectsync-service-manager-ui.md) is geopend op de server. |
| UpgradeAbortedSyncOrConfigurationInProgress |De installatiewizard wordt uitgevoerd of een synchronisatie buiten de planner is gepland. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedAdfsSignInMethod | U kunt AD FS hebt geselecteerd als de methode voor aanmelden. | 
| UpgradeNotSupportedCustomizedSyncRules |U kunt uw eigen aangepaste regels hebt toegevoegd aan de configuratie. |
| UpgradeNotSupportedDeviceWritebackEnabled |U hebt ingeschakeld de [apparaat terugschrijven](active-directory-aadconnect-feature-device-writeback.md) functie. |
| UpgradeNotSupportedGroupWritebackEnabled |U hebt ingeschakeld de [Write-back van groep](active-directory-aadconnect-feature-preview.md#group-writeback) functie. |
| UpgradeNotSupportedInvalidPersistedState |De installatie is niet een snelle instellingen of een upgrade van DirSync. |
| UpgradeNotSupportedMetaverseSizeExceeeded |U hebt meer dan 100.000 objecten in de metaverse. |
| UpgradeNotSupportedMultiForestSetup |U verbinding maakt met meer dan één forest. Snelle installatie is alleen verbinding maakt met één forest. |
| UpgradeNotSupportedNonLocalDbInstall |U maakt geen gebruik van een SQL Server Express LocalDB-database. |
| UpgradeNotSupportedNonMsolAccount |De [AD Connector-account](active-directory-aadconnect-accounts-permissions.md#active-directory-account) is niet het standaardaccount voor MSOL_ meer. |
| UpgradeNotSupportedNotConfiguredSignInMethod | Bij het instellen van AAD Connect, u hebt gekozen *niet configureert* bij het selecteren van de methode eenmalige aanmelding. | 
| UpgradeNotSupportedPtaSignInMethod | U kunt Pass through-verificatie hebt geselecteerd als de methode voor aanmelden. |
| UpgradeNotSupportedStagingModeEnabled |De server is ingesteld in [faseringsmodus](active-directory-aadconnectsync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |U hebt ingeschakeld de [Write-back van gebruiker](active-directory-aadconnect-feature-preview.md#user-writeback) functie. |

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
