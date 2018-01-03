---
title: Gebruik van een Windows-VM-MSI voor toegang tot Azure SQL
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Windows VM beheerde Service identiteit (MSI) voor toegang tot Azure SQL.
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a3fa96b2c709b2b585519fa02a4e105010797947
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Een Windows VM beheerde Service identiteit (MSI) gebruiken voor toegang tot Azure SQL

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u een beheerde Service identiteit (MSI) voor een Windows virtuele machine (VM) gebruiken voor toegang tot een Azure SQL-server. Beheerde Service-identiteiten worden automatisch beheerd door Azure en u te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. Procedures voor:

> [!div class="checklist"]
> * Inschakelen van MSI op een Windows VM 
> * Uw VM toegang verlenen tot een Azure SQL-server
> * Ophalen van een toegangstoken met behulp van de identiteit van de VM en deze query uitvoeren op een Azure SQL-server gebruiken

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows VM.  U kunt ook MSI op een bestaande virtuele machine inschakelen.

1.  Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** waarin u uw virtuele machine maakt, kiest u **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen 

Een VM MSI kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Inschakelen van MSI vertelt Azure maken van een beheerde identiteit voor uw virtuele machine. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI in Azure Resource Manager.

1.  Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.  
2.  Klik op de linkernavigatiebalk **configuratie**. 
3.  U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4.  Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.  
    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als MSI is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Uw VM toegang verlenen tot een database in een Azure SQL-server

U kunt nu uw VM-toegang tot een database in een Azure SQL-server verlenen.  Voor deze stap kunt u een bestaande SQL server gebruiken of een nieuwe maken.  Voor het maken van een nieuwe server en database met de Azure-portal, voert u [Azure SQL-Quick Start](~/articles/sql-database/sql-database-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie van Azure SQL](~/articles/sql-database/index.yml).

Er zijn drie stappen voor uw virtuele machine geen toegang verlenen tot een database:
1.  Een groep maken in Azure AD en de VM MSI lid maken van de groep.
2.  Azure AD-verificatie inschakelen voor de SQL-server.
3.  Maak een **contained-gebruiker** in de database met de Azure AD-groep.

> [!NOTE]
> Doorgaans maakt u een contained-gebruiker die is toegewezen aan de VM-MSI rechtstreeks.  Azure SQL is momenteel niet toegestaan voor de Azure AD Service-Principal die staat voor de VM MSI moeten worden toegewezen aan een contained-gebruiker.  Als een ondersteunde oplossing kunt u de VM MSI lid maken van een Azure AD-groep en vervolgens een contained-gebruiker maken in de database die de groep vertegenwoordigt.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Maken van een groep in Azure AD en de VM MSI een lid van de groep

U kunt een bestaande Azure AD-groep of maak een nieuwe Azure AD PowerShell gebruiken.  

Installeer eerst de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u aan met `Connect-AzureAD`, en voer de volgende opdracht om de groep te maken en opslaan in een variabele:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

De uitvoer ziet er als volgt, die ook de waarde van de variabele moet worden gecontroleerd:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Voeg vervolgens de MSI van de VM aan de groep.  U moet het MSI **ObjectId**, die u kunt ophalen met behulp van Azure PowerShell.  Eerst downloaden [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Meld u aan met `Login-AzureRmAccount`, en voer de volgende opdrachten:
- Zorg ervoor dat de sessiecontext van uw is ingesteld op het gewenste Azure-abonnement, als er meerdere waarden.
- Lijst van de beschikbare bronnen in uw Azure-abonnement, controleert u in de juiste resourcegroep en VM-namen.
- Ophalen van de MSI VM-eigenschappen met behulp van de juiste waarden voor `<RESOURCE-GROUP>` en `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

De uitvoer ziet er als volgt, die ook de service principal-Object-ID van de MSI van de VM moet worden gecontroleerd:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Nu de VM MSI toevoegen aan de groep.  U kunt alleen een service-principal toevoegen aan een groep met Azure AD PowerShell.  Voer deze opdracht uit:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Als u ook het groepslidmaatschap later bekijkt, lijkt de uitvoer als volgt:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Azure AD-verificatie voor de SQL server inschakelen

Nu dat u hebt gemaakt van de groep en de VM MSI toegevoegd aan het lidmaatschap, kunt u [Azure AD authentication configureren voor de SQL server](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) met behulp van de volgende stappen uit:

1.  Selecteer in de Azure-portal **SQL-servers** van de linkernavigatiebalk.
2.  Klik op de SQL-server worden ingeschakeld voor Azure AD-verificatie.
3.  In de **instellingen** sectie van de blade, klikt u op **Active Directory-beheerder**.
4.  Klik in de opdrachtbalk op **beheerder instellen**.
5.  Selecteer een Azure AD-gebruikersaccount voor een beheerder van de server worden uitgevoerd en klikt u op **selecteren.**
6.  Klik in de opdrachtbalk op **opslaan.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Maak een contained-gebruiker in de database met de Azure AD-groep

Voor deze stap, moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Voordat u begint mogelijk ook handig om te controleren van de volgende artikelen voor achtergrondinformatie over Azure AD-integratie:

- [Universele verificatie met SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  Start SQL Server Management Studio.
2.  In de **verbinding maken met Server** dialoogvenster, Voer uw SQL server-naam in de **servernaam** veld.
3.  In de **verificatie** optie **Active Directory - Universal met ondersteuning voor MFA**.
4.  In de **gebruikersnaam** en voer de naam van de Azure AD-account die u hebt ingesteld als de beheerder van de server, bijvoorbeeld:helen@woodgroveonline.com
5.  Klik op **Opties**.
6.  In de **verbinding maken met database** en voer de naam van de systeembestanden-database die u wilt configureren.
7.  Klik op **Verbinden**.  Voltooi de aanmelding.
8.  In de **Objectverkenner**, vouw de **Databases** map.
9.  Met de rechtermuisknop op een gebruikersdatabase en klik op **nieuwe query**.
10.  Voer de volgende regel in het queryvenster en op **Execute** op de werkbalk:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     De opdracht is voltooid, de gebruiker van de opgenomen voor de groep maken.
11.  Schakel het query-venster, voer de volgende regel en klikt u op **Execute** op de werkbalk:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     De opdracht is voltooid, de contained-gebruiker de mogelijkheid om te lezen van de gehele database verlenen.

Code die wordt uitgevoerd in de virtuele machine kunt nu een token ophalen uit MSI en gebruiken van het token om te verifiëren met de SQL server.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Ophalen van een toegangstoken met behulp van de identiteit van de VM en deze gebruiken om aan te roepen Azure SQL 

Azure SQL systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren verkregen met behulp van MSI.  U gebruikt de **toegangstoken** methode voor het maken van een verbinding met SQL.  Dit maakt deel uit van de Azure SQL-integratie met Azure AD en verschilt van het verstrekken van referenties voor de verbindingsreeks.

Hier volgt een voorbeeld van een .net-code van het openen van een verbinding met SQL met behulp van een toegangstoken.  Deze code moet uitvoeren op de virtuele machine worden toegang kunnen krijgen tot het eindpunt VM MSI.  **.NET framework 4.6** of hoger is vereist om de toegang tot token methode te gebruiken.  Vervang de waarden van AZURE-SQL-servernaam en DATABASE dienovereenkomstig.  Opmerking: de resource-ID voor Azure SQL-is 'https://database.windows.net/'.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call MSI endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

U kunt ook een snelle manier om de end-to-end-installatie testen zonder om te schrijven en implementeren van een app op de virtuele machine wordt met behulp van PowerShell.

1.  Navigeer in de portal naar **virtuele Machines** en gaat u naar uw Windows-machine en in de **overzicht**, klikt u op **Connect**. 
2.  Voer in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd tijdens het maken van de virtuele machine van Windows. 
3.  Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie. 
4.  Met behulp van PowerShell `Invoke-WebRequest`, maak een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure SQL.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
    ```
    
    Het antwoord van een JSON-object converteren naar een PowerShell-object. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Haal het toegangstoken op uit het antwoord.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Open een verbinding met de SQL-server. Vergeet niet ter vervanging van de waarden voor AZURE-SQL-servernaam en -DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Vervolgens maken en verzenden van een query naar de server.  Vergeet niet de waarde voor de tabel te vervangen.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Bekijk de waarde van `$DataSet.Tables[0]` de resultaten van de query wilt weergeven.  Gefeliciteerd, u de database met een VM MSI hebt opgevraagd en zonder op te geven referenties!

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Meer informatie over [Azure SQL-ondersteuning voor verificatie met Azure AD](~/articles/sql-database/sql-database-aad-authentication.md).
- Meer informatie over [Azure SQL-ondersteuning voor verificatie met Azure AD configureren](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Meer informatie over [verificatie en toegang in SQL server](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
