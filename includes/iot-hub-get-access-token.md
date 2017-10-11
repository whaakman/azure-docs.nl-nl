## <a name="obtain-an-azure-resource-manager-token"></a>Een Azure Resource Manager-token verkrijgen
De taken die u uitvoert op resources met behulp van de Azure Resource Manager moeten worden geverifieerd door Azure Active Directory. Het voorbeeld gebruikt wachtwoordverificatie, voor Zie andere benaderingen [verificatie van Azure Resource Manager-aanvragen][lnk-authenticate-arm].

1. Voeg de volgende code naar de **Main** methode in Program.cs ophalen van een token van Azure AD met behulp van de toepassings-id en het wachtwoord.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Maak een **ResourceManagementClient** -object dat u het token gebruikt door de volgende code toe te voegen aan het einde van de **Main** methode:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Maken of een verwijzing naar de resourcegroep die u gebruikt:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx