---
title: Azure CLI inschakelen voor Azure Stack-gebruikers | Microsoft Docs
description: Meer informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om te beheren en implementeren van resources in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 01/15/2019
ms.author: mabrigg
ms.lastreviewed: 01/15/2019
ms.openlocfilehash: 0d6a93b396e90f3bc27a1eb879556d774075c86f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243297"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure CLI inschakelen voor Azure Stack-gebruikers

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de CA-basiscertificaat bieden aan gebruikers van Azure Stack zodat Azure CLI kan worden gebruikt op de ontwikkeling van virtuele machines. Uw gebruikers moeten het certificaat voor het beheren van resources via de CLI.

 - **De Azure Stack-CA-basiscertificaat** is vereist als gebruikers zijn met behulp van CLI op een werkstation buiten de Azure Stack Development Kit.  

 - **Het eindpunt van de virtuele machine-aliassen** biedt een alias, zoals "UbuntuLTS" of "Win2012Datacenter,' die verwijst naar een installatiekopie-uitgever, aanbieding, SKU en versie als één parameter bij het implementeren van virtuele machines.  

De volgende secties wordt beschreven hoe u deze waarden ophalen.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Het Azure Stack-CA-basiscertificaat exporteren

Als u van een geïntegreerd systeem gebruikmaakt, moet u niet de CA-basiscertificaat te exporteren. U moet de CA-basiscertificaat op een Azure Stack Development Kit (ASDK) te exporteren.

Meld u aan voor het exporteren van het basiscertificaat ASDK in PEM-indeling, en voer het volgende script:

```powershell
$label = "<Your Azure Stack CA root certificate name>"
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

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Instellen van het eindpunt van de virtuele machine-aliassen

Azure Stack-operators moeten instellen met een openbaar toegankelijk eindpunt dat als host fungeert voor een virtuele machine alias-bestand. De virtuele machine alias-bestand is een JSON-bestand dat een algemene naam voor een afbeelding biedt. U gebruikt de naam bij het implementeren van een virtuele machine als een parameter van de Azure CLI.  

Voordat u een vermelding aan een aliasbestand toevoegen, zorg ervoor dat u [downloaden van installatiekopieën van Azure Marketplace](azure-stack-download-azure-marketplace-item.md) of [gepubliceerd van uw eigen aangepaste installatiekopie](azure-stack-add-vm-image.md). Als u een aangepaste installatiekopie publiceert, noteer de uitgever, aanbieding, SKU en versie-informatie die u hebt opgegeven tijdens het publiceren. Als een installatiekopie van de marketplace is, kunt u de informatie weergeven met behulp van de ```Get-AzureVMImage``` cmdlet.  

Een [alias-voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) met veel algemene installatiekopie aliassen beschikbaar is. U kunt die gebruiken als uitgangspunt. Dit bestand in een ruimte waarin uw CLI-clients deze bereiken kunnen host. Eén manier is het hosten van het bestand in een blob storage-account en de URL delen met uw gebruikers:

1. Download de [voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) vanuit GitHub.
2. Maak een opslagaccount in Azure Stack. Wanneer u dat hebt gedaan, maakt u een blob-container. Instellen van het toegangsbeleid voor 'openbare'.  
3. Upload het JSON-bestand naar de nieuwe container. Wanneer u dat hebt gedaan, kunt u de URL van de blob weergeven. Selecteer de blobnaam van de en vervolgens de URL in de blob-eigenschappen te selecteren.

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
- [Verbinding maken met PowerShell](azure-stack-connect-powershell.md)
- [Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
