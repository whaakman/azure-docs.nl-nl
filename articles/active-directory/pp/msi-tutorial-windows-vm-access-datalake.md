---
title: Het gebruik van een Windows VM beheerde Service identiteit (MSI) voor toegang tot Azure Data Lake Store
description: Een zelfstudie waarin wordt getoond hoe u een Windows VM beheerde Service identiteit (MSI) wordt gebruikt voor toegang tot Azure Data Lake Store.
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 01cb63e3b56a0e629c0dee92723db19f5e3dc3c1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Een Windows VM beheerde Service identiteit (MSI) gebruiken voor toegang tot Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u op een beheerde Service identiteit (MSI) voor een Windows virtuele machine (VM) voor toegang tot een Azure Data Lake Store. Beheerde Service-identiteiten worden automatisch beheerd door Azure en u te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. Procedures voor:

> [!div class="checklist"]
> * Inschakelen van MSI op een Windows VM 
> * Uw VM-toegang verlenen aan een Azure Data Lake Store
> * Ophalen van een toegangstoken met behulp van de identiteit van de VM en deze gebruiken voor toegang tot een Azure Data Lake Store

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows VM.  U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3. Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4. Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** waarin u uw virtuele machine maakt, kiest u **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen 

Een VM MSI kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Inschakelen van MSI vertelt Azure maken van een beheerde identiteit voor uw virtuele machine. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI in Azure Resource Manager.

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.  
2. Klik op de linkernavigatiebalk **configuratie**. 
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.  
   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als MSI is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw VM-toegang verlenen tot Azure Data Lake Store

U kunt nu uw VM-toegang tot bestanden en mappen in een Azure Data Lake Store verlenen.  U kunt voor deze stap maakt gebruik van een bestaande Data Lake Store of een nieuwe maken.  Voor het maken van een nieuwe Data Lake Store met de Azure portal, voert u [Azure Data Lake Store Quick Start](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-overview.md).

Maak een nieuwe map in uw Data Lake Store en uw VM MSI machtiging verlenen om te lezen, schrijven en uitvoeren van bestanden in de map:

1. Klik in de Azure-portal op **Data Lake Store** in de linkernavigatiebalk.
2. Klik op de Data Lake Store die u wilt gebruiken voor deze zelfstudie.
3. Klik op **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van de Data Lake Store is geselecteerd.  Klik op **toegang** in de opdrachtbalk.
5. Klik op **Add**.  In de **Selecteer** en voer de naam van uw virtuele machine, bijvoorbeeld **DevTestVM**.  Selecteer uw virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
6. Klik op **machtigingen selecteren**.  Selecteer **lezen** en **Execute**, toevoegen aan **deze map**, en toevoegen als **alleen een toegangsmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.
7. Sluit de **toegang** blade.
8. Maak een nieuwe map voor deze zelfstudie.  Klik op **nieuwe map** in de opdrachtbalk en verleen de nieuwe map een naam, bijvoorbeeld **TestFolder**.  Klik op **OK**.
9. Klik op de map die u hebt gemaakt en klik vervolgens op **toegang** in de opdrachtbalk.
10. Vergelijkbaar is met stap 5, klikt u op **toevoegen**, in de **Selecteer** veld Geef de naam van uw virtuele machine, selecteert u deze en klikt u op **Selecteer**.
11. Vergelijkbaar is met stap 6, klikt u op **Selecteer machtigingen**, selecteer **lezen**, **schrijven**, en **Execute**, toevoegen aan **deze map**, en toevoegen als **een machtigingsvermelding toegang en een standaard machtigingsvermelding**.  Klik op **OK**.  De machtiging moet worden toegevoegd.

Uw VM MSI kunnen alle bewerkingen op bestanden nu uitvoeren in de map die u hebt gemaakt.  Lees voor meer informatie over het beheren van toegang tot Data Lake Store in dit artikel op [toegangsbeheer in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Ophalen van een toegangstoken met behulp van de VM MSI en deze gebruiken om aan te roepen van het bestandssysteem van Azure Data Lake Store

Azure Data Lake Store systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren verkregen met behulp van MSI.  Om te verifiëren voor het verzenden van een toegangstoken dat is uitgegeven door Azure AD aan uw Data Lake Store filesystem-eindpunt in een autorisatie-header in de indeling 'Bearer < ACCESS_TOKEN_VALUE >' Data Lake Store-bestandssysteem.  Lees voor meer informatie over Data Lake Store-ondersteuning voor verificatie met Azure AD [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

> [!NOTE]
> De client-SDK's van de bestandssysteem in Data Lake Store-Service-identiteit beheerd niet ondersteunen.  Deze zelfstudie wordt bijgewerkt wanneer ondersteuning wordt toegevoegd aan de SDK.

In deze zelfstudie maakt verifiëren u voor het Data Lake Store-bestandssysteem REST-API met behulp van PowerShell om de REST-aanvragen. Voor het gebruik van de VM-MSI voor verificatie, moet u de aanvragen van de virtuele machine.

1. Navigeer in de portal naar **virtuele Machines**, gaat u op uw Windows-VM en in de **overzicht** klikt u op **Connect**.
2. Voer in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd tijdens het maken van de virtuele machine van Windows. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie. 
4. Met behulp van PowerShell `Invoke-WebRequest`, maak een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Data Lake Store.  De resource-id van Data Lake Store is 'https://datalake.azure.net/'.  Data Lake biedt een exacte overeenkomst op de bron-id en de afsluitende slash is belangrijk.

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

5. Maak met behulp van PowerShell 'Invoke-WebRequest', een aanvraag naar uw Data Lake Store REST-eindpunt voor het weergeven van de mappen in de hoofdmap.  Dit is een eenvoudige manier om dat alles juist is geconfigureerd.  Het is belangrijk dat de tekenreeks 'Bearer' in de autorisatie-header is een hoofdletter "B".  U vindt de naam van uw Data Lake Store in de **overzicht** sectie van de blade Data Lake Store in de Azure portal.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Een geslaagde reactie ziet eruit als:

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

6. Nu kunt u proberen een bestand te uploaden naar uw Data Lake Store.  Maak eerst een bestand te uploaden.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Met behulp van PowerShell `Invoke-WebRequest`, een aanvraag aanbrengen in uw Data Lake Store REST-eindpunt het bestand te uploaden naar de map die u eerder hebt gemaakt.  Deze aanvraag bestaat uit twee stappen.  In de eerste stap een indienen en ophalen van een omleiding naar waar het bestand moet worden geüpload.  In de tweede stap moet u daadwerkelijk het bestand uploaden.  Vergeet niet de naam van de map instellen en bestanden op de juiste wijze als u andere waarden dan in deze zelfstudie hebt gebruikt. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Als u de waarde van inspecteren `$HdfsRedirectResponse` het moet eruitzien als in het volgende antwoord:

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

   Het uploaden is voltooid door een aanvraag verzonden naar de omleidings-eindpunt:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Een geslaagde reactie eruit:

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

Met andere bestandssysteem Data Lake Store downloaden API's die u kunt toevoegen aan bestanden, bestanden en meer.

Gefeliciteerd!  U hebt geverifieerd voor het Data Lake Store-bestandssysteem met behulp van een VM MSI.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Bewerkingen Data Lake Store maakt gebruik van Azure Resource Manager voor beheer.  Lees voor meer informatie over het gebruik van een VM MSI om te verifiëren naar Resource Manager [een Linux VM beheerde Service identiteit (MSI) gebruiken voor toegang tot de Resource Manager](msi-tutorial-linux-vm-access-arm.md).
- Meer informatie over [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Meer informatie over [Bestandssysteembewerkingen op Azure Data Lake Store met REST-API](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) of de [WebHDFS FileSystem APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Meer informatie over [toegangsbeheer in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.