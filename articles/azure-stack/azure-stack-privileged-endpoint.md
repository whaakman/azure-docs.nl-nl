---
title: Met behulp van de bevoegde eindpunt in Azure-Stack | Microsoft Docs
description: Laat zien hoe het eindpunt te gebruiken bevoorrechte in Azure-Stack (voor een Azure-Stack-operator).
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 80c3f248edb40b66e3177c512f3caf77295c6c5d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Met behulp van de bevoegde eindpunt in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als een Azure-Stack-operator, moet u de beheerdersportal PowerShell of Azure Resource Manager-API's gebruiken voor meest dagelijkse beheertaken. Evenwel sommige minder algemene bewerkingen u wilt gebruiken, de *bevoegde eindpunt* (PEP). Dit eindpunt is een vooraf geconfigureerde externe PowerShell console waarmee u net voldoende mogelijkheden om u te helpen bij het uitvoeren van een vereiste taak. Het eindpunt maakt gebruik van PowerShell JEA (net genoeg beheer) om een beperkte set cmdlets weer te geven. Als u wilt toegang tot de bevoegde eindpunt en de beperkte set cmdlets worden aangeroepen, wordt een account met beperkte bevoegdheden gebruikt. Er is geen administrator-accounts zijn vereist. Voor extra beveiliging, is uitvoeren van scripts niet toegestaan.

Het eindpunt bevoorrechte kunt u taken uitvoert zoals het volgende:

- Taken kunnen uitvoeren op laag niveau, zoals [verzamelen van diagnostische logboeken](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Taken uit te voeren veel datacenter na de implementatie-integratie voor geïntegreerde systemen, zoals het toevoegen van Domain Name System (DNS) doorstuurservers na de implementatie grafiek integratie, integratie van Active Directory Federation Services (AD FS), certificaat instellen wisselen, enzovoort.
- Werken met ondersteuning voor tijdelijke, op hoog niveau van toegang voor diepgaande probleemoplossing van geïntegreerde. 

Het eindpunt bevoorrechte registreert elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. Dit biedt volledige doorzichtigheid en volledige controle van bewerkingen. U kunt deze logboekbestanden voor toekomstige controledoeleinden behouden.

> [!NOTE]
> In Azure Stack Development Kit (ASDK), kunt u uitvoeren enkele van de opdrachten die beschikbaar zijn in de beschermde eindpunt rechtstreeks vanuit een PowerShell-sessie op de host van development kit. U kunt echter testen van bepaalde bewerkingen met behulp van de bevoegde eindpunt, zoals logboekgegevens verzameld, omdat dit de enige beschikbare methode voor bepaalde bewerkingen uitvoeren in een geïntegreerde omgeving.

## <a name="access-the-privileged-endpoint"></a>Toegang tot de bevoegde eindpunt

U toegang tot de bevoegde eindpunt via een externe PowerShell-sessie op de virtuele machine die als host fungeert voor de privileged eindpunt. In de ASDK heet deze virtuele machine AzS ERCS01. Als u een geïntegreerd systeem, er zijn drie exemplaren van de bevoegde eindpunt waarop wordt uitgevoerd binnen een virtuele machine (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*-ERCS03) op verschillende hosts voor tolerantie. 

Voordat u deze procedure voor een geïntegreerde, zorg er dan voor dat u toegang hebt tot een bevoorrechte eindpunt door IP-adres of via DNS is vastgesteld. Na de eerste implementatie van Azure-Stack kunt u het bevoegde eindpunt openen alleen op IP-adres, omdat de DNS-integratie is nog niet ingesteld. Uw hardwareleverancier OEM krijgt u een JSON-bestand met de naam 'AzureStackStampDeploymentInfo', dat de bevoegde eindpunt IP-adressen bevat.

Het is raadzaam dat u verbinding met het bevoorrechte eindpunt alleen vanaf de host van de levenscyclus van hardware of op een computer toegewezen, vergrendelde zoals maken een [bevoorrechte toegang werkstation](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Voer een van de volgende, afhankelijk van uw omgeving:

    - Voer de volgende opdracht om de bevoorrechte eindpunt toevoegen als een vertrouwde host in de hardware lifecycle host of het bevoorrechte toegang werkstation op een geïntegreerd systeem.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Als u de ADSK uitvoert, moet u zich aanmelden bij de host van development kit.

2. Open een Windows PowerShell-sessie met verhoogde bevoegdheden op uw hardware lifecycle host of bevoorrechte toegang werkstation. Voer de volgende opdrachten tot stand brengen van een externe sessie op de virtuele machine die als host fungeert voor de privileged eindpunt:
 
    - Op een geïntegreerde systeem:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      De `ComputerName` parameter is het IP-adres of de DNS-naam van een van de virtuele machines die als host fungeert voor een bevoorrechte-eindpunt. 
    - Als u de ADSK uitvoert:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wanneer u wordt gevraagd, gebruikt u de volgende referenties:

      - **Gebruikersnaam**: Geef het account CloudAdmin in de indeling  **&lt;* Azure Stack domein*&gt;\cloudadmin**. (Voor ASDK, de gebruikersnaam is **azurestack\cloudadmin**.)
      - **Wachtwoord**: Voer het wachtwoord dat is opgegeven tijdens de installatie voor de administrator-account van het AzureStackAdmin.
    
3.  Nadat u verbinding maakt, verandert de prompt voor  **[*IP-adres of ERCS VM naam*]: PS > ** of **[azs ercs01]: PS >**, afhankelijk van de omgeving. Hier kunt uitvoeren `Get-Command` om de lijst met beschikbare cmdlets weer te geven.

    ![De cmdlet Get-Command uitvoer met een lijst met beschikbare opdrachten](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Veel van deze cmdlets zijn alleen bedoeld voor geïntegreerde systeemomgevingen (zoals de cmdlets die betrekking hebben op datacenter-integratie). In de ASDK zijn de volgende cmdlets gevalideerd:

    - Clear-Host
    - Sluit PrivilegedEndpoint
    - Exit-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Opdracht GET
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Meting-Object
    - Nieuwe CloudAdminUser
    - Uitgaande standaard
    - Verwijder CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test AzureStack
    - Stop AzureStack
    - Get-ClusterLog

4.  Omdat het uitvoeren van scripts is niet toegestaan, kunt u tab-aanvulling niet gebruiken voor parameterwaarden. Als u de lijst met parameters voor een bepaalde cmdlet, moet u de volgende opdracht uitvoeren:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    Als u een willekeurig type Scriptbewerking probeert, mislukt de bewerking met de fout **ScriptsNotAllowed**. Dit is verwacht gedrag.

## <a name="close-the-privileged-endpoint-session"></a>De sessie bevoorrechte eindpunt sluiten

 Zoals eerder gezegd, registreert het bevoegde eindpunt elke actie (en de bijbehorende uitvoer) die u in de PowerShell-sessie uitvoeren. U moet de sessie sluiten met behulp van de `Close-PrivilegedEndpoint` cmdlet. Deze cmdlet correct wordt het eindpunt wordt gesloten en de logboekbestanden worden overgedragen naar een externe bestandsshare voor het bewaren van.

De eindpunt-sessie te sluiten:

1. Maak een externe bestandsshare die toegankelijk is door het bevoegde eindpunt. In een ontwikkelingsomgeving kit, kunt u een bestandsshare maken op de host van development kit.
2. Voer de `Close-PrivilegedEndpoint` cmdlet. 
3. U wordt gevraagd om een pad op om op te slaan van het logboekbestand van de tekst. Geef de bestandsshare die u eerder hebt gemaakt, in de indeling &#92; &#92; *servername*&#92; *sharenaam*. Als u geen pad opgeeft, wordt de cmdlet mislukt en blijft de sessie geopend. 

    ![Uitvoer van de cmdlet sluiten PrivilegedEndpoint waarin wordt weergegeven waarin u het doelpad van de tekst opgeven](media/azure-stack-privileged-endpoint/closeendpoint.png)

Nadat de logboekbestanden van de tekst worden overgebracht naar de bestandsshare, zijn ze automatisch verwijderd van het bevoegde eindpunt. Als u de bevoorrechte endpoint-sessie met de cmdlets sluiten `Exit-PSSession` of `Exit`, of u alleen de PowerShell-console sluit, wordt de logboeken van de tekst niet overbrengen naar een bestandsshare. Ze blijven in de beschermde eindpunt. De volgende keer dat u uitvoert `Close-PrivilegedEndpoint` en een bestandsshare omvatten, maar de logboeken van de tekst van de vorige sessie (s) ook draagt.

## <a name="next-steps"></a>Volgende stappen
[Azure Stack diagnostische hulpprogramma 's](azure-stack-diagnostics.md)







