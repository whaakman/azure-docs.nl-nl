---
title: Breng uw eigen sleutel voor Apache Kafka op Azure HDInsight (Preview)
description: In dit artikel wordt beschreven hoe u uw eigen sleutel uit Azure Key Vault gebruiken voor het versleutelen van gegevens die zijn opgeslagen in Apache Kafka in Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: cb18f0e1b682434c5069c2a02524a6f16551e9e2
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545974"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight"></a>Breng uw eigen sleutel voor Apache Kafka op Azure HDInsight

Azure HDInsight biedt Bring Your Own Key (BYOK) ondersteuning voor Apache Kafka. Deze mogelijkheid kunt u de eigenaar en beheren van de sleutels die worden gebruikt om gegevens in rust te versleutelen. 

Alle beheerde schijven in HDInsight worden beveiligd met Azure Storage Service Encryption (SSE). Standaard worden de gegevens op deze schijven versleuteld door Microsoft beheerde sleutels. Als u BYOK inschakelt, geeft u de versleutelingssleutel voor HDInsight te gebruiken en beheren met behulp van Azure Key Vault. 

BYOK-codering is een proces één stap is verwerkt tijdens het maken van een cluster zonder extra kosten. U hoeft alleen is HDInsight registreren als een beheerde identiteit met Azure Key Vault en de versleutelingssleutel toevoegen wanneer u uw cluster maakt.

Alle berichten naar het Kafka-cluster (met inbegrip van replica's die door Kafka worden onderhouden) worden versleuteld met een symmetrische gegevens Gegevensversleutelingsleutel (DEK). De DEK is beveiligd met behulp van de sleutel versleuteling sleutel (KEK) van uw key vault. De processen van versleuteling en ontsleuteling worden volledig door Azure HDInsight afgehandeld. 

U kunt de Azure portal of Azure CLI gebruiken veilig de om sleutels te rouleren in de key vault. Wanneer een sleutel draait, begint de HDInsight Kafka-cluster met behulp van de nieuwe sleutel binnen enkele minuten. De functies van de sleutelbeveiliging 'Voorlopig verwijderen' ter bescherming tegen ransomware-scenario's en onbedoelde verwijdering inschakelen. Sleutelkluizen zonder deze beveiligingsfunctie worden niet ondersteund.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="get-started-with-byok"></a>Aan de slag met BYOK
Maakt een BYOK Kafka-cluster ingeschakeld, wordt de volgende stappen doorlopen:
1. Een beheerde identiteit voor Azure-resources maken
2. Azure Key Vault en sleutels instellen
3. HDInsight Kafka-cluster maken met BYOK ingeschakeld
4. De versleutelingssleutel draaien

## <a name="create-managed-identities-for-azure-resources"></a>Een beheerde identiteit voor Azure-resources maken

   Om te verifiëren naar Key Vault, maak een beheerde identiteit gebruiker toegewezen met de [Azure-portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md), of [ Azure CLI](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Zie voor meer informatie over de manier waarop beheerde identiteiten werk in Azure HDInsight, [beheerde identiteiten in Azure HDInsight](../hdinsight-managed-identities.md). Hoewel Azure Active directory vereist voor beheerde identiteiten en BYOK met Kafka is, niet Enterprise Security Package (ESP) vereist. Zorg ervoor dat u de resource-ID van de beheerde identiteit voor wanneer u deze aan het toegangsbeleid van Key Vault toevoegen.

   ![De gebruiker toegewezen beheerde identiteit maken in Azure portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

## <a name="setup-the-key-vault-and-keys"></a>Instellen van de Key Vault en sleutels

   HDInsight biedt alleen ondersteuning voor Azure Key Vault. Als u uw eigen key vault hebt, kunt u uw sleutels importeren in Azure Key Vault. Houd er rekening mee dat de sleutels hebben moeten 'Voorlopig verwijderen'. De functie 'Voorlopig verwijderen' is beschikbaar via de REST, .NET /C#, PowerShell en Azure CLI-interfaces.

   1. Voor het maken van een nieuwe sleutelkluis, volgt u de [Azure Key Vault](../../key-vault/key-vault-overview.md) Quick Start. Bezoek voor meer informatie over het importeren van bestaande sleutels [over sleutels, geheimen en certificaten](../../key-vault/about-keys-secrets-and-certificates.md).

   2. 'Voorlopig verwijderen' in voor de sleutelkluis inschakelen met behulp van de [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) cli-opdracht.
        '''Azure CLI-az keyvault update--naam <Key Vault Name> --inschakelen voorlopig verwijderen
        ```

   3. Create keys

        a. To create a new key, select **Generate/Import** from the **Keys** menu under **Settings**.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

        b. Set **Options** to **Generate** and give the key a name.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

        c. Select the key you created from the list of keys.

        ![Azure Key Vault key list](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

        d. When you use your own key for Kafka cluster encryption, you need to provide the key URI. Copy the **Key identifier** and save it somewhere until you're ready to create your cluster.

        ![Copy key identifier](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
    4. Add managed identity to the key vault access policy.

        a. Create a new Azure Key Vault access policy.

        ![Create new Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy.png)

        b. Under **Select Principal**, choose the user-assigned managed identity you created.

        ![Set Select Principal for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

        c. Set **Key Permissions** to **Get**, **Unwrap Key**, and **Wrap Key**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

        d. Set **Secret Permissions** to **Get**, **Set**, and **Delete**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

        e. Click on **Save**. 

        ![Save Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-save.png)

## Create HDInsight cluster

   You're now ready to create a new HDInsight cluster. BYOK can only be applied to new clusters during cluster creation. Encryption can't be removed from BYOK clusters, and BYOK can't be added to existing clusters.

   ![Kafka disk encryption in Azure portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   During cluster creation, provide the full key URL, including the key version. For example, `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. You also need to assign the managed identity to the cluster and provide the key URI.

## Rotating the Encryption key
   There might be scenarios where you might want to change the encryption keys used by the Kafka cluster after it has been created. This can be easily via the portal. For this operation, the cluster must have access to both the current key and the intended new key, otherwise the rotate key operation will fail.

   To rotate the key, you must have the full url of the new key (See Step 3 of [Setup the Key Vault and Keys](#setup-the-key-vault-and-keys)). Once you have that, go to the Kafka cluster properties section in the portal and click on **Change Key** under **Disk Encryption Key URL**. Enter in the new key url and submit to rotate the key.

   ![Kafka rotate disk encryption key](./media/apache-kafka-byok/kafka-change-key.png)

## FAQ for BYOK to Apache Kafka

**How does the Kafka cluster access my key vault?**

   Associate a managed identity with the HDInsight Kafka cluster during cluster creation. This managed identity can be created before or during cluster creation. You also need to grant the managed identity access to the key vault where the key is stored.

**Is this feature available for all Kafka clusters on HDInsight?**

   BYOK encryption is only possible for Kafka 1.1 and above clusters.

**Can I have different keys for different topics/partitions?**

   No, all managed disks in the cluster are encrypted by the same key.

**What happens if the cluster loses access to the key vault or the key?**
   If the cluster loses access to the key, warnings will be shown in the Ambari portal. In this state, the **Change Key** operation will fail. Once key access is restored, ambari warnings will go away and operations such as key rotation can be successfully performed.

   ![Kafka key access ambari alert](./media/apache-kafka-byok/kafka-byok-ambari-alert.png)

**How can I recover the cluster if the keys are deleted?**

   Since only “Soft Delete” enabled keys are supported, if the keys are recovered in the key vault, the cluster should regain access to the keys. To recover an Azure Key Vault key, see [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) or [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Can I have producer/consumer applications working with a BYOK cluster and a non-BYOK cluster simultaneously?**

   Yes. The use of BYOK is transparent to producer/consumer applications. Encryption happens at the OS layer. No changes need to be made to existing producer/consumer Kafka applications.

**Are OS disks/Resource disks also encrypted?**

   No. OS disks and Resource disks are not encrypted.

**If a cluster is scaled up, will the new brokers support BYOK seamlessly?**

   Yes. The cluster needs access to the key in the key vault during scale up. The same key is used to encrypt all managed disks in the cluster.

**Is BYOK available in my location?**

   Kafka BYOK is available in all public clouds.

## Next steps

* For more information about Azure Key Vault, see [What is Azure Key Vault](../../key-vault/key-vault-whatis.md)?
* To get started with Azure Key Vault, see [Getting Started with Azure Key Vault](../../key-vault/key-vault-overview.md).
