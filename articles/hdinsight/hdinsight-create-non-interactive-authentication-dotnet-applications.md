---
title: Niet-interactieve verificatie voor .NET-toepassingen maken in Azure HDInsight
description: Meer informatie over het maken van niet-interactieve verificatie Microsoft .NET-toepassingen in Azure HDInsight.
editor: jasonwhowell
services: hdinsight
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: f5f1aae62d8f2959f26a361f3c1187037cafcca5
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591516"
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Een niet-interactieve verificatie HDInsight .NET-toepassing maken
U kunt uw Microsoft .NET-Azure HDInsight-toepassing onder de identiteit van de toepassing (niet-interactieve) of onder de identiteit van de aangemelde gebruiker van de toepassing (interactief) uitvoeren. Dit artikel laat u het maken van een niet-interactieve verificatie .NET-toepassing verbinding maakt met Azure en beheren van HDInsight. Zie voor een voorbeeld van een interactieve toepassing [verbinding maken met Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). 

Via uw niet-interactieve .NET-toepassing, hebt u het volgende nodig:

* De tenant-ID van uw Azure-abonnement (ook wel een *map-ID*). Zie [tenant-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* De Azure Active Directory (Azure AD)-toepassing-client-ID. Zie [maken van een Azure Active Directory-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) en [een toepassings-ID ophalen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
* Azure AD geheime sleutel van de toepassing. Zie [Get toepassing verificatiesleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

## <a name="prerequisites"></a>Vereisten
* Een HDInsight-cluster. Zie de [zelfstudie aan de slag](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Een rol toewijzen aan de Azure AD-toepassing
Toewijzen van uw Azure AD-toepassing een [rol](../role-based-access-control/built-in-roles.md), om deze machtiging acties uit te voeren. U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. De machtigingen worden overgenomen op lagere niveaus van bereik. (Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat de toepassing kan de resourcegroep en alle resources in het lezen.) In deze zelfstudie, kunt u het bereik instellen op niveau van de resourcegroep. Zie voor meer informatie, [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md).

**De rol van eigenaar toevoegen aan de Azure AD-toepassing**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **resourcegroep**.
3. Selecteer de resourcegroep waaraan de HDInsight-cluster op waarop u uw Hive-query verderop in deze zelfstudie wordt uitgevoerd. Als u een groot aantal resourcegroepen hebt, kunt u het filter om te zoeken die u wilt.
4. Selecteer op het menu resourcegroep **toegangsbeheer (IAM)**.
5. Onder **gebruikers**, selecteer **toevoegen**.
6. Volg de instructies voor de rol van eigenaar toevoegen aan uw Azure AD-toepassing. Nadat u de functie is toegevoegd, de toepassing wordt vermeld onder **gebruikers**, met de rol van eigenaar. 

## <a name="develop-an-hdinsight-client-application"></a>Een HDInsight-client-toepassing ontwikkelen

1. Maak een C#-consoletoepassing.
2. De volgende NuGet-pakketten toevoegen:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Voer de volgende code:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```


## <a name="next-steps"></a>Volgende stappen
* [Een Azure Active Directory-toepassing en service-principal maken in Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).
* Meer informatie over het [een service-principal met Azure Resource Manager verifiëren](../azure-resource-manager/resource-group-authenticate-service-principal.md).
* Meer informatie over [Azure Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md).
