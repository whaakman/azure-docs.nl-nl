---
ms.openlocfilehash: b6ea8c7b3a6374572c8bd31e3c62b788efbafcbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65827473"
---
## <a name="obtain-an-azure-resource-manager-token"></a>Een Azure Resource Manager-token verkrijgen
Alle taken die u uitvoert op resources met behulp van Azure Resource Manager moeten worden geverifieerd door Azure Active Directory. In het voorbeeld hieronder wordt wachtwoordverificatie gebruikt, gebruikt voor andere methoden raadpleegt [verificatie van Azure Resource Manager-aanvragen][lnk-authenticate-arm].

1. Voeg de volgende code aan de **Main** methode in Program.cs om op te halen van een token uit Azure AD met behulp van de toepassings-id en het wachtwoord.

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

2. Maak een **ResourceManagementClient** -object dat het token wordt gebruikt door de volgende code toe te voegen aan het einde van de **Main** methode:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Maken of verkrijgen van een verwijzing naar de resourcegroep die u gebruikt:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/zh-cn/library/azure/dn790557.aspx