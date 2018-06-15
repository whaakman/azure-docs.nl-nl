---
title: Schijven op een Linux-VM met de Azure CLI 1.0 versleutelen | Microsoft Docs
description: Het coderen van schijven op een Linux-VM met de Azure CLI 1.0 en de Resource Manager-implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: d1704ba37b1d2acc595cb0c354b22bfcf1c57036
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30911677"
---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a>Versleutelen van schijven op een Linux-VM met behulp van de Azure CLI 1.0
Voor de uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven in Azure worden versleuteld in rust. Schijven worden versleuteld met behulp van de cryptografische sleutels die worden beveiligd in een Azure Sleutelkluis. U kunt het gebruik ervan controleren en beheren van deze cryptografische sleutels. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Linux-VM met de Azure CLI 1.0 en de Resource Manager-implementatiemodel.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie details snel de base-opdrachten voor het versleutelen van virtuele schijven op de virtuele machine. Meer gedetailleerde informatie en context voor elke stap u de rest van het document vindt [vanaf hier](#overview-of-disk-encryption).

U moet de [nieuwste Azure CLI 1.0](../../xplat-cli-install.md) geïnstalleerd en geregistreerd in de modus Resource Manager als volgt gebruiken:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `myKeyVault`, en `myVM`.

Eerst de provider voor Azure Sleutelkluis in uw Azure-abonnement inschakelen en een resourcegroep maken. Het volgende voorbeeld wordt een Resourcegroepnaam `myResourceGroup` in de `WestUS` locatie:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Maak een Azure Sleutelkluis. Het volgende voorbeeld wordt een Sleutelkluis met de naam `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Een cryptografische sleutel in uw Sleutelkluis maken en inschakelen voor schijfversleuteling. Het volgende voorbeeld wordt een sleutel met de naam `myKey`:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Maak een eindpunt met Azure Active Directory voor het verwerken van de verificatie en uitwisselen van cryptografische sleutels uit Sleutelkluis. De `--home-page` en `--identifier-uris` hoeft niet te worden werkelijke routeerbare adresruimte hebben. Voor het hoogste niveau van beveiliging, moeten de clientgeheimen worden gebruikt in plaats van wachtwoorden. De Azure CLI kan momenteel niet clientgeheimen genereren. Clientgeheimen kunnen alleen worden gegenereerd in de Azure portal. Het volgende voorbeeld wordt een Azure Active Directory-eindpunt met de naam `myAADApp` en maakt gebruik van een wachtwoord van `myPassword`. Geef uw eigen wachtwoord als volgt:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Opmerking de `applicationId` wordt weergegeven in de uitvoer van de voorgaande opdracht. Deze toepassing-ID wordt gebruikt in de volgende stappen uit:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Een gegevensschijf toevoegen aan een bestaande virtuele machine. Het volgende voorbeeld wordt een gegevensschijf voor een virtuele machine met de naam `myVM`:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Lees de informatie voor uw Sleutelkluis en de sleutel die u hebt gemaakt. U moet de Sleutelkluis-ID, URI en sleutel URL in de laatste stap. Het volgende voorbeeld controleert u de details voor een Sleutelkluis met de naam `myKeyVault` en de sleutel met de naam `myKey`:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Versleutelen van uw schijven als volgt invoeren van uw eigen namen van parameters in:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

De Azure CLI biedt geen uitgebreide fouten tijdens het versleutelingsproces. Raadpleeg voor aanvullende informatie voor probleemoplossing `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Als de voorgaande opdracht veel variabelen heeft en u niet veel aanwijzing krijgt van waarom het proces is mislukt, wordt een voorbeeld van een volledige opdracht zou als volgt zijn:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Ten slotte wordt de versleutelingsstatus opnieuw om te bevestigen dat de virtuele schijven nu zijn gecodeerd bekijken. Het volgende voorbeeld controleert de status van een virtuele machine met de naam `myVM` in de `myResourceGroup` resourcegroep:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op virtuele Linux-machines zijn versleuteld met behulp van rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. Een Azure Active Directory-eindpunt biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als virtuele machines van stroom in- of uitschakelen voorzien worden.

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Sleutelkluis.
2. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
3. Maak een eindpunt met Azure Active Directory met de juiste machtigingen om de cryptografiesleutel van de Azure Sleutelkluis gelezen.
4. Geef de opdracht voor het versleutelen van uw virtuele schijven, waarin de Azure Active Directory-eindpunt en de juiste cryptografische sleutel moet worden gebruikt.
5. Het Azure Active Directory-eindpunt vraagt de cryptografiesleutel van de vereiste van Azure Sleutelkluis.
6. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.

## <a name="supporting-services-and-encryption-process"></a>Ondersteuning van services en versleuteling
Schijfversleuteling is afhankelijk van de volgende extra onderdelen:

* **Azure Sleutelkluis** : wordt gebruikt ter bescherming van de cryptografische sleutels en geheimen gebruikt voor het proces van de codering/decodering schijf.
  * Als dit bestaat, kunt u een bestaande Azure Sleutelkluis. U hoeft niet te reserveren van een Sleutelkluis voor het versleutelen van schijven.
  * Afzonderlijke administratieve grenzen en sleutel zichtbaarheid, kunt u een speciale Sleutelkluis.
* **Azure Active Directory** -het veilig uitwisselen van vereiste cryptografische sleutels en verificatie voor de aangevraagde acties worden verwerkt.
  * Doorgaans kunt u een bestaand Azure Active Directory-exemplaar waarin de toepassing.
  * De toepassing is van een eindpunt voor de Sleutelkluis en de virtuele Machine services aan te vragen en de juiste cryptografische sleutels ophalen uitgegeven. U ontwikkelt een werkelijke toepassing die is geïntegreerd met Azure Active Directory niet.

## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor de schijfversleuteling:

* De volgende Linux-server SKU's - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES) en Red Hat Enterprise Linux.
* Alle resources (zoals Sleutelkluis, een opslagaccount en een VM) moeten zich in dezelfde Azure-regio en abonnement.
* Standard A, D, DS, G en GS-serie biedt virtuele machines.

Schijfversleuteling is momenteel niet ondersteund in de volgende scenario's:

* Basisstaffel virtuele machines.
* Virtuele machines gemaakt met behulp van het klassieke implementatiemodel.
* OS-schijfversleuteling op Linux VM's uitschakelen.
* Bijwerken van de cryptografische sleutels op een Linux-VM al is versleuteld.

## <a name="create-the-azure-key-vault-and-keys"></a>De Azure Sleutelkluis en de sleutels maken
Als u de rest van deze handleiding, moet u de [nieuwste Azure CLI 1.0](../../xplat-cli-install.md) geïnstalleerd en geregistreerd in de modus Resource Manager als volgt gebruiken:

```azurecli
azure config mode arm
```

In de opdrachtvoorbeelden vervangen door alle voorbeeldparameters uw eigen namen, locatie en sleutelwaarden. Een conventie voor gebruik van de volgende voorbeelden `myResourceGroup`, `myKeyVault`, `myAADApp`, enzovoort.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Azure Sleutelkluis kunt opslaan sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig implementeert in uw toepassingen en services. Voor versleuteling van de virtuele schijf, kunt u Sleutelkluis gebruiken voor het opslaan van een cryptografische sleutel die wordt gebruikt voor het versleutelen of ontsleutelen van de virtuele schijven.

De provider Azure Sleutelkluis in uw Azure-abonnement inschakelen en vervolgens een resourcegroep maken. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

De Azure Sleutelkluis die de cryptografische sleutels en de bijbehorende compute-bronnen zoals opslag en de virtuele machine zelf moet zich bevinden in dezelfde regio. Het volgende voorbeeld wordt een Azure Sleutelkluis met de naam `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

U kunt met behulp van software of Hardware Security Model (HSM) protection cryptografiesleutels opslaan. Een HSM, moet een premium Sleutelkluis. Er is een extra kosten voor het maken van een premium Sleutelkluis in plaats van standaard Sleutelkluis waarmee softwarematige beveiligde sleutels worden opgeslagen. Voor het maken van een premium Sleutelkluis in de vorige stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld wordt de softwarematige beveiligde sleutels omdat er een standaard Sleutelkluis hebt gemaakt.

Voor beide beveiligingsmodellen moet het Azure-platform toegang om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven worden verleend. Een coderingssleutel binnen uw Sleutelkluis maken en inschakelen voor gebruik met versleuteling van de virtuele schijf. Het volgende voorbeeld wordt een sleutel met de naam `myKey` en schakelt u deze voor de schijfversleuteling:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>De Azure Active Directory-toepassing maken
Wanneer virtuele schijven worden versleuteld en ontsleuteld, kunt u een eindpunt gebruiken om de verificatie- en uitwisselen van cryptografische sleutels uit Sleutelkluis te verwerken. Dit eindpunt, een Azure Active Directory-toepassing, kunt de Azure-platform om aan te vragen van de juiste cryptografische sleutels namens de virtuele machine. Een standaardexemplaar van de Azure Active Directory is beschikbaar in uw abonnement, hoewel veel organisaties hebben speciale mappen van Azure Active Directory.

Als u niet een volledige Azure Active Directory-toepassing maakt de `--home-page` en `--identifier-uris` parameters in het volgende voorbeeld hoeft niet te worden werkelijke routeerbare adresruimte hebben. Het volgende voorbeeld bevat ook een geheim op basis van wachtwoorden in plaats van genereren van sleutels uit binnen de Azure-portal. Als deze tijd kan niet genereren van sleutels worden uitgevoerd vanuit de Azure CLI.

Uw Azure Active Directory-toepassing maken. Het volgende voorbeeld wordt een toepassing met de naam `myAADApp` en maakt gebruik van een wachtwoord van `myPassword`. Geef uw eigen wachtwoord als volgt:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Noteer de `applicationId` die wordt geretourneerd in de uitvoer van de voorgaande opdracht. Deze toepassing-ID wordt gebruikt in een aantal van de resterende stappen. Maak vervolgens een service principal name (SPN) zodat de toepassing toegankelijk binnen uw omgeving. Om deel te versleutelen of ontsleutelen van virtuele schijven, moeten de machtigingen voor de cryptografische sleutel die is opgeslagen in de Sleutelkluis toe te staan van de Azure Active Directory-toepassing te lezen van de sleutels worden ingesteld.

De SPN maken en de juiste machtigingen als volgt instellen:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Toevoegen van een virtuele schijf en versleutelingsstatus controleren
Voor het versleutelen van daadwerkelijk bepaalde virtuele schijven, kunt u een schijf toevoegen aan een bestaande virtuele machine. Een gegevensschijf 5Gb als volgt naar een bestaande virtuele machine toevoegen:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

De virtuele schijven zijn momenteel niet versleuteld. Controleer de huidige versleutelingsstatus van uw virtuele machine als volgt:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuele schijven versleutelen
Als u wilt coderen nu de virtuele schijven, brengt u samen de eerdere onderdelen:

1. Geef de Azure Active Directory-toepassing en het wachtwoord.
2. Geef de Sleutelkluis voor het opslaan van de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels moet worden gebruikt voor de daadwerkelijke versleuteling en ontsleuteling.
4. Geef op of u wilt versleutelen schijf met het besturingssysteem, de gegevensschijven of alle.

U kunt de details bekijken voor uw Azure Sleutelkluis en de sleutel die u hebt gemaakt, als u de Sleutelkluis-ID URI, nodig en vervolgens sleutel URL in de laatste stap:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Versleutelen van uw virtuele schijven met behulp van de uitvoer van de `azure keyvault show` en `azure keyvault key show` opdrachten als volgt:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Als de voorgaande opdracht veel variabelen heeft, is het volgende voorbeeld de volledige opdracht ter referentie:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

De Azure CLI biedt geen uitgebreide fouten tijdens het versleutelingsproces. Raadpleeg voor aanvullende informatie voor probleemoplossing `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` op de virtuele machine die u versleutelt.

Ten slotte kunt de versleuteling opnieuw om te bevestigen dat de virtuele schijven nu zijn gecodeerd bekijken:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Toevoegen van extra gegevensschijven
Nadat u uw gegevensschijven hebt versleuteld, kunt u later extra virtuele schijven toevoegen aan uw virtuele machine en ook versleutelen. Bij het uitvoeren van de `azure vm enable-disk-encryption` opdracht, verhogen van de reeks versie met behulp van de `--sequence-version` parameter. Deze reeks versieparameter kunt u herhaalde bewerkingen uitvoeren op dezelfde virtuele machine.

U kunt bijvoorbeeld een tweede virtuele schijf toevoegen aan uw virtuele machine als volgt:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Opnieuw uitvoeren van de opdracht voor het versleutelen van de virtuele schijven, het toevoegen van deze tijd de `--sequence-version` parameter en de waarde van de eerste keer uitvoert als volgt oplopend in stappen:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van Azure Sleutelkluis, met inbegrip van verwijderen van de cryptografische sleutels en kluizen, [Key Vault beheren met CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
