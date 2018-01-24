---
title: Bash in Azure Cloud Shell-onderdelen | Microsoft Docs
description: Overzicht van de functies van Bash in de Azure-Cloud-Shell
services: Azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: juluk
ms.openlocfilehash: a6627ab6febc763ae3f1cd464f26ad641f7c717d
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="features--tools-for-bash-in-azure-cloud-shell"></a>Functies en hulpprogramma's voor Bash in de Azure-Cloud-Shell

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Functies en hulpprogramma's [PowerShell](features-powershell.md) is ook beschikbaar.

In de Cloud Shell wordt uitgevoerd op Bash `Ubuntu 16.04 LTS`.

## <a name="features"></a>Functies

### <a name="secure-automatic-authentication"></a>Automatische verificatie beveiligen

Bash in de Cloud-Shell verifieert veilig en automatisch accounttoegang voor Azure CLI 2.0.

### <a name="ssh-into-azure-linux-virtual-machines"></a>SSH in Linux Azure virtuele machines

Maken van een Linux-VM van Azure CLI 2.0 kunt maken van een standaard SSH-sleutel en plaats deze in uw `$Home` directory. Brengen van SSH-sleutels `$Home` kunnen rechtstreeks SSH-verbindingen naar Azure Linux VM's rechtstreeks vanuit de Cloud-Shell. Sleutels zijn ondergebracht in acc_<user>.img in uw bestandsshare gebruik aanbevolen procedures wanneer met behulp van of toegang tot de bestandsshare of sleutels delen.

### <a name="home-persistence-across-sessions"></a>$Home persistentie over de sessies

Om te blijven behouden bestanden over de sessies, helpt Cloud Shell u bij het koppelen van een Azure-bestandsshare op de eerste keer opstarten.
Zodra de voltooid, Cloud Shell uw opslag automatisch koppelen (gekoppeld als `$Home\clouddrive`) voor alle toekomstige sessies.
Bovendien in Bash in de Cloud-Shell uw `$Home` directory wordt bewaard als een .img in uw Azure-bestandsshare.
Bestanden buiten `$Home` en status van de machine blijven niet bestaan tussen sessies.

[Meer informatie over persistent maken van de bestanden in Bash in de Cloud-Shell.](persisting-shell-storage.md)

## <a name="tools"></a>Hulpprogramma's

|Category   |Naam   |
|---|---|
|Linux-hulpprogramma 's            |Bash<br> servicel<br> tmux<br> dig<br>               |
|Azure-hulpprogramma 's            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) en [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard) <br> [Service Fabric-CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) <br> [blobxfer](https://github.com/Azure/blobxfer#blobxfer) |
|Teksteditors           |VIM<br> nano<br> emacs       |
|Resourcebeheer         |GIT                    |
|Hulpprogramma's van build            |Maken<br> maven<br> npm<br> PIP         |
|Containers             |[Docker CLI](https://github.com/docker/cli)/[Docker-Machine](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS CLI](https://github.com/dcos/dcos-cli)         |
|Databases              |MySQL-client<br> PostgreSql-client<br> [Sqlcmd-hulpprogramma](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Overige                  |iPython Client<br> [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/) |

## <a name="language-support"></a>Taalondersteuning

|Taal   |Versie   |
|---|---|
|.NET       |2.0.0       |
|Aan de slag         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|PowerShell |[6.0 (beta)](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 en 3.5 (standaard)|

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md) <br>
[Meer informatie over Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)
