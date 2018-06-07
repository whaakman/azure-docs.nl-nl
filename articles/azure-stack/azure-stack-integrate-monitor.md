---
title: Oplossing voor externe controle integreren met Azure-Stack | Microsoft Docs
description: Informatie over het Azure-Stack integreren met een oplossing voor externe controle in uw datacenter.
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
ms.date: 05/10/2018
ms.author: jeffgilb
ms.reviewer: thoroet
ms.openlocfilehash: d7c8520602132722fd0c7138de4a276b9ac2208a
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807336"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Oplossing voor externe controle integreren met Azure-Stack

Voor externe controle van de Azure-Stack-infrastructuur, moet u de Azure-Stack-software, de fysieke computers en het fysieke netwerk-switches bewaken. Elk van deze gebieden biedt een methode voor het ophalen van de waarschuwing de status en informatie:

- Azure Stack-software biedt een op basis van REST-API om op te halen van de status en waarschuwingen. Het gebruik van door software gedefinieerde technologieën zoals opslagruimten Direct, opslag-status en waarschuwingen maken deel uit van de bewaking van de software.
- Fysieke computers kunnen status en informatie over waarschuwingen beschikbaar maken via de baseboard management controllers (BMC).
- Fysieke netwerkapparaten kunnen status en informatie over waarschuwingen beschikbaar maken via het SNMP-protocol.

Elke Azure-Stack-oplossing wordt geleverd met een hardware-lifecycle-host. Controle van de hardwareleverancier van de Original Equipment Manufacturer (OEM)-software voor de fysieke servers en netwerkapparaten wordt uitgevoerd op deze host. Indien gewenst, kunt u deze bewakingsoplossingen omzeilen en rechtstreeks integreren met bestaande bewakingsoplossingen in uw datacenter.

> [!IMPORTANT]
> De externe bewakingsoplossing die u gebruikt moet zonder agent. U kunt van derden agents binnen Azure Stack-componenten niet installeren.

Het volgende diagram toont de verkeersstroom tussen een Azure-Stack geïntegreerd systeem, de host van de levenscyclus van hardware, een oplossing voor externe controle en een externe tickets/gegevens verzameling systeem.

![Diagram van verkeer tussen Azure Stack, bewaking en tickets oplossing.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

In dit artikel wordt uitgelegd hoe Azure-Stack integreren met externe bewakingsoplossingen zoals System Center Operations Manager en Nagios. Dit omvat ook het werken met waarschuwingen programmatisch met behulp van PowerShell of via REST API-aanroepen.

## <a name="integrate-with-operations-manager"></a>Integreert met Operations Manager

U kunt de Operations Manager gebruiken voor externe controle van Azure-Stack. De System Center Management Pack voor Microsoft Azure-Stack kunt u voor het bewaken van meerdere Azure-Stack-implementaties met slechts één exemplaar van de Operations Manager. Het management pack maakt gebruik van de Health-resourceprovider en Update resourceprovider REST-API's om te communiceren met de Azure-Stack. Als u van plan bent voor het overslaan van de OEM bewaking van de software die wordt uitgevoerd op de host van de levenscyclus van hardware, kunt u de leverancier van management packs voor het bewaken van fysieke servers kunt installeren. U kunt ook de detectie van netwerkapparaten Operations Manager gebruiken voor het bewaken van netwerkswitches.

Het management pack voor Azure-Stack biedt de volgende mogelijkheden:

- U kunt meerdere Azure-Stack-implementaties beheren
- Er is ondersteuning voor Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS)
- U kunt ophalen en waarschuwingen sluiten
- Er is een status en een dashboard capaciteit
- Bevat de onderhoudsmodus automatische detectie voor wanneer patch en bij te werken (P & U) wordt uitgevoerd
- Omvat taken Update afdwingen voor implementatie en regio
- U kunt aangepaste gegevens toevoegen aan een regio
- Ondersteunt meldingen en rapportage

U kunt de System Center Management Pack voor Microsoft Azure-netwerkstack en de bijbehorende downloaden [gebruikershandleiding](https://www.microsoft.com/en-us/download/details.aspx?id=55184), of rechtstreeks uit Operations Manager.

Voor een ticketsysteem oplossing, kunt u Operations Manager integreren met System Center Service Manager. De geïntegreerde productconnector kunnen bidirectionele communicatie waarmee u een waarschuwing in Azure-Stack en Operations Manager sluiten nadat u een serviceaanvraag in Service Manager hebt opgelost.

Het volgende diagram toont de integratie van Azure-Stack met een bestaande implementatie van System Center. U kunt de Service Manager verder met System Center Orchestrator of Service Management Automation (SMA) voor het uitvoeren van bewerkingen in Azure-Stack automatiseren.

![Diagram van de integratie met OM-, Service-beheer- en SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integreren met Nagios

Een invoegtoepassing bewaking Nagios is ontwikkeld samen met de Cloudbase partneroplossingen die beschikbaar onder de strikte gratis software-licentie – MIT (Massachusetts Institute of Technology is).

De invoegtoepassing is geschreven in Python en maakt gebruik van de resourceprovider health REST-API. Biedt basisfuncties voor het ophalen en waarschuwingen in de Azure-Stack sluiten. Als het System Center management pack kunt u meerdere Azure-Stack implementaties toevoegen en om meldingen te verzenden.

De invoegtoepassing werkt met Nagios Enterprise en Nagios Core. U kunt dit downloaden [hier](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). De downloadsite bevat ook de installatie en configuratie-informatie.

### <a name="plugin-parameters"></a>Parameters van de invoegtoepassing

Configureer de invoegtoepassingbestand 'Azurestack_plugin.py' met de volgende parameters:

| Parameter | Beschrijving | Voorbeeld |
|---------|---------|---------|
| *arm_endpoint* | Azure Resource Manager (beheerder)-eindpunt |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Resource Manager (beheerder)-eindpunt  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Beheerder abonnements-ID | Via de beheerdersportal of PowerShell ophalen |
| *Gebruikersnaam* | Operator abonnement gebruikersnaam | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | Operator abonnement wachtwoord | mijnwachtwoord |
| *Client_id* | Client | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *Regio* |  Naam van de regio Azure Stack | lokaal |
|  |  |

* De PowerShell-GUID die opgegeven is universal. U kunt deze gebruiken voor elke implementatie.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>PowerShell gebruiken voor de monitor-status en waarschuwingen

Als u Operations Manager, Nagios of een oplossing op basis van Nagios niet gebruikt, kunt u PowerShell gebruiken om in te schakelen van een breed scala aan bewakingsoplossingen integreren met Azure-Stack.

1. Zorg ervoor dat u hebt voor het gebruik van PowerShell [PowerShell is geïnstalleerd en geconfigureerd](azure-stack-powershell-configure-quickstart.md) voor een Azure-Stack operator-omgeving. PowerShell installeren op een lokale computer die toegang heeft tot de Resource Manager (beheerder)-eindpunt (https://adminmanagement. [ de regio]. [External_FQDN]).

2. Voer de volgende opdrachten verbinding maken met de Azure-Stack-omgeving als een Azure-Stack-operator:

   ```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Gebruik de opdrachten zoals de volgende voorbeelden om te werken met waarschuwingen:
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

Zie voor meer informatie over ingebouwde statuscontrole [status en waarschuwingen in de Azure-Stack controleren](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Volgende stappen

[Beveiligingsintegratie](azure-stack-integrate-security.md)
