---
title: Azure Cloud Shell-functies | Microsoft Docs
description: Overzicht van de functies van Bash in Azure Cloud Shell
services: Azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: damaerte
ms.openlocfilehash: 6b5f0e96b90ee0515c0a86f41c6ee2161d6c54a6
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752712"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Functies en hulpprogramma's voor Azure Cloud Shell

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Cloud Shell wordt uitgevoerd op `Ubuntu 16.04 LTS`.

## <a name="features"></a>Functies

### <a name="secure-automatic-authentication"></a>Automatische verificatie beveiligen

Cloudshell veilig en automatisch toegang tot het worden geverifieerd voor de Azure CLI en Azure PowerShell.

### <a name="home-persistence-across-sessions"></a>$HOME persistentie in verschillende sessies

Als u wilt bestanden in verschillende sessies persistent, helpt Cloud Shell u bij het koppelen van een Azure-bestandsshare op de eerste keer opstarten.
Als voltooid, Cloud Shell automatisch uw opslag koppelt (gekoppeld als `$HOME\clouddrive`) voor alle toekomstige sessies.
Bovendien uw `$HOME` directory worden opgeslagen als een .img in uw Azure-bestandsshare.
Bestanden buiten `$HOME` en status van de machine zijn niet blijven behouden tussen sessies. Gebruik de aanbevolen procedures bij het opslaan van geheimen zoals SSH-sleutels. Services, zoals [Azure Key Vault-zelfstudies voor setup hebt](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites).

[Meer informatie over het behoud van bestanden in de Cloud Shell.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure-station (Azure:)

PowerShell in Cloud Shell start u in Azure-station (`Azure:`).
De Azure-station kunt u eenvoudige detectie en navigatie van Azure-resources, zoals Compute, netwerk, opslag enz. die vergelijkbaar is met het bestandssysteem navigatie.
U kunt echter ook doorgaan met de vertrouwde [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure) voor het beheren van deze resources, ongeacht het station dat u zich in.
Eventuele wijzigingen in de Azure-resources, hetzij rechtstreeks in Azure portal of via Azure PowerShell-cmdlets, worden weerspiegeld in de Azure-station.  U kunt uitvoeren `dir -Force` vernieuwen van uw resources.

![](media/features-powershell/azure-drive.png)

### <a name="manage-exchange-online"></a>Exchange Online beheren

PowerShell in Cloud Shell bevat een persoonlijke build van de Exchange Online-module.  Voer `Connect-EXOPSSession` om op te halen van uw Exchange-cmdlets.

![](media/features-powershell/exchangeonline.png)

 Voer `Get-Command -Module tmp_*` uit.
> [!NOTE]
> De modulenaam moet beginnen met `tmp_`, als u modules met hetzelfde voorvoegsel hebt geïnstalleerd, wordt ook de cmdlets worden opgehaald. 

![](media/features-powershell/exchangeonlinecmdlets.png)

### <a name="deep-integration-with-open-source-tooling"></a>Diepe integratie met open source-programma 's

Cloudshell bevat vooraf geconfigureerde verificatie voor open-source hulpprogramma's zoals Terraform, Ansible en Chef inspectie mogelijk. Probeer het nu uit de voorbeeld-scenario's.

## <a name="tools"></a>Hulpprogramma's

|Category   |Name   |
|---|---|
|Hulpprogramma's voor Linux            |Bash<br> zsh<br> Sh<br> tmux<br> graven<br>               |
|Azure-hulpprogramma 's            |[Azure CLI](https://github.com/Azure/azure-cli) en [Azure klassieke CLI](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#writing-your-first-azcopy-command)<br> [Service Fabric-CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)<br> [blobxfer](https://github.com/Azure/blobxfer)|
|Teksteditors           |code (Cloud Shell-editor)<br> vim<br> nano<br> emacs    |
|Broncodebeheer         |git                    |
|Hulpprogramma's bouwen            |Maken<br> maven<br> npm<br> pip         |
|Containers             |[Docker Machine](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS CLI](https://github.com/dcos/dcos-cli)         |
|Databases              |MySQL-client<br> PostgreSql-client<br> [sqlcmd Utility](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Overige                  |iPython Client<br> [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Chef inspectie mogelijk](https://www.chef.io/inspec/)|

## <a name="language-support"></a>Taalondersteuning

|Taal   |Versie   |
|---|---|
|.NET Core  |2.0.0       |
|Aan de slag         |1.9        |
|Java       |1.8        |
|Node.js    |8.9.4      |
|PowerShell |[6.2.0](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 en 3.5 (standaard)|

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md) <br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md) <br>
[Meer informatie over Azure CLI](https://docs.microsoft.com/cli/azure/) <br>
[Meer informatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/) <br>
