---
title: 'Azure Storage-Service: versleuteling met behulp van de klant beheerd sleutels in Azure Sleutelkluis | Microsoft Docs'
description: 'De functie Azure Storage-Service: versleuteling gebruiken voor het versleutelen van uw Azure-blobopslag aan de kant van de service bij het opslaan van de gegevens en bij het ophalen van de gegevens met behulp van de klant beheerd sleutels worden gedecodeerd.'
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/07/2018
ms.author: lakasa
ms.openlocfilehash: b40858640d10e5661be420976520774bd50837cb
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Versleuteling van opslag-Service met behulp van de klant beheerd sleutels in Azure Sleutelkluis

Microsoft Azure is zeer belangrijk dat u kunt beschermen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen. Een manier die Azure Storage beschermt u uw gegevens is via opslag Service versleuteling (SSE), waarmee u uw gegevens bij het schrijven van het naar de opslag, gegevens gecodeerd en gedecodeerd uw bij het ophalen van het. De versleuteling en ontsleuteling is automatische, transparant en maakt gebruik van 256-bits [AES-versleuteling](https://wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

U kunt Microsoft beheerde versleutelingssleutels SSE of kunt u uw eigen versleutelingssleutels. Dit artikel wordt beschreven hoe u uw eigen versleutelingssleutels. Zie voor meer informatie over het gebruik van door Microsoft beheerde sleutels of over SSE in het algemeen [Service versleuteling van opslag voor gegevens in rust](storage-service-encryption.md).

SSE voor opslag-Blob en het bestand is geïntegreerd met Azure Sleutelkluis, zodat u een sleutelkluis gebruiken kunt voor het beheren van uw versleutelingssleutels. U kunt uw eigen versleutelingssleutels maken en deze opslaan op een sleutelkluis of u kunt Azure Key Vault API's gebruiken voor het genereren van versleutelingssleutels. Met Azure Sleutelkluis, kunt u beheren en instellen van uw sleutels en ook het gebruik van uw sleutel te controleren.

Waarom maakt u uw eigen sleutels? Aangepaste sleutels biedt u meer flexibiliteit, zodat u kunt maken, draaien, uitschakelen en toegangsbeheer definiëren. Aangepaste sleutels kunnen u de versleutelingssleutels die wordt gebruikt voor het beveiligen van uw gegevens te controleren.

## <a name="get-started-with-customer-managed-keys"></a>Aan de slag met de sleutels door de klant beheerd

Als u wilt gebruiken in combinatie met de klant beheerd met SSE, kunt u een nieuwe sleutelkluis maken en sleutel of u een bestaande sleutelkluis en sleutel kunt gebruiken. Het opslagaccount en de sleutelkluis moet in dezelfde regio, maar ze kunnen zich in verschillende abonnementen behoren. 

### <a name="step-1-create-a-storage-account"></a>Stap 1: Een opslagaccount maken

Als u nog niet hebt, maak eerst een opslagaccount. Zie voor meer informatie [maken van een nieuw opslagaccount](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Stap 2: SSE voor Blob en File storage inschakelen

Twee sleutelbeveiliging functies voorlopig verwijderen en niet verwijderen, moeten ook zijn ingeschakeld zodat SSE met sleutels die door de klant beheerd. Deze instellingen Zorg ervoor dat de sleutels niet per ongeluk of opzettelijk verwijderde. De maximale bewaarperiode van de sleutels is ingesteld op 90 dagen, beveiligt gebruikers tegen schadelijke actoren of ransomware aanvallen.

Als u inschakelen via een programma door de klant beheerd sleutels voor SSE wilt, kunt u de [REST API van Azure Storage Resource Provider](https://docs.microsoft.com/rest/api/storagerp), wordt de [Storage Resource Provider-clientbibliotheek voor .NET](https://docs.microsoft.com/dotnet/api), [ Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), of de [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Als u wilt gebruiken in combinatie met de klant beheerd met SSE, moet u een account-id van opslag toewijzen aan het opslagaccount. U kunt de identiteit van de instellen door het uitvoeren van de volgende PowerShell-opdracht:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

U kunt voorlopig verwijderd en niet wilt opschonen inschakelen door het uitvoeren van de volgende PowerShell-opdrachten:

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Stap 3: Schakel codering met sleutels door de klant beheerd

SSE gebruikt standaard door Microsoft beheerde sleutels. U kunt de SSE inschakelen met de klant beheerd sleutels voor de storage-account met de [Azure-portal](https://portal.azure.com/). Op de **instellingen** blade voor de storage-account, klikt u op **versleuteling**. Selecteer de **gebruik van uw eigen sleutel** optie, zoals wordt weergegeven in de volgende afbeelding.

![De optie versleuteling Portal schermopname](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Stap 4: De sleutel selecteren

U kunt uw sleutel opgeven als een URI of door de sleutel in een sleutelkluis te selecteren.

#### <a name="specify-a-key-as-a-uri"></a>Geef een sleutel op als een URI

Geef uw sleutel van een URI door de volgende stappen uit:

1. Kies de **Enter-toets URI** optie.  
2. In de **URI van Key** veld, geeft u de URI.

    ![Portal schermafbeelding versleuteling met sleutel uri optie invoeren](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

#### <a name="specify-a-key-from-a-key-vault"></a>Geef een sleutel van een sleutelkluis 

Geef uw sleutel uit een sleutelkluis door de volgende stappen uit:

1. Kies de **selecteert in de Sleutelkluis** optie.  
2. Kies de sleutelkluis met de sleutel die u wilt gebruiken.
3. Kies de sleutel in de sleutelkluis.

    ![Portal schermopname Versleutelingen gebruik uw eigen sleutel optie](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Als het storage-account geen toegang tot de sleutelkluis heeft, kunt u de Azure PowerShell-opdracht weergegeven in de volgende afbeelding om toegang te verlenen uitvoeren.

![Portal schermafbeelding toegang is geweigerd voor sleutelkluis](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

U kunt ook toegang via de Azure-portal door te navigeren naar de Azure Sleutelkluis in Azure portal en het verlenen van toegang tot het opslagaccount.

### <a name="step-5-copy-data-to-storage-account"></a>Stap 5: Gegevens kopiëren naar de storage-account

Overbrengen van gegevens in uw nieuwe opslagaccount zodat ze zijn versleuteld, raadpleegt u stap 3 van [aan de slag in Service-versleuteling van opslag voor gegevens in rust](storage-service-encryption.md#step-3-copy-data-to-storage-account).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Stap 6: De status van de versleutelde gegevens opvragen

Als u wilt de status van de versleutelde gegevens opvragen, raadpleegt u stap 4 van [aan de slag in Service-versleuteling van opslag voor gegevens in rust](storage-service-encryption.md#step-4-query-the-status-of-the-encrypted-data).

## <a name="faq-for-sse-with-customer-managed-keys"></a>Veelgestelde vragen over de SSE met klant-beheerde sleutels

**V: ik gebruik Premium-opslag; kan ik de klant beheerd sleutels met SSE gebruiken?**

A: Ja, de SSE met door Microsoft beheerd en door de klant beheerde sleutels op zowel Standard-opslag- en Premium-opslag wordt ondersteund.

**V: kan ik nieuwe storage-accounts met SSE maken met de klant beheerd sleutels die worden ingeschakeld met behulp van Azure PowerShell en Azure CLI?**

A: Ja.

**V: hoe veel meer kost Azure Storage als ik de sleutels door de klant beheerd met SSE gebruiken?**

A: Er is een waarde die is gekoppeld voor het gebruik van Azure Sleutelkluis. Voor meer informatie gaat u naar [Sleutelkluis prijzen](https://azure.microsoft.com/pricing/details/key-vault/). Er is geen extra kosten voor SSE die is ingeschakeld voor alle opslagaccounts.

**V: kan ik de toegang tot de versleutelingssleutels intrekken?**

A: Ja, kunt u de toegang intrekken op elk gewenst moment. Er zijn verschillende manieren toegang tot uw sleutels in te trekken. Raadpleeg [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) en [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault) voor meer informatie. Toegang intrekken wordt toegang tot alle blobs in de storage-account effectief geblokkeerd omdat de versleutelingssleutel van de account niet toegankelijk door Azure Storage is.

**V: kan ik een opslagaccount en de sleutel in andere regio maken?**

A: Nee, de storage-account en de Azure Sleutelkluis en de sleutel moeten in dezelfde regio.

**V: kan ik de klant beheerd sleutels inschakelen voor SSE tijdens het maken van het storage-account?**

A: Nee. Wanneer u het opslagaccount voor het eerst maakt, zijn alleen door Microsoft beheerde sleutels beschikbaar voor SSE. Voor het gebruik van sleutels door de klant beheerd, moet u de eigenschappen van het opslagaccount bijwerken. U kunt gebruik REST of een van de opslagclientbibliotheken programmatisch uw storage-account bijwerken of bijwerken van de eigenschappen van het opslagaccount met de Azure portal na het maken van het account.

**V: kan ik versleuteling tijdens het gebruik van de klant beheerd sleutels met SSE uitschakelen?**

A: Nee, u versleuteling niet uitschakelen. Versleuteling is standaard ingeschakeld voor alle services: Blob, bestand, Table en Queue storage. U kunt desgewenst overschakelen van het gebruik van sleutels voor het gebruik van sleutels door de klant beheerd door Microsoft beheerd en vice versa.

**V: is SSE standaard ingeschakeld wanneer ik een nieuw opslagaccount maken?**

A: SSE is standaard voor alle opslagaccounts en voor alle services: Blob, bestand, Table en Queue storage ingeschakeld.

**V: ik inschakelen SSE met behulp van de klant beheerde sleutels op mijn storage-account niet.**

A: is het een Azure Resource Manager-storage-account? Klassieke opslagaccounts worden niet ondersteund met sleutels door de klant beheerd. SSE met sleutels door de klant beheerd kan alleen worden ingeschakeld voor Resource Manager storage-accounts.

**V: Wat is voorlopig verwijderd en niet wilt verwijderen? Moet ik deze instelling voor het gebruik van SSE met de klant beheerd sleutels wilt inschakelen?**

A: soft verwijderen en niet wilt opschonen moet zijn ingeschakeld voor gebruik van SSE met sleutels door de klant beheerd. Deze instellingen Zorg ervoor dat uw sleutel niet per ongeluk of opzettelijk verwijderde. De maximale bewaarperiode van de sleutels is ingesteld op 90 dagen, beveiligt gebruikers tegen schadelijke actoren en ransomware aanvallen. Deze instelling kan niet worden uitgeschakeld.

**V: is SSE met de klant beheerd sleutels alleen toegestaan in specifieke gebieden?**

A: SSE met sleutels door de klant beheerd is beschikbaar in alle regio's voor opslag-Blob en het bestand.

**V: hoe neem ik iemand als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) van eventuele problemen met versleuteling van de opslagruimte.

## <a name="next-steps"></a>Volgende stappen

-   Voor meer informatie over de uitgebreide set van beveiliging mogelijkheden die ontwikkelaars helpen bij het ontwikkelen van beveiligde toepassingen, Zie de [opslag beveiligingshandleiding](storage-security-guide.md).

-   Zie voor informatie over Azure Sleutelkluis [wat is Azure Sleutelkluis](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?

-   Zie voor aan de slag op Azure Sleutelkluis, [aan de slag met Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
