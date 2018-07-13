---
title: Het gebruik van een Windows VM Managed Service Identity (MSI) voor toegang tot Azure Data Lake Store
description: Een zelfstudie waarin wordt uitgelegd hoe u een Windows VM Managed Service Identity (MSI) gebruikt voor toegang tot Azure Data Lake Store.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: daef85164793dd6183c41604f200864aabadf8d8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610878"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Een Windows VM Managed Service Identity (MSI) gebruiken voor toegang tot Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u een Managed Service Identity (MSI) voor een Windows virtuele machine (VM) gebruikt voor toegang tot een Azure Data Lake Store. Beheerde Service-identiteiten automatisch worden beheerd door Azure en kunt u voor de verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Inschakelen van MSI-bestand op een Windows VM 
> * Uw VM-toegang verlenen aan een Azure Data Lake Store
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken voor toegang tot een Azure Data Lake Store

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows-VM.  U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1. Klik op **een resource maken** in de linkerbovenhoek van Azure portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3. Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakt als volgt de referenties die u gebruiken voor aanmelding bij de virtuele machine.
4. Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** waarin u kunt uw virtuele machine maakt, kiest **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen 

Een VM MSI kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Inschakelen van MSI weet Azure te maken van een beheerde identiteit voor uw virtuele machine. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en Hiermee MSI in Azure Resource Manager.

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI-bestand wilt op.  
2. Klik op de linker navigatiebalk op **configuratie**. 
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4. Controleer of u klikken op **opslaan** aan de configuratie op te slaan.  
   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw VM-toegang verlenen tot Azure Data Lake Store

U kunt nu uw VM-toegang tot bestanden en mappen in een Azure Data Lake Store verlenen.  U kunt voor deze stap maakt gebruik van een bestaande Data Lake Store of een nieuwe maken.  Voor het maken van een nieuwe Data Lake Store met behulp van de Azure portal, volgt u deze [Azure Data Lake Store-snelstartgids](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-overview.md).

Maak een nieuwe map in uw Data Lake Store, en uw VM MSI toestemming geven om te lezen, schrijven en uitvoeren van bestanden in die map:

1. Klik in de Azure-portal op **Data Lake Store** in de navigatiebalk links.
2. Klik op het Data Lake Store die u wilt gebruiken voor deze zelfstudie.
3. Klik op **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van de Data Lake Store is geselecteerd.  Klik op **toegang** in de opdrachtbalk.
5. Klik op **Add**.  In de **Selecteer** en voer de naam van uw virtuele machine, bijvoorbeeld **DevTestVM**.  Klik om te selecteren van de virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
6. Klik op **machtigingen selecteren**.  Selecteer **lezen** en **Execute**, toevoegen aan **deze map**, en toevoegen als **alleen een toegangsmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.
7. Sluit de **toegang** blade.
8. Voor deze zelfstudie, moet u een nieuwe map maken.  Klik op **nieuwe map** in de opdrachtbalk en geef de nieuwe map een naam, bijvoorbeeld **TestFolder**.  Klik op **OK**.
9. Klik op de map die u hebt gemaakt en klik vervolgens op **toegang** in de opdrachtbalk.
10. Vergelijkbaar met stap 5, klikt u op **toevoegen**, in de **Selecteer** veld Voer de naam van uw virtuele machine, selecteert u deze en klikt u op **Selecteer**.
11. Vergelijkbaar met stap 6, klikt u op **Selecteer machtigingen**, selecteer **lezen**, **schrijven**, en **Execute**, toevoegen aan **deze map**, en toevoegen als **vermelding van een toegangsmachtiging en een standaardmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.

Uw VM MSI kunnen alle bewerkingen op bestanden nu uitvoeren in de map die u hebt gemaakt.  Voor meer informatie over het beheren van toegang tot Data Lake Store, in dit artikel leest u over [Access Control in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Een toegangstoken met behulp van de VM-MSI en deze gebruiken om aan te roepen van het Azure Data Lake Store-bestandssysteem

Azure Data Lake Store systeemeigen ondersteunt Azure AD-verificatie, zodat deze direct access tokens die zijn verkregen met behulp van MSI.  Om te verifiëren voor het verzenden van een toegangstoken dat is uitgegeven door Azure AD aan uw Data Lake Store-bestandssysteem-eindpunt, in een autorisatie-header in de indeling 'Bearer < ACCESS_TOKEN_VALUE >' Data Lake Store-bestandssysteem.  Lees voor meer informatie over Data Lake Store ondersteuning voor Azure AD-verificatie, [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

> [!NOTE]
> De Data Lake Store-bestandssysteemclient SDK's bieden nog geen ondersteuning voor beheerde Service-identiteit.  In deze zelfstudie wordt bijgewerkt wanneer ondersteuning wordt toegevoegd aan de SDK.

In deze zelfstudie gaat verifiëren u naar het Data Lake Store-bestandssysteem-REST-API met behulp van PowerShell om REST-aanvragen. Als u wilt de VM-MSI gebruiken voor verificatie, moet u de aanvragen van de virtuele machine maken.

1. Navigeer in de portal naar **virtuele Machines**, gaat u naar uw Windows-VM en in de **overzicht** klikt u op **Connect**.
2. Geef in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd bij het maken van de Windows-VM. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie. 
4. Met behulp van PowerShell `Invoke-WebRequest`, het lokale eindpunt van de MSI-bestand op een toegangstoken ophalen voor Azure Data Lake Store te vragen.  De resource-id voor Data Lake Store is 'https://datalake.azure.net/'.  Data Lake biedt een exacte overeenkomst in de resource-id en de afsluitende schuine streep is belangrijk.

   ```powershell
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://datalake.azure.net/"} -Headers @{Metadata="true"}
   ```
    
   Het antwoord van een JSON-object converteren naar een PowerShell-object. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Haal het toegangstoken op uit het antwoord.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. Met behulp van PowerShell 'Invoke-WebRequest', een aanvraag indienen met uw Data Lake Store REST-eindpunt om de mappen in de hoofdmap weer te geven.  Dit is een eenvoudige manier om te controleren of dat alles juist is geconfigureerd.  Het is belangrijk dat de tekenreeks "Bearer" in de autorisatie-header is een hoofdletter "B".  U vindt de naam van uw Data Lake Store in de **overzicht** sectie van de Data Lake Store-blade in Azure portal.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Een geslaagde respons ziet eruit zoals:

   ```powershell
   StatusCode        : 200
   StatusDescription : OK
   Content           : {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY", "blockSize":0,"accessTime":1507934941392, "modificationTime":1507944835699,"replication":0, "permission":"770","ow..."
   RawContent        : HTTP/1.1 200 OK
                       Pragma: no-cache
                       x-ms-request-id: b4b31e16-e968-46a1-879a-3474aa7d4528
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict-Transport-Security: ma...
   Forms             : {}
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b4b31e16-e968-46a1-879a-3474aa7d4528],
                       [x-ms-webhdfs-version, 17.04.22.00], [Status, 0x0]...}
   Images            : {}
   InputFields       : {}
   Links             : {}
   ParsedHtml        : System.__ComObject
   RawContentLength  : 556
   ```

6. Nu kunt u proberen een bestand uploaden naar uw Data Lake Store.  Maak eerst een bestand te uploaden.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Met behulp van PowerShell `Invoke-WebRequest`, te vragen van uw Data Lake Store REST-eindpunt voor het uploaden van het bestand naar de map die u eerder hebt gemaakt.  Deze aanvraag bestaat uit twee stappen.  In de eerste stap hebt u een aanvraag indienen en ophalen van een omleiding naar waar het bestand moet worden geüpload.  In de tweede stap, moet u daadwerkelijk het bestand uploaden.  Vergeet niet de naam van de map en bestand op de juiste wijze als u andere waarden dan in deze zelfstudie gebruikt. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Als u de waarde van inspecteren `$HdfsRedirectResponse` deze ziet er als het volgende antwoord:

   ```powershell
   PS C:\> $HdfsRedirectResponse

   StatusCode        : 307
   StatusDescription : Temporary Redirect
   Content           : {}
   RawContent        : HTTP/1.1 307 Temporary Redirect
                       Pragma: no-cache
                       x-ms-request-id: b7ab492f-b514-4483-aada-4aa0611d12b3
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosn...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b7ab492f-b514-4483-aada-4aa0611d12b3], 
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

   Het uploaden van een aanvraag verzenden naar het eindpunt van de omleidings-voltooien:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Een geslaagde respons eruit:

   ```powershell
   StatusCode        : 201
   StatusDescription : Created
   Content           : {}
   RawContent        : HTTP/1.1 201 Created
                       Pragma: no-cache
                       x-ms-request-id: 1e70f36f-ead1-4566-acfa-d0c3ec1e2307
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, 1e70f36f-ead1-4566-acfa-d0c3ec1e2307],
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

Andere Data Lake Store-bestandssysteem met de API's die u kunt toevoegen aan bestanden downloaden en meer.

Gefeliciteerd!  U hebt geverifieerd voor de Data Lake Store-bestandssysteem met behulp van een VM MSI.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Bewerkingen Data Lake Store gebruikt voor het beheer van Azure Resource Manager.  Lees voor meer informatie over het gebruik van een VM MSI om te verifiëren naar Resource Manager [gebruiken van een Linux VM Managed Service Identity (MSI) voor toegang tot Resource Manager](../managed-service-identity/msi-tutorial-linux-vm-access-arm.md).
- Meer informatie over [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Meer informatie over [bestandssysteembewerkingen in Azure Data Lake Store met REST-API](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) of de [WebHDFS FileSystem APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Meer informatie over [Access Control in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.