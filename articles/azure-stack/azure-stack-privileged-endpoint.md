---
title: Met behulp van het eindpunt van de bevoegde in Azure Stack | Microsoft Docs
description: Ziet u hoe u het eindpunt van de bevoegde (PEP) in Azure Stack (voor Azure Stack-operators).
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.openlocfilehash: d91ad42d4d84802c4579a4d228a35030970da8e4
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377238"
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Met behulp van het eindpunt van de bevoegde in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als Azure Stack-operators, moet u de administrator-portal, PowerShell of Azure Resource Manager-API's gebruiken voor meest dagelijkse beheertaken. Voor sommige minder veelvoorkomende bewerkingen, u moet echter gebruik van de *bevoegde eindpunt* (PEP). De PEP is een vooraf geconfigureerde externe PowerShell console waarmee u net voldoende mogelijkheden om u te helpen bij het uitvoeren van een vereiste taak. Maakt gebruik van het eindpunt [PowerShell JEA (Just Enough Administration)](https://docs.microsoft.com/powershell/jea/overview) om slechts een beperkt aantal cmdlets zichtbaar te maken. Voor toegang tot de PEP en aanroepen van de beperkte set van cmdlets, wordt een account met beperkte bevoegdheden gebruikt. Er is geen administrator-accounts zijn vereist. Voor extra beveiliging is uitvoeren van scripts niet toegestaan.

U kunt de PEP gebruiken om uit te voeren taken, zoals het volgende:

- Taken uit te voeren op laag niveau, zoals [verzamelen van diagnostische logboeken](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Om uit te voeren veel taken voor na de implementatie datacenter-integratie voor geïntegreerde systemen, zoals het toevoegen van Domain Name System (DNS)-doorstuurservers na de implementatie, instellen van Microsoft Graph-integratie, integratie van Active Directory Federation Services (AD FS), rotatie van certificaten, enzovoort.
- Werken met ondersteuning voor het verkrijgen van toegang tot de tijdelijke, op hoog niveau voor het grondig oplossen van problemen van een geïntegreerd systeem.

De PEP registreert elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. Dit biedt volledige controle van bewerkingen en volledige transparantie. U kunt deze logboekbestanden voor toekomstige controle behouden.

> [!NOTE]
> In de Azure Stack Development Kit (ASDK), kunt u uitvoeren enkele van de opdrachten die beschikbaar zijn in de PEP rechtstreeks vanuit een PowerShell-sessie op de host van development kit. U wilt echter bepaalde bewerkingen met behulp van de PEP, zoals logboekverzameling, testen, omdat dit de enige beschikbare methode voor bepaalde bewerkingen uitvoeren in een omgeving met geïntegreerde systemen.

## <a name="access-the-privileged-endpoint"></a>Het eindpunt van de bevoegde toegang

U toegang tot de PEP via een externe PowerShell-sessie op de virtuele machine die als host fungeert voor de PEP. In de ASDK deze virtuele machine met de naam **AzS-ERCS01**. Als u een geïntegreerd systeem, er zijn drie exemplaren van de PEP, elk uitgevoerd binnen een virtuele machine (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*- ERCS03) op verschillende hosts voor tolerantie. 

Voordat u deze procedure voor een geïntegreerd systeem, zorg er dan voor dat u kunt toegang krijgen tot de PEP door IP-adres of via DNS. Na de eerste implementatie van Azure Stack, kunt u de PEP openen alleen door IP-adres, omdat de DNS-integratie is nog niet ingesteld. De leverancier van de OEM-hardware beschikt u over een JSON-bestand met de naam **AzureStackStampDeploymentInfo** die de PEP IP-adressen bevat.


> [!NOTE]
> Uit veiligheidsoverwegingen, vereist dat u verbinding met de PEP alleen vanaf een beveiligde virtuele machine die wordt uitgevoerd boven op de host van de levenscyclus van hardware, of op een computer toegewezen, veilige, zoals maken een [Privileged Access Workstation](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations). De oorspronkelijke configuratie van de host van de levenscyclus van hardware moet niet worden gewijzigd vanuit de oorspronkelijke configuratie, met inbegrip van nieuwe software installeren en het verbinding maken met de PEP mag worden gebruikt.

1. De vertrouwensrelatie.

    - Voer de volgende opdracht uit vanuit een verhoogde Windows PowerShell-sessie de PEP toevoegen als een vertrouwde host op de beveiligde virtuele machine die wordt uitgevoerd op de host van de levenscyclus van hardware of het werkstation met bevoegde toegang op een geïntegreerd systeem.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Als u de ADSK uitvoert, moet u zich aanmelden bij de host van development kit.

2. Open op de beveiligde virtuele machine die wordt uitgevoerd op de host van de levenscyclus van hardware of het werkstation met bevoegde toegang, een Windows PowerShell-sessie. Voer de volgende opdrachten tot stand brengen van een externe sessie op de virtuele machine die als host fungeert voor de PEP:
 
    - Op een geïntegreerd systeem:
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
   Wanneer u hierom wordt gevraagd, gebruikt u de volgende referenties:

      - **Gebruikersnaam**: Geef het account CloudAdmin in de notatie  **&lt; *Azure Stack-domein*&gt;\cloudadmin**. (Voor ASDK, de gebruikersnaam van de is **azurestack\cloudadmin**.)
      - **Wachtwoord**: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domein administrator-account.

    > [!NOTE]
    > Als u geen verbinding maken met het eindpunt ERCS, probeert u stap 1 en 2 opnieuw met het IP-adres van een ERCS VM waaraan u dit nog niet hebt al geprobeerd om verbinding te maken.

3.  Nadat u verbinding hebt gemaakt, verandert de prompt in **[*IP-adres of ERCS VM de naam van*]: PS >** of **[azs-ercs01]: PS >**, afhankelijk van de omgeving. Voer hier `Get-Command` om de lijst met beschikbare cmdlets weer te geven.

    Veel van deze cmdlets zijn alleen bedoeld voor omgevingen geïntegreerd systeem (zoals de cmdlets met betrekking tot datacenter-integratie). In de ASDK, zijn de volgende cmdlets gevalideerd:

    - Clear-Host
    - Sluit PrivilegedEndpoint
    - Afsluiten PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Meting-Object
    - New-CloudAdminUser
    - Uitgaande standaard
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Tips voor het gebruik van het eindpunt van de bevoegde 

Zoals eerder vermeld, de PEP is een [PowerShell JEA](https://docs.microsoft.com/powershell/jea/overview) eindpunt. Terwijl er een sterke beveiligingslaag, vermindert u enkele van de basisfuncties van PowerShell, zoals scripts of het tabblad voltooiing een JEA-eindpunt. Als u elk type Scriptbewerking probeert, mislukt de bewerking met de fout **ScriptsNotAllowed**. Dit is normaal.

Dus bijvoorbeeld uitvoeren als u de lijst met parameters voor een bepaalde cmdlet, u de volgende opdracht:

```PowerShell
    Get-Command <cmdlet_name> -Syntax
```

U kunt ook kunt u de [Import-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) cmdlet voor het importeren van alle PEP-cmdlets in de huidige sessie op uw lokale computer. Op deze manier zijn alle cmdlets en -functies van de PEP nu beschikbaar op uw lokale computer, samen met de tab-Aanvulling en informatie over het algemeen uitvoeren van scripts. 

Voor het importeren van de sessie PEP op uw lokale computer, moet u de volgende stappen uitvoeren:

1. De vertrouwensrelatie.

    -Voer de volgende opdracht uit vanaf een verhoogde Windows PowerShell-sessie de PEP toevoegen als een vertrouwde host op de beveiligde virtuele machine die wordt uitgevoerd op de host van de levenscyclus van hardware of het Privileged Access Workstation op een geïntegreerd systeem.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Als u de ADSK uitvoert, moet u zich aanmelden bij de host van development kit.

2. Open op de beveiligde virtuele machine die wordt uitgevoerd op de host van de levenscyclus van hardware of het werkstation met bevoegde toegang, een Windows PowerShell-sessie. Voer de volgende opdrachten tot stand brengen van een externe sessie op de virtuele machine die als host fungeert voor de PEP:
 
    - Op een geïntegreerd systeem:
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
   Wanneer u hierom wordt gevraagd, gebruikt u de volgende referenties:

      - **Gebruikersnaam**: Geef het account CloudAdmin in de notatie  **&lt; *Azure Stack-domein*&gt;\cloudadmin**. (Voor ASDK, de gebruikersnaam van de is **azurestack\cloudadmin**.)
      - **Wachtwoord**: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domein administrator-account.

3. De sessie PEP importeren in uw lokale computer
    ````PowerShell 
        Import-PSSession $session
    ````
4. U kunt nu tab-aanvulling gebruiken en voer scripts gewoon op uw lokale PowerShell-sessie met de functies en -cmdlets van de PEP zonder het verlagen van de beveiligingsstatus van Azure Stack. Veel plezier!


## <a name="close-the-privileged-endpoint-session"></a>Sluit de sessie bevoegde eindpunt

 Zoals eerder vermeld, registreert de PEP voor elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. U moet de sessie sluiten met behulp van de `Close-PrivilegedEndpoint` cmdlet. Deze cmdlet correct wordt het eindpunt wordt gesloten en de logboekbestanden naar een externe bestandsshare voor het bewaren van brengt.

De eindpunt-sessie sluiten:

1. Een externe bestandsshare die toegankelijk is via de PEP maken. In een ontwikkelomgeving kit, kunt u alleen een bestandsshare maken op de host van development kit.
2. Voer de `Close-PrivilegedEndpoint` cmdlet. 
3. U wordt gevraagd naar een pad op om op te slaan van het logboekbestand transcript. Geef de bestandsshare die u eerder hebt gemaakt, in de indeling &#92; &#92; *servername*&#92;*sharename*. Als u geen pad opgeeft, wordt de cmdlet mislukt en blijft de sessie geopend. 

    ![Uitvoer van de cmdlet sluiten PrivilegedEndpoint waarin wordt weergegeven waarin u het transcript doelpad opgeven](media/azure-stack-privileged-endpoint/closeendpoint.png)

Nadat de logboekbestanden van het transcript worden overgebracht naar de bestandsshare, worden ze automatisch verwijderd uit de PEP. 

> [!NOTE]
> Als u de sessie PEP sluiten met behulp van de cmdlets `Exit-PSSession` of `Exit`, of u alleen de PowerShell-console sluit, wordt het transcript logboeken worden niet overgedragen naar een bestandsshare. Ze blijven in de PEP. De volgende keer dat u uitvoert `Close-PrivilegedEndpoint` en zijn onder andere een bestandsshare, het transcript logboeken van de vorige sessie (s) worden ook overgebracht. Gebruik geen `Exit-PSSession` of `Exit` te sluiten van de sessie PEP; gebruik `Close-PrivilegedEndpoint` in plaats daarvan.


## <a name="next-steps"></a>Volgende stappen
[Azure Stack diagnostische hulpprogramma 's](azure-stack-diagnostics.md)
