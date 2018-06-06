---
title: Gebruik van een Linux VM beheerd identiteit voor toegang tot Azure Storage
description: Een zelfstudie die u bij het proces helpt van het gebruik van een identiteit System-Assigned beheerd op een Linux-VM voor toegang tot Azure Storage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: 1bd4c422bd952cb6abe2432904adfd9ae9e2fd4f
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801701"
---
# <a name="tutorial-use-a-linux-vms-managed-identity-to-access-azure-storage"></a>Zelfstudie: Een Linux-VM beheerd identiteit gebruiken voor toegang tot Azure Storage 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Deze zelfstudie laat zien hoe u maken en gebruiken van een Linux VM beheerd identiteit voor toegang tot Azure Storage. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een virtuele Linux-machine in een nieuwe resourcegroep maken
> * Beheerde identiteit op een Linux-virtuele Machine (VM) inschakelen
> * Een blob-container in een opslagaccount maken
> * De Linux-VM identiteit beheerde toegang verlenen tot een Azure Storage-container
> * Een toegangstoken ophalen en deze gebruiken om aan te roepen Azure Storage

> [!NOTE]
> Azure Active Directory-verificatie voor Azure Storage is openbare preview.

## <a name="prerequisites"></a>Vereisten

Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com) voordat u doorgaat.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de **probeert het** knop in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

In deze sectie maakt u een Linux-VM wordt een identiteit beheerd later verleend.

1. Selecteer de **nieuw** knop in de linkerbovenhoek van de Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

   ![Deelvenster 'Basisbeginselen' voor het maken van een virtuele machine](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. In de **abonnement** , selecteert u een abonnement voor de virtuele machine.
5. Om te selecteren van een nieuwe resourcegroep die u wilt dat de virtuele machine moet worden gemaakt **resourcegroep** > **nieuw**. Wanneer u klaar bent, selecteert u **OK**.
6. Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Houd de standaardinstellingen en selecteer in het deelvenster instellingen **OK**.

## <a name="enable-managed-identity-on-your-vm"></a>Beheerde identiteit op de virtuele machine inschakelen

Een beheerde identiteit van de virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder referenties in uw code te plaatsen. Achter de inschakelen beheerd identiteit op een virtuele Machine via de Azure-portal biedt twee dingen: registreert uw virtuele machine met Azure AD voor het maken van een beheerde identiteit en configureert deze identiteit van de op de virtuele machine.

1. Navigeer naar de resourcegroep van de nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Onder de **instellingen** categorie, klik op **configuratie**.
3. Als u beheerd identiteit, schakelt **Ja**.
4. Klik op **opslaan** toepassen van de configuratie. 

## <a name="create-a-storage-account"></a>Create a storage account 

In deze sectie maakt u een opslagaccount. 

1. Klik op de **+ maken van een resource** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **opslag**, klikt u vervolgens **opslagaccount - blob, bestand, tabel, wachtrij**.
3. Onder **naam**, voer een naam voor het opslagaccount.  
4. **Implementatiemodel** en **Account kind** moet worden ingesteld op **resourcemanager** en **opslag (algemeen v1)**. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Maken van een blob-container en een bestand uploaden naar het storage-account

Bestanden vereist blob-opslag, dus u moet maken van een blob-container waarin het bestand wilt opslaan. U kunt vervolgens een bestand uploaden naar de blob-container in het nieuwe opslagaccount.

1. Ga terug naar uw nieuwe opslagaccount.
2. Onder **Blob-Service**, klikt u op **Containers**.
3. Klik op **+ Container** boven aan de pagina.
4. Onder **nieuwe container**, Geef een naam voor de container en onder **openbare toegangsniveau** hou de standaardwaarde.

    ![Opslagcontainer maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Met een editor naar keuze, maak een bestand met de titel *world.txt Hallo* op uw lokale machine.  Open het bestand en het toevoegen van de tekst (zonder aanhalingstekens) "Hello world! :) ' en vervolgens opslaan. 

6. Upload het bestand naar de zojuist gemaakte container door te klikken op de containernaam van de, klikt u vervolgens **uploaden**
7. In de **blob uploaden** deelvenster onder **bestanden**, klik op het pictogram van de map en blader naar het bestand **hello_world.txt** op uw lokale computer, selecteert u het bestand en klik op **Uploaden**.

    ![Tekstbestand uploaden](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Uw VM toegang verlenen tot een Azure Storage-container 

U kunt beheerde identiteit van de VM's gebruiken voor het ophalen van de gegevens in de Azure storage-blob.   

1. Ga terug naar uw nieuwe opslagaccount.  
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine.
4. Onder **rol**, selecteer in de vervolgkeuzelijst **gegevenslezer Storage-Blob (Preview)**. 
5. In de volgende vervolgkeuzelijst onder **toewijzen van toegang tot**, kies **virtuele Machine**.  
6. Controleer vervolgens het juiste abonnement wordt vermeld in **abonnement** dropdown en stel vervolgens **resourcegroep** naar **alle resourcegroepen**.  
7. Onder **Selecteer**, kies uw virtuele machine en klik vervolgens op **opslaan**.

    ![Machtigingen toewijzen](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Een toegangstoken ophalen en deze gebruiken om aan te roepen Azure Storage

Azure Storage systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren verkregen met behulp van de identiteit van een beheerd. Dit maakt deel uit van de integratie van Azure Storage met Azure AD en verschilt van het verstrekken van referenties voor de verbindingsreeks.

De volgende stappen uit te voeren, moet u te laten werken vanuit de virtuele machine eerder hebt gemaakt en moet u een SSH-client tot stand te brengen. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect**. Kopieer de tekenreeks verbinding maken met uw virtuele machine.
2. **Verbinding maken met** naar de virtuele machine met de SSH-client van uw keuze. 
3. Het terminalvenster met CURL, zorg er in een aanvraag naar het lokale eindpunt beheerd identiteit aan een toegangstoken ophalen voor Azure Storage.
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true
    ```
4. Het toegangstoken nu gebruiken voor toegang tot Azure Storage, bijvoorbeeld lezen van de inhoud van het voorbeeldbestand die u eerder hebt geüpload naar de container. Vervang de waarden van `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, en `<FILE NAME>` met de waarden die u eerder hebt opgegeven en `<ACCESS TOKEN>` aan het token geretourneerd in de vorige stap.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Het antwoord bevat de inhoud van het bestand:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een virtuele Linux-machine beheerde identiteit voor toegang tot Azure Storage inschakelen.  Zie voor meer informatie over Azure Storage:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)
