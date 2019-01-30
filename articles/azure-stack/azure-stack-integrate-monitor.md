---
title: Oplossing voor externe controle integreren met Azure Stack | Microsoft Docs
description: Meer informatie over het integreren van Azure Stack met een oplossing voor externe controle in uw datacenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: thoroet
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 419b7c436e79b00d8f49d9e3801886f4e37e2c32
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241641"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Oplossing voor externe controle integreren met Azure Stack

Voor externe bewaking van de Azure Stack-infrastructuur, moet u de Azure Stack-software, de fysieke computers en het fysieke netwerk-switches bewaken. Elk van deze gebieden biedt een methode voor het ophalen van gegevens over status en waarschuwing:

- Azure Stack-software biedt een REST-API om op te halen van de status en waarschuwingen. Het gebruik van software gedefinieerde technologieën zoals opslagruimten Direct, opslag-status en waarschuwingen maken deel uit van de bewaking van de software.
- Fysieke computers kunnen status en informatie over waarschuwingen beschikbaar maken via de baseboard management controllers (bmc's).
- Fysieke netwerkapparaten kunnen status en informatie over waarschuwingen beschikbaar maken via het SNMP-protocol.

Elke Azure Stack-oplossing wordt geleverd met een host van de levenscyclus van hardware. Controle van de hardwareleverancier van de Original Equipment Manufacturer (OEM)-software voor de fysieke servers en netwerkapparaten wordt uitgevoerd op deze host. Als gewenst is, kunt u deze oplossingen voor de controle omzeilen en rechtstreeks integreren met bestaande bewakingsoplossingen in uw datacenter.

> [!IMPORTANT]
> De externe bewakingsoplossing die u moet zonder agent. U kunt agents in Azure Stack-onderdelen van derden niet installeren.

Het volgende diagram toont de verkeersstroom tussen een geïntegreerde Azure Stack-systeem, de host van de levenscyclus van hardware, een oplossing voor externe controle en een externe ticketing/gegevens verzameling-systeem.

![Diagram met verkeer tussen Azure Stack, bewaking en ticketing oplossing.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

In dit artikel wordt uitgelegd hoe u Azure Stack integreren met externe bewakingsoplossingen, zoals System Center Operations Manager en Nagios. Dit omvat ook het werken met waarschuwingen programmatisch met behulp van PowerShell of via REST API-aanroepen.

## <a name="integrate-with-operations-manager"></a>Integreert met Operations Manager

U kunt Operations Manager gebruiken voor externe bewaking van Azure Stack. De System Center Management Pack voor Microsoft Azure Stack kunt u voor het bewaken van meerdere Azure Stack-implementaties met een enkele instantie van de Operations Manager. Het managementpack maakt gebruik van de Health-resourceprovider en Update-resourceprovider REST-API's om te communiceren met Azure Stack. Als u van plan bent om over te slaan van de OEM controlesoftware die wordt uitgevoerd op de host van de levenscyclus van hardware, kunt u de leverancier van management packs voor het controleren van fysieke servers installeren. U kunt ook de detectie van netwerkapparaten Operations Manager gebruiken voor het bewaken van netwerkswitches.

Het managementpack voor Azure Stack biedt de volgende mogelijkheden:

- U kunt meerdere Azure Stack-implementaties beheren
- Er is ondersteuning voor Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS)
- U kunt ophalen en waarschuwingen sluiten
- Er is een status en een dashboard capaciteit
- Inclusief onderhoudsmodus automatische detectie voor wanneer patch en update (P & U) wordt uitgevoerd
- Bevat taken voor het bijwerken van kracht voor implementatie en regio
- U kunt aangepaste gegevens toevoegen aan een regio
- Ondersteunt meldingen en rapportage

U kunt de System Center Management Pack downloaden voor Microsoft Azure Stack en de bijbehorende [gebruikershandleiding](https://www.microsoft.com/en-us/download/details.aspx?id=55184), of rechtstreeks vanuit de Operations Manager.

Voor een ticketsysteem oplossing, kunt u Operations Manager integreren met System Center Service Manager. De geïntegreerde productconnector kunt bidirectionele communicatie waarmee u een waarschuwing in Azure Stack en Operations Manager sluiten nadat u een serviceaanvraag in Service Manager hebt opgelost.

Het volgende diagram toont de integratie van Azure Stack met een bestaande implementatie van System Center. U kunt Service Manager verder met System Center Orchestrator of Service Management Automation (SMA) voor het uitvoeren van bewerkingen in Azure Stack kunt automatiseren.

![Diagram met de integratie met Operations Manager, Service Manager en SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integreren met Nagios

Een Nagios bewaking van de invoegtoepassing is ontwikkeld samen met de oplossingen van partners Cloudbase, die beschikbaar onder de strikte gratis software-licentie – MIT (Massachusetts Institute of Technology is).

De invoegtoepassing is geschreven in Python en maakt gebruik van de health-resourceprovider REST-API. Het biedt basisfuncties voor het ophalen en waarschuwingen in Azure Stack sluiten. Als het System Center management pack kunt u om toe te voegen van meerdere Azure Stack-implementaties en om meldingen te verzenden.

De invoegtoepassing kan worden gebruikt met Nagios Enterprise en Nagios Core. U kunt dit downloaden [hier](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). De downloadsite bevat ook de installatie en configuratie-informatie.

### <a name="plugin-parameters"></a>Parameters van de invoegtoepassing

De invoegtoepassingbestand "Azurestack_plugin.py" configureren met de volgende parameters:

| Parameter | Beschrijving | Voorbeeld |
|---------|---------|---------|
| *arm_endpoint* | Azure Resource Manager (beheerder)-eindpunt |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Resource Manager (beheerder)-eindpunt  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Beheerder abonnements-ID | Ophalen via de beheerdersportal of PowerShell |
| *User_name* | Operator abonnement gebruikersnaam | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | Operator abonnement wachtwoord | mijnwachtwoord |
| *Client_id* | Client | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *region* |  Azure Stack voor de naam van de regio | lokaal |
|  |  |

* De PowerShell-GUID die wordt geleverd is universal. U kunt deze gebruiken voor elke implementatie.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>PowerShell gebruiken om te status en waarschuwingen bewaken

Als u Operations Manager, Nagios of een oplossing op basis van Nagios niet gebruikt, kunt u PowerShell gebruiken om in te schakelen van een breed scala aan oplossingen voor integratie met Azure Stack voor de controle.

1. Zorg ervoor dat u voor het gebruik van PowerShell, [PowerShell is geïnstalleerd en geconfigureerd](azure-stack-powershell-configure-quickstart.md) voor een Azure Stack-operator-omgeving. PowerShell installeren op een lokale computer die de Resource Manager (beheerder)-eindpunt kan bereiken (https://adminmanagement. [ de regio]. [External_FQDN]).

2. Voer de volgende opdrachten om te verbinden met de Azure Stack-omgeving als Azure Stack-operators:

   ```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Gebruik de opdrachten, zoals de volgende voorbeelden om te werken met waarschuwingen:
   ```PowerShell
    #Retrieve all alerts
    Get-AzsAlert

    #Filter for active alerts
    $Active=Get-AzsAlert | Where {$_.State -eq "active"}
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    Get-AzsRPHealth

    #Retrieve infrastructure role instance health
    $FRPID=Get-AzsRPHealth|Where-Object {$_.DisplayName -eq "Capacity"}
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId

    ```

## <a name="learn-more"></a>Meer informatie

Zie voor meer informatie over het controleren van de gezondheid van ingebouwde [status en waarschuwingen in Azure Stack controleren](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Volgende stappen

[Beveiligingsintegratie van](azure-stack-integrate-security.md)
