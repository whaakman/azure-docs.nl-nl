---
title: Beheren van Azure Key Vault met behulp van CLI - Azure Key Vault | Microsoft Docs
description: In dit artikel gebruiken voor het automatiseren van algemene taken in Key Vault met behulp van de Azure CLI
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 3d97ce7588642462920e98eb90c5c6b5d3748067
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076360"
---
# <a name="manage-key-vault-using-the-azure-cli"></a>Key Vault met de Azure CLI beheren 

In dit artikel wordt uitgelegd hoe u aan de slag met Azure Key Vault met de Azure CLI.  U ziet informatie op:

- Over het maken van een geharde container (een kluis) in Azure
- Een sleutel, het geheim of het certificaat toe te voegen aan de sleutelkluis
- Een toepassing registreren bij Azure Active Directory
- Een toepassing voor het gebruik van een sleutel of geheim autoriseren
- Geavanceerde toegangsbeleid voor sleutelkluis instellen
- Werken met Hardware security modules (HSM's)
- De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
- Diverse Azure platformoverschrijdende opdrachtregelinterface opdrachten


Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

> [!NOTE]
> In dit artikel bevat geen instructies voor het schrijven van de Azure-toepassing die een van de stappen bevat, laat zien hoe een toepassing machtigen voor het gebruik van een sleutel of geheim in de key vault.
>

Zie voor een overzicht van Azure Key Vault, [wat is Azure Key Vault?](key-vault-whatis.md)
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de Azure CLI-opdrachten in dit artikel, hebt u de volgende items:

* Een abonnement op Microsoft Azure Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](https://azure.microsoft.com/pricing/free-trial).
* Azure-opdrachtregelinterface versie 2.0 of hoger. Zie voor het installeren van de meest recente versie [Azure CLI installeren](/cli/azure/install-azure-cli).
* Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of het wachtwoord die u in dit artikel maakt. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=45343). Zie het ingesloten Leesmij-bestand voor instructies.

### <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Hulp met Azure platformoverschrijdende opdrachtregelinterface

In dit artikel wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal-opdrachtprompt bent).

De--help of -h parameter kan worden gebruikt om help voor specifieke opdrachten weer te geven. U kunt ook de Azure-help [opdracht] [opties]-indeling kan ook te worden gebruikt. Raadpleeg de help bij twijfel over de parameters die nodig zijn voor een opdracht. Bijvoorbeeld retourneren de volgende opdrachten alle dezelfde informatie:

```azurecli
az account set --help
az account set -h
```

U kunt ook de volgende artikelen om vertrouwd te raken met Azure Resource Manager in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Azure-CLI installeren](/cli/azure/install-azure-cli)
* [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli)

## <a name="how-to-create-a-hardened-container-a-vault-in-azure"></a>Over het maken van een geharde container (een kluis) in Azure

Kluizen zijn containers die worden ondersteund door hardware security modules worden beveiligd. Kluizen verminderen de kans op onbedoeld verlies van beveiligingsinformatie door de opslag van toepassingsgeheimen te centraliseren. Sleutelkluizen regelen en registreren ook de toegang tot alles wat erin is opgeslagen. Azure Key Vault kan het aanvragen en vernieuwen van Transport Layer Security-certificaten (TLS) verwerken, met behulp van de functies die vereist zijn voor een robuuste oplossing voor het beheren van de levenscyclus van certificaten. In de volgende stappen maakt u een kluis.

### <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Als u wilt zich interactief aanmelden, gebruik de volgende opdracht:

```azurecli
az login
```
U kunt doorgeven om aan te melden met behulp van een organisatie-account, in uw gebruikersnaam en wachtwoord.

```azurecli
az login -u username@domain.com -p password
```

Als u meer dan één abonnement hebt en u hoeft op te geven voor het gebruik, typt u het volgende als u wilt zien van de abonnementen voor uw account:

```azurecli
az account list
```

Een abonnement met de parameter subscription opgeven.

```azurecli
az account set --subscription <subscription name or ID>
```

Zie voor meer informatie over het configureren van Azure-opdrachtregelinterface voor meerdere platformen [Azure CLI installeren](/cli/azure/install-azure-cli).

### <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken

Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources in een resourcegroep gemaakt. U kunt een key vault maken in een bestaande resourcegroep. Als u een nieuwe resourcegroep gebruikt wilt, kunt u een nieuwe maken.

```azurecli
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Voor een lijst van alle mogelijke locaties type:

```azurecli
az account list-locations
``` 

### <a name="register-the-key-vault-resource-provider"></a>Registreer de resourceprovider voor Key Vault

 U ziet mogelijk de fout 'het abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.KeyVault' ' wanneer u probeert te maken van een nieuwe sleutelkluis. Als dit bericht wordt weergegeven, controleert u dat de resourceprovider van die Key Vault is geregistreerd in uw abonnement. Dit is een eenmalige bewerking voor elk abonnement.

```azurecli
az provider register -n Microsoft.KeyVault
```

### <a name="create-a-key-vault"></a>Een sleutelkluis maken

Gebruik de `az keyvault create` opdracht om een sleutelkluis te maken. Met dit script heeft drie verplichte parameters: naam van een resourcegroep, een key vault-naam en de geografische locatie.

Een nieuwe kluis maken met de naam van de **ContosoKeyVault**, in de resourcegroep **ContosoResourceGroup**, die in de **Oost-Azië** locatie, type: 

```azurecli
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

De uitvoer van deze opdracht toont eigenschappen van de sleutelkluis die u hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **Naam**: In het voorbeeld is de naam van de ContosoKeyVault. U gebruikt deze naam voor andere Key Vault-opdrachten.
* **vaultUri**: In het voorbeeld wordt de URI is https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Vanaf nog is niemand anders gemachtigd.

## <a name="adding-a-key-secret-or-certificate-to-the-key-vault"></a>Een sleutel, het geheim of het certificaat toe te voegen aan de sleutelkluis

Als u wilt dat Azure Key Vault een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `az key create` opdracht.

```azurecli
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```

Als u een bestaande sleutel in een PEM-bestand hebt, kunt u deze kunt uploaden naar Azure Key Vault. U kunt kiezen om de sleutel met software of HSM te beveiligen. Gebruik de volgende naar de sleutel van het .pem-bestand importeren en deze beveiligen met software:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'Pa$$w0rd' --protection software
```

U kunt nu verwijzen naar de sleutel die u hebt gemaakt of geüpload naar Azure Key Vault met behulp van de URI. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** naar altijd kunnen genieten van de huidige versie. Gebruik https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] voor deze specifieke versie. Bijvoorbeeld **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

Een geheim toevoegen aan de kluis, dit is een wachtwoord met de naam SQLPassword en waarvoor de waarde van Pa$ $w0rd naar Sleutelkluizen van Azure. 

```azurecli
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Verwijzen naar dit wachtwoord met behulp van de URI. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** naar altijd kunnen genieten van de huidige versie en https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id] voor deze specifieke versie. Bijvoorbeeld **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Een certificaat importeren naar de kluis met behulp van een PEM- of .pfx.

```azurecli
az keyvault certificate import --vault-name 'ContosoKeyVault' --file 'c:\cert\cert.pfx' --name 'ContosoCert' --password 'Pa$$w0rd'
```

Laten we bekijken de sleutel, een geheim of een certificaat dat u hebt gemaakt:

* Als uw sleutels, typt u: 

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Als uw geheimen, typt u: 

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

* Als certificaten, typt u: 

```azurecli
az keyvault certificate list --vault-name 'ContosoKeyVault'
```

## <a name="registering-an-application-with-azure-active-directory"></a>Een toepassing registreren bij Azure Active Directory

Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. Het is niet specifiek voor Azure Key Vault, maar dat is opgenomen, awareness. Als u wilt de app-registratie hebt voltooid, moeten uw account, de kluis en de toepassing zich in dezelfde Azure-directory.

Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren.  De eigenaar van de toepassing moet registreren in Azure Active Directory eerst. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

- Een **toepassings-ID** (ook wel bekend als de AAD-Client-ID of een toepassings-id)
- Een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). 

De toepassing moet beide waarden aan Azure Active Directory verstrekken om een token te verkrijgen. Hoe een toepassing is geconfigureerd voor een token verkrijgen afhankelijk van de toepassing. In de [Key Vault-voorbeeldtoepassing](https://www.microsoft.com/download/details.aspx?id=45343) moet de eigenaar van de toepassing deze waarden instellen in het bestand app.config.

Voor gedetailleerde stappen voor het registreren van een toepassing met Azure Active Directory moet u de artikelen met de titel controleren [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md), [portal gebruiken om te maken van een Azure-Active Directory-toepassing en service-principal die toegang hebben tot resources](../active-directory/develop/howto-create-service-principal-portal.md), en [een Azure-service-principal maken met de Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli).

Het registreren van een toepassing in Azure Active Directory:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password 'Pa$$w0rd' --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorizing-an-application-to-use-a-key-or-secret"></a>Een toepassing voor het gebruik van een sleutel of geheim autoriseren

Als u wilt toestaan dat de toepassing voor toegang tot de sleutel of geheim in de kluis, gebruikt u de `az keyvault set-policy` opdracht.

Bijvoorbeeld, als de kluisnaam van uw ContosoKeyVault is, de toepassing een appID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u toestaan dat de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, gebruik de volgende opdracht:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Voor het autoriseren van dezelfde toepassing te lezen van geheimen in uw kluis, typ de volgende opdracht:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="bkmk_KVperCLI"></a> Geavanceerde toegangsbeleid voor sleutelkluis instellen

Gebruik [az keyvault update](/cli/azure/keyvault#az-keyvault-update) om in te schakelen Geavanceerde beleidsregels voor de key vault. 

 Key Vault voor implementatie inschakelen: Hiermee kunt virtuele machines om op te halen van certificaten die zijn opgeslagen als geheimen van de kluis.
 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-deployment 'true'
 ``` 

Key Vault voor schijfversleuteling inschakelen: Vereist als u de kluis voor Azure Disk encryption.

 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-disk-encryption 'true'
 ```  

Key Vault inschakelen voor sjabloonimplementatie: Kan Resource Manager geheimen ophalen uit de kluis.
 ```azurecli 
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-template-deployment 'true'
 ```

## <a name="working-with-hardware-security-modules-hsms"></a>Werken met Hardware security modules (HSM's)

Voor extra zekerheid kunt u importeren of genereren van sleutels in hardware security modules (HSM's) die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

Deze HSM beveiligde sleutels wilt maken, moet u een kluisabonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben.

Wanneer u de Key Vault maakt, voegt u de parameter 'sku':

```azurecli
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een met HSM beveiligde sleutel:

```azurecli
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel uit een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```

Zie voor meer gedetailleerde instructies over het genereren van dit BYOK-pakket, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="deleting-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen

Als u de sleutelkluis en de sleutels of geheimen niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de `az keyvault delete` opdracht:

```azurecli
az keyvault delete --name 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```azurecli
az group delete --name 'ContosoResourceGroup'
```

## <a name="miscellaneous-azure-cross-platform-command-line-interface-commands"></a>Diverse Azure platformoverschrijdende opdrachtregelinterface opdrachten

Andere opdrachten die handig kunnen zijn voor het beheren van Azure Key Vault.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

Met deze opdracht geeft een volledige lijst met eigenschappen voor de opgegeven sleutel weer:

```azurecli
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

```azurecli
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Hier volgt een voorbeeld van hoe u een specifiek geheim verwijdert:

```azurecli
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor de volledige Azure CLI-verwijzing voor key vault-opdrachten, [naslaginformatie voor Key Vault CLI](/cli/azure/keyvault).

- Zie voor het programmeren van verwijzingen [de ontwikkelaarsgids van de Azure Key Vault](key-vault-developers-guide.md)

- Zie voor informatie over Azure Key Vault en HSM's, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).
