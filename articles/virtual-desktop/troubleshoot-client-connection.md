---
title: Verbindingen van extern bureaublad-client in Windows virtueel bureaublad - Azure
description: Over het oplossen van problemen bij het instellen van clientverbindingen in een omgeving met virtuele Windows-bureaublad tenants.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: f88dee579e44a01dc1a7404ef6a670de34063552
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833575"
---
# <a name="remote-desktop-client-connections"></a>Clientverbindingen met extern bureaublad

Gebruik dit artikel voor het oplossen van problemen met virtuele Windows-bureaublad-clientverbindingen.

## <a name="provide-feedback"></a>Feedback geven

We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden.

## <a name="you-cant-open-a-web-client"></a>U kunt een webclient niet openen

Controleer of er is verbinding met internet via een andere website. bijvoorbeeld, [www.Bing.com](https://www.bing.com).

Gebruik **nslookup** om te bevestigen DNS de FQDN-naam kunt oplossen:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Probeer verbinding te maken met een andere client, zoals Extern bureaublad-client voor Windows 7 of Windows 10 en controle om te zien als u de webclient kunt openen.

### <a name="error-opening-another-site-fails"></a>Fout: Openen van een andere site mislukt

**Oorzaak:** Netwerkproblemen en/of storingen.

**Fix:** Neem contact op met ondersteuning voor netwerken.

### <a name="error-nslookup-cannot-resolve-the-name"></a>Fout: Nslookup kan de naam niet omzetten

**Oorzaak:** Netwerkproblemen en/of storingen.

**Fix:** Neem contact op met ondersteuning voor netwerken

### <a name="error-you-cant-connect-but-other-clients-can-connect"></a>Fout: Andere clients verbinding kunnen maken, maar u geen verbinding maken

**Oorzaak:** De browser zich niet gedraagt als verwacht en gestopt werken.

**Fix:** Volg deze instructies voor het oplossen van de browser.

1. De browser opnieuw te starten.
2. Browsercookies wissen. Zie [verwijderen van cookiebestanden in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Wis de browsercache. Zie [Wis de browsercache van de voor uw browser](https://binged.it/2RKyfdU).
4. Geopende browservenster in de privémodus.

## <a name="web-client-stops-responding-or-disconnects"></a>Web-client niet meer reageert of de verbinding verbreekt

Probeer verbinding te maken met behulp van een andere browser of de client.

### <a name="error-other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Fout: Andere browsers en de clients ook niet goed of is niet te openen

**Oorzaak:** Problemen met het netwerk en/of bewerking systeem of stroomonderbrekingen

**Fix:** Neem contact op met voor ondersteuningsteams.

## <a name="web-client-keeps-prompting-for-credentials"></a>Web-client blijft vragen om referenties

Als de webclient om referenties vragen blijft, volgt u deze instructies.

1. Controleer of de dat web client-URL juist is.
2. Bevestig dat de referenties voor de virtuele Windows-bureaublad-omgeving is gebonden aan de URL zijn.
3. Browsercookies wissen. Zie [verwijderen van cookiebestanden in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Wis de browsercache. Zie [Wis de browsercache van de voor uw browser](https://binged.it/2RKyfdU).
5. Geopende browservenster in de privémodus.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Extern-bureaubladclient voor Windows 7 of Windows 10 niet meer reageert of kan niet worden geopend

Gebruik de volgende PowerShell-cmdlets voor het opschonen van de out-of-band (OOB)-client registers.

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Navigeer naar **%AppData%\RdClientRadc** en alle inhoud verwijderen.

Verwijderen en opnieuw installeren van extern bureaublad-client voor Windows 7 en Windows 10.

## <a name="troubleshooting-end-user-connectivity"></a>Oplossen van problemen met connectiviteit voor eindgebruikers

Soms gebruikers kunnen toegang krijgen tot hun feed en lokale bronnen, maar nog steeds configuratie, de beschikbaarheid of prestatieproblemen die verhinderen dat ze toegang tot externe bronnen. In deze gevallen wordt ontvangt de gebruiker berichten die vergelijkbaar is met deze:

![Foutbericht voor extern bureaublad-verbinding.](media/eb76b666808bddb611448dfb621152ce.png)

![Kan geen verbinding maken met het foutbericht van de gateway.](media/a8fbb9910d4672147335550affe58481.png)

Volg deze instructies voor algemene probleemoplossing voor de client verbinding-foutcodes.

1. Controleer of de gebruikersnaam en het tijdstip waarop het probleem is opgetreden.
2. Open **PowerShell** en verbinding maken met de virtuele Windows-bureaublad-tenant waar het probleem is gerapporteerd.
3. Controleer of de verbinding met de juiste tenant met **Get-RdsTenant.**
4. Met behulp van **Get-RdsHostPool** en **Get-RdsSessionHost** cmdlets, bevestig dat probleemoplossing is wordt uitgevoerd op de juiste host-groep.
5. Voer de volgende opdracht om een lijst van alle mislukte activiteiten van het type verbinding voor het opgegeven tijdvenster:

    ```PowerShell
     Get-RdsDiagnosticActivities -TenantName <TenantName> -username <UPN> -StartTime
     "11/21/2018 1:07:03 PM" -EndTime "11/21/2018 1:27:03 PM" -Outcome Failure -ActivityType Connection
    ```

6. Met behulp van de **ActivityId** uit de vorige cmdlet-uitvoer, de onderstaande opdracht uitvoeren:

    ```PowerShell
    (Get-RdsDiagnosticActivities -TenantName $tenant -ActivityId <ActivityId> -Detailed).Errors
    ```

7. De opdracht geeft een resultaat vergelijkbaar met de uitvoer die hieronder wordt weergegeven. Gebruik **ErrorCodeSymbolic** en **ErrorMessage** om op te lossen de hoofdoorzaak te achterhalen.

    ```PowerShell
    ErrorSource       : <Source>
    ErrorOperation    : <Operation>
    ErrorCode         : <Error code>
    ErrorCodeSymbolic : <Error code string>
    ErrorMessage      : <Error code message>
    ErrorInternal     : <Internal for the OS>
    ReportedBy        : <Reported by component>
    Time              : <Timestampt>
    ```

### <a name="error-oaddusertogroupfailed--failed-to-add-user--username-to-group--remote-desktop-users-reason-win32errornosuchmember"></a>Fout: O_ADD_USER_TO_GROUP_FAILED / gebruiker toevoegen is mislukt = ≤username≥ aan groep = Remote Desktop Users. Reden: Win32.ERROR_NO_SUCH_MEMBER

**Oorzaak:** Virtuele machine is niet toegevoegd aan het domein waarin de gebruikersobject zich.

**Fix:** VM toevoegen aan het juiste domein. Zie [een Windows Server-machine toevoegen aan een beheerd domein](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).

### <a name="error-nslookup-cannot-resolve-the-name"></a>Fout: Nslookup kan de naam niet omzetten

**Oorzaak:** Netwerkproblemen of stroomonderbrekingen.

**Fix:** Neem contact op met ondersteuning voor netwerken

### <a name="error-connectionfailedclientprotocolerror"></a>Fout: ConnectionFailedClientProtocolError

**Oorzaak:** Virtuele machines die gebruiker probeert verbinding maken met zijn niet toegevoegd aan een domein.

**Fix:** Neem deel aan alle virtuele machines die deel van een groep host van de domeincontroller uitmaken.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Gebruiker verbinding maakt, maar er niets wordt weergegeven (Er is geen invoer)

Een gebruiker kunt beginnen met extern bureaublad-clients en is geverifieerd, maar de gebruiker geen pictogrammen in de web-detectie-feed niet zien.

Bevestig dat de gebruiker die de problemen melden is toegewezen aan groepen met behulp van deze vanaf de opdrachtregel:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Bevestig dat de gebruiker met de juiste referenties aanmeldt zich.

Als de WebClient wordt gebruikt, controleert u of er zijn geen referenties in de cache-problemen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht over het oplossen van virtuele Windows-bureaublad en de sporen escalatie [overzicht, feedback en ondersteuning voor probleemoplossing](troubleshoot-set-up-overview.md).
- Zie voor het oplossen van problemen tijdens het maken van een tenant en host-pool in een omgeving met virtuele Windows-bureaublad, [Tenant en de host een pool maken](troubleshoot-set-up-issues.md).
- Zie voor het oplossen van problemen tijdens het configureren van een virtuele machine (VM) in virtuele Windows-bureaublad, [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md).
- Zie voor het oplossen van problemen bij het gebruik van PowerShell met virtuele Windows-bureaublad, [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup?).
- Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
