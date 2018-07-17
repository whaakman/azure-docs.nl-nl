---
title: Managed Service Identity voor een Linux-VM gebruiken om toegang te krijgen tot Azure Data Lake Storage
description: Een zelfstudie die laat zien hoe u toegang krijgt tot Azure Data Lake Storage met een Managed Service Identity (MSI) voor een Linux-VM.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 92bd7190832da6ee9da7d1679b9f27b66a15e3a4
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904302"
---
# <a name="tutorial-use-managed-service-identity-for-a-linux-vm-to-access-azure-data-lake-store"></a>Zelfstudie: Managed Service Identity voor een Linux-VM gebruiken om toegang te krijgen tot Azure Data Lake Storage

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u toegang krijgt tot Azure Data Lake Storage met een Managed Service Identity (MSI) voor een virtuele Linux-machine (VM). Identiteiten die u maakt via MSI, worden automatisch beheerd in Azure. U kunt MSI gebruiken voor verificatie bij services die Azure AD-verificatie (Azure Active Directory) ondersteunen, zonder referenties in uw code te hoeven invoegen. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * MSI inschakelen op een Linux-VM. 
> * Uw virtuele machine toegang verlenen tot Azure Data Lake Storage.
> * Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en dat token gebruiken om toegang te krijgen tot Azure Data Lake Storage.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine maken in een nieuwe resourcegroep

Voor deze zelfstudie maken we een nieuwe virtuele Linux-machine. U kunt MSI ook inschakelen op een bestaande virtuele machine.

1. Selecteer de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Bij **Verificatietype** selecteert u **Openbare SSH-sleutel** of **Wachtwoord**. Met de gemaakte referenties kunt u zich aanmelden bij de virtuele machine.

   ![‘Basis’-deelvenster voor het maken van een virtuele machine](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Selecteer een abonnement voor de virtuele machine in de lijst **Abonnement**.
5. Als u de virtuele machine in een nieuwe resourcegroep wilt maken, selecteert u **Resourcegroep** > **Nieuwe maken**. Selecteer **OK** wanneer u klaar bent.
6. Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf de standaardinstellingen in het deelvenster Instellingen en selecteer **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

Met een MSI op de VM kunt u toegangstokens uit Azure AD ophalen zonder referenties in uw code te hoeven opnemen. Er gebeuren twee dingen als u Managed Service Identity inschakelt op een virtuele machine: de virtuele machine wordt bij Azure Active Directory geregistreerd om de beheerde identiteit te maken, en de identiteit wordt geconfigureerd op de virtuele machine.

1. Selecteer bij **Virtuele machine** de virtuele machine waarop u MSI wilt inschakelen.
2. Selecteer **Configuratie** in het linkerdeelvenster.
3. U ziet **Managed Service Identity**. Selecteer **Ja** om MSI te registreren en in te schakelen. Als u dit wilt uitschakelen, selecteert u **Nee**.
   ![Selectie ‘Registreren bij Azure Active Directory’](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)
4. Selecteer **Opslaan**.

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw virtuele machine toegang verlenen tot Azure Data Lake Storage

U kunt nu uw virtuele machine toegang tot bestanden en mappen in Azure Data Lake Storage verlenen. Voor deze stap kunt u een bestaand Data Lake Storage-exemplaar gebruiken, of een nieuw exemplaar maken. Als u een Data Lake Storage wilt maken met behulp van Azure Portal, volgt u deze [snelstart voor Azure Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Er zijn ook snelstarts in de [documentatie over Azure Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) voor het gebruik van Azure CLI en Azure Powershell.

Maak een nieuwe map in Data Lake Storage, en verleen MSI toestemming voor het lezen, schrijven en uitvoeren van bestanden in die map:

1. Selecteer in Azure Portal **Data Lake Storage** in het linkerdeelvenster.
2. Selecteer het Data Lake Storage-exemplaar dat u wilt gebruiken.
3. Selecteer **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van het Data Lake Storage-exemplaar wordt geselecteerd. Selecteer **Toegang** op de opdrachtbalk.
5. Selecteer **Toevoegen**.  Voer in het vak **Selecteren** de naam van uw virtuele machine in, bijvoorbeeld **DevTestVM**. Selecteer uw virtuele machine in de zoekresultaten en klik op **Selecteren**.
6. Klik op **Machtigingen selecteren**.  Selecteer **Lezen** en **Uitvoeren**, voeg deze toe aan **Deze map** als **Alleen een toegangsmachtiging**. Selecteer **OK**.  De machtiging wordt toegevoegd.
7. Sluit het deelvenster **Toegang**.
8. Voor deze zelfstudie maakt u een nieuwe map. Selecteer **Nieuwe map** in de opdrachtbalk en geef de nieuwe map een naam, bijvoorbeeld **TestFolder**.  Selecteer **OK**.
9. Selecteer de map die u hebt gemaakt en selecteer vervolgens **Toegang** op de opdrachtbalk.
10. Selecteer **Toevoegen**, net als in stap 5. Voer in het vak **Selecteren** de naam van uw virtuele machine in. Selecteer uw virtuele machine in de zoekresultaten en klik op **Selecteren**.
11. Selecteer **Machtigingen selecteren**, net als bij stap 6. Selecteer **Lezen**, **Schrijven** en **Uitvoeren**, voeg deze toe aan **Deze map** als **Een toegangsmachtiging en een standaardmachtiging**. Selecteer **OK**.  De machtiging wordt toegevoegd.

MSI kan nu alle bewerkingen uitvoeren op bestanden in de map die u hebt gemaakt. Lees het artikel over [toegangsbeheer in Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control) voor meer informatie over het beheren van de toegang tot Data Lake Storage.

## <a name="get-an-access-token-and-call-the-data-lake-store-file-system"></a>Een toegangstoken downloaden en het bestandssysteem van Data Lake Storage aanroepen

Azure Data Lake Storage biedt systeemeigen ondersteuning voor Azure AD-verificatie, zodat toegangstokens die zijn verkregen via MSI direct kunnen worden geaccepteerd. Voor verificatie bij het bestandssysteem van Data Lake Storage verzendt u een door Azure AD uitgegeven toegangstoken naar het eindpunt van het Data Lake Storage-bestandssysteem. Het toegangstoken bevindt zich in een autorisatie-header met de notatie "Bearer \<ACCESS_TOKEN_VALUE\>".  Zie [Verificatie bij Data Lake Storage met Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory) voor meer informatie over ondersteuning voor Azure AD-verificatie in Data Lake Storage.

In deze zelfstudie gebruikt u cURL voor het maken van REST-aanvragen voor verificatie bij de REST-API van het Data Lake Storage-bestandssysteem.

> [!NOTE]
> Managed Service Identity wordt nog niet ondersteund in de client-SDK's van het Data Lake Storage-bestandssysteem.

U hebt een SSH-client nodig om deze stappen uit te voeren. Als u Windows gebruikt, kunt u de SSH-client in het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) gebruiken. Zie [De sleutels van uw SSH-client gebruiken onder Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) of [Een sleutelpaar met een openbare SSH-sleutel en een privé-sleutel maken en gebruiken voor virtuele Linux-machines in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md) als u hulp nodig hebt bij het configureren van de sleutels van uw SSH-client.

1. Blader in de portal naar uw Linux-VM. Selecteer **Verbinden** in **Overzicht**.  
2. Maak verbinding met de VM met behulp van de SSH-client van uw keuze. 
3. Dien in het terminalvenster met behulp van cURL een aanvraag in bij het lokale MSI-eindpunt om een toegangstoken voor het Data Lake Storage-bestandssysteem op te halen. De resource-id voor Data Lake Storage is ‘https://datalake.azure.net/’.  Het is belangrijk om de afsluitende slash in de resource-id op te nemen.
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatalake.azure.net%2F' -H Metadata:true   
   ```
    
   Een succesvol antwoord retourneert het toegangstoken dat u gebruikt voor verificatie bij Data Lake Storage:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Verzend met behulp van cURL een aanvraag naar het REST-eindpunt van het Data Lake Storage-bestandssysteem om de mappen in de hoofdmap weer te geven. Dit is een eenvoudige manier om te controleren of alles juist is geconfigureerd. Kopieer de waarde van het toegangstoken uit de vorige stap. Het is belangrijk dat de tekenreeks 'Bearer' in de autorisatie-header begint met de hoofdletter 'B'. U vindt de naam van uw Data Lake Storage-exemplaar in de sectie **Overzicht** van het deelvenster **Data Lake Storage** in Azure Portal.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Een geslaagd antwoord ziet er zo uit:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Nu kunt u proberen een bestand te uploaden naar uw Data Lake Storage-exemplaar. Maak eerst een bestand om te uploaden.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Verzend met behulp van cURL een aanvraag naar het REST-eindpunt van uw Data Lake Storage-bestandssysteem om het bestand te uploaden naar de map die u eerder hebt gemaakt. Het uploaden omvat een omleiding en cURL volgt de omleiding automatisch. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Een geslaagd antwoord ziet er zo uit:

   ```bash
   HTTP/1.1 100 Continue
   HTTP/1.1 307 Temporary Redirect
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: 756f6b24-0cca-47ef-aa12-52c3b45b954c
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
       
   HTTP/1.1 100 Continue
       
   HTTP/1.1 201 Created
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: af5baa07-3c79-43af-a01a-71d63d53e6c4
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
   ```

Met andere API's voor het Data Lake Storage-bestandssysteem kunt u toevoegen aan bestanden, bestanden downloaden, en nog veel meer.

Gefeliciteerd. U bent geverifieerd bij het bestandssysteem van Data Lake Storage door MSI te gebruiken voor een Linux-VM.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u toegang krijgt tot Azure Data Lake Storage met behulp van een Managed Service Identity voor een virtuele Linux-machine. Zie voor meer informatie over Azure Data Lake Storage:

> [!div class="nextstepaction"]
>[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview)
