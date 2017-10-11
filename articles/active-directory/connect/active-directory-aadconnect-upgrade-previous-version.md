---
title: 'Azure AD Connect: Upgraden van een vorige versie | Microsoft Docs'
description: Verklaart de verschillende methoden om te upgraden naar de nieuwste versie van Azure Active Directory Connect, met inbegrip van een in-place upgrade en een swing-migratie.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 52fd9375c71c42feaf87f4a0f4220e1cb3889e63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Upgraden van een eerdere versie naar de nieuwste versie
Dit onderwerp beschrijft de verschillende methoden waarmee u kunt de Azure Active Directory (Azure AD) Connect-installatie bijwerken naar de meest recente versie. We adviseren dat u zelf de meest recente de releases van Azure AD Connect. U ook de stappen in de [migratie bewegen](#swing-migration) sectie wanneer u een aanzienlijke configuratie wijzigen.

Als u upgraden van DirSync wilt, Zie [een upgrade uitvoert van Azure AD-synchronisatiehulpprogramma (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) in plaats daarvan.

Er zijn enkele verschillende strategieën die u gebruiken kunt voor het upgraden van Azure AD Connect.

| Methode | Beschrijving |
| --- | --- |
| [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) |Dit is de eenvoudigste methode voor klanten met een snelle installatie. |
| [In-place upgrade](#in-place-upgrade) |Als u één server hebt, kunt u de installatie van ter plekke op dezelfde server bijwerken. |
| [Migratie bewegen](#swing-migration) |U kunt met twee servers voorbereiden op een van de servers met de nieuwe release of de configuratie en de actieve server wijzigen wanneer u klaar bent. |

Zie voor informatie machtigingen de [machtigingen die vereist zijn voor een upgrade](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Nadat u uw nieuwe Azure AD Connect-server om te beginnen met het synchroniseren van wijzigingen naar Azure AD hebt ingeschakeld, moet u niet terugdraaien met behulp van DirSync of Azure AD Sync. Downgraden van Azure AD Connect op oudere clients, waaronder DirSync en Azure AD Sync, wordt niet ondersteund en kan leiden tot problemen zoals verlies van gegevens in Azure AD.

## <a name="in-place-upgrade"></a>In-place upgrade
Een in-place upgrade werkt voor het verplaatsen van Azure AD Sync of Azure AD Connect. Werkt niet voor het verplaatsen van DirSync of voor een oplossing met Forefront Identity Manager (FIM) + Azure AD-Connector.

Deze methode verdient de voorkeur wanneer u één server en minder dan ongeveer 100.000 objecten. Er zijn wijzigingen in de out-of-box-sync-regels, een volledige import en een volledige synchronisatie uitgevoerd als na de upgrade. Deze methode ervoor zorgt dat de nieuwe configuratie is toegepast op alle bestaande objecten in het systeem. Dit duurt enkele uren, afhankelijk van het aantal objecten die in het bereik van de synchronisatie-engine. De normale delta synchronisatie scheduler (die wordt standaard gesynchroniseerd elke 30 minuten) is onderbroken, maar Wachtwoordsynchronisatie blijft. U kunt overwegen de in-place upgrade tijdens een weekend. Als er dat geen wijzigingen aangebracht in de out-of-box-configuratie met de nieuwe Azure AD Connect release, vervolgens een normale delta-import/synchronisatie wordt gestart in plaats daarvan.  
![In-place upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Als u wijzigingen aan de out-of-box-synchronisatieregels aangebracht hebt, worden de deze regels weer ingesteld op de standaardconfiguratie bij een upgrade. Om ervoor te zorgen dat uw configuratie wordt gehouden tussen upgrades, zorg ervoor dat u wijzigingen aanbrengt, zoals ze zijn beschreven in [aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Tijdens een upgrade ter plekke, kunnen er wijzigingen die zijn geïntroduceerd waarvoor specifieke synchronisatiebewerkingen (met inbegrip van de stap van de volledige Import en volledige synchronisatie) moet worden uitgevoerd nadat de upgrade is voltooid. Om uit te stellen dergelijke activiteiten, Raadpleeg het gedeelte [het uitstellen van de volledige synchronisatie na de upgrade](#how-to-defer-full-synchronization-after-upgrade).

## <a name="swing-migration"></a>Swingmigratie
Als u de implementatie van een complexe of veel objecten hebt, is dit mogelijk niet praktisch een in-place upgrade op het systeem live doen. Voor sommige klanten dit proces kan meerdere dagen--duren en gedurende deze tijd geen wijzigingen zijn verwerkt. U kunt deze methode ook gebruiken wanneer u grote wijzigingen aanbrengen in uw configuratie wilt maken en wilt ze om uit te proberen voordat ze naar de cloud bent gepusht.

De aanbevolen methode voor deze scenario's is het gebruik van een migratie swing. U moet (ten minste) twee servers--één actieve server en één server met tijdelijke bestanden. De actieve server (weergegeven met ononderbroken blauwe lijnen in de volgende afbeelding) is verantwoordelijk voor de productie-active-belasting. De staging-server (weergegeven met onderbroken paarse lijnen) is voorbereid met de nieuwe release of de configuratie. Wanneer deze volledig klaar is, wordt deze server actief gemaakt. De vorige active server, waarop nu de oude versie of de configuratie die zijn geïnstalleerd, in de staging-server wordt uitgevoerd en wordt bijgewerkt.

De twee servers kunnen verschillende versies gebruiken. Bijvoorbeeld, de active server die u wilt stellen Azure AD Sync kunt gebruiken en de nieuwe testserver Azure AD Connect kunt gebruiken. Als u swing migratie gebruikt voor het ontwikkelen van een nieuwe configuratie, is het een goed idee om dezelfde versies voor de twee servers.  
![Tijdelijke server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Bepaalde klanten graag voor drie of vier servers voor dit scenario. Wanneer de staging-server is geüpgraded, hebt u niet een back-upserver voor [herstel na noodgevallen](active-directory-aadconnectsync-operations.md#disaster-recovery). Met drie of vier servers, kunt u één set van primaire/stand-by-servers met de nieuwe versie, die zorgt ervoor dat er altijd een testserver die gereed is om over te nemen voorbereiden.

Deze stappen werken ook van Azure AD Sync of een oplossing met FIM + Azure AD-Connector te verplaatsen. Deze stappen werken niet voor DirSync, maar de dezelfde swing migratiemethode (ook wel parallelle implementatie) met de stappen voor DirSync is in [Upgrade Azure Active Directory-synchronisatie (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Een swing migratie gebruiken om te werken
1. Als u Azure AD Connect op beide servers gebruiken en alleen een configuratie wijzigt maken, zorg ervoor dat zijn uw actieve server- en staging-server beide gebruikmaken van dezelfde versie. Dat kunt gemakkelijker verschillen later te vergelijken. Als u een van Azure AD Sync upgrade uitvoert, hebben deze servers verschillende versies. Als u een van een oudere versie van Azure AD Connect upgrade uitvoert, is het een goed idee om te beginnen met de twee servers die gebruikmaken van dezelfde versie, maar dit is niet vereist.
2. Als u een aangepaste configuratie hebt aangebracht en staging-server heeft het geen, volgt u de stappen onder [een aangepaste configuratie van de active server verplaatsen naar de testserver](#move-custom-configuration-from-active-to-staging-server).
3. Als u een van een oudere versie van Azure AD Connect upgrade uitvoert, moet u de tijdelijke server upgraden naar de nieuwste versie. Als u, Azure AD Sync verplaatsen wilt, installeert u Azure AD Connect op uw server met tijdelijke bestanden.
4. Laat de synchronisatie-engine volledige import en volledige synchronisatie uitvoeren op uw server met tijdelijke bestanden.
5. Controleer of de nieuwe configuratie niet dat onverwachte wijzigingen met behulp van de stappen onder 'Verifiëren' [Controleer de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Als er iets niet zoals verwacht, corrigeren, voert u het importeren en synchroniseren en controleer of de gegevens pas goed uitziet, door de stappen te volgen.
6. Overschakelen van de staging-server als de actieve server. Dit is de laatste stap 'Switch active server' [Controleer de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Als u een Azure AD Connect upgrade uitvoert, moet u de server die nu in de faseringsmodus om de meest recente versie te upgraden. Volg dezelfde stappen als voordat u gegevens en configuratie is bijgewerkt. Als u Azure AD Sync bijgewerkt, kunt u nu uitschakelen en de oude server buiten gebruik stellen.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Een aangepaste configuratie van de active server verplaatsen naar de staging-server
Als u wijzigingen in de configuratie hebt aangebracht in de actieve server, moet u ervoor zorgen dat de dezelfde wijzigingen worden toegepast op de staging-server. Om te helpen met deze verplaatsen, kunt u de [documentatie voor Azure AD Connect-configuratie](https://github.com/Microsoft/AADConnectConfigDocumenter).

U kunt verplaatsen om dat de synchronisatie van de aangepaste regels die u hebt gemaakt met behulp van PowerShell. U moet andere wijzigingen dezelfde manier toepassen op beide systemen en de wijzigingen kunnen niet worden gemigreerd. De [configuratie documentatie](https://github.com/Microsoft/AADConnectConfigDocumenter) kunt u het vergelijken van de twee systemen om ervoor te zorgen dat ze identiek zijn. Het hulpprogramma kan ook helpen bij de automatisering van de stappen in deze sectie worden gevonden.

U moet de volgende zaken dezelfde manier configureren op beide servers:

* Verbinding met de dezelfde forests
* Elk domein en OE filteren
* De dezelfde optionele functies, zoals Wachtwoordsynchronisatie en wachtwoord terugschrijven

**Aangepaste synchronisatieregels verplaatsen**  
Als u wilt verplaatsen aangepaste synchronisatieregels, het volgende doen:

1. Open **synchronisatie regeleditor** op uw actieve server.
2. Selecteer een aangepaste regel. Klik op **exporteren**. Hiermee wordt een Kladblok-venster. Het tijdelijke bestand opslaan met een PS1-uitbreiding. Hierdoor kunt u een PowerShell-script. Kopieer het PS1-bestand naar de staging-server.  
   ![Exporteren van de synchronisatie-regel](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. De Connector-GUID is anders zijn op de testserver en deze moeten wijzigen. Als u de GUID, start **synchronisatie regeleditor**, selecteer een van de out-of-box-regels die hetzelfde verbonden systeem vertegenwoordigen en klikt u op **exporteren**. De GUID in uw PS1-bestand vervangen door de GUID van de staging-server.
4. Voer het PS1-bestand in een PowerShell-prompt. Hiermee maakt u de regel voor het aangepaste synchroniseren op de testserver.
5. Herhaal dit voor alle aangepaste regels.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Het uitstellen van de volledige synchronisatie na de upgrade
Tijdens een upgrade ter plekke, kunnen er wijzigingen die zijn geïntroduceerd waarvoor specifieke synchronisatiebewerkingen (met inbegrip van de stap van de volledige Import en volledige synchronisatie) wordt uitgevoerd. Bijvoorbeeld connector schemawijzigingen vereisen **volledige import** stap en out-of-box synchronisatie regel wordt gewijzigd, moeten **volledige synchronisatie** stap worden uitgevoerd op de betrokken connectors. Tijdens de upgrade, Azure AD Connect bepaalt welke synchronisatiebewerkingen zijn vereist en registreert deze als *overschrijft*. In de volgende synchronisatiecyclus wordt de synchronisatie-planner deze overschrijvingen wordt opgehaald en deze worden uitgevoerd. Zodra een onderdrukking met succes wordt uitgevoerd, wordt deze verwijderd.

Mogelijk zijn er situaties waarin u niet dat deze overschrijvingen wilt te kunnen uitvoeren onmiddellijk na de upgrade. Bijvoorbeeld, u talrijke gesynchroniseerde objecten hebt en u wilt dat deze synchronisatie stappen na de kantooruren. Deze overschrijvingen verwijderen:

1. Tijdens de upgrade, **schakelt** de optie **Start het synchronisatieproces wanneer configuratie is voltooid**. Hiermee schakelt de synchronisatie-planner en voorkomt dat synchronisatiecyclus die hebben plaatsgevonden automatisch voordat de onderdrukkingen worden verwijderd.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. Nadat de upgrade is voltooid, voert u de volgende cmdlet om erachter te komen welke onderdrukkingen zijn toegevoegd:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > De onderdrukkingen zijn specifiek voor een connector. In het volgende voorbeeld zijn stap van de volledige Import en volledige synchronisatie toegevoegd aan zowel de on-premises AD-Connector en Azure AD-Connector.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Noteer de bestaande onderdrukkingen die zijn toegevoegd.
   
4. Als u wilt de onderdrukkingen voor zowel volledige import en een volledige synchronisatie op een willekeurige connector verwijderen, moet u de volgende cmdlet uitvoeren:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Voor het verwijderen van de onderdrukkingen op alle connectors kunt u het volgende PowerShell-script uitvoeren:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Als u wilt doorgaan met de scheduler, voer de volgende cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Vergeet niet de vereiste synchronisatie stappen zo snel mogelijk uitvoeren. U kunt handmatig uitvoeren van deze stappen Synchronization Service Manager of de onderdrukkingen back-met de cmdlet Set-ADSyncSchedulerConnectorOverride toevoegen.

Als u wilt de onderdrukkingen voor zowel volledige import en een volledige synchronisatie op een willekeurige connector toevoegt, moet u de volgende cmdlet uitvoeren:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
