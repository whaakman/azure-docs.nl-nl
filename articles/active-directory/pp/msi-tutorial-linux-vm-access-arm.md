---
title: Gebruik van een MSI Linux VM gebruiker toegewezen voor toegang tot Azure Resource Manager
description: Een zelfstudie die u bij het proces helpt van het gebruik van een User-Assigned beheerde Service identiteit (MSI) op een Linux-VM voor toegang tot Azure Resource Manager.
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: arluca
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: bebdccb616a4677fdf36ac257ac36f1827958af7
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-resource-manager"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) op een Linux-VM te gebruiken voor toegang tot Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie wordt uitgelegd hoe u een gebruiker toegewezen beheerde Service identiteit (MSI) maken, wijst u deze toe aan een Linux-virtuele Machine (VM) en die identiteit vervolgens gebruiken voor toegang tot de Azure Resource Manager-API. 

Beheerde Service-identiteiten worden automatisch beheerd door Azure. Hiermee kunt verificatie voor services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties insluiten in uw code.

Procedures voor:

> [!div class="checklist"]
> * Maken van een MSI gebruiker toegewezen
> * Het MSI-bestand toewijzen aan een virtuele Linux-machine 
> * Het MSI-toegang verlenen aan een resourcegroep in Azure Resource Manager 
> * Ophalen van een toegangstoken met behulp van het MSI-bestand en het aanroepen van Azure Resource Manager 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de knop 'Deze probeer' zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-Machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maakt u eerst een nieuwe Linux VM. U kunt ook kiezen voor het gebruik van een bestaande virtuele machine.

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![Virtuele Linux-machine maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="create-a-user-assigned-msi"></a>Maken van een MSI gebruiker toegewezen

1. Als u gebruikmaakt van de console CLI (in plaats van een Azure-Cloud-Shell-sessie), eerst aanmelden bij Azure. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt maken van de nieuwe MSI:

    ```azurecli
    az login
    ```

2. Maak een gebruiker toegewezen MSI met [az identiteit maken](/cli/azure/identity#az_identity_create). De `-g` parameter geeft u de resourcegroep waar het MSI-bestand is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Het antwoord bevat de details voor de gebruiker toegewezen MSI gemaakte, vergelijkbaar met het volgende voorbeeld. Opmerking de `id` waarde voor uw MSI zoals deze wordt gebruikt in de volgende stap:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>De gebruiker toegewezen MSI toewijzen aan uw Linux-VM

In tegenstelling tot een MSI systeem toegewezen kan een gebruiker toegewezen MSI worden gebruikt door clients in meerdere Azure-resources. Voor deze zelfstudie maakt toewijzen u deze aan een enkele virtuele machine. U kunt deze ook toewijzen aan meer dan één virtuele machine.

De gebruiker toegewezen MSI toewijzen aan uw Linux-VM met [az vm toewijzen-identity](/cli/azure/vm#az_vm_assign_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. Gebruik de `id` eigenschap geretourneerd in de vorige stap voor de `--identities` parameterwaarde:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Uw MSI gebruiker toegewezen toegang verlenen aan een resourcegroep in Azure Resource Manager 

MSI biedt uw code met een toegangstoken voor verificatie bij resource API's die ondersteuning bieden voor Azure AD-verificatie. Uw code in deze zelfstudie heeft toegang tot de Azure Resource Manager-API. 

Voordat uw code echter toegang krijgen de API tot kan, moet u het MSI identiteit toegang verlenen tot een resource in Azure Resource Manager. In dit geval de resourcegroep waarin de virtuele machine is opgenomen. Zorg ervoor dat u de `<CLIENT ID>`, `<SUBSCRIPTION ID>`, en `<RESOURCE GROUP>` parameterwaarden met uw eigen waarden. Vervang `<CLIENT ID>` met de `clientId` eigenschap die is geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi): 

```azurecli-interactive
az role assignment create --assignee <CLIENT ID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

Het antwoord bevat de details voor de roltoewijzing gemaakt, vergelijkbaar met het volgende voorbeeld:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Ophalen van een toegangstoken die met de identiteit van de VM en deze gebruiken om aan te roepen Resource Manager 

Voor het restant van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks verbinding maken met uw virtuele machine.
2. **Verbinding maken met** naar de virtuele machine met de SSH-client van uw keuze.  
3. Het terminalvenster met CURL, zorg er in een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Resource Manager.  

   De aanvraag CURL te verkrijgen van een toegangstoken wordt weergegeven in het volgende voorbeeld. Zorg ervoor dat u `<CLIENT ID>` met de `clientId` eigenschap die is geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<CLIENT ID>"   
   ```
    
    > [!NOTE]
    > De waarde van de `resource` parameter moet een exacte overeenkomst voor door Azure AD wordt verwacht. Wanneer u de resource-ID van Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen. 
    
    Het antwoord bevat het toegangstoken dat u nodig hebt voor toegang tot Azure Resource Manager. 
    
    Voorbeeld van antwoord:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

4. Nu het toegangstoken gebruiken voor toegang tot Azure Resource Manager en de eigenschappen van de resourcegroep die u eerder de gebruiker toegewezen MSI toegang verleend gelezen. Zorg ervoor dat u `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` met de waarden die u eerder hebt opgegeven en `<ACCESS TOKEN>` aan het token geretourneerd in de vorige stap.

    > [!NOTE]
    > De URL is hoofdlettergevoelig, dus zorg ervoor dat u eerder hebt gebruikt wanneer u met de resourcegroep en de hoofdletters 'G' in de naam exact hetzelfde geval u `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Het antwoord bevat de specifieke resourcegroep informatie vergelijkbaar met het volgende voorbeeld: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
