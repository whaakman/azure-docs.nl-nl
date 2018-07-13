---
title: Een Windows VM-MSI gebruiken voor toegang tot Azure SQL
description: Een zelfstudie die u helpt bij het proces van het gebruik van een Windows VM Managed Service Identity (MSI) voor toegang tot Azure SQL.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5c3e2af8c6864204a4c373ac4e1c12090389ac32
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007423"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Een Windows VM Managed Service Identity (MSI) gebruiken voor toegang tot Azure SQL

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u een Managed Service Identity (MSI) voor een Windows virtuele machine (VM) gebruikt voor toegang tot een Azure SQL-server. Beheerde Service-identiteiten automatisch worden beheerd door Azure en kunt u voor de verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Inschakelen van MSI-bestand op een Windows VM 
> * Uw virtuele machine toegang verlenen tot een Azure SQL-server
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en Hiermee query uitvoeren op een Azure SQL-server

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows-VM.  U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1.  Klik op **een resource maken** in de linkerbovenhoek van Azure portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakt als volgt de referenties die u gebruiken voor aanmelding bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** waarin u kunt uw virtuele machine maakt, kiest **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen 

Een VM MSI kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Inschakelen van MSI weet Azure te maken van een beheerde identiteit voor uw virtuele machine. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en Hiermee MSI in Azure Resource Manager.

1.  Selecteer de **virtuele Machine** dat u inschakelen van MSI-bestand wilt op.  
2.  Klik op de linker navigatiebalk op **configuratie**. 
3.  U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4.  Controleer of u klikken op **opslaan** aan de configuratie op te slaan.  
    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Uw virtuele machine toegang verlenen tot een database in een Azure SQL-server

Nu kunt u uw virtuele machine toegang verlenen tot een database in een Azure SQL-server.  Voor deze stap kunt u een bestaande SQL server gebruiken of een nieuwe maken.  Voor het maken van een nieuwe server en database met behulp van de Azure-portal, volgt u deze [Azure SQL-snelstartgids](~/articles/sql-database/sql-database-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie voor Azure SQL](~/articles/sql-database/index.yml).

Er zijn drie stappen voor het verlenen van uw VM-toegang tot een database:
1.  Een groep maken in Azure AD en de VM-MSI een lid van de groep te maken.
2.  Azure AD-verificatie inschakelen voor de SQL-server.
3.  Maak een **contained-gebruiker** in de database die staat voor de Azure AD-groep.

> [!NOTE]
> Doorgaans maakt u een contained-gebruiker die direct aan de VM MSI-bestand.  Azure SQL is momenteel niet toegestaan voor de Azure AD Service-Principal die staat voor de VM-MSI worden toegewezen aan een contained-gebruiker.  Als een ondersteunde oplossing, kunt u de VM-MSI een lid van een Azure AD-groep maken en vervolgens een contained-gebruiker maken in de database die staat voor de groep.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Een groep maken in Azure AD en de VM-MSI een lid van de groep maken

U kunt een bestaande Azure AD-groep of maak een nieuwe Azure AD PowerShell gebruiken.  

Installeer eerst de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u vervolgens met behulp van `Connect-AzureAD`, en voer de volgende opdracht uit om de groep te maken en opslaan in een variabele:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

De uitvoer lijkt op het volgende voorbeeld, waarin ook de waarde van de variabele controleert:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Voeg vervolgens van de VM-MSI-bestand toe aan de groep.  U moet het MSI **ObjectId**, die u kunt ophalen met behulp van Azure PowerShell.  Download eerst [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Meld u vervolgens met behulp van `Connect-AzureRmAccount`, en voer de volgende opdrachten uit:
- Controleer of dat de sessiecontext van uw is ingesteld op de gewenste Azure-abonnement, hebt u meerdere waarden.
- Lijst van de beschikbare resources in uw Azure-abonnement, controleert u in of de juiste resourcegroep en VM-namen.
- Hiermee worden de eigenschappen van de MSI VM, met behulp van de juiste waarden voor `<RESOURCE-GROUP>` en `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

De uitvoer lijkt op het volgende voorbeeld, waarin ook de service principal-Object-ID van de VM MSI onderzoekt:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Nu de VM-MSI toevoegen aan de groep.  U kunt alleen een service-principal toevoegen aan een groep met behulp van Azure AD PowerShell.  Voer deze opdracht uit:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Als u ook het groepslidmaatschap later, de uitvoer ziet er als volgt uit:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Azure AD-verificatie voor de SQL server inschakelen

Nu dat u hebt gemaakt van de groep en de VM-MSI toegevoegd aan het lidmaatschap, kunt u [Azure AD-verificatie voor de SQL server configureren](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) met behulp van de volgende stappen uit:

1.  Selecteer in de Azure portal, **SQL-servers** in de navigatie aan de linkerkant.
2.  Klik op de SQL-server worden ingeschakeld voor Azure AD-verificatie.
3.  In de **instellingen** sectie van de blade, klikt u op **Active Directory-beheerder**.
4.  Klik in de opdrachtbalk op **beheerder instellen**.
5.  Selecteer een Azure AD-gebruikersaccount voor een beheerder van de server worden gemaakt en klikt u op **selecteren.**
6.  Klik in de opdrachtbalk op **opslaan.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Een contained-gebruiker maken in de database die staat voor de Azure AD-groep

Voor deze stap moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Voordat u begint, kan het ook handig om te controleren van de volgende artikelen voor achtergrondinformatie over Azure AD-integratie zijn:

- [Universele authenticatie met SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  Start SQL Server Management Studio.
2.  In de **verbinding maken met Server** dialoogvenster, voer de naam van uw SQL-server in de **servernaam** veld.
3.  In de **verificatie** veld **Active Directory - Universal met ondersteuning voor MFA**.
4.  In de **gebruikersnaam** en voer de naam van de Azure AD-account die u hebt ingesteld als de beheerder van de server, bijvoorbeeld: helen@woodgroveonline.com
5.  Klik op **Opties**.
6.  In de **verbinding maken met database** en voer de naam van de niet-systeem-database die u wilt configureren.
7.  Klik op **Verbinden**.  Voltooi de aanmelding.
8.  In de **Objectverkenner**, vouw de **Databases** map.
9.  Met de rechtermuisknop op een gebruikersdatabase en klikt u op **nieuwe query**.
10.  Voer in het queryvenster de volgende regel en klikt u op **Execute** in de werkbalk:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     De opdracht voltooid is, het maken van de ingesloten gebruiker voor de groep.
11.  Schakelt u het queryvenster, voer de volgende regel en klikt u op **Execute** in de werkbalk:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     De opdracht voltooid is, de ingesloten gebruikers de mogelijkheid om te lezen van de gehele database verlenen.

Code die wordt uitgevoerd in de virtuele machine kunt nu een token verkrijgen van MSI-bestand en het token gebruikt voor verificatie bij de SQL-server.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Een toegangstoken met behulp van de identiteit van de virtuele machine en gebruiken voor het aanroepen van Azure SQL 

Azure SQL systeemeigen ondersteunt Azure AD-verificatie, zodat deze direct access tokens die zijn verkregen met behulp van MSI.  U gebruikt de **toegangstoken** methode voor het maken van een verbinding met SQL.  Dit maakt deel uit van de Azure SQL-integratie met Azure AD en wijkt af van het opgeven van referenties voor de verbindingsreeks.

Hier volgt een voorbeeld van .net-code van het openen van een verbinding met SQL met behulp van een toegangstoken.  Deze code moet worden uitgevoerd op de virtuele machine kunnen toegang hebben tot de VM MSI-eindpunt.  **.NET framework 4.6** of hoger is vereist voor het gebruik van de methode van de access-token.  Vervang de waarden van AZURE-SQL-servernaam en de DATABASE dienovereenkomstig.  Houd er rekening mee de resource-ID voor Azure SQL-is 'https://database.windows.net/'.

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

U kunt ook een snelle manier om het testen van de end-to-setup zonder te hoeven schrijven en een app implementeren op de virtuele machine wordt met behulp van PowerShell.

1.  Navigeer in de portal naar **virtuele Machines** en gaat u naar uw Windows-machine en klik in de **overzicht**, klikt u op **Connect**. 
2.  Geef in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd bij het maken van de Windows-VM. 
3.  Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie. 
4.  Met behulp van PowerShell `Invoke-WebRequest`, het lokale eindpunt van de MSI-bestand op een toegangstoken ophalen voor Azure SQL te vragen.

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

5.  Open een verbinding met de SQL-server. Vergeet niet om u te vervangen door de waarden voor AZURE-SQL-servernaam en -DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Vervolgens maken en verzenden van een query naar de server.  Vergeet niet om u te vervangen door de waarde voor de tabel.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Bekijk de waarde van `$DataSet.Tables[0]` om de resultaten van de query weer te geven.  Gefeliciteerd, u hebt query uitgevoerd op de database met behulp van een VM MSI en zonder dat u hoeft op te geven referenties.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Meer informatie over [Azure SQL-ondersteuning voor Azure AD-verificatie](~/articles/sql-database/sql-database-aad-authentication.md).
- Meer informatie over [configureren van Azure SQL-ondersteuning voor Azure AD-verificatie](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Meer informatie over [verificatie en toegangsbeheer in SQL server](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.
