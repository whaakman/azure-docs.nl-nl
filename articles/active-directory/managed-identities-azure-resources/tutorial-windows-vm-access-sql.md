---
title: een door het Windows-VM-systeem toegewezen beheerde identiteit gebruiken voor toegang tot Azure SQL
description: Een zelfstudie die u helpt bij het doorlopen van het proces voor het gebruiken van een door het Windows-VM-systeem toegewezen beheerde identiteit om toegang te krijgen tot Azure SQL.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: bryanla
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6d5452f23e830ca7a9ffe5ca5ed3d4aa12fb717
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66236043"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-sql"></a>Zelfstudie: een door het Windows-VM-systeem toegewezen beheerde identiteit gebruiken voor toegang tot Azure SQL

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u toegang krijgt tot een Azure SQL-server met een door het systeem toegewezen identiteit voor een virtuele Windows-machine (VM). Managed Service Identity's worden automatisch beheerd in Azure en stellen u in staat om te verifiëren bij services die Azure AD-verificatie ondersteunen, zonder referenties in code te hoeven invoegen. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Uw virtuele machine toegang verlenen tot een Azure SQL-server
> * Azure AD-verificatie inschakelen voor de SQL-server
> * Een ingesloten gebruiker maken in de database die staat voor de door de systeem toegewezen id van de VM
> * Een toegangstoken ophalen met de identiteit van de virtuele machine en daarmee een query uitvoeren op een Azure SQL-server

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Uw virtuele machine toegang verlenen tot een database op een Azure SQL-server

Als u uw virtuele machine toegang wilt verlenen tot een database in een Azure SQL-server, kunt u een bestaande SQL-server gebruiken of een nieuwe server maken. Voor het maken van een nieuwe server en database met behulp van Azure Portal, volgt u deze [Azure SQL-snelstart](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). Er zijn ook snelstarts in de [documentatie over Azure SQL](https://docs.microsoft.com/azure/sql-database/) voor het gebruik van Azure CLI en Azure Powershell.

U moet twee stappen uitvoeren om uw virtuele machine toegang te verlenen tot een database:

1. Schakel Azure AD-verificatie in voor de SQL-server.
2. Maak een **ingesloten gebruiker** in de database die staat voor de door de systeem toegewezen id van de VM.

## <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Azure AD-verificatie inschakelen voor de SQL-server

[Azure AD-verificatie voor de SQL-server configureren](/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen:

1.  Selecteer **SQL-servers** in het linkernavigatievenster van Azure Portal.
2.  Klik op de SQL-server waarvoor Azure AD-verificatie moet worden ingeschakeld.
3.  Klik in de sectie **Instellingen** van de blade op **Active Directory-beheerder**.
4.  Klik in de opdrachtbalk op **Beheerder instellen**.
5.  Selecteer een Azure AD-gebruikersaccount dat beheerder van de server moet worden gemaakt en klik op **Selecteren**.
6.  Klik in de opdrachtbalk op **Opslaan**.

## <a name="create-a-contained-user-in-the-database-that-represents-the-vms-system-assigned-identity"></a>Een ingesloten gebruiker maken in de database die staat voor de door de systeem toegewezen id van de VM

Voor de volgende stap hebt u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) nodig. Voordat u begint, kan het ook handig zijn de volgende artikelen te lezen voor meer achtergrondinformatie over Azure AD-integratie:

- [Universele verificatie met SQL Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Azure Active Directory-verificatie configureren en beheren met SQL Database of SQL Data Warehouse](/azure/sql-database/sql-database-aad-authentication-configure)

Voor SQL DB zijn unieke AAD-weergavenamen vereist. Hiermee moeten de AAD-accounts, zoals gebruikers, groepen en service-principals (toepassingen), en VM-namen die voor een beheerde identiteit zijn ingeschakeld, voor wat betreft de weergavenamen uniek zijn gedefinieerd in AAD. SQL-database controleert de weergavenaam van de AAD tijdens het maken van dergelijke gebruikers T-SQL en als deze niet uniek is, mislukt de opdracht vraagt om een unieke AAD-weergavenaam voor een bepaald account invoeren.

1. Start SQL Server Management Studio.
2. Voer in het dialoogvenster **Verbinding maken met server** de naam van de SQL-server in het veld **Servernaam** in.
3. Selecteer in het veld **Verificatie** **Active Directory - Universeel met ondersteuning voor MFA**.
4. Voer in het veld **Gebruikersnaam** de naam in van het Azure AD-account dat u hebt ingesteld als de beheerder van de server, bijvoorbeeld helen@woodgroveonline.com
5. Klik op **Opties**.
6. Voer in het veld **Verbinding maken met database** de naam in van de niet-systeemdatabase die u wilt configureren.
7. Klik op **Verbinden**. Voltooi het aanmeldingsproces.
8. Vouw in **Objectverkenner** de map **Databases** uit.
9. Klik met de rechtermuisknop op een gebruikersdatabase en klik op **Nieuwe query**.
10. Voer in het queryvenster de volgende regel in en klik op **Uitvoeren** in de werkbalk:

    > [!NOTE]
    > `VMName` in de volgende opdracht staat de naam van de VM waarvoor u door het systeem toegewezen id's hebt ingeschakeld in het gedeelte met vereisten.
    
    ```
    CREATE USER [VMName] FROM EXTERNAL PROVIDER
    ```
    
    De opdracht voor het maken van de ingesloten gebruiker voor de door het systeem toegewezen id voor de VM wordt uitgevoerd.
11. Wis het queryvenster, voer de volgende regel in en klik op **Uitvoeren** in de werkbalk:

    > [!NOTE]
    > `VMName` in de volgende opdracht staat de naam van de VM waarvoor u door het systeem toegewezen id's hebt ingeschakeld in het gedeelte met vereisten.
    
    ```
    ALTER ROLE db_datareader ADD MEMBER [VMName]
    ```

    De opdracht wordt uitgevoerd en de ingesloten gebruiker heeft nu leestoegang tot de gehele database.

Code die wordt uitgevoerd op de VM kan nu een token verkrijgen via de door het systeem toegewezen beheerde identiteit en het token gebruiken voor verificatie bij de SQL-server.

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-sql"></a>Een toegangstoken ophalen met behulp van de door het systeem toegewezen beheerde identiteit van de VM en dit gebruiken om Azure SQL aan te roepen

Azure SQL biedt systeemeigen ondersteuning voor Azure AD-verificatie, zodat toegangstokens die zijn verkregen met behulp van beheerde identiteiten voor Azure-resources direct kunnen worden geaccepteerd. U gebruikt de toegangsmethode met het **toegangstoken** voor het maken van een verbinding met SQL. Dit maakt deel uit van de integratie van Azure SQL met Azure AD en wijkt af van het opgeven van referenties in de verbindingsreeks.

Hier volgt een voorbeeld van .NET-code van het openen van een verbinding met SQL met behulp van een toegangstoken. Deze code moet worden uitgevoerd op de virtuele machine om toegang te krijgen tot het eindpunt van de door het systeem toegewezen beheerde identiteit van de virtuele machine. **.NET framework 4.6** of hoger is vereist voor het gebruik van de methode van de access-token. Vervang AZURE-SQL-SERVERNAME en DATABASE door de benodigde waarden. Houd er rekening mee de resource-ID voor Azure SQL is `https://database.windows.net/`.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call managed identities for Azure resources endpoint.
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

Met PowerShell kunt u snel de end-to-end-instellingen testen zonder een app te hoeven schrijven en implementeren op de virtuele machine.

1.  Navigeer in Azure Portal naar **Virtuele machines**, ga naar uw virtuele Windows-machine en klik op de pagina **Overzicht** op **Verbinden**.
2.  Voer uw referenties (**gebruikersnaam** en **wachtwoord**) in die u hebt toegevoegd bij het maken van de virtuele Windows-machine.
3.  Nu u een **Verbinding met extern bureaublad** met de virtuele machine hebt gemaakt, opent u **PowerShell** in de externe sessie.
4.  Verstuur met de cmdlet `Invoke-WebRequest` van Powershell een aanvraag naar het lokale eindpunt van de beheerde identiteit om een toegangstoken voor Azure SQL op te halen.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Converteer de reactie van een JSON-object naar een PowerShell-object. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Extraheer het toegangstoken uit de reactie.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5. Open een verbinding met de SQL-server. Vervang de waarden voor AZURE-SQL-SERVERNAME en DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Maak vervolgens een query en verzend de query naar de server. Vergeet niet de waarde voor TABLE te vervangen.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Bekijk de waarde van `$DataSet.Tables[0]` om de resultaten van de query weer te geven.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd om met een door het systeem toegewezen beheerde identiteit toegang te krijgen tot Azure SQL Server. Zie voor meer informatie over Azure SQL Server:

> [!div class="nextstepaction"]
> [Azure SQL Database Service](/azure/sql-database/sql-database-technical-overview)
