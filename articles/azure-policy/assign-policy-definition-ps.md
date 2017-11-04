---
title: PowerShell gebruiken voor het maken van een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken voor het maken van een Azure-beleidstoewijzing om te identificeren van niet-compatibele bronnen.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 02afe946e5e1ad9730ab07df19676e90485ecf98
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving met behulp van PowerShell maken

De eerste stap bij de naleving van inzicht in Azure is weten waar u met uw eigen huidige resources staan. Deze snelstartgids begeleidt u door het proces van het maken van een beleidstoewijzing om te identificeren van virtuele machines die geen gebruik van beheerde schijven maakt.

Aan het einde van dit proces voor u wordt hebt is geïdentificeerd welke virtuele machines niet beheerde schijven gebruikt en zijn daarom *niet-compatibele*.


PowerShell wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding gegevens met behulp van PowerShell om de beleidstoewijzing van een om te identificeren van niet-compatibele bronnen in uw Azure-omgeving.

Deze handleiding is de Azure PowerShell-moduleversie 4.0 of hoger vereist. Voer ```Get-Module -ListAvailable AzureRM``` de versie te vinden. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Voor u begint, moet u ervoor zorgen dat de nieuwste versie van PowerShell is geïnstalleerd. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="opt-in-to-azure-policy"></a>U meldt zich aan Azure-beleid

Azure-beleid is nu beschikbaar in Public Preview en u wilt registreren bij aanvragen voor toegang.

1. Ga naar de Azure-beleid op https://aka.ms/getpolicy en selecteer **aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. U meldt zich aan Azure-beleid door het selecteren van de abonnementen in de **abonnement** u werken wilt met lijst. Selecteer vervolgens **registreren**.

   ![U meldt zich aan het beleid van Azure gebruiken](media/assign-policy-definition/preview-opt-in.png)

   Uw aanvraag is automatisch goedgekeurd voor de Preview. Wacht tot 30 minuten voor het systeem voor het verwerken van uw registratie.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze snelstartgids we een beleidstoewijzing maken en toewijzen de *Audit virtuele Machines zonder schijven beheerd* definitie. De beleidsdefinitie voor dit wordt bepaald welke bronnen die niet aan de voorwaarden in de beleidsdefinitie van het voldoen.

Volg deze stappen voor het maken van een nieuwe beleidstoewijzing.

Voer de volgende opdracht alle beleidsdefinities weergeven en vindt dat u wilt toewijzen:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure beleid wordt geleverd met al ingebouwde beleidsdefinities die u kunt gebruiken. Ziet u de beleidsdefinities van de ingebouwde, zoals:

- Afdwingen van code en de bijbehorende waarde
- Label en de waarde ervan toepassen
- SQL Server-versie 12.0 vereisen

Vervolgens kunt toewijzen aan de beleidsdefinitie aan het gewenste bereik met behulp van de `New-AzureRmPolicyAssignment` cmdlet.

Voor deze zelfstudie bieden we de volgende informatie voor de opdracht:
- Weergave **naam** voor de beleidstoewijzing. In dit geval gebruiken we Audit virtuele Machines zonder schijven beheerd.
- **Beleid** – dit is de beleidsdefinitie, op basis van uit dat u de toewijzing te maken. In dit geval is de beleidsdefinitie – *Audit virtuele Machines zonder schijven beheerd*
- Een **bereik** : een bereik bepaalt welke resources of groeperen van resources de toewijzing van beleid wordt afgedwongen op. Dit kan variëren van een abonnement aan resourcegroepen. In dit voorbeeld wordt de beleidsdefinitie van het wilt toewijst de **FabrikamOMS** resourcegroep.
- **$definition** – moet u de bron-ID van de beleidsdefinitie – In dit geval we gebruiken de ID voor de beleidsdefinitie - *Audit virtuele Machines zonder schijven beheerd*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

U bent nu klaar om u te identificeren van niet-compatibele bronnen voor informatie over de compatibiliteitsstatus van uw omgeving.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele bronnen identificeren

1. Ga terug naar de startpagina van Azure-beleid.
2. Selecteer **naleving** in het linkerdeelvenster en zoek naar de **beleidstoewijzing** u hebt gemaakt.

   ![Naleving van beleid](media/assign-policy-definition/policy-compliance.png)

   Als er bestaande resources die niet compatibel met deze nieuwe toewijzing, ze wordt weergegeven in de **niet-compatibel resources** tabblad, zoals hierboven.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling bouwen voort op deze snelstartgids. Als u van plan bent om door te gaan werken met de volgende zelfstudies, geen clean up maakt van de resources in deze snelstartgids hebt gemaakt. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt met deze opdracht uit te voeren:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, moet u de beleidsdefinitie van een voor het identificeren van niet-compatibele bronnen in uw Azure-omgeving toegewezen.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat **toekomstige** resources die worden gemaakt die compatibel zijn, blijven de zelfstudie voor:

> [!div class="nextstepaction"]
> [Maken en beheren van beleid](./create-manage-policy.md)
