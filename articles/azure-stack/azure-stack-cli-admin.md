---
title: Azure CLI inschakelen voor Azure Stack-gebruikers | Microsoft Docs
description: Meer informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om te beheren en implementeren van resources in Azure Stack
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.openlocfilehash: ba8bed71d24c1b4ed71611b5cd2dfeb7800408b8
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304020"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure CLI inschakelen voor Azure Stack-gebruikers

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de CA-basiscertificaat bieden aan gebruikers van Azure Stack zodat Azure CLI kan worden gebruikt op de ontwikkeling van virtuele machines. Uw gebruikers moeten het certificaat voor het beheren van resources via de CLI.

* **De Azure Stack-CA-basiscertificaat** is vereist als gebruikers zijn met behulp van CLI op een werkstation buiten de Azure Stack Development Kit.  

* **Het eindpunt van de virtuele machine-aliassen** biedt een alias, zoals "UbuntuLTS" of "Win2012Datacenter,' die verwijst naar een installatiekopie-uitgever, aanbieding, SKU en versie als één parameter bij het implementeren van virtuele machines.  

De volgende secties wordt beschreven hoe u deze waarden ophalen.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Het Azure Stack-CA-basiscertificaat exporteren

U vindt het Azure Stack-CA-basiscertificaat in de development kit en op een tenant virtuele machine die wordt uitgevoerd in de ontwikkelomgeving van de kit. Voor het exporteren van het Azure Stack-basiscertificaat in de PEM-indeling, zich aanmelden bij uw development kit of de tenant-VM en voer het volgende script:

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

Azure Stack-operators moeten instellen met een openbaar toegankelijk eindpunt dat als host fungeert voor een virtuele machine alias-bestand. De virtuele machine alias-bestand is een JSON-bestand dat een algemene naam voor een afbeelding biedt. Deze naam is later opgegeven wanneer een virtuele machine wordt geïmplementeerd als een parameter van de Azure CLI.  

Voordat u een vermelding aan een aliasbestand toevoegen, zorg ervoor dat u [downloaden van installatiekopieën van Azure Marketplace](azure-stack-download-azure-marketplace-item.md), of [gepubliceerd van uw eigen aangepaste installatiekopie](azure-stack-add-vm-image.md). Als u een aangepaste installatiekopie publiceert, noteer de uitgever, aanbieding, SKU en versie-informatie die u hebt opgegeven tijdens het publiceren. Als een installatiekopie van de marketplace is, kunt u de informatie weergeven met behulp van de ```Get-AzureVMImage``` cmdlet.  

Een [alias-voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) met veel algemene installatiekopie aliassen beschikbaar is. U kunt die gebruiken als uitgangspunt. Dit bestand in een ruimte waarin uw CLI-clients deze bereiken kunnen host. Eén manier is het hosten van het bestand in een blob storage-account en de URL delen met uw gebruikers:

1. Download de [voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) vanuit GitHub.
2. Maak een nieuw opslagaccount in Azure Stack. Wanneer dat gebeurd, maakt u een nieuwe blob-container. Instellen van het toegangsbeleid voor 'openbare'.  
3. Upload het JSON-bestand naar de nieuwe container. Wanneer u dat hebt gedaan, kunt u de URL van de blob weergeven door de blobnaam van de selecteren en vervolgens de URL in de blob-eigenschappen te selecteren.

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
- [Verbinding maken met PowerShell](azure-stack-connect-powershell.md)
- [Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
