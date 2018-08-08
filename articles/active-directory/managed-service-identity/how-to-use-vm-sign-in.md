---
title: Het gebruik van een Azure VM Managed Service Identity voor aanmelden
description: Stapsgewijze openen instructies en voorbeelden om met behulp van een service-principal voor Azure VM-MSI voor script client zich aanmelden en resources te.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 205938bbf615face0768028717a333c13c1fafa1
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590310"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Het gebruik van een Azure VM Managed Service Identity (MSI) voor aanmelding 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Dit artikel vindt PowerShell en CLI-scriptvoorbeelden voor aanmelden met een MSI-service-principal en richtlijnen over belangrijke onderwerpen, zoals foutafhandeling.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u van plan bent de Azure PowerShell of Azure CLI-voorbeelden in dit artikel gebruiken, moet u Installeer de nieuwste versie van [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) of [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Alle voorbeeld van een script in dit artikel wordt ervan uitgegaan dat de client voor de opdrachtregel wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Connect'-functie in Azure portal, op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI-bestand op een virtuele machine [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 
> - Om fouten te voorkomen tijdens toegang tot bronnen, van de VM-MSI-bestand minimaal moet worden opgegeven op het juiste bereik 'Lezer' toegang tot (de virtuele machine of hoger) om toe te staan van Azure Resource Manager-bewerkingen op de virtuele machine. Zie [een Managed Service Identity (MSI) toegang toewijzen aan een resource met de Azure-portal](howto-assign-access-portal.md) voor meer informatie.

## <a name="overview"></a>Overzicht

Een MSI-bestand bevat een [service-principal-object](../develop/developer-glossary.md#service-principal-object) , die is [gemaakt bij het inschakelen van MSI](overview.md#how-does-it-work) op de virtuele machine. De service-principal kan toegang krijgen tot Azure-resources en gebruikt als een identiteit door script/vanaf de opdrachtregel-line-clients voor aanmelden en toegang tot bronnen. Traditioneel voor toegang tot beveiligde resources onder een eigen identiteit wordt moet een script-client:  

   - worden geregistreerd en heeft ingestemd met Azure AD als een vertrouwelijk/web-clienttoepassing
   - Meld u aan bij de service-principal met behulp van de app-referenties (dit zijn waarschijnlijk ingesloten in het script)

Met MSI-bestand moet de script-client niet meer doen, omdat deze kan zich aanmelden bij de MSI-service-principal. 

## <a name="azure-cli"></a>Azure-CLI

Het volgende script laat zien hoe u:

1. Aanmelden bij Azure AD onder de VM MSI-service-principal  
2. Aanroepen van Azure Resource Manager en ophalen van de virtuele machine service principal-ID. CLI zorgt dat token ophalen en gebruiken van automatisch voor u beheren. Zorg ervoor dat u vervangen door de naam van uw virtuele machine voor `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Het volgende script laat zien hoe u:

1. Aanmelden bij Azure AD onder de VM MSI-service-principal  
2. Aanroepen van een Azure Resource Manager-cmdlet voor informatie over de virtuele machine. PowerShell zorgt automatisch beheren van token gebruikt voor u.  

   ```azurepowershell
   Add-AzureRmAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Resource-id's voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD-verificatie](services-support-msi.md#azure-services-that-support-azure-ad-authentication) voor een lijst van resources met Azure AD ondersteunen en die zijn getest met MSI-bestand en hun respectieve resource-id's.

## <a name="error-handling-guidance"></a>Richtlijnen voor het verwerken van fout 

Antwoorden, zoals het volgende kunnen duiden dat de VM MSI niet correct geconfigureerd:

- PowerShell: *Invoke-WebRequest: kan geen verbinding maken met de externe server*
- CLI: *MSI: kan niet ophalen van een token van 'http://localhost:50342/oauth2/token' met een fout van ' HTTPConnectionPool (host = 'localhost', poort = 50342)* 

Als u een van deze fouten ontvangt, terug naar de Azure-VM in de [Azure-portal](https://portal.azure.com) en:

- Ga naar de **configuratie** pagina en zorg ervoor dat 'Beheerde service-identiteit' is ingesteld op "Ja".
- Ga naar de **extensies** pagina en zorg ervoor dat de MSI-extensie is geïmplementeerd.

Als een onjuist is, moet u mogelijk het MSI-bestand voor uw resource opnieuw implementeren of problemen met de implementatie mislukt. Zie [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md) als u hulp nodig met VM-configuratie.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor het inschakelen van MSI-bestand op een Azure VM [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van PowerShell](qs-configure-powershell-windows-vm.md), of [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van Azure CLI](qs-configure-cli-windows-vm.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.








