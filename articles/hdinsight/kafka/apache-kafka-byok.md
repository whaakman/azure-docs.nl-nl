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
ms.openlocfilehash: 85fea195b05bea8a1db70f8b5b81cabdfe7c6c72
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041506"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight-preview"></a>Breng uw eigen sleutel voor Apache Kafka op Azure HDInsight (Preview)

Azure HDInsight biedt Bring Your Own Key (BYOK) ondersteuning voor Apache Kafka. Deze mogelijkheid kunt u de eigenaar en beheren van de sleutels die worden gebruikt om gegevens in rust te versleutelen. 

Alle beheerde schijven in HDInsight worden beveiligd met Azure Storage Service Encryption (SSE). Standaard worden de gegevens op deze schijven versleuteld door Microsoft beheerde sleutels. Als u BYOK inschakelt, geeft u de versleutelingssleutel voor HDInsight te gebruiken en beheren met behulp van Azure Key Vault. 

BYOK-codering is een proces één stap is verwerkt tijdens het maken van een cluster zonder extra kosten. U hoeft alleen is HDInsight registreren als een beheerde identiteit met Azure Key Vault en de versleutelingssleutel toevoegen wanneer u uw cluster maakt.

Alle berichten naar het Kafka-cluster (met inbegrip van replica's die door Kafka worden onderhouden) worden versleuteld met een symmetrische gegevens Gegevensversleutelingsleutel (DEK). De DEK is beveiligd met behulp van de sleutel versleuteling sleutel (KEK) van uw key vault. De processen van versleuteling en ontsleuteling worden volledig door Azure HDInsight afgehandeld. 

U kunt de Azure portal of Azure CLI gebruiken veilig de om sleutels te rouleren in de key vault. Wanneer een sleutel draait, begint de HDInsight Kafka-cluster met behulp van de nieuwe sleutel binnen enkele minuten. De functies van de sleutelbeveiliging "Niet leegmaken" en 'Voorlopig verwijderen' om te beveiligen tegen ransomware-scenario's en onopzettelijk verwijderen inschakelen. Sleutels zonder deze beveiligingsfuncties worden niet ondersteund.

## <a name="get-started-with-byok"></a>Aan de slag met BYOK

1. Een beheerde identiteit voor Azure-resources maken.

   Om te verifiëren naar Key Vault, maak een beheerde identiteit gebruiker toegewezen met de [Azure Portal](../../active-directory/managed-service-identity/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-service-identity/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-service-identity/how-to-manage-ua-identity-arm.md), of [ Azure CLI](../../active-directory/managed-service-identity/how-to-manage-ua-identity-cli.md). Hoewel Azure Active directory vereist voor beheerde identiteiten en BYOK met Kafka is, niet Enterprise Security Package (ESP) vereist. Zorg ervoor dat u de resource-ID van de beheerde identiteit voor wanneer u deze aan het toegangsbeleid van Key Vault toevoegen.

   ![De gebruiker toegewezen beheerde identiteit maken in Azure portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

2. Importeren van een bestaande sleutelkluis of een nieuwe maken.

   HDInsight biedt alleen ondersteuning voor Azure Key Vault. Als u uw eigen key vault hebt, kunt u uw sleutels importeren in Azure Key Vault. Houd er rekening mee dat de sleutels hebben moeten 'Voorlopig verwijderen' en 'Kan niet opschonen' ingeschakeld. De functies 'Voorlopig verwijderen' en 'Niet opschonen' zijn beschikbaar via de REST, .NET / C#, PowerShell en Azure CLI-interfaces.

   Voor het maken van een nieuwe sleutelkluis, volgt u de [Azure Key Vault](../../key-vault/key-vault-get-started.md) Quick Start. Bezoek voor meer informatie over het importeren van bestaande sleutels [over sleutels, geheimen en certificaten](../../key-vault/about-keys-secrets-and-certificates.md).

   Voor het maken van een nieuwe sleutel selecteert **genereren/importeren** uit de **sleutels** menu onder **instellingen**.

   ![Genereer een nieuwe sleutel in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

   Stel **opties** naar **genereren** en geef een naam op voor de sleutel.

   ![Genereer een nieuwe sleutel in Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

   Selecteer de sleutel die u hebt gemaakt in de lijst met sleutels.

   ![Lijst met Azure Key Vault sleutels](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

   Wanneer u uw eigen sleutel voor versleuteling van Kafka-cluster gebruikt, moet u de sleutel-URI opgeven. Kopieer de **sleutel-id** en sla deze ergens totdat u bent klaar om uw cluster te maken.

   ![Kopieer de sleutel-id](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
3. Beheerde identiteit toevoegen aan het toegangsbeleid voor key vault.

   Maak een nieuw Azure Key Vault-toegangsbeleid.

   ![Nieuwe Azure Key Vault-toegangsbeleid maken](./media/apache-kafka-byok/add-key-vault-access-policy.png)

   Onder **Principal selecteren**, kies de gebruiker toegewezen beheerde identiteit die u hebt gemaakt.

   ![Principal selecteren voor Azure Key Vault-toegangsbeleid instellen](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

   Stel **sleutelmachtigingen** naar **ophalen**, **sleutel uitpakken**, en **sleutel inpakken**.

   ![Sleutel-machtigingen instellen voor Azure Key Vault-toegangsbeleid](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

   Stel **geheime machtigingen** naar **ophalen**, **ingesteld**, en **verwijderen**.

   ![Sleutel-machtigingen instellen voor Azure Key Vault-toegangsbeleid](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

4. HDInsight-cluster maken

   U bent nu klaar om te maken van een nieuw HDInsight-cluster. BYOK kan alleen worden toegepast op nieuwe clusters tijdens het maken van clusters. Versleuteling kan niet worden verwijderd uit de BYOK-clusters en BYOK kan niet worden toegevoegd aan bestaande clusters.

   ![Kafka-schijfversleuteling in Azure portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   Tijdens het maken van een cluster, bieden u de volledige sleutel-URL, met inbegrip van de belangrijkste versie. Bijvoorbeeld `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. U moet ook de beheerde identiteit toewijzen aan het cluster en geef de sleutel-URI.

## <a name="faq-for-byok-to-kafka"></a>Veelgestelde vragen over BYOK met Kafka

**Hoe het Kafka-cluster toegang heeft tot mijn sleutelkluis?**

   Een beheerde identiteit koppelen aan de HDInsight Kafka-cluster tijdens het maken van clusters. Deze beheerde identiteit kan worden gemaakt vóór of tijdens het maken van clusters. U moet ook de beheerde identiteit toegang verlenen tot de sleutelkluis waar de sleutel is opgeslagen.

**Is deze functie beschikbaar voor alle Kafka-clusters op HDInsight?**

   BYOK-codering is alleen mogelijk voor Kafka 1.1 en hoger-clusters.

**Kan ik verschillende sleutels voor verschillende onderwerpen/partities hebben?**

   Nee, alle beheerde schijven in het cluster zijn versleuteld met dezelfde sleutel.

**Hoe kan ik het cluster herstellen als de sleutels worden verwijderd?**

   Omdat alleen 'Voorlopig verwijderen' ingeschakeld sleutels worden ondersteund als de sleutels zijn hersteld in de key vault, moet het cluster toegang tot de sleutels weer. Zie voor het herstellen van een Azure Key Vault-sleutel, [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey).

**Kan ik de producent/klanttoepassingen werken met een cluster met BYOK en een niet-BYOK-cluster tegelijk hebben?**

   Ja. Het gebruik van BYOK is transparant voor de producent/klanttoepassingen. Versleuteling gebeurt op het niveau van het besturingssysteem. Er hoeven geen wijzigingen worden aangebracht aan bestaande producent/consumenten van Kafka-toepassingen.

**Worden schijven/Resource besturingssysteemschijven ook versleuteld?**

   Nee. Besturingssysteemschijven en Resource-schijven zijn niet versleuteld.

**Als een cluster omhoog wordt geschaald, wordt de nieuwe brokers ondersteuning voor BYOK naadloos?**

   Ja. Het cluster moet toegang hebben tot de sleutel in de key vault tijdens de schaal van. Dezelfde sleutel wordt gebruikt voor het versleutelen van alle beheerde schijven in het cluster.

**BYOK is beschikbaar in mijn locatie?**

   Kafka BYOK is beschikbaar in alle openbare clouds.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure Key Vault [wat is Azure Key Vault](../../key-vault/key-vault-whatis.md)?
* Als u wilt aan de slag met Azure Key Vault, Zie [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md).
