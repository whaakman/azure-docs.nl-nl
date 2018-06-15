## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Voorbereiden om Azure Resource Manager-aanvragen te verifiëren
U moet verifiëren dat de bewerkingen die u uitvoert op resources met behulp van de [Azure Resource Manager] [ lnk-authenticate-arm] met Azure Active Directory (AD). Er is de eenvoudigste manier om dit te configureren met PowerShell of Azure CLI.

Installeer de [Azure PowerShell-cmdlets] [ lnk-powershell-install] voordat u doorgaat.

De volgende stappen laten zien hoe wachtwoordverificatie voor een AD-toepassing met behulp van PowerShell instellen. U kunt deze opdrachten uitvoeren in een standaard PowerShell-sessie.

1. Aanmelden bij uw Azure-abonnement met de volgende opdracht:

    ```powershell
    Connect-AzureRmAccount
    ```

1. Als u meerdere Azure-abonnementen hebt, verleent aanmelden bij Azure u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht voor een lijst met de Azure-abonnementen beschikbaar moet worden gebruikt:

    ```powershell
    Get-AzureRMSubscription
    ```

    Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het beheren van uw IoT-hub te selecteren. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Maak een notitie van uw **TenantId** en **SubscriptionId**. U nodig ze later.
3. Maak een nieuwe Azure Active Directory-toepassing met de volgende opdracht, vervangt de houders voldaan:
   
   * **{Weergavenaam}:** een weergavenaam voor uw toepassing, zoals **MySampleApp**
   * **{URL startpagina}:** de URL van de startpagina van uw app, zoals **http://mysampleapp/home**. Deze URL hoeft niet te verwijzen naar een echte toepassing.
   * **{Toepassings-id}:** een unieke id zoals **http://mysampleapp**. Deze URL hoeft niet te verwijzen naar een echte toepassing.
   * **{Wachtwoord}:** een wachtwoord dat u gebruikt om te verifiëren met uw app.
     
     ```powershell
     $SecurePassword=ConvertTo-SecureString {password} –asplaintext –force
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password $SecurePassword
     ```
4. Noteer de **ApplicationId** van de toepassing die u hebt gemaakt. U nodig dit later.
5. Maak een nieuwe service-principal met behulp van de volgende opdracht en vervangt **{MyApplicationId}** met de **ApplicationId** uit de vorige stap:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Instellen van een roltoewijzing met behulp van de volgende opdracht en vervangt **{MyApplicationId}** met uw **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

U bent nu klaar voor het maken van de Azure AD-toepassing waarmee u om verificatie van uw aangepaste C#-toepassing. Verderop in deze zelfstudie moet u de volgende waarden:

* TenantId
* SubscriptionId
* ApplicationId
* Wachtwoord

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
