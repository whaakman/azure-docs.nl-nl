---
title: 'Azure AD Connect: Upgraden van een vorige versie | Microsoft Docs'
description: De verschillende methoden om te upgraden naar de nieuwste versie van Azure Active Directory Connect, met inbegrip van een in-place upgrade en een swingmigratie uitgelegd.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: b730f80faa031b1866d3c11d8a2c885ec67f965e
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144317"
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Upgraden van een eerdere versie naar de nieuwste versie
Dit onderwerp beschrijft de verschillende methoden die u kunt uw Azure Active Directory (Azure AD) Connect-installatie bijwerken naar de nieuwste versie. U wordt aangeraden dat u zelf actueel met de versies van Azure AD Connect houden. U ook de stappen in de [Swingmigratie](#swing-migration) sectie bij het maken van een aanzienlijke configuratie wijzigen.

Als u upgraden van DirSync wilt, Zie [upgraden van Azure AD-synchronisatiehulpprogramma (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) in plaats daarvan.

Er zijn een paar verschillende strategieën die u gebruiken kunt om te upgraden van Azure AD Connect.

| Methode | Beschrijving |
| --- | --- |
| [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) |Dit is de eenvoudigste methode voor klanten met een snelle installatie. |
| [In-place upgrade](#in-place-upgrade) |Als u één server hebt, kunt u de installatie van ter plekke op dezelfde server bijwerken. |
| [Swingmigratie](#swing-migration) |U kunt met twee servers voorbereiden op een van de servers met de nieuwe versie of de configuratie en de actieve server wijzigen wanneer u klaar bent. |

Zie voor meer informatie voor machtigingen, het [machtigingen die vereist zijn voor een upgrade](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Nadat u uw nieuwe Azure AD Connect-server om te beginnen met het synchroniseren van wijzigingen in Azure AD hebt ingeschakeld, moet u niet terugdraaien met DirSync of Azure AD Sync. Downgraden van Azure AD Connect naar verouderde clients, met inbegrip van DirSync en Azure AD Sync, wordt niet ondersteund en kan leiden tot problemen, zoals verlies van gegevens in Azure AD.

## <a name="in-place-upgrade"></a>In-place upgrade
Een in-place upgrade werkt voor het verplaatsen van Azure AD Sync of Azure AD Connect. Werkt niet voor het verplaatsen van DirSync of voor een oplossing met Forefront Identity Manager (FIM) + Azure AD-Connector.

Deze methode verdient de voorkeur wanneer u één server en over minder dan 100.000 objecten. Er zijn wijzigingen in de out-of-box-synchronisatieregels, een volledige import en een volledige synchronisatie uitgevoerd als na de upgrade. Deze methode zorgt ervoor dat de nieuwe configuratie wordt toegepast op alle bestaande objecten in het systeem. Deze uitvoering duurt enkele uren, afhankelijk van het aantal objecten die in het bereik van de synchronisatie-engine. De planning van de normale delta-synchronisatie (die standaard wordt elke 30 minuten gesynchroniseerd) is onderbroken, maar de synchronisatie van wachtwoord blijft. U kunt overwegen de in-place upgrade tijdens een weekend. Als er dat geen wijzigingen aangebracht in de configuratie van de out-of-box met de nieuwe Azure AD Connect vrijgeven, klikt u vervolgens een normale delta-import/synchronisatie wordt gestart in plaats daarvan.  
![In-place upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Als u wijzigingen in de out-of-box-synchronisatieregels aangebracht hebt, worden de deze regels weer ingesteld op de standaardconfiguratie bij een upgrade. Om ervoor te zorgen dat de configuratie van uw tussen upgrades worden bewaard, zorg ervoor dat u wijzigingen aanbrengt, zoals ze zijn beschreven in [aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Tijdens de upgrade ter plekke, kunnen er wijzigingen die zijn geïntroduceerd waarvoor specifieke synchronisatiebewerkingen (met inbegrip van de stap van de volledige Import en volledige synchronisatie) moet worden uitgevoerd nadat de upgrade is voltooid. Als u wilt dat deze activiteiten moeten worden uitgesteld, Raadpleeg de sectie [het uitstellen van volledige synchronisatie na de upgrade](#how-to-defer-full-synchronization-after-upgrade).

Als u gebruikmaakt van Azure AD Connect met niet-standaard-connector (bijvoorbeeld algemene LDAP-Connector en algemene SQL Connector), vernieuwt u de bijbehorende connectorconfiguratie in de [Synchronization Service Manager](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-connectors) na een upgrade ter plekke. Voor informatie over het vernieuwen van de connectorconfiguratie, Raadpleeg de sectie [Connectorversie - probleemoplossing](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history#troubleshooting). Als u de configuratie niet vernieuwen, werkt importeren en exporteren stappen uitvoeren niet correct voor de connector. U ontvangt de volgende fout in het logboek voor toepassingsgebeurtenissen met bericht *'Assembly-versie in de configuratie van de AAD-Connector ("X.X.XXX. X') is lager dan de werkelijke versie ("X.X.XXX. X)' van 'C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll'.*

## <a name="swing-migration"></a>Swingmigratie
Als u de implementatie van een complexe of veel objecten hebt, is het mogelijk niet praktisch een in-place upgrade van het live systeem doen. Voor sommige klanten dit proces kan meerdere dagen--duren en gedurende deze periode worden geen wijzigingen zijn verwerkt. U kunt deze methode ook gebruiken wanneer u van plan bent aanzienlijke wijzigingen aanbrengen in uw configuratie en u ze kunt uitproberen voordat ze naar de cloud worden gepusht.

De aanbevolen methode voor deze scenario's is het gebruik van een swingmigratie. U moet (minimaal) twee servers--één actieve server en een staging-server. De actieve server (weergegeven met ononderbroken blauwe lijnen in de volgende afbeelding) is verantwoordelijk voor de actieve productiebelasting. De staging-server (weergegeven met stippellijnen paarse) is voorbereid met de nieuwe versie of de configuratie. Wanneer dit volledig klaar is, wordt deze server actief gemaakt. De vorige active server, die nu is de oude versie of de configuratie die zijn geïnstalleerd, in de staging-server wordt uitgevoerd en wordt bijgewerkt.

De twee servers kunnen verschillende versies gebruiken. Bijvoorbeeld, de actieve server die u van plan bent om uit bedrijf te kan gebruiken Azure AD Sync, en de staging-server van de nieuwe Azure AD Connect kunt gebruiken. Als u swingmigratie gebruikt voor het ontwikkelen van een nieuwe configuratie, is het een goed idee om dezelfde versies hebben op de twee servers.  
![Staging-server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Sommige klanten de voorkeur geeft aan dat drie of vier servers voor dit scenario. Wanneer de upgrade van de staging-server is uitgevoerd, hebt u niet een back-upserver voor [herstel na noodgevallen](active-directory-aadconnectsync-operations.md#disaster-recovery). Met drie of vier servers, kunt u één set met primaire/stand-by-servers met de nieuwe versie, die zorgt ervoor dat er altijd een testserver die gereed is om over te nemen voorbereiden.

Deze stappen werken ook voor het verplaatsen van Azure AD Sync of een oplossing met FIM + Azure AD-Connector. Deze stappen niet werken voor DirSync, maar de dezelfde swing migratiemethode (ook wel een parallelle implementatie genoemd) met stappen voor DirSync wordt [upgraden van Azure Active Directory-synchronisatie (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Een swingmigratie gebruiken om te werken
1. Als u Azure AD Connect op beide servers gebruiken en u van plan bent om alleen een configuratie wijzigt, zorg ervoor dat zijn uw actieve server en de staging-server beide gebruikmaken van dezelfde versie. Daardoor is het eenvoudiger om te vergelijken verschillen later opnieuw. Als u een van Azure AD Sync upgrade uitvoert, hebben deze servers verschillende versies. Als u een van een oudere versie van Azure AD Connect upgrade uitvoert, is het een goed idee om te beginnen met de twee servers die gebruikmaken van dezelfde versie, maar het is niet vereist.
2. Als u een aangepaste configuratie hebt aangebracht en uw server met tijdelijke bestanden niet hebt, volgt u de stappen onder [een aangepaste configuratie van de actieve server verplaatsen naar de staging-server](#move-a-custom-configuration-from-the-active-server-to-the-staging-server).
3. Als u een vanaf een eerdere versie van Azure AD Connect upgrade uitvoert, moet u de staging-server upgraden naar de nieuwste versie. Als u van Azure AD Sync overstapt, moet u vervolgens Azure AD Connect installeert op uw server met tijdelijke bestanden.
4. Laat de synchronisatie-engine volledige import en volledige synchronisatie uitvoeren op uw server met tijdelijke bestanden.
5. Controleer of dat de nieuwe configuratie niet ervoor zorgen eventuele onverwachte wijzigingen dat met behulp van de stappen onder 'Verifiëren' [Controleer de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Als er iets niet zoals verwacht, deze te corrigeren, voert u het importeren en synchroniseren en de gegevens te controleren totdat deze ziet er goed uit, met de volgende stappen.
6. Schakel de staging-server om de actieve server. Dit is de laatste stap 'Switch active server' [Controleer de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Als u Azure AD Connect bijwerkt, moet u de server die nu in de faseringsmodus naar de meest recente versie bijwerken. Volg dezelfde stappen als voordat de gegevens en configuratie van een upgrade uitgevoerd. Als u een upgrade uitgevoerd van Azure AD Sync, kunt u nu uitschakelen en de oude server buiten gebruik stellen.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Een aangepaste configuratie van de actieve server verplaatsen naar de staging-server
Als u wijzigingen in de actieve server aangebracht hebt, moet u om ervoor te zorgen dat de dezelfde wijzigingen worden toegepast op de staging-server. Om u te helpen bij deze verplaatsen, kunt u de [documentatie voor Azure AD Connect-configuratie](https://github.com/Microsoft/AADConnectConfigDocumenter).

U kunt verplaatsen dat van de aangepaste synchronisatie regels die u hebt gemaakt met behulp van PowerShell. U moet andere wijzigingen dezelfde manier toepassen op beide systemen, en u kunt de wijzigingen niet migreren. De [configuratie documentatie](https://github.com/Microsoft/AADConnectConfigDocumenter) kunt u een vergelijking van de twee systemen om ervoor te zorgen dat ze identiek zijn. Het hulpprogramma kan ook helpen bij de automatisering van de stappen in deze sectie.

U moet de volgende zaken op beide servers dezelfde manier configureren:

* Verbinding met de dezelfde forests
* Elk domein en OE's filteren
* De dezelfde optionele functies, zoals Wachtwoordsynchronisatie en wachtwoord terugschrijven

**Aangepaste synchronisatieregels verplaatsen**  
Voor het verplaatsen van aangepaste synchronisatieregels, het volgende doen:

1. Open **Synchronization Rules Editor** op uw actieve server.
2. Selecteer een aangepaste regel. Klik op **exporteren**. Hiermee wordt een Kladblok-venster. Het tijdelijke bestand opslaan met een PS1-extensie. Hierdoor kunt u een PowerShell-script. Kopieer het PS1-bestand naar de staging-server.  
   ![Exporteren van de regel synchroniseren](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. De GUID van de Connector is anders uit op de staging-server en u deze moet wijzigen. Als u de GUID, start **Synchronization Rules Editor**, selecteer een van de out-of-box-regels die hetzelfde verbonden systeem vertegenwoordigen, en klikt u op **exporteren**. De GUID in uw PS1-bestand vervangen door de GUID van de staging-server.
4. Voer het PS1-bestand in een PowerShell-prompt. Hiermee maakt u de regel voor aangepaste synchronisatie op de staging-server.
5. Herhaal dit voor uw aangepaste regels.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Het uitstellen van volledige synchronisatie na de upgrade
Tijdens de upgrade ter plekke, kunnen er wijzigingen die zijn geïntroduceerd waarvoor specifieke synchronisatiebewerkingen (met inbegrip van de stap van de volledige Import en volledige synchronisatie) moet worden uitgevoerd. Bijvoorbeeld, de connector schemawijzigingen vereisen **volledige import** stap en out-of-box wijzigingen in synchronisatie-regel vereist **volledige synchronisatie** stap moet worden uitgevoerd op de betrokken connectors. Tijdens de upgrade, Azure AD Connect bepaalt welke activiteiten synchronisatie zijn vereist en registreert als *overschrijft*. In de volgende synchronisatiecyclus de synchronisatieplanner neemt deze onderdrukkingen en deze worden uitgevoerd. Zodra een onderdrukking met succes wordt uitgevoerd, wordt deze verwijderd.

Mogelijk zijn er situaties waarin u niet dat deze onderdrukkingen wilt moet plaatsvinden onmiddellijk na de upgrade. Bijvoorbeeld, u talloze gesynchroniseerde objecten hebt en u wilt dat deze synchronisatie stappen na de kantooruren. Deze onderdrukkingen verwijderen:

1. Tijdens de upgrade, **Schakel** de optie **Start het synchronisatieproces wanneer de configuratie is voltooid**. Hiermee schakelt de synchronisatie-scheduler en voorkomt u dat synchronisatiecyclus plaatsvinden automatisch voordat de onderdrukkingen worden verwijderd.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. Nadat de upgrade is voltooid, voert u de volgende cmdlet om erachter te komen welke onderdrukkingen zijn toegevoegd: `Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > De onderdrukkingen zijn specifiek voor connector. In het volgende voorbeeld wordt zijn stap voor volledige Import en volledige synchronisatie toegevoegd aan de on-premises AD-Connector en Azure AD-Connector.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Noteer de bestaande onderdrukkingen die zijn toegevoegd.
   
4. Als u wilt verwijderen van de onderdrukkingen voor zowel volledige import en een volledige synchronisatie op een willekeurige-connector, moet u de volgende cmdlet uitvoeren: `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Als u wilt verwijderen van de onderdrukkingen op alle connectors, voert u de volgende PowerShell-script uit:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Als u wilt de scheduler hervatten, moet u de volgende cmdlet uitvoeren: `Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Houd er rekening mee voor het uitvoeren van de synchronisatie van de vereiste stappen zo snel mogelijk. U kunt handmatig uitvoeren van deze stappen met behulp van de Synchronization Service Manager of de onderdrukkingen back-ups maken met de cmdlet Set-ADSyncSchedulerConnectorOverride toevoegen.

Als u wilt toevoegen de onderdrukkingen voor zowel volledige import en een volledige synchronisatie op een willekeurige-connector, moet u de volgende cmdlet uitvoeren:  `Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="troubleshooting"></a>Problemen oplossen
De volgende sectie bevat oplossen van problemen en informatie die u gebruiken kunt als u een Azure AD Connect upgraden probleem ondervindt.

### <a name="azure-active-directory-connector-missing-error-during-azure-ad-connect-upgrade"></a>Azure Active Directory-connector ontbreekt fout tijdens het Azure AD Connect upgraden

Wanneer u Azure AD Connect van een eerdere versie upgraden, u de volgende fout aan het begin van de upgrade mogelijk bereikt 

![Fout](./media/active-directory-aadconnect-upgrade-previous-version/error1.png)

Deze fout treedt op omdat de Azure Active Directory-connector met de id, b891884f-051e-4a83-95af - 2544101c 9083, niet in de huidige configuratie van Azure AD Connect bestaat. Als u wilt controleren of dat dit het geval is, opent u een PowerShell-venster, Cmdlet uitvoeren `Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083`

```
PS C:\> Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
Get-ADSyncConnector : Operation failed because the specified MA could not be found.
At line:1 char:1
+ Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ReadError: (Microsoft.Ident...ConnectorCmdlet:GetADSyncConnectorCmdlet) [Get-ADSyncConne
   ctor], ConnectorNotFoundException
    + FullyQualifiedErrorId : Operation failed because the specified MA could not be found.,Microsoft.IdentityManageme
   nt.PowerShell.Cmdlet.GetADSyncConnectorCmdlet

```

De PowerShell-Cmdlet de fout gemeld **de opgegeven MA kan niet worden gevonden**.

De reden dat dit gebeurt is omdat de huidige Azure AD Connect-configuratie wordt niet ondersteund voor upgrade. 

Als u wilt installeren van een nieuwere versie van Azure AD Connect: de Azure AD Connect-wizard te sluiten, verwijder de bestaande Azure AD Connect en voert u een schone installatie van de nieuwere Azure AD Connect.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
