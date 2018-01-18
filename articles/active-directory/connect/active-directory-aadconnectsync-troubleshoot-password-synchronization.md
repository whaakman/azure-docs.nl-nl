---
title: Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie oplossen | Microsoft Docs
description: In dit artikel bevat informatie over het oplossen van problemen met synchronisatie van wachtwoorden.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 0bf07e80e575309fe7fa44661776c23da5db6dce
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie oplossen
Dit onderwerp bevat stappen voor het oplossen van problemen met synchronisatie van wachtwoorden. Als wachtwoorden zijn niet zoals verwacht wordt gesynchroniseerd, kan het zijn voor een subset van gebruikers of voor alle gebruikers.

Voor de implementatie van Azure Active Directory (Azure AD) verbinding maken met versie 1.1.614.0 of na, gebruikt u de taak voor het oplossen van problemen in de wizard wachtwoord synchronisatieproblemen kunt oplossen:

* Als er een probleem waarbij geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: oplossen met behulp van de taak voor het oplossen van problemen](#no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object worden wachtwoorden niet gesynchroniseerd: oplossen met behulp van de taak voor het oplossen van problemen](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task) sectie.

Voor implementatie met versie 1.1.524.0 of hoger, er is een diagnostische cmdlet kunt u problemen met synchronisatie wachtwoord:

* Als er een probleem waarbij geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: oplossen met behulp van de diagnostische cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object worden wachtwoorden niet gesynchroniseerd: oplossen met behulp van de diagnostische cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) sectie.

Voor oudere versies van Azure AD Connect-implementatie:

* Als er een probleem waarbij geen wachtwoorden worden gesynchroniseerd, raadpleegt u de [geen wachtwoorden worden gesynchroniseerd: handmatige stappen voor probleemoplossing](#no-passwords-are-synchronized-manual-troubleshooting-steps) sectie.

* Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u de [één object worden wachtwoorden niet gesynchroniseerd: handmatige stappen voor probleemoplossing](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) sectie.



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task"></a>Er is geen wachtwoorden worden gesynchroniseerd: oplossen met behulp van de taak voor het oplossen van problemen
U kunt de taak voor het oplossen van problemen gebruiken om te achterhalen waarom geen wachtwoorden worden gesynchroniseerd.

> [!NOTE]
> De taak voor het oplossen van problemen is alleen beschikbaar voor Azure AD Connect versie 1.1.614.0 of hoger.

### <a name="run-the-troubleshooting-task"></a>Uitvoeren van de taak voor het oplossen van problemen
Oplossen van problemen waarbij geen wachtwoorden worden gesynchroniseerd met:

1. Open een nieuw Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Start de Azure AD Connect-wizard.

4. Navigeer naar de **extra taken** pagina **oplossen**, en klik op **volgende**.

5. Klik op de pagina probleemoplossing **starten** starten van het menu voor het oplossen van problemen in PowerShell.

6. Selecteer in het hoofdmenu **Wachtwoordsynchronisatie oplossen**.

7. Selecteer in het submenu **Wachtwoordsynchronisatie werkt niet op alle**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
De taak voor het oplossen van problemen worden de volgende controles uitgevoerd:

* Hiermee valideert u of de functie wachtwoord synchronisatie is ingeschakeld voor uw Azure AD-tenant.

* Valideert de Azure AD Connect-server is niet in de faseringsmodus.

* Voor elk bestaand on-premises Active Directory-connector (die overeenkomt met een bestaand Active Directory-forest):

   * Hiermee valideert u of de functie wachtwoord synchronisatie is ingeschakeld.
   
   * Hiermee zoekt u naar wachtwoord synchronisatie heartbeat gebeurtenissen in de gebeurtenislogboeken van Windows-toepassing.

   * Voor elk Active Directory-domein onder de lokale Active Directory-connector:

      * Wordt gecontroleerd of het domein bereikbaar is vanaf de Azure AD Connect-server.

      * Hiermee valideert u of de Active Directory Domain Services (AD DS)-accounts die worden gebruikt door de lokale Active Directory-connector heeft de juiste gebruikersnaam, wachtwoord en machtigingen die vereist zijn voor Wachtwoordsynchronisatie.

Het volgende diagram illustreert de resultaten van de cmdlet voor een lokale Active Directory-topologie van één domein:

![Diagnostische uitvoer voor Wachtwoordsynchronisatie](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

De rest van deze sectie beschrijft specifieke resultaten die zijn geretourneerd door de taak en de bijbehorende problemen.

#### <a name="password-synchronization-feature-isnt-enabled"></a>De synchronisatiefunctie wachtwoord is niet ingeschakeld
Als u dit nog niet hebt Wachtwoordsynchronisatie met behulp van de Azure AD Connect-wizard ingeschakeld, wordt de volgende fout geretourneerd:

![Wachtwoordsynchronisatie is niet ingeschakeld](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect-server is in de faseringsmodus
Als de Azure AD Connect-server zich in de faseringsmodus, Wachtwoordsynchronisatie is tijdelijk uitgeschakeld en de volgende fout is geretourneerd:

![Azure AD Connect-server is in de faseringsmodus](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a>Er zijn geen wachtwoord synchronisatie heartbeat-gebeurtenissen
Elke lokale Active Directory-connector heeft een eigen synchronisatiekanaal wachtwoord. Wanneer het wachtwoord synchronisatiekanaal tot stand is gebracht en er zijn geen wachtwoordwijzigingen kunnen worden gesynchroniseerd, wordt elke 30 minuten onder het gebeurtenislogboek van Windows-toepassing in een heartbeat-gebeurtenis (EventId 654) gegenereerd. Voor elke lokale Active Directory-connector zoekt u de cmdlet heartbeat er overeenkomende gebeurtenissen in de afgelopen drie uur. Als er geen heartbeat-gebeurtenis wordt gevonden, wordt de volgende fout geretourneerd:

![Er is geen wachtwoord synchronisatie hartslag gebeurtenis](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS-account heeft geen juiste machtigingen
Als de AD DS-account dat wordt gebruikt door de lokale Active Directory-connector synchroniseren wachtwoord-hashes geen de juiste machtigingen heeft, wordt de volgende fout geretourneerd:

![Onjuiste referenties](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Onjuiste gebruikersnaam van de AD DS-account of wachtwoord
Als de AD DS-account gebruikt door de lokale Active Directory-connector voor het synchroniseren van de wachtwoord-hashes een onjuiste gebruikersnaam of wachtwoord heeft, wordt de volgende fout geretourneerd:

![Onjuiste referenties](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task"></a>Wachtwoorden niet kan één object worden gesynchroniseerd: oplossen met behulp van de taak voor het oplossen van problemen

U kunt de taak voor het oplossen van problemen gebruiken om te bepalen waarom één object wachtwoorden niet wordt gesynchroniseerd.

> [!NOTE]
> De taak voor het oplossen van problemen is alleen beschikbaar voor Azure AD Connect versie 1.1.614.0 of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Oplossen van problemen voor een specifieke gebruiker-object:

1. Open een nieuw Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Start de Azure AD Connect-wizard.

4. Navigeer naar de **extra taken** pagina **oplossen**, en klik op **volgende**.

5. Klik op de pagina probleemoplossing **starten** starten van het menu voor het oplossen van problemen in PowerShell.

6. Selecteer in het hoofdmenu **Wachtwoordsynchronisatie oplossen**.

7. Selecteer in het submenu **wachtwoord niet is gesynchroniseerd voor een specifiek gebruikersaccount**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Inzicht in de resultaten van de taak voor het oplossen van problemen
De taak voor het oplossen van problemen worden de volgende controles uitgevoerd:

* Controleert de status van het Active Directory-object in de Active Directory-connectorruimte, Metaverse en Azure AD-connectorruimte.

* Controleert of er synchronisatieregels Wachtwoordsynchronisatie is ingeschakeld en toegepast op de Active Directory-object.

* Probeert op te halen en de resultaten van de laatste poging tot het synchroniseren van het wachtwoord voor het object weer te geven.

Het volgende diagram illustreert de resultaten van de cmdlet bij het oplossen van Wachtwoordsynchronisatie voor één object:

![Diagnostische uitvoer voor Wachtwoordsynchronisatie - enkel object](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

De rest van deze sectie beschrijft specifieke resultaten geretourneerd door de cmdlet en de bijbehorende problemen.

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a>Het Active Directory-object is niet geëxporteerd naar Azure AD
Wachtwoordsynchronisatie voor deze lokale Active Directory-account is mislukt omdat er geen bijbehorende object in de Azure AD-tenant. De volgende fout is geretourneerd:

![Azure AD-object ontbreekt.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Gebruiker heeft een tijdelijk wachtwoord
Azure AD Connect ondersteunt momenteel geen tijdelijke wachtwoorden synchroniseren met Azure AD. Een wachtwoord wordt beschouwd als tijdelijke als de **wachtwoord bij volgende aanmelding wijzigen** optie is ingesteld op de on-premises Active Directory-gebruiker. De volgende fout is geretourneerd:

![Tijdelijke wachtwoord wordt niet geëxporteerd.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a>Resultaten van de laatste poging om te synchroniseren van wachtwoord niet beschikbaar
Azure AD Connect, slaat de resultaten van synchronisatiepogingen van wachtwoord voor de zeven dagen. Als er geen resultaten beschikbaar voor het geselecteerde object in de Active Directory zijn, wordt de volgende waarschuwing geretourneerd:

![Diagnostische uitvoer voor één object - geen wachtwoordgeschiedenis voor synchronisatie](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Er is geen wachtwoorden worden gesynchroniseerd: oplossen met behulp van de diagnostische cmdlet
U kunt de `Invoke-ADSyncDiagnostics` cmdlet om te achterhalen waarom geen wachtwoorden worden gesynchroniseerd.

> [!NOTE]
> De `Invoke-ADSyncDiagnostics` cmdlet is alleen beschikbaar voor Azure AD Connect versie 1.1.524.0 of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Oplossen van problemen waarbij geen wachtwoorden worden gesynchroniseerd met:

1. Open een nieuw Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.

3. Voer `Import-Module ADSyncDiagnostics` uit.

4. Voer `Invoke-ADSyncDiagnostics -PasswordSync` uit.



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Wachtwoorden niet kan één object worden gesynchroniseerd: oplossen met behulp van de diagnostische cmdlet
U kunt de `Invoke-ADSyncDiagnostics` cmdlet om te bepalen waarom één object wachtwoorden niet wordt gesynchroniseerd.

> [!NOTE]
> De `Invoke-ADSyncDiagnostics` cmdlet is alleen beschikbaar voor Azure AD Connect versie 1.1.524.0 of hoger.

### <a name="run-the-diagnostics-cmdlet"></a>Voer de cmdlet diagnostische gegevens
Oplossen van problemen waarbij geen wachtwoorden voor een gebruiker worden gesynchroniseerd met:

1. Open een nieuw Windows PowerShell-sessie op uw Azure AD Connect-server met de **als Administrator uitvoeren** optie.

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



## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Er is geen wachtwoorden worden gesynchroniseerd: handmatige stappen voor probleemoplossing
Volg deze stappen om te bepalen waarom er geen wachtwoorden worden gesynchroniseerd:

1. De server verbinden in [faseringsmodus](active-directory-aadconnectsync-operations.md#staging-mode)? Een server in de faseringsmodus wachtwoorden niet wordt gesynchroniseerd.

2. Het script uitvoeren in de [ophalen van de status van synchronisatie-instellingen voor wachtwoord](#get-the-status-of-password-sync-settings) sectie. Dit biedt u een overzicht van de configuratie van de wachtwoord-synchronisatie.  

    ![PowerShell-scriptuitvoer van de synchronisatie-instellingen voor wachtwoord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. Als de functie is niet ingeschakeld in Azure AD of als de synchronisatiestatus van het kanaal niet is ingeschakeld, moet u de wizard voor Connect-installatie uitvoeren. Selecteer **aanpassen Synchronisatieopties**, en Wachtwoordsynchronisatie te deselecteren. Deze wijziging wordt de functie tijdelijk uitgeschakeld. Vervolgens voert u de wizard opnieuw en Wachtwoordsynchronisatie opnieuw in te schakelen. Voer het script opnieuw uit om te controleren of de configuratie juist is.

4. Kijk in het gebeurtenislogboek voor fouten. Zoek naar de volgende reeks gebeurtenissen die op een probleem opgetreden wijzen:
    * Bron: 'Adreslijstsynchronisatie' ID: 0, 611, 652, 655 als u deze gebeurtenissen, ziet u hebt een verbindingsprobleem. Het bericht van gebeurtenislogboek beschreven forest waar u problemen ondervindt. Zie voor meer informatie [verbindingsprobleem](#connectivity problem).

5. Als er geen heartbeat of niets anders gewerkt, uitvoeren [activeren van een volledige synchronisatie van wachtwoorden voor alle](#trigger-a-full-sync-of-all-passwords). Voer het script slechts één keer.

6. Zie de [oplossen van één object dat wachtwoorden niet synchroniseert](#one-object-is-not-synchronizing-passwords) sectie.

### <a name="connectivity-problems"></a>Verbindingsproblemen

Hebt u verbinding met Azure AD?

Heeft het account vereist machtigingen om te lezen van de wachtwoord-hashes in alle domeinen? Als u verbinding maken met behulp van snelle instellingen hebt geïnstalleerd, kunnen de machtigingen moeten al zijn juist. 

Als u aangepaste installatie gebruikt, de machtigingen handmatig instellen door de volgende te doen:
    
1. Ga voor het account dat wordt gebruikt door de Active Directory-connector start **Synchronization Service Manager**. 
 
2. Ga naar **Connectors**, en zoek vervolgens naar de lokale Active Directory-forest die u wilt oplossen. 
 
3. Selecteer de connector en klik vervolgens op **eigenschappen**. 
 
4. Ga naar **verbinding maken met Active Directory-Forest**.  
    
    ![Account dat wordt gebruikt door Active Directory-connector](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Noteer de gebruikersnaam en het domein waar het account zich bevindt.
    
5. Start **Active Directory: gebruikers en Computers**, en controleer vervolgens of het account dat u eerder hebt gevonden heeft de volgende machtigingen zijn ingesteld in de hoofdmap van alle domeinen in uw forest:
    * Directorywijzigingen repliceren
    * Alle repliceren Directory gewijzigd

6. De domeincontrollers bereikbaar zijn voor Azure AD Connect? Als de Connect-server kan geen verbinding met alle domeincontrollers, configureert u **voorkeur domeincontroller alleen gebruiken**.  
    
    ![-Domeincontroller die wordt gebruikt door Active Directory-connector](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. Ga terug naar **Synchronization Service Manager** en **mappartitie configureren**. 
 
8. Selecteer uw domein in **mappartities selecteren**, selecteer de **alleen gebruiken voorkeur domeincontrollers** selectievakje en klik vervolgens op **configureren**. 

9. Voer de domeincontrollers die verbinding maken voor Wachtwoordsynchronisatie moet worden gebruikt in de lijst. Dezelfde lijst wordt gebruikt voor importeren en exporteren ook. Voer deze stappen uit voor alle domeinen.

10. Als het script wordt aangegeven dat er geen heartbeat, voert u het script in [activeren van een volledige synchronisatie van wachtwoorden voor alle](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Wachtwoorden niet kan één object worden gesynchroniseerd: handmatige stappen voor probleemoplossing
U kunt eenvoudig wachtwoord synchronisatieproblemen oplossen aan de hand van de status van een object.

1. In **Active Directory: gebruikers en Computers**, zoeken naar de gebruiker en controleer vervolgens of de **gebruiker moet wachtwoord bij volgende aanmelding wijzigen** selectievakje is uitgeschakeld.  

    ![Active Directory-productief wachtwoorden](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    Als het selectievakje is ingeschakeld, vraagt de gebruiker aanmelden en het wachtwoord te wijzigen. Tijdelijke wachtwoorden zijn niet gesynchroniseerd met Azure AD.

2. Als het wachtwoord in Active Directory juist zoekt, volgt u de gebruiker in de synchronisatie-engine. Door de gebruiker van de lokale Active Directory naar Azure AD te volgen, kunt u zien of er een beschrijvend foutbericht op het object is.

    a. Start de [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).

    b. Klik op **Connectors**.

    c. Selecteer de **Active Directory-Connector** waarin de gebruiker zich bevindt.

    d. Selecteer **Connectorgebied zoeken**.

    e. In de **bereik** de optie **DN-naam of het anker**, en voer de volledige DN-naam van de gebruiker die u wilt oplossen.

    ![Zoeken naar de gebruiker in de connectorruimte met de DN-naam](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    f. Zoek de gebruiker die u zoekt, en klik vervolgens op **eigenschappen** voor een overzicht van alle kenmerken. Als de gebruiker zich niet in de zoekresultaten, controleert u of uw [filterregels](active-directory-aadconnectsync-configure-filtering.md) en zorg ervoor dat u uitvoert [toepassen en controleer of de wijzigingen](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) voor de gebruiker worden weergegeven in het Connect.

    g. Het wachtwoord synchronisatie van het object voor de afgelopen week, klik op details **logboek**.  

    ![Object-logboekgegevens](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    Of het object logboek leeg is, is Azure AD Connect kan niet lezen van de wachtwoord-hash uit Active Directory. Gaat u uw verder met [Connectiviteitsfouten](#connectivity-errors). Als er een andere waarde dan **geslaagd**, Zie de tabel in [wachtwoord sync logboek](#password-sync-log).

    h. Selecteer de **afkomst** tabblad en zorg ervoor dat ten minste één synchronisatieregel in de **PasswordSync** kolom is **True**. In de standaardconfiguratie wordt de naam van de synchronisatieregel is **In uit Active Directory - gebruiker AccountEnabled**.  

    ![Informatie over een gebruiker Lineage](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    i. Klik op **eigenschappen Metaverse-Object** om een lijst met gebruikerskenmerken weer te geven.  

    ![Metaverse-informatie](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    Controleer of er geen **cloudFiltered** kenmerk aanwezig. Zorg ervoor dat de domeinkenmerken (domainFQDN en domainNetBios) de verwachte waarden.

    j. Klik op de **Connectors** tabblad. Zorg ervoor dat u zowel op de lokale Active Directory en Azure AD connectors ziet.

    ![Metaverse-informatie](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    k. Selecteer de rij die vertegenwoordigt Azure AD, klikt u op **eigenschappen**, en klik vervolgens op de **afkomst** tabblad. De connector space-object moet een uitgaande regel de **PasswordSync** kolom ingesteld op **True**. In de standaardconfiguratie wordt de naam van de synchronisatieregel is **buiten het AAD - gebruiker toevoegen**.  

    ![Dialoogvenster voor eigenschappen van het Object ruimte connector](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Wachtwoord sync-logboek
De statuskolom kan de volgende waarden hebben:

| Status | Beschrijving |
| --- | --- |
| Geslaagd |Wachtwoord is gesynchroniseerd. |
| FilteredByTarget |Wachtwoord is ingesteld op **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**. Wachtwoord is niet gesynchroniseerd. |
| NoTargetConnection |Er is geen object in de metaverse of in de ruimte van Azure AD-connector. |
| SourceConnectorNotPresent |Er is geen object gevonden in de ruimte op de lokale Active Directory-connector. |
| TargetNotExportedToDirectory |Het object in de ruimte van Azure AD-connector is nog niet geëxporteerd. |
| MigratedCheckDetailsForMoreInfo |Logboekvermelding is gemaakt voordat de build 1.0.9125.0 en wordt weergegeven in de oude status. |
| Fout |Service heeft een onbekende fout geretourneerd. |
| Onbekend |Er is een fout opgetreden tijdens het verwerken van een batch wachtwoord-hashes.  |
| MissingAttribute |Specifieke kenmerken die vereist zijn door Azure AD Domain Services (bijvoorbeeld, Kerberos-hash) zijn niet beschikbaar. |
| RetryRequestedByTarget |Specifieke kenmerken die vereist zijn door Azure AD Domain Services (bijvoorbeeld, Kerberos-hash) was eerder niet beschikbaar. Een poging de wachtwoord-hash van de gebruiker opnieuw synchroniseren is gedaan. |

## <a name="scripts-to-help-troubleshooting"></a>Scripts bij het oplossen

### <a name="get-the-status-of-password-sync-settings"></a>De status van wachtwoord synchronisatie-instellingen ophalen
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

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Activeren van een volledige synchronisatie van alle wachtwoorden
> [!NOTE]
> Dit script wordt slechts één keer uitgevoerd. Als u meer dan één keer uitgevoerd wilt, is iets anders het probleem. U kunt het probleem oplossen door contact op met Microsoft ondersteuning.

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
* [Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Azure AD Connect-synchronisatie: Synchronisatie-opties voor aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
