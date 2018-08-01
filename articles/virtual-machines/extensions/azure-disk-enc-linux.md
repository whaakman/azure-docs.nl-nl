---
title: Azure Disk Encryption voor Linux | Microsoft Docs
description: Azure Disk Encryption implementeert voor Linux op een virtuele machine met behulp van een VM-extensie.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: danis
ms.openlocfilehash: adf0ce6c8424d2350578d9bfd19c70ebb15fc473
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389861"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Azure Disk Encryption voor Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>Overzicht

Azure Disk Encryption maakt gebruik van het subsysteem dm-crypt in Linux voor volledige schijfversleuteling op [Selecteer Azure Linux-distributies](https://aka.ms/adelinux).  Deze oplossing is geïntegreerd met Azure Key Vault om sleutels en geheimen te beheren.

## <a name="prerequisites"></a>Vereisten

Zie voor een volledige lijst met vereisten, [vereisten voor Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Besturingssysteem

Azure Disk Encryption wordt momenteel ondersteund in select-distributies en versies.  Zie de [Veelgestelde vragen over Azure Disk Encryption](../../security/azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) voor een lijst van Linux-distributies die worden ondersteund.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Azure Disk Encryption voor Linux is verbinding met Internet vereist voor toegang tot Active Directory, Key Vault, opslag- en eindpunten voor het beheer van pakket.  Zie voor meer informatie, [vereisten voor Azure Disk Encryption](../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schema"></a>Extensieschema

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Azure.Security | tekenreeks |
| type | AzureDiskEncryptionForLinux | tekenreeks |
| typeHandlerVersion | 0.1, 1.1 (VMSS) | int |
| AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| AADClientSecret | wachtwoord | tekenreeks |
| AADClientCertificate | vingerafdruk | tekenreeks |
| DiskFormatQuery | {"dev_path": "", "naam": "","file_system": ""} | JSON-woordenlijst |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | tekenreeks | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | tekenreeks |
| KeyEncryptionKeyURL | url | tekenreeks |
| KeyVaultURL | url | tekenreeks |
| Wachtwoordzin | wachtwoord | tekenreeks | 
| SequenceVersion | uniqueidentifier | tekenreeks |
| VolumeType | OS-, gegevens, alle | tekenreeks |

## <a name="template-deployment"></a>Sjabloonimplementatie

Zie voor een voorbeeld van sjabloonimplementatie [versleuteling inschakelen op een actieve Linux-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

Instructies vindt u in de meest recente [documentatie van Azure CLI](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Voor het oplossen van problemen, raadpleegt u de [probleemoplossingsgids voor Azure Disk Encryption](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/community/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over VM-extensies, [extensies en functies voor Linux virtuele machines](features-linux.md).