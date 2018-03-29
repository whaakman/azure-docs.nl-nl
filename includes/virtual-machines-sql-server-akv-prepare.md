## <a name="prepare-for-akv-integration"></a>Voorbereiden voor Azure Sleutelkluis-integratie
Voor het gebruik van Azure Sleutelkluis-integratie voor het configureren van uw SQL Server VM, zijn er verschillende vereisten: 

1. [Azure Powershell installeren](#install-azure-powershell)
2. [Maak een Azure Active Directory](#create-an-azure-active-directory)
3. [Een sleutelkluis maken](#create-a-key-vault)

De volgende secties worden deze vereisten en de informatie die u moet verzamelen om uit te voeren later de PowerShell-cmdlets.

### <a name="install-azure-powershell"></a>Azure PowerShell installeren
Zorg ervoor dat u de nieuwste Azure PowerShell SDK hebt geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor meer informatie.

### <a name="register-an-application-in-your-azure-active-directory"></a>Een toepassing registreren in uw Azure Active Directory
Eerst moet u hebt een [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) in uw abonnement. Tussen veel voordelen kunt hiermee u een leesmachtiging voor uw sleutelkluis voor bepaalde gebruikers en toepassingen.

Vervolgens moet u een toepassing registreren met AAD. Hierdoor krijgt u een Service-Principal-account dat toegang heeft tot de sleutelkluis die uw virtuele machine nodig. In het artikel Azure Key Vault vindt u deze stappen in de [een toepassing registreren met Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) sectie, of u kunt zien welke stappen met schermopnamen in de **een identiteit voor de sectie van de toepassing ophalen**  van [dit blogbericht](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Voordat u deze stappen uitvoert, houd er rekening mee dat u verzamelen van de volgende informatie tijdens deze registratie die later nodig moet wanneer u Azure Sleutelkluis-integratie op uw SQL-VM inschakelt.

* Nadat de toepassing is toegevoegd, vinden de **toepassings-ID** op de **geregistreerde app** blade.   
    De toepassings-ID is toegewezen, later naar de **$spName** parameter (Service Principal name) in de PowerShell-script voor het inschakelen van Azure Sleutelkluis-integratie. 
* Tijdens deze stappen wanneer u uw sleutel maakt, Kopieer het geheim voor uw sleutel wordt weergegeven in de volgende schermafbeelding. Deze sleutel geheim krijgt later naar de **$spSecret** (Service Principal-geheim)-parameter in het PowerShell-script.  

* De toepassings-ID en het geheim wordt ook gebruikt voor het maken van een referentie in SQL Server. 

* U moet deze nieuwe client-ID hebben de volgende machtigingen toestaan: **versleutelen**, **ontsleutelen**, **wrapKey**, **unwrapKey**, **aanmelding**, en **controleren**. Dit wordt gedaan met de [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) cmdlet. Zie voor meer informatie [autoriseren van de toepassing voor het gebruik van de sleutel of geheim](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Een sleutelkluis maken
Als u wilt gebruiken Azure Key Vault voor het opslaan van de sleutels die u voor versleuteling in uw virtuele machine gebruikt, moet u toegang tot een sleutelkluis. Als u de sleutelkluis al geen hebt ingesteld, maken door de stappen in de [aan de slag met Azure Key Vault](../articles/key-vault/key-vault-get-started.md) onderwerp. Voordat u deze stappen uitvoert, houd er rekening mee dat er bepaalde informatie die u moet verzamelen tijdens deze instellen hoger is vereist wanneer u Azure Sleutelkluis-integratie op de SQL-VM inschakelen.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Als u de maken een stap sleutelkluis, Let op de geretourneerde **vaultUri** eigenschap, die de sleutelkluis-URL. In het voorbeeld in deze stap hieronder aangegeven, de naam van de sleutelkluis is ContosoKeyVault, dus de sleutelkluis URL zou zijn https://contosokeyvault.vault.azure.net/.

De sleutelkluis-URL is toegewezen, later naar de **$akvURL** parameter in de PowerShell-script voor het inschakelen van Azure Sleutelkluis-integratie.

Nadat u de sleutelkluis hebt gemaakt, moeten we een sleutel toevoegen aan de sleutelkluis, kunnen deze sleutel, zoals wanneer maken we een asymmetrische sleutel later in SQL Server maken.
