---
title: Azure Disk Encryption voor Windows | Microsoft Docs
description: Azure Disk Encryption implementeert op een Windows virtuele machine met behulp van een VM-extensie.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 46699fb1add42d23a11234d5cd05e4a9627a91fd
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56983462"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Azure Disk Encryption voor Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Overzicht

Azure Disk Encryption maakt gebruik van BitLocker voor volledige schijfversleuteling op Azure virtual machines waarop Windows wordt uitgevoerd.  Deze oplossing is geïntegreerd met Azure Key Vault om sleutels en geheimen in uw key vault-abonnement te beheren. 

## <a name="prerequisites"></a>Vereisten

Zie voor een volledige lijst met vereisten, [vereisten voor Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Besturingssysteem

Zie voor een lijst van momenteel Windows-versies, [vereisten voor Azure Disk Encryption](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>Internetconnectiviteit

Azure Disk Encryption is verbinding met Internet vereist voor toegang tot Active Directory, Key Vault, opslag- en eindpunten voor het beheer van pakket.  Zie voor meer informatie over instellingen voor netwerkbeveiliging, [vereisten voor Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

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
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KekVaultResourceId": "[keyVaultResourceID]",
      
      "KeyVaultURL": "[keyVaultURL]",
          "KeyVaultResourceId": "[keyVaultResourceID]",

      "EncryptionOperation": "[encryptionOperation]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Uitgever | Microsoft.Azure.Security | string |
| type | AzureDiskEncryptionForWindows| string |
| typeHandlerVersion | 1.0, 1.1, 2.2 (VMSS) | int |
| (optioneel) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (optional) AADClientSecret | wachtwoord | string |
| (optioneel) AADClientCertificate | vingerafdruk | string |
| EncryptionOperation | EnableEncryption | string | 
| KeyEncryptionAlgorithm | RSA-OAEP, RSA1_5 | string |
| KeyEncryptionKeyURL | url | string |
| KeyVaultResourceId | resource uri | string |
| KekVaultResourceId | resource uri | string |
| KeyVaultURL | url | string |
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS-, gegevens, alle | string |

## <a name="template-deployment"></a>Sjabloonimplementatie
Zie voor een voorbeeld van sjabloonimplementatie [ een nieuwe versleutelde Windows-VM maken van galerijafbeelding](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

Instructies vindt u in de meest recente [documentatie van Azure CLI](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Raadpleeg de [probleemoplossingsgids voor Azure Disk Encryption](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/community/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over extensies [extensies voor virtuele machines en functies voor Windows](features-windows.md).
