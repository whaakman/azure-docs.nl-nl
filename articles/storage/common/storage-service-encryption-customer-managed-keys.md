---
title: Azure Storage-Serviceversleuteling door de klant beheerde sleutels in Azure Key Vault | Microsoft Docs
description: De functie Azure Storage-Serviceversleuteling gebruiken voor het versleutelen van Azure Blob storage en Azure Files aan de servicezijde bij het opslaan van de gegevens en ontsleutelen van het bij het ophalen van de gegevens die door de klant beheerde sleutels.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: e2497233ec97ffc88bf13797f62d601d4da373a1
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628490"
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Versleuteling voor opslagservice met behulp van de klant beheerde sleutels in Azure Key Vault

Microsoft Azure is belangrijk dat u kunt beschermen en beveiligen van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen goed. Een manier die het opslagplatform van Azure beschermt uw gegevens is via versleuteling SSE (Storage Service), die uw gegevens worden versleuteld bij het schrijven van deze naar de opslag en ontsleutelt de gegevens bij het ophalen van het. De versleuteling en ontsleuteling is automatische, transparant en maakt gebruik van 256-bits [AES-versleuteling](https://wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blokversleutelingsmethoden die er bestaan.

U kunt Microsoft beheerde sleutels gebruiken met SSE of kunt u uw eigen versleutelingssleutels. Dit artikel wordt beschreven hoe u uw eigen versleutelingssleutels. Zie voor meer informatie over het gebruik van Microsoft beheerde sleutels of over SSE in het algemeen, [Storage Service Encryption voor data-at-rest](storage-service-encryption.md).

SSE voor Azure Blob storage en Azure Files is geïntegreerd met Azure Key Vault, zodat u een key vault gebruiken kunt voor het beheren van uw versleutelingssleutels. U kunt uw eigen versleutelingssleutels maken en op te slaan in een key vault, of u kunt Azure Key Vault-API's gebruiken voor het genereren van sleutels voor gegevenscodering. Met Azure Key Vault, kunt u beheren en besturen van uw sleutels en ook controle uitvoeren op uw gebruik van de sleutel.

> [!Note]  
> Versleuteling voor opslagservice met behulp van de klant beheerde sleutels is niet beschikbaar voor [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md). [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md) maakt gebruik van industriestandaard [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) op Windows en [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) op Linux om een oplossing versleuteling te bieden die is geïntegreerd met Key Vault.

Waarom uw eigen sleutels maken? Aangepaste sleutels biedt u meer flexibiliteit, zodat u kunt maken, draaien, uitschakelen en definiëren van besturingselementen voor toegang. Aangepaste sleutels maken het ook mogelijk om te controleren van de versleutelingssleutels die wordt gebruikt om uw gegevens te beveiligen.

## <a name="get-started-with-customer-managed-keys"></a>Aan de slag met de klant beheerde sleutels

Als u wilt gebruiken door de klant beheerde sleutels met SSE, kunt u een nieuwe sleutelkluis maken en sleutel of u een bestaande sleutelkluis en de sleutel kunt gebruiken. Het opslagaccount en de key vault moeten zich in dezelfde regio, maar ze kunnen zich in verschillende abonnementen.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="step-1-create-a-storage-account"></a>Stap 1: Create a storage account

Maak eerst een storage-account als u er nog geen hebt. Zie [Een opslagaccount maken](storage-quickstart-create-account.md) voor meer informatie.

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Stap 2: Serverversleuteling inschakelen voor de Blob-en bestand

Als u wilt met behulp van de klant beheerde sleutels SSE inschakelt, moeten twee belangrijke functies voor gegevensbeveiliging, voorlopig verwijderen en niet verwijderen, ook worden ingeschakeld in Azure Key Vault. Deze instellingen ervoor dat de sleutels kunnen niet per ongeluk of opzettelijk verwijderde. De maximale bewaarperiode van de sleutels is ingesteld op 90 dagen, beveiligt gebruikers tegen kwaadwillende actoren of ransomware-aanvallen.

Als u inschakelen via een programma door de klant beheerde sleutels voor SSE wilt, kunt u de [Azure Storage Resource Provider REST API](https://docs.microsoft.com/rest/api/storagerp), wordt de [Storage Resource Provider-clientbibliotheek voor .NET](https://docs.microsoft.com/dotnet/api), [ Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), of de [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Als u wilt gebruiken door de klant beheerde sleutels met SSE, moet u de identiteit van een storage-account toewijzen aan de storage-account. U kunt de identiteit instellen door het uitvoeren van de volgende PowerShell of Azure CLI-opdracht:

```powershell
Set-AzStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

```azurecli-interactive
az storage account \
    --account-name <account_name> \
    --resource-group <resource_group> \
    --assign-identity
```

U kunt voorlopig verwijderen en kan niet opschonen inschakelen door het uitvoeren van de volgende PowerShell of Azure CLI-opdrachten:

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

```azurecli-interactive
az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enableSoftDelete=true

az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enablePurgeProtection=true
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Stap 3: Versleuteling inschakelen met de klant beheerde sleutels

SSE maakt standaard gebruik van Microsoft beheerde sleutels. U kunt SSE inschakelen met de klant beheerde sleutels voor de storage-account met de [Azure-portal](https://portal.azure.com/). Op de **instellingen** blade voor de storage-account, klikt u op **versleuteling**. Selecteer de **uw eigen sleutel gebruiken** optie, zoals wordt weergegeven in de volgende afbeelding.

![Versleutelingsoptie Portal schermopname](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Stap 4: Selecteer de sleutel

U kunt uw sleutel opgeven als een URI of door het selecteren van de sleutel in een key vault.

#### <a name="specify-a-key-as-a-uri"></a>Een sleutel opgeeft als een URI

Als u uw sleutel van een URI, de volgende stappen uit:

1. Kies de **Enter-toets URI** optie.
2. In de **Key URI** veld, geeft u de URI.

   ![Portal schermafbeelding versleuteling met de optie key uri invoeren](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

#### <a name="specify-a-key-from-a-key-vault"></a>Geef een sleutel van een key vault

Als u uw sleutel uit een key vault, de volgende stappen uit:

1. Kies de **selecteren uit Key Vault** optie.
2. Kies de key vault met de sleutel die u wilt gebruiken.
3. Kies de sleutel uit de key vault.

   ![Portal schermopname coderingen gebruik uw eigen sleutel optie](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Als het opslagaccount heeft geen toegang tot de key vault, kunt u de Azure PowerShell-opdracht weergegeven in de volgende afbeelding om toegang te verlenen uitvoeren.

![Portal schermopname die laat zien hoe de toegang is geweigerd voor key vault](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

U kunt ook toegang via Azure portal door te navigeren naar de Azure Key Vault in Azure portal en het verlenen van toegang tot het opslagaccount.

U kunt de bovenstaande sleutel koppelen aan een bestaand opslagaccount met behulp van de volgende PowerShell-opdrachten:

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```

### <a name="step-5-copy-data-to-storage-account"></a>Stap 5: Gegevens kopiëren naar storage-account

Gegevens over te dragen naar uw nieuwe opslagaccount zodat ze zijn versleuteld. Zie voor meer informatie [Veelgestelde vragen over de Storage-Serviceversleuteling](storage-service-encryption.md#faq-for-storage-service-encryption).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Stap 6: De status van de versleutelde gegevens opvragen

De status van de versleutelde gegevens opvragen.

## <a name="faq-for-sse-with-customer-managed-keys"></a>Veelgestelde vragen over SSE met klant-beheerde sleutels

**Ik gebruik Premium-opslag; kan ik door de klant beheerde sleutels gebruiken met SSE?**  
Ja, SSE met door Microsoft beheerd en door de klant beheerde sleutels op zowel Standard storage en Premium storage wordt ondersteund.

**Kan ik nieuwe storage-accounts met SSE met de klant beheerde sleutels die worden ingeschakeld met behulp van Azure PowerShell en Azure CLI maken?**  
Ja.

**Hoe veel meer kost Azure Storage als ik de klant beheerde sleutels met SSE gebruiken?**  
Er is een kosten die gepaard gaan voor het gebruik van Azure Key Vault. Ga voor meer informatie naar [prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/). Er is geen extra kosten voor SSE, dat is ingeschakeld voor alle opslagaccounts.

**Storage-Serviceversleuteling beschikbaar op Azure Managed Disks is?**  
Storage-Serviceversleuteling is beschikbaar voor Azure Managed Disks met door Microsoft beheerde sleutels, maar niet met de klant beheerde sleutels. In plaats van Managed Disks SSE ondersteunen met de klant beheerde sleutels, wordt aangeraden [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md), die gebruikmaakt van industriestandaard [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) op Windows en [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)in Linux voor versleuteling die is geïntegreerd met Key Vault.

**Hoe verschilt versleuteling voor opslagservice van Azure Disk Encryption?**  
Azure Disk Encryption biedt integratie tussen OS gebaseerde oplossingen, zoals BitLocker en DM-Crypt en Azure Key Vault. Storage Service-versleuteling biedt versleuteling standaard op de laag van een Azure storage-platform, onder de virtuele machine.

**Kan ik toegang tot de versleutelingssleutels intrekken?**
Ja, kunt u de toegang op elk gewenst moment intrekken. Er zijn verschillende manieren toegang tot uw sleutels te trekken. Raadpleeg [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault/) en [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault) voor meer informatie. De toegang blokkeren wordt toegang tot alle blobs in de storage-account effectief geblokkeerd als de sleutel voor het account niet toegankelijk door Azure Storage is.

**Kan ik een storage-account en een sleutel maken in verschillende regio's?**  
Nee, het opslagaccount en de Azure Key Vault en de sleutel moeten zich in dezelfde regio bevinden.

**Kan ik door de klant beheerde sleutels voor Serverversleuteling inschakelen tijdens het maken van het storage-account?**  
Nee. Wanneer u het opslagaccount voor het eerst maakt, zijn alleen door Microsoft beheerde sleutels voor SSE beschikbaar. Als u wilt gebruiken door de klant beheerde sleutels, moet u de eigenschappen van het opslagaccount bijwerken. U kunt REST of een van de opslagclientbibliotheken gebruiken voor het programmatisch uw storage-account bijwerken of bijwerken van de eigenschappen van het opslagaccount met behulp van de Azure-portal nadat u het account hebt gemaakt.

**Kan ik tijdens het gebruik van de klant beheerde sleutels met SSE-versleuteling uitschakelen?**  
Nee, u kunt versleuteling niet uitschakelen. Versleuteling is standaard ingeschakeld voor Azure Blob-opslag, Azure Files, Azure-wachtrij en Azure-tabelopslag. U kunt eventueel overschakelen van door Microsoft beheerde sleutels voor het gebruik van de klant beheerde sleutels, en vice versa.

**SSE is is ingeschakeld wanneer ik een nieuw opslagaccount maken?**  
SSE is ingeschakeld voor alle opslagaccounts en Azure Blob-opslag, Azure Files Azure Queue storage en Azure-tabelopslag.

**Ik kan SSE met behulp van de klant beheerde sleutels op mijn opslagaccount niet inschakelen.**  
Is het een storage-account van Azure Resource Manager? Klassieke opslagaccounts worden niet ondersteund met de klant beheerde sleutels. SSE met de klant beheerde sleutels kan alleen worden ingeschakeld voor Resource Manager-opslagaccounts.

**Wat is voorlopig verwijderen en niet kan wissen? Moet ik deze instelling SSE gebruiken met de klant beheerde sleutels wilt inschakelen?**  
Voorlopig verwijderen en kan niet opschonen moet zijn ingeschakeld voor gebruik van SSE met door de klant beheerde sleutels. Deze instellingen zorgen ervoor dat de sleutel niet per ongeluk of opzettelijk verwijderde. De maximale bewaarperiode van de sleutels is ingesteld op 90 dagen, beveiligt gebruikers tegen kwaadwillende actoren en ransomware-aanvallen. Deze instelling kan niet worden uitgeschakeld.

**Is SSE met de klant beheerde sleutels alleen toegestaan in specifieke regio's?**  
SSE met de klant beheerde sleutels is beschikbaar in alle regio's voor Azure-blobopslag en Azure Files.

**Hoe ik contact opnemen met iemand als ik problemen hebt of feedback wilt geven?**  
Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) voor eventuele problemen met betrekking tot de versleuteling voor opslagservice.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over de uitgebreide reeks beveiligingsprogramma mogelijkheden die ontwikkelaars helpen bij het bouwen van veilige toepassingen, Zie de [Storage-beveiligingshandleiding](storage-security-guide.md).
- Zie voor informatie over Azure Key Vault, [wat is Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
- Zie voor het aan de slag met Azure Key Vault, [aan de slag met Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
