---
title: Service-identiteit beheerd voor een Linux-VM te gebruiken voor toegang tot Azure Data Lake Store
description: Een zelfstudie waarin u het gebruik van beheerde Service identiteit (MSI) voor een Linux-VM voor toegang tot Azure Data Lake Store.
services: active-directory
documentationcenter: 
author: skwan
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: skwan
ms.openlocfilehash: 6bc3f893697ba39c3608f0bc866be89da25596ef
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="use-managed-service-identity-for-a-linux-vm-to-access-azure-data-lake-store"></a>Service-identiteit beheerd voor een Linux-VM te gebruiken voor toegang tot Azure Data Lake Store

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u Service-identiteit beheerd voor virtuele Linux-machine (VM) voor toegang tot Azure Data Lake Store. Identiteiten die u via de MSI wordt automatisch beheerd door Azure. U kunt MSI gebruiken om de services die ondersteuning bieden voor verificatie van Azure Active Directory (Azure AD), zonder referenties invoegen in uw code te verifiëren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Schakel MSI op een virtuele Linux-machine. 
> * Uw VM-toegang verlenen tot Azure Data Lake Store.
> * Een toegangstoken ophalen met behulp van de identiteit van de VM en deze gebruiken voor toegang tot Azure Data Lake Store.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux VM. U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Selecteer de **nieuw** knop in de linkerbovenhoek van de Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

   ![Deelvenster 'Basisbeginselen' voor het maken van een virtuele machine](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. In de **abonnement** , selecteert u een abonnement voor de virtuele machine.
5. Om te selecteren van een nieuwe resourcegroep die u wilt dat de virtuele machine moet worden gemaakt **resourcegroep** > **nieuw**. Wanneer u klaar bent, selecteert u **OK**.
6. Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Houd de standaardinstellingen en selecteer in het deelvenster instellingen **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

MSI voor een virtuele machine kunt u toegangstokens ophalen uit Azure AD zonder referenties te plaatsen in uw code. De MSI-VM-extensie inschakelen MSI worden geïnstalleerd op de virtuele machine en maakt MSI in Azure Resource Manager.  

1. Voor **virtuele Machine**, selecteer de virtuele machine die u inschakelen van MSI wilt op.
2. Selecteer in het linkerdeelvenster **configuratie**.
3. U ziet **beheerde service-identiteit**. Als u wilt registreren en MSI inschakelt, selecteer **Ja**. Als u uitschakelen wilt, selecteert u **Nee**.
   !['Registreren bij Azure Active Directory' selectie](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)
4. Selecteer **Opslaan**.
5. Als u wilt controleren welke uitbreidingen zijn op deze Linux VM, selecteer **extensies**. Als MSI is ingeschakeld, **ManagedIdentityExtensionforLinux** wordt weergegeven in de lijst.

   ![Lijst met extensies](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw VM-toegang verlenen tot Azure Data Lake Store

U kunt nu uw VM-toegang tot bestanden en mappen in Azure Data Lake Store verlenen. Voor deze stap kunt u een bestaand exemplaar van Data Lake Store gebruiken of een nieuwe maken. Ga als volgt om een Data Lake Store-exemplaar met behulp van de Azure-portal, de [Azure Data Lake Store Quick Start](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Er zijn ook snelstartgidsen die gebruikmaken van Azure CLI en Azure PowerShell in de [documentatie Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).

Maak een nieuwe map in Data Lake Store en MSI machtiging verlenen om te lezen, schrijven en uitvoeren van bestanden in de map:

1. Selecteer in de Azure-portal **Data Lake Store** in het linkerdeelvenster.
2. Selecteer het Data Lake Store-exemplaar dat u wilt gebruiken.
3. Selecteer **Data Explorer** op de opdrachtbalk.
4. De hoofdmap van het exemplaar van Data Lake Store is geselecteerd. Selecteer **toegang** op de opdrachtbalk.
5. Selecteer **Toevoegen**.  In de **Selecteer** bijvoorbeeld de naam van uw VM-- Geef **DevTestVM**. Selecteer uw virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
6. Klik op **machtigingen selecteren**.  Selecteer **lezen** en **Execute**, toevoegen aan **deze map**, en toevoegen als **alleen een toegangsmachtiging**. Selecteer **Ok**.  De machtiging moet worden toegevoegd.
7. Sluit de **toegang** deelvenster.
8. Maak een nieuwe map voor deze zelfstudie. Selecteer **nieuwe map** op de opdrachtbalk en de nieuwe map--bijvoorbeeld een naam geven **TestFolder**.  Selecteer **Ok**.
9. Selecteer de map die u gemaakt en selecteer vervolgens **toegang** op de opdrachtbalk.
10. Vergelijkbaar is met stap 5, selecteer **toevoegen**. In de **Selecteer** en voer de naam van uw virtuele machine. Selecteer uw virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
11. Net als bij stap 6 selecteert **Selecteer machtigingen**. Selecteer **lezen**, **schrijven**, en **Execute**, toevoegen aan **deze map**, en toevoegen als **een machtigingsvermelding toegang en een standaardbericht vermelding van machtiging**. Selecteer **Ok**.  De machtiging moet worden toegevoegd.

MSI kunt nu alle bewerkingen uitvoeren op bestanden in de map die u hebt gemaakt. Zie voor meer informatie over het beheren van toegang tot Data Lake Store [toegangsbeheer in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-and-call-the-data-lake-store-file-system"></a>Ophalen van een toegangstoken en roept het bestandssysteem van Data Lake Store

Azure Data Lake Store systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren is verkregen via MSI. Om te verifiëren naar het Data Lake Store-bestandssysteem, verzendt u een toegangstoken dat is uitgegeven door Azure AD aan uw Data Lake Store file system-eindpunt. Het toegangstoken is in een autorisatie-header in de indeling ' Bearer \<ACCESS_TOKEN_VALUE\>'.  Zie voor meer informatie over Data Lake Store-ondersteuning voor Azure AD-verificatie, [verificatie met Data Lake Store met Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).

In deze zelfstudie maakt verifiëren u de REST-API voor het bestandssysteem van Data Lake Store met cURL REST-aanvragen.

> [!NOTE]
> De client-SDK's voor het bestandssysteem van Data Lake Store-Service-identiteit beheerd niet ondersteunen.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](../virtual-machines/linux/ssh-from-windows.md) of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

1. Blader in de portal voor uw Linux-VM. In **overzicht**, selecteer **Connect**.  
2. Verbinding maken met de virtuele machine met behulp van de SSH-client van uw keuze. 
3. Het terminalvenster via cURL, zorg er in een aanvraag naar het lokale eindpunt MSI ophalen van een toegangstoken voor de Data Lake Store-bestandssysteem. De resource-id van Data Lake Store is 'https://datalake.azure.net/'.  Het is belangrijk dat u de afsluitende slash bevatten in de resource-id.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Een geslaagde reactie retourneert het toegangstoken die u gebruikt om te verifiëren naar Data Lake Store:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Met behulp van cURL een indienen naar uw Data Lake Store-bestandssysteem REST-eindpunt voor het weergeven van de mappen in de hoofdmap. Dit is een eenvoudige manier om te controleren of alles correct is geconfigureerd. Kopieer de waarde van het toegangstoken van de vorige stap. Het is belangrijk dat de tekenreeks 'Bearer' in de autorisatie-header een hoofdletter "B is" U vindt de naam van uw Data Lake Store-exemplaar in de **overzicht** sectie van de **Data Lake Store** deelvenster in de Azure-portal.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Een geslaagde reactie ziet er als volgt:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Nu kunt u proberen het uploaden van een bestand met de Data Lake Store-sessie. Maak eerst een bestand te uploaden.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Maak een aanvraag naar uw Data Lake Store-bestandssysteem REST-eindpunt het bestand te uploaden naar de map die u eerder hebt gemaakt met behulp van cURL. Het uploaden omvat een omleiding en cURL volgt de omleiding automatisch. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Een geslaagde reactie ziet er als volgt:

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

Met andere API's voor het bestandssysteem van Data Lake Store, kunt u toevoegen aan bestanden, downloadbestanden en meer.

Gefeliciteerd! U hebt geverifieerd naar het bestandssysteem van Data Lake Store met behulp van MSI voor een Linux-VM.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](../active-directory/msi-overview.md).
- Data Lake Store gebruikt voor beheerbewerkingen, Azure Resource Manager.  Zie voor meer informatie over het gebruik van MSI om te verifiëren naar Resource Manager [een Linux VM beheerde Service identiteit (MSI) gebruiken voor toegang tot de Resource Manager](https://docs.microsoft.com/azure/active-directory/msi-tutorial-linux-vm-access-arm).
- Meer informatie over [verificatie met Data Lake Store met behulp van Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).
- Meer informatie over [file systeembewerkingen op Azure Data Lake Store met behulp van de REST-API](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-operations-rest-api) of de [WebHDFS FileSystem APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Meer informatie over [toegangsbeheer in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.