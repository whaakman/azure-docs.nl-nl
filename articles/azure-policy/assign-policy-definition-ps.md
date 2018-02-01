---
title: PowerShell gebruiken om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 1/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 67c779b96dab088d810d22ad3053ade106aec56a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving met PowerShell

De eerste stap in het begrijpen van naleving in Azure is het identificeren van de status van uw resources. In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u dit proces helemaal hebt doorlopen, kunt u virtuele machines identificeren die geen beheerde schijven gebruiken. Deze zijn *niet-compatibel* met de beleidstoewijzing.

PowerShell wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding biedt informatie over het gebruik van PowerShell om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving.

Voor deze handleiding is moduleversie 4.0 of hoger van Azure PowerShell vereist. Voer  `Get-Module -ListAvailable AzureRM`  uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Voor u begint, moet u ervoor zorgen dat de nieuwste versie van PowerShell is geïnstalleerd. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor gedetailleerde informatie.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.


## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maakt u een beleidstoewijzing en wijzen we de definitie *Virtuele machines zonder beheerde schijven controleren* toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Volg deze stappen als u een nieuwe beleidstoewijzing wilt maken.

1. Registreer de Policy Insights-resourceprovider, zodat u zeker weet dat uw abonnement hiervoor geschikt is. Als u een resourceprovider wilt registreren, moet u toestemming hebben de registreeractie voor de resourceprovider kunnen uitvoeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

    Voer de volgende opdracht uit om de resourceprovider te registreren:

    ```
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
```

    U kunt de registratie van een resourceprovider niet ongedaan maken terwijl er resourcetypen van de resourceprovider in uw abonnement aanwezig zijn.

    Zie [Resourceproviders en -typen](../azure-resource-manager/resource-manager-supported-services.md) voor meer details over het registreren en het weergeven van resourceproviders.

2. Als u de resourceprovider hebt geregistreerd, voert u de volgende opdracht uit om alle beleidsdefinities weer te geven en degene te vinden die u wilt toewijzen:

    ```powershell
$definition = Get-AzureRmPolicyDefinition
```

    Azure Policy wordt geleverd met ingebouwde beleidsdefinities die u kunt gebruiken. U ziet ingebouwde beleidsdefinities, zoals:

    - Tag en waarde afdwingen
    - Tag en waarde toepassen
    - SQL Server-versie 12.0 vereisen

3. Vervolgens kunt u de beleidsdefinitie toewijzen aan het gewenste bereik met behulp van de `New-AzureRmPolicyAssignment`-cmdlet.

Voor deze zelfstudie gebruikt u de volgende informatie voor de opdracht:

- **Weergavenaam** voor de beleidstoewijzing. In dit geval gebruiken we Virtuele machines zonder beheerde schijven controleren.
- **Beleid**: de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*
- Het **bereik**: het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen. In dit voorbeeld wijst u de beleidsdefinitie toe aan de resourcegroep **FabrikamOMS**.
- **$definition**: u moet de resource-id van de beleidsdefinitie opgeven. In dit geval gebruikt u de id voor de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

U bent er nu klaar voor om niet-compatibele resources te identificeren om inzicht te krijgen in de nalevingsstatus van uw omgeving.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

1. Ga terug naar de startpagina van Azure Policy.
2. Selecteer **Naleving** in het linkerdeelvenster en zoek de **beleidstoewijzing** die u hebt gemaakt.

   ![Beleidsnaleving](media/assign-policy-definition/policy-compliance.png)

   Als er bestaande resources zijn die niet compatibel zijn met deze nieuwe toewijzing, worden deze weergegeven onder het tabblad **Niet-compatibele resources**.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u door wilt gaan met andere zelfstudies, verwijdert u de resources die u in deze quickstart hebt gemaakt, niet. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt door de volgende opdracht uit te voeren:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat **toekomstige** resources die worden gemaakt compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
