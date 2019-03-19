---
title: PowerShell gebruiken voor het vrijgeven van Azure Security Center en uw netwerk beveiligen | Microsoft Docs
description: Dit document begeleidt u bij het proces van onboarding van Azure Security Center met behulp van PowerShell-cmdlets.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 1b4cf6b5a27d985ee3d250a7d25a4a267a6810ba
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58133506"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Onboarding van Azure Security Center met behulp van PowerShell automatiseren

U kunt uw Azure-workloads via een programma, beveiligen met behulp van de Azure Security Center PowerShell-module.
Met behulp van PowerShell, kunt u taken automatiseren en te voorkomen dat de intrinsieke handmatige taken menselijke fouten. Dit is vooral nuttig in grootschalige implementaties die betrekking hebben op tientallen abonnementen met honderden en duizenden resources: die allemaal moeten worden beveiligd vanaf het begin.

Onboarding van Azure Security Center met behulp van PowerShell kunt u programmatisch onboarding en het beheer van uw Azure-resources automatiseren en de nodige maatregelen toevoegen.

Dit artikel bevat een voorbeeld van PowerShell-script dat kan worden gewijzigd en gebruikt in uw omgeving voor implementatie Security Center voor uw abonnementen. 

In dit voorbeeld zullen we Security Center inschakelen voor een abonnement met ID: d07c0080-170c-4c24-861d-9c817742786c en de aanbevolen instellingen waarmee u een hoog niveau van beveiliging, door het implementeren van de prijscategorie Standard van Security Center, waarmee u toepassen Geavanceerde threat protection en detectie van mogelijkheden:

1. Stel de [ASC standaard beveiligingsniveau](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Stel de Log Analytics-werkruimte die de Microsoft Monitoring Agent wordt verzonden met de gegevens die worden verzameld op de virtuele machines die zijn gekoppeld aan het abonnement: in dit voorbeeld wordt een bestaande werkruimte van de gebruiker gedefinieerde (myWorkspace).

3. Automatische van Security Center-agent die wordt ingericht activeren [implementeert u de Microsoft Monitoring Agent](security-center-enable-data-collection.md#auto-provision-mma).

5. Instellen van de organisatie [CISO als de contactpersoon voor beveiliging voor de ASC-waarschuwingen en gebeurtenissen die aandacht vereisen](security-center-provide-security-contact-details.md).

6. Toewijzen van Security Center bevinden [standaard beveiligingsbeleid](tutorial-security-policy.md).

## <a name="prerequisites"></a>Vereisten

Deze stappen moeten worden uitgevoerd voordat u de Security Center-cmdlets uitvoert:

1.  Voer PowerShell uit als beheerder.
2.  Voer de volgende opdrachten uit in PowerShell:
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>Onboarding Security Center met behulp van PowerShell

1.  Registreer uw abonnementen bij de Resourceprovider van Security Center:

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  Optioneel: Stel het dekkingsniveau (prijscategorie) van de abonnementen (als niet is gedefinieerd, de prijscategorie is ingesteld op gratis):

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Configureer een Log Analytics-werkruimte waarop de agents rapporteren. U moet een Log Analytics-werkruimte die u al hebt gemaakt, dat virtuele machines van het abonnement rapporteren aan hebben. U kunt meerdere abonnementen om te rapporteren aan dezelfde werkruimte definiëren. Als niet is gedefinieerd, kunt u de standaardwerkruimte wordt gebruikt.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  De installatie van het automatisch inrichten van de Microsoft Monitoring Agent op uw Azure Virtual machines:
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Het verdient aanbeveling om in te schakelen automatische inrichting om ervoor te zorgen dat uw Azure virtual machines automatisch worden beveiligd door Azure Security Center.
    >

5.  Optioneel: Het is raadzaam dat u definieert de contactgegevens voor beveiliging voor de abonnementen die u met het voorbereiden, die wordt gebruikt als de ontvangers van waarschuwingen en meldingen die worden gegenereerd door Security Center:

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  De standaard Security Center-beleid initiatief toewijzen:

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

U nu is toegevoegd Azure Security Center met PowerShell.

U kunt nu deze PowerShell-cmdlets met automatiseringsscripts gebruiken om via een programma voor abonnementen en resources. Dit bespaart tijd en vermindert de kans op menselijke fouten. U kunt deze [voorbeeldscript](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) als verwijzing.






## <a name="see-also"></a>Zie ook
Zie het volgende artikel voor meer informatie over hoe u PowerShell gebruiken kunt voor het automatiseren van onboarding naar Security Center:

* [Az.Security](https://github.com/Azure/azure-powershell/blob/preview/src/ResourceManager/Security/Commands.Security/help/AzureRM.Security.md).

Zie het volgende artikel voor meer informatie over Security Center:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
