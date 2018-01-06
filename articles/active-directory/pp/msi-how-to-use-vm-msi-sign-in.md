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
ms.date: 01/05/2018
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c5f71d27a9e07cc6d6a260b809e91aaa2a50270c
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Meld u aan met een virtuele machine door de gebruiker toegewezen beheerde Service identiteit (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Dit artikel vindt voorbeelden van de CLI-scripts voor aanmelden met een MSI-service-principal gebruiker toegewezen en richtlijnen over belangrijke onderwerpen, zoals foutafhandeling.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Als u wilt gebruiken in de Azure CLI-voorbeelden in dit artikel, moet u voor het installeren van de nieuwste versie van [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Alle voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client voor de opdrachtregel wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Verbinding'-functie in de Azure portal op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een VM beheerde Service identiteit (MSI) met Azure CLI](msi-qs-configure-cli-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, Portal, een sjabloon of een Azure-SDK). 
> - Om fouten te voorkomen tijdens het aanmelden en resource toegang, het MSI-bestand minimaal worden besteed 'Lezer' toegang tot op het juiste bereik (de virtuele machine of hoger) om toe te staan van Azure Resource Manager-bewerkingen op de virtuele machine. Zie [een beheerde Service identiteit (MSI) toegang toewijzen aan een resource met Azure CLI](msi-howto-assign-access-cli.md) voor meer informatie.

## <a name="overview"></a>Overzicht

Een MSI-bestand bevat een [service-principal](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), namelijk [gemaakt bij het inschakelen van MSI](msi-overview.md#how-does-it-work) op de virtuele machine. De service-principal kunt toegang krijgen tot Azure-resources en gebruikt als een identiteit door clients script/vanaf de opdrachtregel-line voor aanmelden en resource-toegang. Traditioneel voor toegang tot beveiligde bronnen met een eigen identiteit wordt moet een script-client:  

   - worden geregistreerd en ingestemd met Azure AD als een clienttoepassing vertrouwelijke of web
   - Meld u aan bij de service-principal met behulp van de app-referenties (die zijn waarschijnlijk ingesloten in het script)

Met MSI moet uw script-client niet meer doen, als deze onder de MSI-service-principal kunt aanmelden. 

## <a name="azure-cli"></a>Azure-CLI

Het volgende script toont hoe:

1. Aanmelden bij Azure AD onder de gebruiker toegewezen MSI-service-principal.  
2. Aanroepen van Azure Resource Manager en de locatie van de Azure-regio voor een virtuele machine ophalen. CLI zorgt voor het beheren van de token overname/gebruik automatisch voor u. Zorg ervoor dat uw virtuele machine van de naam `<VM NAME>`, en de gebruiker toegewezen MSI resource-id voor `<MSI ID>`. De MSI-bron-id wordt geretourneerd als de `id` eigenschap tijdens het maken van een gebruiker toegewezen MSI (Zie [configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine met Azure CLI](msi-qs-configure-cli-windows-vm.md) voor voorbeelden van de `az identity create` opdracht ).

    ```azurecli
    az login --msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Voorbeeld van antwoorden:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>Resource-id voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met bronnen die ondersteuning voor Azure AD en met MSI zijn getest en hun respectieve resource-id.

## <a name="error-handling-guidance"></a>Fout bij verwerking richtlijnen 

De antwoorden in de volgende kunnen erop wijzen dat het MSI-bestand niet correct geconfigureerd:

- CLI: *MSI: kan niet ophalen van een token van 'http://localhost:50342/oauth2/token' met een fout van ' HTTPConnectionPool (host = 'localhost', poort = 50342)* 

Als u een van deze fouten ontvangt, terug naar de Azure VM in de [Azure-portal](https://portal.azure.com) en:

- Ga naar de **configuratie** pagina en zorg ervoor dat "Beheerde service-identiteit" is ingesteld op 'Ja'.
- Ga naar de **extensies** pagina en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u wellicht opnieuw toewijzen van het MSI-bestand naar de resource of problemen met de implementatie is mislukt. Zie [configureren van een VM beheerde Service identiteit (MSI) met Azure CLI](msi-qs-configure-cli-windows-vm.md) als u hulp bij het VM-configuratie nodig.

## <a name="next-steps"></a>Volgende stappen

- Zie voor het inschakelen van MSI op een Azure VM [configureren van een VM beheerde Service identiteit (MSI) met Azure CLI](msi-qs-configure-cli-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.








