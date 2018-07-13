---
title: Een gebruiker toegewezen MSI-bestand op een Linux-VM gebruiken voor toegang tot Azure Storage
description: Een zelfstudie die u helpt bij het proces van het gebruik van een gebruiker toegewezen Managed Service Identity (MSI) op een Linux-VM voor toegang tot Azure Storage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4a1a2d0c40012649f6cd89193fd3f704f325e38a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611041"
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Een gebruiker toegewezen Managed Service Identity (MSI) op een Linux-VM gebruiken voor toegang tot Azure Storage

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u kunt maken en gebruiken van een gebruiker toegewezen Managed Service Identity (MSI) van een virtuele Linux-Machine, waarna deze gebruiken voor toegang tot Azure Storage. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maak een beheerde Service-identiteit (MSI) toegewezen gebruiker
> * De gebruiker toegewezen MSI toewijzen aan een virtuele Linux-Machine
> * De MSI toegang verlenen tot een exemplaar van Azure Storage
> * Een toegangstoken met behulp van de MSI-bestand door de gebruiker toegewezen identiteit, en deze gebruiken voor toegang tot Azure Storage

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de voorbeelden van CLI-script in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit Azure portal of via de knop 'Try It' bevindt zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of later) als u liever een lokale CLI-console.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

U maakt eerst een nieuwe Linux-VM. U kunt ook een MSI-bestand op een bestaande virtuele machine inschakelen als u liever.

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Meer groottes Selecteer **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="create-a-user-assigned-msi"></a>Een gebruiker toegewezen MSI maken

1. Als u gebruikmaakt van de CLI-console (in plaats van een Azure Cloud Shell-sessie), eerst aanmelden bij Azure. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt maken van het nieuwe MSI-bestand:

    ```azurecli
    az login
    ```

2. Maak een gebruiker toegewezen MSI met [az-identiteit maken](/cli/azure/identity#az_identity_create). De `-g` parameter geeft u de resourcegroep waar het MSI-bestand wordt gemaakt, en de `-n` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden door uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Het antwoord bevat details voor de gebruiker toegewezen MSI-bestand gemaakt, vergelijkbaar met het volgende voorbeeld. Houd er rekening mee de `id` waarde voor de MSI-bestand, zoals deze wordt gebruikt in de volgende stap:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
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

In tegenstelling tot een MSI-systeem is toegewezen, kan een gebruiker toegewezen MSI worden gebruikt door clients in meerdere Azure-resources. Voor deze zelfstudie maakt toewijzen u deze aan een enkele virtuele machine. U kunt deze ook toewijzen aan meer dan één virtuele machine.

De gebruiker toegewezen MSI toewijzen aan uw Linux-VM met [az vm toewijzen-identity](/cli/azure/vm#az-vm-identity-assign). Vervang de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden door uw eigen waarden. Gebruik de `id` eigenschap geretourneerd in de vorige stap voor de `--identities` parameterwaarde:

```azurecli-interactive
az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Create a storage account 

Als u dit niet al hebt, moet u nu een storage-account maken. Ook kunt u deze stap overslaan en een bestaand opslagaccount gebruiken als u liever. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een **naam** voor de storage-account, die u later gebruiken.  
4. **Implementatiemodel** en **soort Account** moet worden ingesteld op 'Resource manager' en 'Algemeen', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven tijdens het maken van uw virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Maak een blobcontainer in de storage-account

Omdat bestanden blob-opslag vereist, die u wilt maken van een blob-container waarin het bestand op te slaan. Vervolgens uploaden en downloaden van een bestand naar de blob-container in het nieuwe opslagaccount.

1. Ga terug naar de zojuist gemaakte opslagaccount.
2. Klik op de **Containers** koppelen aan de linkerkant, onder 'blobservice'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe 'container deelvenster schuift.
4. Geef een naam op voor de container, selecteert u een toegangsniveau en klik op **OK**. De opgegeven naam wordt ook gebruikt later in de zelfstudie. 

    ![Opslagcontainer maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Een bestand uploaden naar de zojuist gemaakte container door te klikken op de containernaam van de, klikt u vervolgens **uploaden**, vervolgens selecteert u een bestand en klik vervolgens op **uploaden**.

    ![Tekstbestand uploaden](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>De gebruiker toegewezen MSI toegang verlenen tot een Azure Storage-container

Met behulp van een MSI-bestand, krijgt uw code toegangstokens voor verificatie bij de resources die ondersteuning bieden voor Azure AD-verificatie. In deze zelfstudie gebruikt u Azure Storage.

Eerst verlenen u het MSI-identiteit toegang tot een Azure Storage-container. In dit geval gebruikt u de container die eerder hebt gemaakt. Werk de waarden voor `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, en `<CONTAINER NAME>` die geschikt zijn voor uw omgeving. Bovendien vervangen `<MSI PRINCIPALID>` met de `principalId` eigenschap die wordt geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

Het antwoord bevat de details voor de roltoewijzing is gemaakt:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.storage/storageAccounts/<STORAGE ACCOUNT NAME>/blogServices/default/<CONTAINER NAME>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Een toegangstoken met behulp van de gebruiker toegewezen MSI-identiteit en gebruiken voor het aanroepen van Azure Storage

Voor de rest van de zelfstudie moet u werken van de virtuele machine die u eerder hebt gemaakt.

Als u wilt deze stappen hebt voltooid, moet u een SSH-client. Als u Windows gebruikt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [over het gebruik van SSH-sleutels met Windows op Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en gebruiken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. In de Azure-portal, gaat u naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks die moet verbinding maken met uw virtuele machine.
2. **Verbinding maken met** aan de virtuele machine met de SSH-client van uw keuze. 
3. In het terminalvenster met CURL, een aanvraag indienen op het lokale eindpunt van de MSI-bestand op een toegangstoken ophalen voor Azure Storage.

   De CURL-aanvraag voor het verkrijgen van een toegangstoken wordt weergegeven in het volgende voorbeeld. Vervang `<CLIENT ID>` met de `clientId` eigenschap die wordt geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi):
   
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com/&client_id=<MSI CLIENT ID>" 
   ```

   > [!NOTE]
   > In de vorige aanvraag, de waarde van de `resource` parameter moet een exacte overeenkomst voor wat er door Azure AD wordt verwacht. Wanneer u de Azure Storage-resource-ID gebruikt, moet u de afsluitende slash bevatten in de URI bevatten.
   > In het volgende antwoord, het access_token element als beknopt alternatief ingekort.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Gebruik nu het toegangstoken voor toegang tot Azure Storage, bijvoorbeeld om de inhoud van de voorbeeldbestand dat u eerder hebt geüpload naar de container te lezen. Vervang de waarden van `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, en `<FILE NAME>` door de waarden die u eerder hebt opgegeven, en `<ACCESS TOKEN>` met het token dat is geretourneerd in de vorige stap.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Het antwoord bevat de inhoud van het bestand:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Zie voor meer informatie over deze dezelfde zelfstudie met behulp van een SAS-referenties voor opslag, [een Linux VM beheerde Service-identiteit gebruiken voor toegang tot Azure Storage via SAS-referenties](msi-tutorial-linux-vm-access-storage-sas.md)
- Zie voor meer informatie over de SAS-functie van de Azure Storage-account:
  - [Shared access signatures (SAS) gebruiken](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.





