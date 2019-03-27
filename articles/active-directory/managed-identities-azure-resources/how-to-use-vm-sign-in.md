---
title: Over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM voor aanmelden
description: Stap voor stap beheerd instructies en voorbeelden voor het gebruik van een Azure-VM identiteiten voor service-principal voor Azure-resources voor het script client aanmelden en toegang tot bronnen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43aa0859fa67cc6b2f5c5974f072e7b6d4b29527
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58442133"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-for-sign-in"></a>Over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM voor aanmelden 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Dit artikel bevat voorbeelden van PowerShell en CLI-script voor aanmelding bij gebruik van beheerde identiteiten voor service-principal voor Azure-resources en richtlijnen over belangrijke onderwerpen, zoals foutafhandeling.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u van plan bent de Azure PowerShell of Azure CLI-voorbeelden in dit artikel gebruiken, moet u Installeer de nieuwste versie van [Azure PowerShell](/powershell/azure/install-az-ps) of [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Alle voorbeeld van een script in dit artikel wordt ervan uitgegaan dat de client voor de opdrachtregel wordt uitgevoerd op een VM met beheerde identiteiten voor Azure-resources ingeschakeld. Gebruik de virtuele machine 'Connect'-functie in Azure portal, op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van beheerde identiteiten voor Azure-resources op een virtuele machine [configureren beheerde identiteiten voor een Azure-resources op een virtuele machine met behulp van de Azure-portal](qs-configure-portal-windows-vm.md), of een van de variant (met behulp van PowerShell, CLI, een sjabloon of een Azure-artikelen SDK). 
> - Om fouten te voorkomen tijdens toegang tot bronnen, beheerde identiteit van de virtuele machine ten minste moet worden opgegeven op het juiste bereik 'Lezer' toegang tot (de virtuele machine of hoger) om toe te staan van Azure Resource Manager-bewerkingen op de virtuele machine. Zie [toewijzen beheerde identiteiten voor een Azure-resources toegang tot een resource met de Azure-portal](howto-assign-access-portal.md) voor meer informatie.

## <a name="overview"></a>Overzicht

Beheerde identiteiten voor Azure-resources biedt een [service-principal-object](../develop/developer-glossary.md#service-principal-object) , die is [bij het inschakelen van beheerde identiteiten voor Azure-resources gemaakt](overview.md#how-does-it-work) op de virtuele machine. De service-principal kan toegang krijgen tot Azure-resources en gebruikt als een identiteit door script/vanaf de opdrachtregel-line-clients voor aanmelden en toegang tot bronnen. Traditioneel voor toegang tot beveiligde resources onder een eigen identiteit wordt moet een script-client:  

   - worden geregistreerd en heeft ingestemd met Azure AD als een vertrouwelijk/web-clienttoepassing
   - Meld u aan bij de service-principal met behulp van de app-referenties (dit zijn waarschijnlijk ingesloten in het script)

Met beheerde identiteiten voor Azure-resources, uw script client moet niet meer doen, omdat deze kan zich aanmelden bij de beheerde identiteiten voor service-principal voor Azure-resources. 

## <a name="azure-cli"></a>Azure-CLI

Het volgende script laat zien hoe u:

1. Aanmelden bij Azure AD onder beheerde identiteit van de virtuele machine voor service-principal voor Azure-resources  
2. Aanroepen van Azure Resource Manager en ophalen van de virtuele machine service principal-ID. CLI zorgt dat token ophalen en gebruiken van automatisch voor u beheren. Zorg ervoor dat u vervangen door de naam van uw virtuele machine voor `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The managed identity for Azure resources service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Het volgende script laat zien hoe u:

1. Aanmelden bij Azure AD onder beheerde identiteit van de virtuele machine voor service-principal voor Azure-resources  
2. Aanroepen van een Azure Resource Manager-cmdlet voor informatie over de virtuele machine. PowerShell zorgt automatisch beheren van token gebruikt voor u.  

   ```azurepowershell
   Add-AzAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's managed identity for Azure resources. 
   $vmInfoPs = Get-AzVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The managed identity for Azure resources service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Resource-id's voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD-verificatie](services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) voor een lijst met resources die zijn getest met beheerde identiteiten voor een Azure-resources en hun respectieve resource-id's en Azure AD ondersteunen.

## <a name="error-handling-guidance"></a>Richtlijnen voor het verwerken van fout 

Antwoorden, zoals het volgende kunnen duiden dat de beheerde identiteit van de virtuele machine voor Azure-resources niet correct geconfigureerd:

- PowerShell: *Invoke-WebRequest: Kan geen verbinding maken met de externe server*
- CLI: *MSI: Kan niet ophalen van een token van `http://localhost:50342/oauth2/token` met een fout van ' HTTPConnectionPool (host = 'localhost', poort = 50342)* 

Als u een van deze fouten ontvangt, terug naar de Azure-VM in de [Azure-portal](https://portal.azure.com) en:

- Ga naar de **identiteit** pagina en zorg ervoor dat **systeem toegewezen** is ingesteld op "Ja".
- Ga naar de **extensies** pagina en zorg ervoor dat de beheerde identiteit voor Azure-resources extensie **(gepland voor de afschaffing in januari 2019)** is geïmplementeerd.

Als een onjuist is, moet u mogelijk opnieuw implementeren van de beheerde identiteit voor Azure-resources voor uw resource of problemen met de implementatie mislukt. Zie [configureren van beheerde identiteiten voor een Azure-resources op een virtuele machine met behulp van de Azure-portal](qs-configure-portal-windows-vm.md) als u hulp nodig met VM-configuratie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor het inschakelen van beheerde identiteiten voor Azure-resources op een Azure VM [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell](qs-configure-powershell-windows-vm.md), of [configureren beheerde identiteiten voor een Azure-resources op een Azure VM met behulp van Azure CLI](qs-configure-cli-windows-vm.md)






