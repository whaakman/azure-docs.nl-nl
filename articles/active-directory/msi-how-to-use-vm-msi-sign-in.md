---
title: Het gebruik van een Azure VM beheerde Service-identiteit voor aanmelden
description: Stapsgewijze openen instructies en voorbeelden voor het gebruik van een Azure VM MSI-service-principal voor script-client zich aanmelden en resource.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 2e4da8cd02a1d07a3225a0c1fda4c60928dba8a4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Het gebruik van een Azure VM beheerde Service identiteit (MSI) voor aanmelding 

[!INCLUDE [preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Dit artikel vindt voorbeelden van PowerShell en CLI scripts voor aanmelden met een MSI-service-principal en richtlijnen over belangrijke onderwerpen, zoals foutafhandeling.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u gebruiken in de Azure PowerShell of Azure CLI-voorbeelden in dit artikel wilt, moet u Installeer de nieuwste versie van [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) of [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Alle voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client voor de opdrachtregel wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Verbinding'-functie in de Azure portal op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 
> - Om fouten te voorkomen tijdens toegang tot bedrijfsbronnen, MSI van de virtuele machine ten minste worden besteed 'Lezer' toegang tot op het juiste bereik (de virtuele machine of hoger) om toe te staan van Azure Resource Manager-bewerkingen op de virtuele machine. Zie [een beheerde Service identiteit (MSI) toegang toewijzen aan een resource met de Azure portal](msi-howto-assign-access-portal.md) voor meer informatie.

## <a name="overview"></a>Overzicht

Een MSI-bestand bevat een [service-principal](develop/active-directory-dev-glossary.md#service-principal-object), namelijk [gemaakt bij het inschakelen van MSI](msi-overview.md#how-does-it-work) op de virtuele machine. De service-principal kunt toegang krijgen tot Azure-resources en gebruikt als een identiteit door script/vanaf de opdrachtregel-line-clients voor aanmelden en toegang tot bedrijfsbronnen. Traditioneel voor toegang tot beveiligde bronnen met een eigen identiteit wordt moet een script-client:  

   - worden geregistreerd en ingestemd met Azure AD als een clienttoepassing vertrouwelijke of web
   - Meld u aan bij de service-principal met behulp van de app-referenties (die zijn waarschijnlijk ingesloten in het script)

Met MSI moet uw script-client niet meer doen, als deze onder de MSI-service-principal kunt aanmelden. 

## <a name="azure-cli"></a>Azure-CLI

Het volgende script toont hoe:

1. aanmelden bij Azure AD onder de VM MSI service-principal  
2. Aanroepen van Azure Resource Manager en ophalen van de VM service principal-ID. CLI zorgt voor het beheren van de token overname/gebruik automatisch voor u. Zorg ervoor dat uw virtuele machine van de naam `<VM-NAME>`.  

   ```azurecli
   az login --msi
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Het volgende script toont hoe:

1. Verkrijgen van een MSI-toegangstoken voor de virtuele machine.  
2. Gebruik het toegangstoken aan te melden bij Azure AD, onder de bijbehorende service-principal van MSI.   
3. Aanroepen van een cmdlet voor Azure Resource Manager voor informatie over de virtuele machine. PowerShell zorgt voor het beheren van token gebruikt voor u automatisch.  

   ```azurepowershell
   # Get an access token for the MSI
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                                 -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
   $content =$response.Content | ConvertFrom-Json
   $access_token = $content.access_token
   echo "The MSI access token is $access_token"

   # Use the access token to sign in under the MSI service principal. -AccountID can be any string to identify the session.
   Login-AzureRmAccount -AccessToken $access_token -AccountId "MSI@50342"

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Resource-id voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met bronnen die ondersteuning voor Azure AD en met MSI zijn getest en hun respectieve resource-id.

## <a name="error-handling-guidance"></a>Fout bij verwerking richtlijnen 

Antwoorden zoals de volgende kunnen erop wijzen dat de VM-MSI niet correct geconfigureerd:

- PowerShell: *Invoke-WebRequest: kan geen verbinding maken met de externe server*
- CLI: *MSI: kan niet ophalen van een token van 'http://localhost:50342/oauth2/token' met een fout van ' HTTPConnectionPool (host = 'localhost', poort = 50342)* 

Als u een van deze fouten ontvangt, terug naar de Azure VM in de [Azure-portal](https://portal.azure.com) en:

- Ga naar de **configuratie** pagina en zorg ervoor dat "Beheerde service-identiteit" is ingesteld op 'Ja'.
- Ga naar de **extensies** pagina en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u wellicht de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt. Zie [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md) als u hulp bij het VM-configuratie nodig.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor het inschakelen van MSI op een Azure VM [configureren van een VM beheerde Service identiteit (MSI) met behulp van PowerShell](msi-qs-configure-powershell-windows-vm.md), of [configureren van een VM beheerde Service identiteit (MSI) met Azure CLI](msi-qs-configure-cli-windows-vm.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.








