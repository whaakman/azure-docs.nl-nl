---
title: Azure CLI inschakelen voor gebruikers van de Azure-Stack | Microsoft Docs
description: Informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) kunt beheren en implementeren van resources op Azure-Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: mabrigg
ms.openlocfilehash: d0103d211608514848da7d789d32d37d8385f33f
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247853"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure CLI inschakelen voor Azure-Stack-gebruikers

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Er zijn niet alle Azure-Stack operator-specifieke taken die u uitvoeren kunt met behulp van Azure CLI. Maar voordat gebruikers van resources via CLI beheren kunnen, Azure Stack operators ze moeten opgeven met de volgende opties:

* **De Azure-Stack-CA-basiscertificaat** is vereist als u gebruikers vanaf een werkstation buiten de Stack Development Kit van Azure CLI gebruiken.  

* **De virtuele machine aliassen eindpunt** biedt een alias, zoals 'UbuntuLTS' of 'Win2012Datacenter,' dat verwijst naar een installatiekopie-uitgever, aanbieding, SKU en versie als een enkele parameter bij het implementeren van virtuele machines.  

De volgende secties wordt beschreven hoe u deze waarden.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exporteer het Azure-Stack-CA-basiscertificaat

De Azure-Stack-CA-basiscertificaat is beschikbaar op de development kit en op een tenant virtuele machine die wordt uitgevoerd vanuit de ontwikkelomgeving kit. Als u wilt exporteren van het basiscertificaat van de Azure-Stack in PEM-indeling, aanmelden bij uw development kit of de virtuele machine van de tenant en voer het volgende script:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Het eindpunt van de aliassen virtuele machine instellen

Azure Stack-operators moeten een openbaar toegankelijk eindpunt dat als host fungeert voor een virtuele machine alias-bestand instellen. De virtuele machine alias-bestand is een JSON-bestand waarmee u een algemene naam voor een afbeelding. Die naam is vervolgens opgegeven als een virtuele machine wordt geïmplementeerd als een parameter van de Azure CLI.  

Voordat u een vermelding aan een aliasbestand toevoegen, zorg ervoor dat u [afbeeldingen downloaden vanuit Azure Marketplace](azure-stack-download-azure-marketplace-item.md), of [gepubliceerd van uw eigen aangepaste installatiekopie](azure-stack-add-vm-image.md). Als u een aangepaste installatiekopie publiceert, noteer de uitgever, aanbieding, SKU en versie-informatie die u hebt opgegeven tijdens de publicatie. Als u een installatiekopie van de marketplace is, kunt u de informatie weergeven met behulp van de ```Get-AzureVMImage``` cmdlet.  

Een [alias voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) met veel voorkomende installatiekopie aliassen beschikbaar is. U kunt die gebruiken als uitgangspunt. Dit bestand in een ruimte waarop uw CLI-clients deze bereiken kunnen host. Een manier is het hosten van het bestand in een blob storage-account en de URL deelt met uw gebruikers:

1. Download de [voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) vanuit GitHub.
2. Maak een nieuw opslagaccount in Azure-Stack. Wanneer u dat hebt gedaan, maakt u een nieuwe blob-container. Stel het toegangsbeleid worden 'openbare'.  
3. Het JSON-bestand uploaden naar de nieuwe container. Wanneer u dat hebt gedaan, kunt u de URL van de blob weergeven door te selecteren van de blob-naam en vervolgens de URL in de blob-eigenschappen te selecteren.

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

- [Verbinding maken met PowerShell](azure-stack-connect-powershell.md)

- [Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
