---
title: Met behulp van de bevoegde eindpunt in Azure-Stack | Microsoft Docs
description: Laat zien hoe de bevoorrechte eindpunt (PEP) te gebruiken in Azure-Stack (voor een Azure-Stack-operator).
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.openlocfilehash: f176e0689c630a406ab6e2f82e9320a214ff8a1a
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Met behulp van de bevoegde eindpunt in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als een Azure-Stack-operator, moet u de beheerdersportal PowerShell of Azure Resource Manager-API's gebruiken voor meest dagelijkse beheertaken. Evenwel sommige minder algemene bewerkingen u wilt gebruiken, de *bevoegde eindpunt* (PEP). De PEP is een vooraf geconfigureerde externe PowerShell console waarmee u net voldoende mogelijkheden om u te helpen bij het uitvoeren van een vereiste taak. Maakt gebruik van het eindpunt [PowerShell JEA (net genoeg beheer)](https://docs.microsoft.com/en-us/powershell/jea/overview) om slechts een beperkt aantal cmdlets weer te geven. Voor toegang tot de PEP en de beperkte set cmdlets worden aangeroepen, wordt een account met beperkte bevoegdheden gebruikt. Er is geen administrator-accounts zijn vereist. Voor extra beveiliging, is uitvoeren van scripts niet toegestaan.

De PEP kunt u taken uitvoert zoals het volgende:

- Taken kunnen uitvoeren op laag niveau, zoals [verzamelen van diagnostische logboeken](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Taken uit te voeren veel datacenter na de implementatie-integratie voor geïntegreerde systemen, zoals het toevoegen van Domain Name System (DNS) doorstuurservers na de implementatie, instellen van integratie met Microsoft Graph, integratie van Active Directory Federation Services (AD FS), certificaat wisselen, enzovoort.
- Werken met ondersteuning voor tijdelijke, op hoog niveau van toegang voor diepgaande probleemoplossing van geïntegreerde.

De PEP registreert elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. Dit biedt volledige doorzichtigheid en volledige controle van bewerkingen. U kunt deze logboekbestanden voor toekomstige controles behouden.

> [!NOTE]
> In de Azure Stack Development Kit (ASDK), kunt u uitvoeren enkele van de opdrachten die beschikbaar zijn in de PEP rechtstreeks vanuit een PowerShell-sessie op de host van development kit. U wilt echter bepaalde bewerkingen met behulp van de PEP, zoals logboekgegevens verzameld testen omdat dit de enige beschikbare methode is voor bepaalde bewerkingen uitvoeren in een geïntegreerde omgeving.

## <a name="access-the-privileged-endpoint"></a>Toegang tot de bevoegde eindpunt

U opent de PEP met een externe PowerShell-sessie op de virtuele machine die als host fungeert voor de PEP. In de ASDK deze virtuele machine met de naam **AzS ERCS01**. Als u een geïntegreerd systeem, er zijn drie exemplaren van de PEP, elke uitgevoerd binnen een virtuele machine (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*- ERCS03) op verschillende hosts voor tolerantie. 

Voordat u deze procedure voor een geïntegreerde, zorg er dan voor dat u toegang hebt tot de PEP door IP-adres of via DNS is vastgesteld. Na de eerste implementatie van Azure-Stack, kunt u de PEP alleen op IP-adres openen omdat de DNS-integratie is nog niet ingesteld. Uw hardwareleverancier OEM krijgt u een JSON-bestand met de naam **AzureStackStampDeploymentInfo** die de PEP IP-adressen bevat.


> [!NOTE]
> Om veiligheidsredenen vereist dat u verbinding met de PEP alleen vanuit een beperkte virtuele machine wordt uitgevoerd boven op de host van de levenscyclus van hardware of vanaf een speciale, beveiligde computer, zoals maken een [bevoorrechte toegang werkstation](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations). De oorspronkelijke configuratie van de host van de levenscyclus van hardware moet niet worden gewijzigd van de oorspronkelijke configuratie, met inbegrip van nieuwe software installeren en verbinding maken met de PEP mag worden gebruikt.

1. De vertrouwensrelatie.

    - Voer de volgende opdracht vanaf een Windows PowerShell-sessie met verhoogde bevoegdheid de PEP toevoegen als een vertrouwde host in de geharde virtuele machine op de host van de levenscyclus van hardware of het werkstation met Privileged Access op een geïntegreerd systeem.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Als u de ADSK uitvoert, moet u zich aanmelden bij de host van development kit.

2. Open een Windows PowerShell-sessie op de beperkte virtuele machine uitgevoerd op de host van de levenscyclus van hardware of het bevoorrechte toegang werkstation. Voer de volgende opdrachten tot stand brengen van een externe sessie op de virtuele machine die als host fungeert voor de PEP:
 
    - Op een geïntegreerde systeem:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      De `ComputerName` parameter is het IP-adres of de DNS-naam van een van de virtuele machines die als host fungeert voor de PEP. 
    - Als u de ADSK uitvoert:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wanneer u wordt gevraagd, gebruikt u de volgende referenties:

      - **Gebruikersnaam**: Geef het account CloudAdmin in de indeling ** &lt; *Azure Stack domein*&gt;\cloudadmin**. (Voor ASDK, de gebruikersnaam is **azurestack\cloudadmin**.)
      - **Wachtwoord**: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de administrator-account van het AzureStackAdmin.

    > [!NOTE]
    > Als u geen verbinding maken met het eindpunt ERCS, probeert u stap 1 en 2 opnieuw met het IP-adres van een ERCS VM waarop u dit nog niet hebt al geprobeerd om verbinding te.

3.  Nadat u verbinding maakt, verandert de prompt voor **[*IP-adres of ERCS VM naam*]: PS >** of **[azs ercs01]: PS >**, afhankelijk van de omgeving. Hier kunt uitvoeren `Get-Command` om de lijst met beschikbare cmdlets weer te geven.

    Veel van deze cmdlets zijn alleen bedoeld voor geïntegreerde systeemomgevingen (zoals de cmdlets die betrekking hebben op datacenter-integratie). In de ASDK zijn de volgende cmdlets gevalideerd:

    - Clear-Host
    - Close-PrivilegedEndpoint
    - Exit-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Measure-Object
    - New-CloudAdminUser
    - Out-Default
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Tips voor het gebruik van de bevoegde eindpunt 

Zoals eerder vermeld, de PEP is een [PowerShell JEA](https://docs.microsoft.com/en-us/powershell/jea/overview) eindpunt. Een eindpunt JEA vermindert en tegelijkertijd een laag sterke beveiliging, enkele basic PowerShell mogelijkheden, zoals scripts of tabblad voltooiing. Als u een willekeurig type Scriptbewerking probeert, mislukt de bewerking met de fout **ScriptsNotAllowed**. Dit is verwacht gedrag.

Bijvoorbeeld: als u de lijst met parameters voor een bepaalde cmdlet, loopt u de volgende opdracht:

```PowerShell
    Get-Command <cmdlet_name> -Syntax
```

U kunt ook de [Import-PSSession](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) cmdlet alle PEP cmdlets importeren in de huidige sessie op uw lokale machine. Door dit te doen, zijn alle cmdlets en -functies van de PEP nu beschikbaar op uw lokale computer, samen met de tab-Aanvulling en informatie over het algemeen scripting. 

Voer de volgende stappen uit voor het importeren van de sessie PEP op uw lokale machine:

1. De vertrouwensrelatie.

    -Voer de volgende opdracht vanaf een Windows PowerShell-sessie met verhoogde bevoegdheid de PEP toevoegen als een vertrouwde host in de geharde virtuele machine op de host van de levenscyclus van hardware of het werkstation met Privileged Access op-een geïntegreerd systeem.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Als u de ADSK uitvoert, moet u zich aanmelden bij de host van development kit.

2. Open een Windows PowerShell-sessie op de beperkte virtuele machine uitgevoerd op de host van de levenscyclus van hardware of het bevoorrechte toegang werkstation. Voer de volgende opdrachten tot stand brengen van een externe sessie op de virtuele machine die als host fungeert voor de PEP:
 
    - Op een geïntegreerde systeem:
      ````PowerShell
        $cred = Get-Credential

        $session = New-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      De `ComputerName` parameter is het IP-adres of de DNS-naam van een van de virtuele machines die als host fungeert voor de PEP. 
    - Als u de ADSK uitvoert:
     
      ````PowerShell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wanneer u wordt gevraagd, gebruikt u de volgende referenties:

      - **Gebruikersnaam**: Geef het account CloudAdmin in de indeling ** &lt; *Azure Stack domein*&gt;\cloudadmin**. (Voor ASDK, de gebruikersnaam is **azurestack\cloudadmin**.)
      - **Wachtwoord**: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de administrator-account van het AzureStackAdmin.

3. De sessie PEP in uw lokale machine importeren
    ````PowerShell 
        Import-PSSession $session
    ````
4. U kunt nu-tab-aanvulling gebruiken en doen scripts gewoon op uw lokale PowerShell-sessie met de functies en cmdlets van de PEP zonder het verlagen van de beveiligingsstatus van de Azure-Stack. Veel plezier!


## <a name="close-the-privileged-endpoint-session"></a>De sessie bevoorrechte eindpunt sluiten

 Zoals eerder vermeld, wordt de PEP registreert elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. U moet de sessie sluiten met behulp van de `Close-PrivilegedEndpoint` cmdlet. Deze cmdlet correct wordt het eindpunt wordt gesloten en de logboekbestanden worden overgedragen naar een externe bestandsshare voor het bewaren van.

De eindpunt-sessie te sluiten:

1. Een externe bestandsshare die toegankelijk is voor de PEP maken. In een ontwikkelingsomgeving kit, kunt u een bestandsshare maken op de host van development kit.
2. Voer de `Close-PrivilegedEndpoint` cmdlet. 
3. U wordt gevraagd om een pad op om op te slaan van het logboekbestand van de tekst. Geef de bestandsshare die u eerder hebt gemaakt, in de notatie &#92; &#92; *servername*&#92;*sharename*. Als u geen pad opgeeft, wordt de cmdlet mislukt en blijft de sessie geopend. 

    ![Uitvoer van de cmdlet sluiten PrivilegedEndpoint waarin wordt weergegeven waarin u het doelpad van de tekst opgeven](media/azure-stack-privileged-endpoint/closeendpoint.png)

Nadat de logboekbestanden van de tekst worden overgebracht naar de bestandsshare, zijn ze automatisch verwijderd uit de PEP. 

> [!NOTE]
> Als u de sessie PEP sluiten met behulp van de cmdlets `Exit-PSSession` of `Exit`, of u alleen de PowerShell-console sluit, wordt de logboeken van de tekst niet overbrengen naar een bestandsshare. Ze blijven in de PEP. De volgende keer dat u uitvoert `Close-PrivilegedEndpoint` en een bestandsshare omvatten, maar de logboeken van de tekst van de vorige sessie (s) ook draagt. Gebruik geen `Exit-PSSession` of `Exit` te sluiten van de sessie PEP; gebruik `Close-PrivilegedEndpoint` in plaats daarvan.


## <a name="next-steps"></a>Volgende stappen
[Azure Stack diagnostische hulpprogramma 's](azure-stack-diagnostics.md)