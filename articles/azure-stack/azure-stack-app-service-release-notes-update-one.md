---
title: App Service op Azure Stack update 1 release-opmerkingen | Microsoft Docs
description: Meer informatie over wat er in update voor App Service op Azure Stack, de bekende problemen en het downloaden van de update.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: anwestg
ms.reviewer: sethm
ms.lastreviewed: 03/20/2018
ms.openlocfilehash: 9288ffa406b0e5765b35fa9b23aa13dee5754e9a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240025"
---
# <a name="app-service-on-azure-stack-update-1-release-notes"></a>App Service op Azure Stack update 1 release-opmerkingen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 1 en bekende problemen. Bekende problemen zijn onderverdeeld in problemen direct verband houden met de implementatie, het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1802 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service implementeert.
>
>

## <a name="build-reference"></a>Naslaginformatie over bouwen

De App Service op Azure Stack Update 1 build-nummer is **69.0.13698.9**

### <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Nieuwe implementaties van Azure App Service in Azure Stack nu vereisen een [drie-onderwerp jokertekencertificaat](azure-stack-app-service-before-you-get-started.md#get-certificates) door verbeteringen in de manier waarop SSO voor Kudu nu wordt uitgevoerd in Azure App Service. Nieuw onderwerp is  **\*. sso.appservice.\< regio\>.\< domeinnaam\>.\< extensie\>**
>
>

Raadpleeg de [voordat u aan de slag-documentatie](azure-stack-app-service-before-you-get-started.md) voordat u begint met implementeren.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Azure App Service op Azure Stack Update 1 omvat de volgende verbeteringen en oplossingen:

- **Hoge beschikbaarheid van Azure App Service** -de Azure Stack 1802 update is ingeschakeld workloads om te worden geïmplementeerd op verschillende foutdomeinen. App Service-infrastructuur is daarom kunnen worden fouttolerant omdat deze wordt geïmplementeerd in domeinen met fouten. Standaard alle nieuwe implementaties van Azure App Service heeft deze mogelijkheid maar voor implementaties die zijn voltooid voordat u Azure Stack 1802 update wordt toegepast verwijzen naar de [Foutdomein van App Service-documentatie](azure-stack-app-service-fault-domain-update.md)

- **Implementeren in een bestaand virtueel netwerk** -klanten kunnen nu App-Service in Azure Stack implementeren in een bestaand virtueel netwerk. Implementeren in een bestaand virtueel netwerk kan klanten verbinding maken met de SQL Server en File Server, die vereist zijn voor Azure App Service, via particuliere poorten. Tijdens de implementatie van klanten kunnen selecteren om te implementeren in een bestaand virtueel netwerk, maar [moet subnetten voor gebruik door App Service maken](azure-stack-app-service-before-you-get-started.md#virtual-network) voorafgaand aan de implementatie.

- Updates voor **App Service-Tenant, Admin, portals van functies en hulpprogramma's voor Kudu**. Consistent zijn met Azure Stack-Portal SDK-versie.

- **Updates voor de volgende App-frameworks en hulpmiddelen**:
    - Toegevoegd **.Net Core 2.0** ondersteunen
    - Toegevoegd **Node.JS** versies:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Toegevoegd **NPM** versies:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Toegevoegd **PHP** Updates:
        - 5.6.32
        - 7.0.26 (x86 en x64)
        - 7.1.12 (x86 en x64)
    - Bijgewerkt **Git voor Windows** v 2.14.1
    - Bijgewerkt **Mercurial** naar v4.5.0

  - Ondersteuning toegevoegd voor **alleen HTTPS** functie binnen de functie voor aangepaste domein in de Portal voor App Service-Tenant. 

  - Extra validatie van de opslagverbinding in de kiezer voor aangepaste opslag voor Azure Functions 

#### <a name="fixes"></a>Oplossingen

- Bij het maken van een offline implementatiepakket, ontvangt klanten geen foutbericht toegang geweigerd bij het openen van de map van het App Service-installatieprogramma

- Problemen opgelost bij het werken in de functie voor aangepaste domeinen in de Portal voor App Service-Tenant.

- Voorkomen dat klanten met behulp van de beheerder van de gereserveerde namen tijdens de installatie

- App Service-implementatie met ingeschakeld **toegevoegd aan een domein** bestandsserver

- Verbeterde voor het ophalen van Azure Stack-basiscertificaat van het certificaat in het script en valideert u nu het basiscertificaat in het installatieprogramma van App Service.

- Vaste onjuiste status naar Azure Resource Manager worden geretourneerd wanneer een abonnement is verwijderd die ingesloten resources in de naamruimte Microsoft.Web.

### <a name="known-issues-with-the-deployment-process"></a>Bekende problemen met het implementatieproces

- Certificaatvalidatiefouten

Sommige klanten zijn problemen opgetreden bij het opgeven van certificaten naar de App Service-installatieprogramma bij het implementeren van een geïntegreerd systeem, vanwege een te streng validatie in het installatieprogramma. Het installatieprogramma van de App Service is opnieuw uitgebracht, moeten klanten [download het installatieprogramma van de bijgewerkte](https://aka.ms/appsvconmasinstaller). Als u problemen met het valideren van certificaten met het installatieprogramma van de bijgewerkte zich blijft voordoen, neem dan contact op met ondersteuning.

- Probleem opgetreden bij het ophalen van Azure Stack-basiscertificaat van geïntegreerd systeem.

Een fout in de Get-AzureStackRootCert.ps1 veroorzaakt klanten kunnen niet worden opgehaald van de Azure Stack-basiscertificaat wanneer het script wordt uitgevoerd op een computer die geen het basiscertificaat dat is geïnstalleerd. Het script is nu ook opnieuw uitgebracht, het oplossen van dit probleem en de aanvraag klanten [downloaden van de bijgewerkte helperscripts](https://aka.ms/appsvconmashelpers). Als u problemen bij het ophalen van het basiscertificaat met het bijgewerkte script zich blijft voordoen, neem dan contact op met ondersteuning.

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

- Er zijn geen bekende problemen voor het bijwerken van Azure App Service op Azure Stack Update 1.

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

- Wisselen van sleuf werkt niet

Site-sleuf wisselen is onderverdeeld in deze release. Voor het herstellen van de functionaliteit van de volgende stappen uit:

1. Wijzigen van de ControllersNSG Netwerkbeveiligingsgroep naar **toestaan** verbindingen met extern bureaublad naar de exemplaren van de App controller. AppService.local vervangen door de naam van de resourcegroep waarin die u App Service in geïmplementeerd.

    ```powershell
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

2. Blader naar de **CN0 VM** onder virtuele Machines in de Azure Stack-beheerdersportal en **op verbinding maken met** openen van een extern bureaublad-sessiehost met het exemplaar van de domeincontroller. Gebruik de referenties die zijn opgegeven tijdens de implementatie van App Service.
3. Start **PowerShell als beheerder** en voer het volgende script

    ```powershell
        Import-Module appservice

        $sm = new-object Microsoft.Web.Hosting.SiteManager

        if($sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$true)
        {
          $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$false
        #  'Slot swap mode reverted'
        }
        
        # Confirm new setting is false
        $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus
        
        # Commit Changes
        $sm.CommitChanges()

        Get-AppServiceServer -ServerType ManagementServer | ForEach-Object Repair-AppServiceServer
        
    ```

4. Sluit de extern bureaublad-sessiehost.
5. Herstellen van de ControllersNSG Netwerkbeveiligingsgroep naar **weigeren** verbindingen met extern bureaublad naar de exemplaren van de App controller. AppService.local vervangen door de naam van de resourcegroep waarin die u App Service in geïmplementeerd.

    ```powershell

        Add-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

6. Werknemers kunnen geen bestandsserver bereiken wanneer App Service is geïmplementeerd in een bestaand virtueel netwerk en de bestandsserver alleen beschikbaar op het particuliere netwerk is.

Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen de worker-subnet en de bestandsserver aan te geven. Om dit te doen, gaat u naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen toe:

- Bron: Alle
- Poortbereik van bron: *
- Bestemming: IP-adressen
- Doel-IP-adresbereik: Bereik van IP-adressen voor uw bestandsserver
- Poortbereik van doel: 445
- Protocol: TCP
- Actie: Toestaan
- Prioriteit: 700
- Naam: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure App Service in Azure Stack

Raadpleeg de documentatie in de [opmerkingen bij de Release van Azure Stack 1802](azure-stack-update-1802.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure App Service, [Azure App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden van het App Service implementeren in Azure Stack [voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
