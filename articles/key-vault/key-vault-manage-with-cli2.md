---
title: Beheren van Azure Sleutelkluis met CLI | Microsoft Docs
description: Gebruik dit artikel bij het automatiseren van algemene taken in de Sleutelkluis met behulp van de CLI 2.0
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: barclayn
ms.openlocfilehash: 8c2501b5e89e81709de074c0b0c93b317ecebd7b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316591"
---
# <a name="manage-key-vault-using-cli-20"></a>Beheren met CLI 2.0 Sleutelkluis

In dit artikel wordt beschreven hoe u aan de slag met Azure Sleutelkluis met behulp van de Azure CLI 2.0. U kunt informatie bekijken over:
- Het maken van een geharde container (een kluis) in Azure
- Het opslaan en beheren van de cryptografische sleutels en geheimen in Azure. 
- Voor een kluis maken met behulp van de Azure CLI.
- Maken van een sleutel of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt. 
- Hoe een toepassing kunt gebruiken de gemaakte sleutel of het wachtwoord.

Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.


> [!NOTE]
> In dit artikel bevat geen instructies voor het schrijven van de Azure-toepassing of een van de stappen bevat, die wordt beschreven hoe een toepassing gebruik van een sleutel of geheim in de sleutelkluis toestemming geeft.
>

Zie voor een overzicht van Azure Sleutelkluis [wat is Azure Sleutelkluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van de Azure CLI-opdrachten in dit artikel, hebt u de volgende items:

* Een abonnement op Microsoft Azure Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](https://azure.microsoft.com/pricing/free-trial).
* Opdrachtregelinterface versie 2.0 of hoger. Zie het installeren van de meest recente versie [installeren en configureren van de Azure platformoverschrijdende opdrachtregelinterface 2.0](/cli/azure/install-azure-cli).
* Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of het wachtwoord die u in dit artikel maakt. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie voor instructies het ingesloten Leesmij-bestand.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Help-informatie met Azure platformoverschrijdende opdrachtregelinterface
In dit artikel wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal-opdrachtprompt bent).

De--help- of -h parameter kan worden gebruikt om help voor specifieke opdrachten weer te geven. U kunt ook de Azure help [opdracht] [opties] indeling kan ook te worden gebruikt. Raadpleeg de help bij twijfel over de parameters die nodig is voor een opdracht. Bijvoorbeeld de volgende opdrachten uit alle dezelfde informatie geretourneerd:

```azurecli-interactive
az account set --help
az account set -h
```

U kunt ook de volgende artikelen om vertrouwd te raken met Azure Resource Manager in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Azure CLI installeren](/cli/azure/install-azure-cli)
* [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Meld u interactief met de volgende opdracht:

```azurecli
az login
```
Meld u bij het gebruik van een organisatie-account, kunt u uw gebruikersnaam en wachtwoord doorgeven.

```azurecli
az login -u username@domain.com -p password
```

Als u meer dan één abonnement hebt en wilt opgeven die wordt gebruikt, typt u het volgende als u wilt zien van de abonnementen voor uw account:

```azurecli
az account list
```

Geef een abonnement met de abonnement-parameter.

```azurecli
az account set --subscription <subscription name or ID>
```

Zie voor meer informatie over het configureren van Azure platformoverschrijdende opdrachtregelinterface [Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken
Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. U kunt een sleutelkluis maken in een bestaande resourcegroep. Als u een nieuwe resourcegroep te gebruiken wilt, kunt u een nieuwe maken.

```azurecli
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Voor een lijst met alle mogelijke locaties type:

```azurecli
az account list-locations
``` 

## <a name="register-the-key-vault-resource-provider"></a>De registerbronprovider is Sleutelkluis
 Mogelijk ziet u de fout 'het abonnement is niet geregistreerd voor gebruik van de naamruimte 'Microsoft.KeyVault' ' wanneer u probeert om een nieuwe sleutelkluis te maken. Als dit bericht wordt weergegeven, zorg er dan voor dat die Sleutelkluis resourceprovider is geregistreerd in uw abonnement. Dit is een eenmalige bewerking voor elk abonnement.

```azurecli
az provider register -n Microsoft.KeyVault
```


## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Gebruik de `az keyvault create` opdracht om een sleutelkluis te maken. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een sleutelkluis en de geografische locatie.

Een nieuwe kluis maken met de naam **ContosoKeyVault**, in de resourcegroep **ContosoResourceGroup**, tijdelijk in de **Oost-Azië** locatie, type: 

```azurecli
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **naam**: In het voorbeeld is de naam van de ContosoKeyVault. U gebruikt deze naam voor andere Sleutelkluis-opdrachten.
* **vaultUri**: In het voorbeeld wordt de URI is https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Vanaf nog is niemand anders gemachtigd.

## <a name="add-a-key-secret-or-certificate-to-the-key-vault"></a>Een sleutel, geheim of certificaat toevoegen aan de sleutelkluis

Als u wilt dat Azure Sleutelkluis een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `az key create` opdracht.

```azurecli
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```

Als u een bestaande sleutel in een .pem-bestand hebt, kunt u het uploaden naar Azure Sleutelkluis. U kunt kiezen om de sleutel met de software of HSM te beveiligen. Gebruik de volgende voor de sleutel niet uit het .pem-bestand importeren en te beveiligen met software:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'Pa$$w0rd' --protection software
```

U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure Sleutelkluis, met behulp van de URI verwijzen. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** voor altijd de huidige versie. Gebruik https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] voor deze specifieke versie. Bijvoorbeeld **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

Een geheim toevoegen aan de kluis, dit is een wachtwoord met de naam SQLPassword en dat de waarde van Pa$ $w0rd voor Azure sleutel kluizen. 

```azurecli
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Verwijzen naar dit wachtwoord met behulp van de URI. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** altijd ophalen van de huidige versie en https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id] voor deze specifieke versie. Bijvoorbeeld **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Een certificaat importeren naar de kluis met een .pem of .pfx.

```azurecli
az keyvault certificate import --vault-name 'ContosoKeyVault' --file 'c:\cert\cert.pfx' --name 'ContosoCert' --password 'Pa$$w0rd'
```

We bekijken de sleutel, het geheim of het certificaat dat u hebt gemaakt:

* Als u wilt uw sleutels weergeven, typt u: 

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Als u wilt uw geheimen weergeven, typt u: 

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

* Als certificaten wilt bekijken, typt u: 

```azurecli
az keyvault certificate list --vault-name 'ContosoKeyVault'
```

## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory
Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. Het is niet specifiek zijn voor Azure Sleutelkluis maar opgenomen, voor awareness. Voor het voltooien van de registratie van de app, moeten uw account, de kluis en de toepassing zich in dezelfde Azure-directory.

Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren.  De eigenaar van de toepassing moet registreren bij Azure Active Directory eerst. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

- Een **toepassings-ID** (ook wel bekend als de AAD Client-ID of een toepassings-id)
- Een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). 

De toepassing moet beide waarden aan Azure Active Directory verstrekken om een token te verkrijgen. Hoe een toepassing is geconfigureerd voor het ophalen van een token afhankelijk van de toepassing. In de [Key Vault-voorbeeldtoepassing](https://www.microsoft.com/download/details.aspx?id=45343) moet de eigenaar van de toepassing deze waarden instellen in het bestand app.config.

Voor gedetailleerde stappen voor het registreren van een toepassing met Azure Active Directory rekening met de artikelen [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md), [portal gebruik maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md), en [een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

Een toepassing in Azure Active Directory registreren:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password 'Pa$$w0rd' --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing toestemming geven om de sleutel of het geheim te gebruiken

Voor het autoriseren van de toepassing toegang krijgen tot de sleutel of geheim in de kluis, gebruikt u de `az keyvault set-policy` opdracht.

Als de kluisnaam van uw ContosoKeyVault is, de toepassing een appID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, bijvoorbeeld de volgende opdracht gebruiken:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Voor het autoriseren van dezelfde toepassing te lezen van geheimen in uw kluis, typ de volgende opdracht:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="bkmk_KVperCLI"></a> Sleutelkluis set geavanceerde toegangsbeleid 
Gebruik [az keyvault update](/cli/azure/keyvault#az-keyvault-update) om in te schakelen Geavanceerde beleidsregels voor de sleutelkluis. 

 Sleutelkluis voor implementatie inschakelen: kan de virtuele machine voor het ophalen van certificaten die zijn opgeslagen als geheimen van de kluis.
 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-deployment 'true'
 ``` 

Sleutelkluis inschakelen voor schijfversleuteling: bij gebruik van de kluis voor Azure Disk encryption vereist.

 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-disk-encryption 'true'
 ```  

Sleutelkluis inschakelen voor de sjabloonimplementatie van: Resource Manager kunt geheimen ophalen uit de kluis.
 ```azurecli 
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-template-deployment 'true'
 ```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u een Hardware Security Module (HSM) wilt gebruiken

U kunt voor de zekerheid importeren of genereren van sleutels van hardware security modules (HSM's) die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

U moet een kluis-abonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben voor het maken van deze met HSM beveiligde sleutels.

Wanneer u de keyvault maakt, voegt u de parameter 'sku':

```azurecli
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een HSM beveiligde sleutel:

```azurecli
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel van een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```

Zie voor meer instructies over hoe dit BYOK-pakket te genereren gedetailleerde, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen

Als u de sleutelkluis en de sleutels of geheimen niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de `az keyvault delete` opdracht:

```azurecli
az keyvault delete --name 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```azurecli
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere opdrachten Azure platformoverschrijdende opdrachtregelinterface

Overige opdrachten die handig kunnen voor het beheren van Azure Sleutelkluis.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

Deze opdracht wordt een volledige lijst met eigenschappen voor de opgegeven sleutel weergegeven:

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

- Zie voor een volledig overzicht van Azure CLI voor sleutelkluis-opdrachten, [Sleutelkluis CLI verwijzing](/cli/azure/keyvault).

- Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.

- Zie voor informatie over Azure Sleutelkluis en HSM's, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).
