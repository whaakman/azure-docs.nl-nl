---
title: Een door de gebruiker toegewezen Managed Service Identity op een Linux-VM gebruiken om toegang te krijgen tot Azure Resource Manager
description: Een zelfstudie die u helpt bij het doorlopen van het proces voor het krijgen van toegang tot Azure Resource Manager met een door de gebruiker toegewezen Managed Service Identity op een Linux-VM.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c2735d385b0a3c2201ec2dad83c0c32fe44d458c
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258240"
---
# <a name="tutorial-use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Zelfstudie: een door de gebruiker toegewezen identiteit gebruiken op een Linux-VM om toegang te krijgen tot Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In deze zelfstudie wordt uitgelegd hoe u een door de gebruiker toegewezen identiteit kunt maken, deze toewijzen aan een virtuele Linux-machine (VM) en die identiteit vervolgens gebruiken om toegang te krijgen tot de Azure Resource Manager-API. Beheerde service-identiteiten worden automatisch beheerd door Azure. Ze maken verificatie mogelijk voor services die Azure AD-verificatie ondersteunen, zonder dat er referenties in uw code hoeven te worden ingesloten. 

Beheerde service-identiteiten worden automatisch beheerd door Azure. Ze maken verificatie mogelijk voor services die Azure AD-verificatie ondersteunen, zonder dat er referenties in uw code hoeven te worden ingesloten.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Door de gebruiker toegewezen identiteit maken
> * De door de gebruiker toegewezen identiteit toewijzen aan een Linux-VM 
> * De door de gebruiker toegewezen identiteit toegang verlenen tot een resourcegroep in Azure Resource Manager 
> * Een toegangstoken ophalen met behulp van de door de gebruiker toegewezen identiteit en daarmee Azure Resource Manager aanroepen 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend bent met Managed Service Identity, raadpleeg dan de [overzichtssectie](overview.md). **Zorg ervoor dat u de [verschillen tussen door het systeem en door de gebruiker toegewezen identiteiten](overview.md#how-does-it-work)** kent.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Om de stappen voor het maken van de vereiste resources en het rolbeheer in deze zelfstudie uit te voeren, moet uw account 'Eigenaar'-machtigingen hebben voor het juiste bereik (uw abonnement of resourcegroep). Voor hulp bij roltoewijzing gaat u naar [Op rollen gebaseerd toegangsbeheer gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](/azure/role-based-access-control/role-assignments-portal).

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine maken in een nieuwe resourcegroep

Voor deze zelfstudie maakt u eerst een nieuwe virtuele Linux-machine. U kunt ook een bestaande VM gebruiken.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Bij **Verificatietype** selecteert u **Openbare SSH-sleutel** of **Wachtwoord**. Met de gemaakte referenties kunt u zich aanmelden bij de virtuele machine.

    ![Linux-VM maken](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Om een nieuwe **resourcegroep** te selecteren waarin u de virtuele machine wilt maken, kiest u **Nieuwe maken**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Kies om meer grootten weer te geven de optie **Alle weergeven** of wijzig het filter Ondersteund schijftype. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="create-a-user-assigned-identity"></a>Door de gebruiker toegewezen identiteit maken

1. Als u de CLI-console gebruikt (in plaats van een Azure Cloud Shell-sessie), begint u door u aan te melden bij Azure. Gebruik een account dat is gekoppeld aan het Azure-abonnement waaronder u nieuwe door de gebruiker toegewezen identiteit wilt implementeren:

    ```azurecli
    az login
    ```

2. Maak een door de gebruiker toegewezen identiteit met [az identity create](/cli/azure/identity#az_identity_create). De parameter `-g` geeft de resourcegroep aan waar de Managed Service Identity wordt gemaakt, en de parameter `-n` geeft de naam aan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<MSI NAME>` door uw eigen waarden:
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <MSI NAME>
```

Het antwoord bevat details voor de gemaakte door de gebruiker toegewezen identiteit, vergelijkbaar met het volgende voorbeeld. Noteer de `id`-waarde voor uw door de gebruiker toegewezen identiteit, omdat deze bij de volgende stap wordt gebruikt:

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

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Een door de gebruiker toegewezen identiteit toewijzen aan uw Linux-VM

Een door de gebruiker toegewezen identiteit kan worden gebruikt door clients op meerdere Azure-resources. Gebruik de volgende opdrachten om de door de gebruiker toegewezen identiteit toe te wijzen aan één VM. Gebruik de eigenschap `Id` die in de vorige stap is geretourneerd voor de parameter `-IdentityID`.

Wijs de door de gebruiker toegewezen Managed Service Identity toe aan uw Linux-VM met [az vm assign-identity](/cli/azure/vm#az_vm_assign_identity). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. Gebruik de eigenschap `id` die in de vorige stap is geretourneerd voor de waarde van de parameter `--identities`.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Uw door de gebruiker toegewezen identiteit toegang verlenen tot een resourcegroep in Azure Resource Manager 

Managed Service Identity (MSI) biedt identiteiten die uw code kan gebruiken om toegangstokens aan te vragen voor verificatie bij resource-API's die Azure AD-verificatie ondersteunen. In deze zelfstudie krijgt uw code toegang tot de Azure Resource Manager-API.  

Voordat uw code toegang tot de API kan krijgen, moet u de identiteit toegang geven tot een resource in Azure Resource Manager. In dit geval is dat de resourcegroep waarin de VM zich bevindt. Werk de waarde voor `<SUBSCRIPTION ID>` en `<RESOURCE GROUP>` bij overeenkomstig uw omgeving. Vervang bovendien `<MSI PRINCIPALID>` door de eigenschap `principalId` die wordt geretourneerd door de opdracht `az identity create` in [Door de gebruiker toegewezen MSI maken](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

Het antwoord bevat details voor de gemaakte roltoewijzing, vergelijkbaar met het volgende voorbeeld:

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

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en daarmee Resource Manager aanroepen 

Voor de rest van de zelfstudie werken we op de virtuele machine die we eerder hebben gemaakt.

U hebt een SSH-client nodig om deze stappen uit te voeren. Als u Windows gebruikt, kunt u de SSH-client in het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) gebruiken. 

1. Meld u aan bij de Azure [Portal](https://portal.azure.com).
2. Navigeer in de portal naar **Virtuele machines**, ga naar de virtuele Windows-machine en klik op de pagina **Overzicht** op **Verbinden**. Kopieer de verbindingsreeks voor uw virtuele machine.
3. Maak verbinding met de virtuele machine met de SSH-client van uw keuze. Als u Windows gebruikt, kunt u de SSH-client in het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) gebruiken. Zie [De sleutels van uw SSH-client gebruiken onder Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) of [Een sleutelpaar met een openbare SSH-sleutel en een privé-sleutel maken en gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md) als u hulp nodig hebt bij het configureren van de sleutels van uw SSH-client.
4. Dien in het terminalvenster met behulp van CURL een aanvraag in op het Azure IMDS-eindpunt (Instance Metadata Service) om een toegangstoken voor Azure Resource Manager op te halen.  

   De CURL-aanvraag voor het verkrijgen van een toegangstoken wordt in het volgende voorbeeld weergegeven. Vervang `<CLIENT ID>` door de eigenschap `clientId` die wordt geretourneerd door de opdracht `az identity create` in [Door de gebruiker toegewezen identiteit maken](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > De waarde van de parameter `resource` moet exact overeenkomen met wat er in Azure AD wordt verwacht. Wanneer u de resource-id van Resource Manager gebruikt, moet u de URI opgeven met een slash op het einde. 
    
    Het antwoord bevat het toegangstoken dat u nodig hebt voor toegang tot Azure Resource Manager. 
    
    Voorbeeldantwoord:  

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

5. Gebruik het toegangstoken om toegang te krijgen tot Azure Resource Manager en lees de eigenschappen van de resourcegroep waarvoor u eerder uw door de gebruiker toegewezen identiteit toegang hebt verleend. Vervang de waarden van `<SUBSCRIPTION ID>`, en `<RESOURCE GROUP>` door de waarden die u eerder hebt opgegeven, en `<ACCESS TOKEN>` door het token dat in de vorige stap is geretourneerd.

    > [!NOTE]
    > De URL is hoofdlettergevoelig, dus gebruik precies dezelfde naam die u eerder hebt gebruikt voor de naam van de resourcegroep, en de hoofdletter ‘G’ in `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Het antwoord bevat de gegevens van de specifieke resourcegroep, vergelijkbaar met het volgende voorbeeld: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een door de gebruiker toegewezen identiteit te maken en deze te koppelen aan een virtuele Linux-machine voor toegang tot de Azure Resource Manager-API.  Zie voor meer informatie over Azure Resource Manager:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

