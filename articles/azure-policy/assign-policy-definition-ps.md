---
title: PowerShell gebruiken om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs
description: PowerShell gebruiken om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 5c00d50817e40de0a43d05eb85662b494247d8fa
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving met PowerShell

Om naleving in Azure te begrijpen, moet u eerst weten hoe het er voorstaat met uw eigen huidige resources. In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u het proces helemaal hebt doorlopen, hebt u virtuele machines geïdentificeerd die geen beheerde schijven gebruiken, en daarom *niet compatibel* zijn.


PowerShell wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding biedt informatie over het gebruik van PowerShell om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving.

Voor deze handleiding is moduleversie 4.0 of hoger van Azure PowerShell vereist. Voer  ```Get-Module -ListAvailable AzureRM```  uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Voor u begint, moet u ervoor zorgen dat de nieuwste versie van PowerShell is geïnstalleerd. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="opt-in-to-azure-policy"></a>Registreren voor Azure Policy

Azure Policy is nu beschikbaar in openbare preview en u moet zich registreren om toegang aan te vragen.

1. Ga naar Azure Policy op https://aka.ms/getpolicy en selecteer **Aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. Registreer u voor Azure Policy door in de lijst **Abonnement** de abonnementen te selecteren waarmee u wilt werken. Selecteer vervolgens **Registreren**.

   ![Registreren voor het gebruik van Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   Uw aanvraag wordt automatisch goedgekeurd voor preview. Het duurt maximaal 30 minuten voordat uw registratie door het systeem is verwerkt.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maken we een beleidstoewijzing en wijzen we de definitie *Virtuele machines zonder beheerde schijven controleren* toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Volg deze stappen als u een nieuwe beleidstoewijzing wilt maken.

Voer de volgende opdracht uit om alle beleidsdefinities weer te geven en de definitie te vinden die u wilt toewijzen:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure Policy wordt geleverd met ingebouwde beleidsdefinities die u kunt gebruiken. U ziet ingebouwde beleidsdefinities, zoals:

- Tag en waarde afdwingen
- Tag en waarde toepassen
- SQL Server-versie 12.0 vereisen

Vervolgens kunt u de beleidsdefinitie toewijzen aan het gewenste bereik met behulp van de `New-AzureRmPolicyAssignment`-cmdlet.

Voor deze zelfstudie bieden we de volgende informatie voor de opdracht:
- **Weergavenaam** voor de beleidstoewijzing. In dit geval gebruiken we Virtuele machines zonder beheerde schijven controleren.
- **Beleid**: dit is de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*
- Het **bereik**: het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen. In dit voorbeeld wordt de beleidsdefinitie toegewezen aan de resourcegroep **FabrikamOMS**.
- **$definition**: u moet de resource-ID van de beleidsdefinitie opgeven. In dit geval gebruiken we de ID voor de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*.

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

   Als er bestaande resources zijn die niet compatibel zijn met deze nieuwe toewijzing, worden deze weergegeven op het tabblad **Niet-compatibele resources**, zoals u hierboven ziet.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u door wilt gaan met andere zelfstudies, verwijdert u de resources die u in deze quickstart hebt gemaakt, niet. Als u niet van plan bent om door te gaan, verwijdert u de toewijzing die u hebt gemaakt door de volgende opdracht uit te voeren:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid om ervoor te zorgen dat **toekomstige** resources die worden gemaakt compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
