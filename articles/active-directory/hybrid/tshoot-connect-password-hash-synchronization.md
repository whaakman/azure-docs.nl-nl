---
title: Problemen met wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie oplossen | Microsoft Docs
description: In dit artikel bevat informatie over het oplossen van problemen met synchronisatie van wachtwoord-hash.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29cb7f6799556406c4c5def12b1367bfd581d06b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198657"
---
# <a name="troubleshoot-password-hash-synchronization-with-azure-ad-connect-sync"></a>Problemen met wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie oplossen
Dit onderwerp bevat stappen voor het oplossen van problemen met wachtwoord-hashsynchronisatie. Als de wachtwoorden zijn niet gesynchroniseerd zoals verwacht, kan het zijn voor een subset van gebruikers of voor alle gebruikers.

Voor Azure Active Directory (Azure AD) Connect-implementatie met versie 1.1.614.0 of na, gebruikt u de taak voor het oplossen van problemen in de wizard voor het oplossen van problemen met synchronisatie van wachtwoord-hash:

* Als u een probleem hebt, waar geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: problemen oplossen met behulp van de taak voor het oplossen van problemen](#no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object synchroniseert geen wachtwoorden: problemen oplossen met behulp van de taak voor het oplossen van problemen](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task) sectie.

Voor implementatie met versie 1.1.524.0 en hoger of hoger, er is een diagnostische cmdlet die u gebruiken kunt voor het oplossen van problemen met synchronisatie van wachtwoord-hash:

* Als u een probleem hebt, waar geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: problemen oplossen met behulp van de diagnostische cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object synchroniseert geen wachtwoorden: problemen oplossen met behulp van de diagnostische cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) sectie.

Voor oudere versies van Azure AD Connect-implementatie:

* Als u een probleem hebt, waar geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: handmatige stappen voor probleemoplossing](#no-passwords-are-synchronized-manual-troubleshooting-steps) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object synchroniseert geen wachtwoorden: handmatige stappen voor probleemoplossing](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) sectie.



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task"></a>Er zijn geen wachtwoorden gesynchroniseerd: problemen oplossen met behulp van de taak voor het oplossen van problemen
U kunt de taak voor het oplossen van problemen gebruiken om te achterhalen waarom geen wachtwoorden worden gesynchroniseerd.

> [!NOTE]
> De taak voor het oplossen van problemen is alleen beschikbaar voor Azure AD Connect versie 1.1.614.0 of hoger.

### <a name="run-the-troubleshooting-task"></a>Voer de taak voor het oplossen van problemen
Oplossen van problemen waarbij geen wachtwoorden worden gesynchroniseerd met:

1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Start de Azure AD Connect-wizard.

4. Navigeer naar de **extra taken** weergeeft, schakelt **oplossen**, en klikt u op **volgende**.

5. Klik op de pagina voor problemen oplossen, **starten** voor het oplossen van problemen met het menu start in PowerShell.

6. Selecteer in het hoofdmenu van **wachtwoord-hashsynchronisatie oplossen**.

7. Selecteer in het submenu **wachtwoord-hashsynchronisatie werkt niet helemaal**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
Het oplossen van problemen met taak voert de volgende controles uit:

* Hiermee valideert u dat de wachtwoordfunctie-hash-synchronisatie is ingeschakeld voor uw Azure AD-tenant.

* Valideert de Azure AD Connect-server is niet in de faseringsmodus bevindt.

* Voor elke bestaande on-premises Active Directory-connector (die overeenkomt met een bestaand Active Directory-forest):

   * Hiermee valideert u dat de wachtwoordfunctie-hash-synchronisatie is ingeschakeld.
   
   * Zoekt naar wachtwoord-hash-synchronisatie heartbeat-gebeurtenissen in de gebeurtenislogboeken van Windows-toepassing.

   * Voor elk Active Directory-domein in de on-premises Active Directory-connector:

      * Valideert of het domein bereikbaar is vanaf de Azure AD Connect-server is.

      * Hiermee valideert u dat de Active Directory Domain Services (AD DS)-accounts die worden gebruikt door de on-premises Active Directory-connector heeft de juiste gebruikersnaam, wachtwoord en machtigingen die vereist zijn voor de wachtwoord-hashsynchronisatie.

Het volgende diagram illustreert de resultaten van de cmdlet voor een enkel domein, on-premises Active Directory-topologie:

![Diagnostische uitvoer voor de wachtwoord-hashsynchronisatie](./media/tshoot-connect-password-hash-synchronization/phsglobalgeneral.png)

De rest van deze sectie worden specifieke resultaten die worden geretourneerd door de taak en de bijbehorende problemen beschreven.

#### <a name="password-hash-synchronization-feature-isnt-enabled"></a>wachtwoord-hash-synchronisatie-functie is niet ingeschakeld
Als u wachtwoord-hashsynchronisatie nog niet hebt ingeschakeld met behulp van de Azure AD Connect-wizard, wordt de volgende fout geretourneerd:

![wachtwoord-hashsynchronisatie is niet ingeschakeld](./media/tshoot-connect-password-hash-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect-server zich in de faseringsmodus bevindt
Als de Azure AD Connect-server zich in de faseringsmodus bevindt, wachtwoord-hashsynchronisatie is tijdelijk uitgeschakeld en de volgende fout is geretourneerd:

![Azure AD Connect-server zich in de faseringsmodus bevindt](./media/tshoot-connect-password-hash-synchronization/phsglobalstaging.png)

#### <a name="no-password-hash-synchronization-heartbeat-events"></a>Er is geen wachtwoord-hash synchronisatie heartbeat-gebeurtenissen
Elke on-premises Active Directory-connector heeft een eigen wachtwoord-hash-synchronisatiekanaal. Wanneer de wachtwoord-hash-synchronisatiekanaal tot stand is gebracht en er geen wachtwoordwijzigingen moeten worden gesynchroniseerd, wordt één keer voor elke 30 minuten in het gebeurtenislogboek van Windows-toepassing in een heartbeat-gebeurtenis (EventId 654) gegenereerd. Voor elke on-premises Active Directory-connector, de cmdlet gezocht naar overeenkomende heartbeat-gebeurtenissen in de afgelopen drie uur. Als er geen heartbeat-gebeurtenis wordt gevonden, wordt de volgende fout geretourneerd:

![Er is geen wachtwoord-hash synchronisatie hartslag gebeurtenis](./media/tshoot-connect-password-hash-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS-account heeft geen juiste machtigingen
Als het AD DS-account dat wordt gebruikt door de on-premises Active Directory-connector voor het synchroniseren van wachtwoord-hashes geen de juiste machtigingen heeft, wordt de volgende fout geretourneerd:

![Onjuiste referenties](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Onjuiste gebruikersnaam van de AD DS-account of wachtwoord
Als de AD DS-account gebruikt door de on-premises Active Directory-connector voor het synchroniseren van wachtwoord-hashes een onjuiste gebruikersnaam of wachtwoord heeft, wordt de volgende fout geretourneerd:

![Onjuiste referenties](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectcredential.png)



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task"></a>Een object synchroniseert geen wachtwoorden: problemen oplossen met behulp van de taak voor het oplossen van problemen

U kunt de taak voor het oplossen van problemen gebruiken om te bepalen waarom een object niet kan worden gesynchroniseerd wachtwoorden.

> [!NOTE]
> De taak voor het oplossen van problemen is alleen beschikbaar voor Azure AD Connect versie 1.1.614.0 of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Oplossen van problemen voor een specifieke gebruiker-object:

1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Start de Azure AD Connect-wizard.

4. Navigeer naar de **extra taken** weergeeft, schakelt **oplossen**, en klikt u op **volgende**.

5. Klik op de pagina voor problemen oplossen, **starten** voor het oplossen van problemen met het menu start in PowerShell.

6. Selecteer in het hoofdmenu van **wachtwoord-hashsynchronisatie oplossen**.

7. Selecteer in het submenu **wachtwoord niet is gesynchroniseerd voor een specifiek gebruikersaccount**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
Het oplossen van problemen met taak voert de volgende controles uit:

* Controleert de status van de Active Directory-object in de Active Directory-connectorgebied, Metaverse en Azure AD-connectorgebied.

* Controleert of er synchronisatieregels met wachtwoord-hashsynchronisatie ingeschakeld en toegepast op de Active Directory-object.

* Probeert te halen en de resultaten van de laatste poging tot het synchroniseren van het wachtwoord voor het object weer te geven.

Het volgende diagram illustreert de resultaten van de cmdlet bij het oplossen van wachtwoord-hashsynchronisatie voor een enkel object:

![Diagnostische uitvoer voor de wachtwoord-hashsynchronisatie - één-object](./media/tshoot-connect-password-hash-synchronization/phssingleobjectgeneral.png)

De rest van deze sectie worden specifieke resultaten geretourneerd door de cmdlet en de bijbehorende problemen beschreven.

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a>Het Active Directory-object is niet geëxporteerd naar Azure AD
wachtwoord-hashsynchronisatie voor dit on-premises Active Directory-account is mislukt omdat er geen overeenkomstige object in de Azure AD-tenant is. De volgende fout is geretourneerd:

![Azure AD-object ontbreekt.](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Gebruiker heeft een tijdelijk wachtwoord
Azure AD Connect ondersteunt op dit moment geen tijdelijke wachtwoorden synchroniseren met Azure AD. Een wachtwoord wordt beschouwd als tijdelijk als de **wachtwoord bij volgende aanmelding wijzigen** optie is ingesteld op de on-premises Active Directory-gebruiker. De volgende fout is geretourneerd:

![Tijdelijk wachtwoord is niet geëxporteerd.](./media/tshoot-connect-password-hash-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a>Resultaten van de laatste poging tot het synchroniseren van wachtwoord zijn niet beschikbaar
Standaard worden de resultaten van synchronisatiepogingen van wachtwoord-hash voor de zeven dagen opgeslagen door Azure AD Connect. Als er geen resultaten beschikbaar voor het geselecteerde object van de Active Directory zijn, wordt de volgende waarschuwing geretourneerd:

![Diagnostische uitvoer voor één object - geen geschiedenis van de synchronisatie van wachtwoord](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnohistory.png)



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Er zijn geen wachtwoorden gesynchroniseerd: problemen oplossen met behulp van de diagnostische cmdlet
U kunt de `Invoke-ADSyncDiagnostics` cmdlet om te achterhalen waarom geen wachtwoorden worden gesynchroniseerd.

> [!NOTE]
> De `Invoke-ADSyncDiagnostics` cmdlet is alleen beschikbaar voor Azure AD Connect versie 1.1.524.0 en hoger of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Oplossen van problemen waarbij geen wachtwoorden worden gesynchroniseerd met:

1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Voer `Import-Module ADSyncDiagnostics` uit.

4. Voer `Invoke-ADSyncDiagnostics -PasswordSync` uit.



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Een object synchroniseert geen wachtwoorden: problemen oplossen met behulp van de diagnostische cmdlet
U kunt de `Invoke-ADSyncDiagnostics` cmdlet om te bepalen waarom een object niet kan worden gesynchroniseerd wachtwoorden.

> [!NOTE]
> De `Invoke-ADSyncDiagnostics` cmdlet is alleen beschikbaar voor Azure AD Connect versie 1.1.524.0 en hoger of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Problemen met waar geen wachtwoorden gesynchroniseerd zijn voor een gebruiker:

1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Voer `Import-Module ADSyncDiagnostics` uit.

4. Voer de volgende cmdlet uit:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   Bijvoorbeeld:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```



## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Er zijn geen wachtwoorden gesynchroniseerd: handmatige stappen voor probleemoplossing
Volg deze stappen om te bepalen waarom er zijn geen wachtwoorden worden gesynchroniseerd:

1. Is de Connect-server in [faseringsmodus](how-to-connect-sync-operations.md#staging-mode)? Een server in de faseringsmodus wachtwoorden niet gesynchroniseerd.

2. Het script uitvoeren in de [ophalen van de status van synchronisatie-instellingen van wachtwoord](#get-the-status-of-password-sync-settings) sectie. Dit biedt u een overzicht van de configuratie van de synchronisatie van wachtwoord.  

    ![PowerShell-script-uitvoer van synchronisatie-instellingen van wachtwoord](./media/tshoot-connect-password-hash-synchronization/psverifyconfig.png)  

3. Als de functie is niet ingeschakeld in Azure AD of als de synchronisatiestatus van het kanaal is niet ingeschakeld, worden de Connect-installatiewizard uitgevoerd. Selecteer **aanpassen Synchronisatieopties**, en hef de selectie van Wachtwoordsynchronisatie. Deze wijziging wordt de functie tijdelijk uitgeschakeld. Vervolgens de wizard opnieuw uitvoeren en Wachtwoordsynchronisatie opnieuw in te schakelen. Voer het script opnieuw uit om te controleren of de configuratie juist is.

4. Zoek in het gebeurtenislogboek op fouten. Zoeken naar de volgende reeks gebeurtenissen die op een probleem duiden:
    * Bron: 'Adreslijstsynchronisatie'-ID: 0, 611, 652, 655 als u deze gebeurtenissen, ziet u een probleem met de connectiviteit hebt. Het bericht van gebeurtenislogboek bevat gegevens van forest waar u een probleem hebt. Zie voor meer informatie, [verbindingsprobleem](#connectivity problem).

5. Als er geen heartbeat of als er niets anders heeft gewerkt, [activeren van een volledige synchronisatie van alle wachtwoorden](#trigger-a-full-sync-of-all-passwords). Voer het script slechts één keer.

6. Zie de problemen met een object dat sectie wachtwoorden niet kan worden gesynchroniseerd.

### <a name="connectivity-problems"></a>Problemen met de netwerkverbinding

Hebt u verbinding met Azure AD?

Beschikt over het account vereist machtigingen om te lezen van de wachtwoord-hashes in alle domeinen? Als u verbinding maken met behulp van snelle instellingen hebt geïnstalleerd, kunnen de machtigingen moeten al zijn correct. 

Als u aangepaste installatie gebruikt, stelt u de machtigingen handmatig door het volgende te doen:
    
1. Als u het account dat wordt gebruikt door de Active Directory-connector zoekt, start **Synchronization Service Manager**. 
 
2. Ga naar **Connectors**, en zoek vervolgens naar de on-premises Active Directory-forest die u wilt oplossen. 
 
3. Selecteer de connector en klik vervolgens op **eigenschappen**. 
 
4. Ga naar **verbinding maken met Active Directory-Forest**.  
    
    ![Account dat wordt gebruikt door Active Directory-connector](./media/tshoot-connect-password-hash-synchronization/connectoraccount.png)  
    Houd er rekening mee de gebruikersnaam en het domein waar het account zich bevindt.
    
5. Start **Active Directory: gebruikers en Computers**, en controleer vervolgens of dat het account dat u eerder hebt gevonden, de volgende machtigingen zijn ingesteld in de hoofdmap van alle domeinen in uw forest heeft:
    * Directorywijzigingen repliceren
    * Alle repliceren Directory gewijzigd

6. De domeincontrollers zijn die bereikbaar is via Azure AD Connect? Als de Connect-server kan geen verbinding met alle domeincontrollers maken, configureert u **voorkeur domeincontroller alleen gebruiken**.  
    
    ![Domeincontroller die wordt gebruikt door Active Directory-connector](./media/tshoot-connect-password-hash-synchronization/preferreddc.png)  
    
7. Ga terug naar **Synchronization Service Manager** en **mappartitie configureren**. 
 
8. Selecteer uw domein in **mappartities selecteren**, selecteer de **gebruik alleen domeincontrollers** selectievakje en klik vervolgens op **configureren**. 

9. Voer in de lijst met de domeincontrollers die verbinding maken voor Wachtwoordsynchronisatie moet gebruiken. Dezelfde lijst wordt gebruikt voor het importeren en exporteren ook. Voer deze stappen uit voor alle uw domeinen.

10. Als het script laat zien dat er geen heartbeat van de, het script uitvoeren in [activeren van een volledige synchronisatie van alle wachtwoorden](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Een object synchroniseert geen wachtwoorden: handmatige stappen voor probleemoplossing
U kunt eenvoudig problemen met wachtwoord-hash-synchronisatie oplossen aan de hand van de status van een object.

1. In **Active Directory: gebruikers en Computers**, zoek de gebruiker en controleer vervolgens de **gebruiker moet wachtwoord bij volgende aanmelding wijzigen** selectievakje is uitgeschakeld.  

    ![Active Directory-productief wachtwoorden](./media/tshoot-connect-password-hash-synchronization/adprodpassword.png)  

    Als het selectievakje is ingeschakeld, vraagt de gebruiker zich aanmelden en het wachtwoord te wijzigen. Tijdelijke wachtwoorden zijn niet gesynchroniseerd met Azure AD.

2. Als het wachtwoord dat overeenkomt met uw in Active Directory wijzigingen, volgt u de gebruiker in de synchronisatie-engine. Door de gebruiker te volgen uit on-premises Active Directory naar Azure AD, kunt u zien of er een beschrijvend foutbericht op het object is.

    a. Start de [Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md).

    b. Klik op **Connectors**.

    c. Selecteer de **Active Directory-Connector** waar de gebruiker zich bevindt.

    d. Selecteer **Connectorgebied zoeken**.

    e. In de **bereik** Schakel **DN-naam of het anker**, en voer de volledige DN-naam van de gebruiker die u wilt oplossen.

    ![Zoeken naar gebruikers in de ruimte van de connector met de DN-naam](./media/tshoot-connect-password-hash-synchronization/searchcs.png)  

    f. Zoek de gebruiker die u zoekt, en klik vervolgens op **eigenschappen** om alle kenmerken te zien. Als de gebruiker zich niet in de zoekresultaten, controleert u of uw [filterregels](how-to-connect-sync-configure-filtering.md) en zorg ervoor dat u uitvoert [toepassen en controleer of de wijzigingen](how-to-connect-sync-configure-filtering.md#apply-and-verify-changes) voor de gebruiker in Connect wordt weergegeven.

    g. Voor de details van de synchronisatie van wachtwoord van het object voor de afgelopen week, klikt u op **Log**.  

    ![Logboek objectdetails](./media/tshoot-connect-password-hash-synchronization/csobjectlog.png)  

    Als het logboek object leeg is, is Azure AD Connect kan niet lezen van de wachtwoord-hash uit Active Directory. Doorgaan met het oplossen van problemen met fouten in de basisnetwerkverbinding. Als er een andere waarde dan **succes**, Zie de tabel in [wachtwoord synchronisatielogboek](#password-sync-log).

    h. Selecteer de **afkomst** tabblad en zorg ervoor dat ten minste één synchronisatieregel in de **PasswordSync** kolom **waar**. In de standaardconfiguratie, de naam van de synchronisatieregel is **In uit Active Directory - gebruiker AccountEnabled**.  

    ![Informatie over een gebruiker Lineage](./media/tshoot-connect-password-hash-synchronization/cspasswordsync.png)  

    i. Klik op **eigenschappen van het Metaverseobject** om een lijst met gebruikerskenmerken weer te geven.  

    ![Metaverse-gegevens](./media/tshoot-connect-password-hash-synchronization/mvpasswordsync.png)  

    Controleer of er geen **cloudFiltered** kenmerk aanwezig zijn. Zorg ervoor dat de domeinkenmerken (domainFQDN en domainNetBios) de verwachte waarden.

    j. Klik op de **Connectors** tabblad. Zorg ervoor dat u de connectors voor zowel on-premises Active Directory en Azure AD ziet.

    ![Metaverse-gegevens](./media/tshoot-connect-password-hash-synchronization/mvconnectors.png)  

    k. Selecteer de rij die staat voor Azure AD, klikt u op **eigenschappen**, en klik vervolgens op de **afkomst** tabblad. De connector space-object moet een uitgaande regel de **PasswordSync** kolom is ingesteld op **waar**. In de standaardconfiguratie, de naam van de synchronisatieregel is **Out voor AAD - gebruiker toevoegen**.  

    ![Dialoogvenster met eigenschappen van de ruimte-Object-connector](./media/tshoot-connect-password-hash-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Synchronisatielogboek wachtwoord
De statuskolom kan de volgende waarden hebben:

| Status | Description |
| --- | --- |
| Geslaagd |Wachtwoord is gesynchroniseerd. |
| FilteredByTarget |Wachtwoord is ingesteld op **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**. Wachtwoord is niet gesynchroniseerd. |
| NoTargetConnection |Er is geen object in de metaverse of in de Azure AD-connectorgebied. |
| SourceConnectorNotPresent |Er is geen object gevonden in de on-premises Active Directory-connectorgebied. |
| TargetNotExportedToDirectory |Het object in de Azure AD-connectorgebied is nog niet geëxporteerd. |
| MigratedCheckDetailsForMoreInfo |Logboekvermelding is gemaakt vóór build 1.0.9125.0 en wordt weergegeven in de oude status. |
| Fout |Service heeft een onbekende fout geretourneerd. |
| Onbekend |Er is een fout opgetreden tijdens het verwerken van een batch van wachtwoord-hashes.  |
| MissingAttribute |Specifieke kenmerken die vereist zijn door Azure AD Domain Services (bijvoorbeeld, Kerberos-hash) zijn niet beschikbaar. |
| RetryRequestedByTarget |Specifieke kenmerken die vereist zijn door Azure AD Domain Services (bijvoorbeeld, Kerberos-hash) zijn eerder niet beschikbaar. Een poging om opnieuw te synchroniseren van wachtwoord-hash van de gebruiker is gemaakt. |

## <a name="scripts-to-help-troubleshooting"></a>Scripts voor het oplossen van problemen

### <a name="get-the-status-of-password-sync-settings"></a>De status van synchronisatie-instellingen van wachtwoord ophalen
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Activeer een volledige synchronisatie van alle wachtwoorden
> [!NOTE]
> Voer dit script slechts één keer. Als u meer dan één keer uitgevoerd wilt, is iets anders het probleem. Als u wilt het probleem op te lossen, neem contact op met Microsoft ondersteuning.

U kunt een volledige synchronisatie van alle wachtwoorden activeren met behulp van het volgende script:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Volgende stappen
* [Implementatie van wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](how-to-connect-password-hash-synchronization.md)
* [Azure AD Connect Sync: Synchronisatieopties aanpassen](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
