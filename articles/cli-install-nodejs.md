---
title: Installeer de Azure CLI 1.0 | Microsoft Docs
description: Installeer de 1.0 Azure CLI voor Mac, Linux en Windows te starten met Azure-services
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: 63b35ed25b809a16b61b685fd35aa67474b0a369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-cli-10"></a>Installeer de Azure CLI 1.0
> [!div class="op_single_selector"]
> * [PowerShell](/powershell/azure/overview)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> Dit onderwerp beschrijft het installeren van de Azure CLI 1.0, die is gebaseerd op nodeJs en ondersteunt alle klassieke implementatie API-aanroepen, evenals een groot aantal implementatieactiviteiten van Resource Manager. Moet u de [Azure CLI 2.0](/cli/azure/overview) voor nieuwe of toekomstgerichte CLI-implementaties en -beheer.

Snel het Azure-opdrachtregelinterface (Azure CLI 1.0) voor het gebruik van een set van open-source shell-opdrachten voor het maken en beheren van resources in Microsoft Azure installeren. U hebt verschillende mogelijkheden voor het installeren van deze cross-platform-hulpprogramma's op uw computer:

* **npm pakket** -npm (de package manager voor JavaScript) uitgevoerd voor het installeren van de nieuwste Azure CLI 1.0-pakket op uw Linux-distributie- of OS. Node.js en npm op uw computer vereist.
* **Installatieprogramma** -een installatieprogramma voor een eenvoudige installatie op de Mac- of Windows te downloaden.
* **Docker-container** : begin met de meest recente CLI in een kant-en-klaar Docker-container. Vereist Docker-host op uw computer.

Zie voor meer opties en achtergrond, de project-opslagplaats op [GitHub](https://github.com/azure/azure-xplat-cli).

Nadat de Azure CLI 1.0 is geïnstalleerd, [verbinding te maken met uw Azure-abonnement](xplat-cli-connect.md) en voer de **azure** opdrachten uit vanaf de opdrachtregelinterface (Bash, Terminal, opdrachtprompt, enzovoort) om te werken met uw Azure-resources.

## <a name="option-1-install-an-npm-package"></a>Optie 1: Een npm-pakket installeren
Als u wilt de CLI installeren vanaf een pakket npm, Controleer of u hebt gedownload en geïnstalleerd de [nieuwste Node.js en npm](https://nodejs.org/en/download/package-manager/). Voer **npm installeren** om de azure cli-pakket te installeren:

```bash
npm install -g azure-cli
```

Op Linux-distributies, moet u mogelijk gebruik **sudo** om uit te voeren de **npm** opdracht als volgt:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Als u wilt installeren of bijwerken van Node.js en npm op uw Linux-distributie- of OS, raden wij aan dat u de meest recente versie van Node.js TNS (4.x) installeert. Als u een oudere versie gebruikt, krijgt u mogelijk installatiefouten.

Als u liever, download de meest recente Linux [tar-bestand] [ linux-installer] voor de npm-pakket lokaal. Installeer vervolgens het gedownloade npm-pakket (op Linux-distributies die u wilt gebruiken **sudo**):

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Optie 2: Een installatieprogramma gebruiken
Als u een Mac- of Windows-computer gebruikt, zijn de volgende CLI-installatieprogramma downloaden:

* [Mac OS X-installatieprogramma][mac-installer]
* [Windows MSI][windows-installer]

> [!TIP]
> Op Windows, u kunt ook downloaden via de [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) voor het installeren van de CLI. Dit installatieprogramma kunt u de optie voor het installeren van extra Azure-SDK en opdrachtregelprogramma's na de installatie van de CLI.

## <a name="option-3-use-a-docker-container"></a>Optie 3: Een Docker-container gebruiken
Als u hebt ingesteld van uw computer als een [Docker](https://docs.docker.com/engine/understanding-docker/) host, kunt u de nieuwste Azure CLI 1.0 uitvoeren in een Docker-container. Voer de volgende opdracht (op Linux-distributies die u wilt gebruiken **sudo**):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Azure CLI 1.0-opdrachten uitvoeren
Nadat de Azure CLI 1.0 is geïnstalleerd, voert u de **azure** opdracht uit vanaf de opdrachtregel gebruikersinterface (Bash, Terminal, opdrachtprompt, enzovoort). Bijvoorbeeld als u wilt de help-opdracht uitvoert, typt u het volgende:

```azurecli
azure help
```

> [!NOTE]
> Op sommige Linux-distributies, wordt een foutbericht zoals `/usr/bin/env: ‘node’: No such file or directory`. Deze fout zijn afkomstig van recente installaties van Node.js op /usr/bin/nodejs wordt geïnstalleerd. Om dit te corrigeren, een symbolische koppeling maken naar /usr/bin/node met deze opdracht:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Overzicht van de versie van de Azure CLI 1.0 die u hebt geïnstalleerd, typ het volgende:

```azurecli
azure --version
```

U bent nu klaar! Voor toegang tot de CLI-opdrachten voor het werken met uw eigen resources [verbinding maken met uw Azure-abonnement vanuit de Azure CLI](xplat-cli-connect.md).

> [!NOTE]
> Als u Azure CLI voor het eerst gebruikt, ziet u een bericht waarin wordt gevraagd of u toestaan dat Microsoft wilt voor het verzamelen van informatie over het gebruik. Deelname is niet verplicht. Als u ervoor kiest om deel te nemen, kunt u stoppen op elk gewenst moment door het uitvoeren van `azure telemetry --disable`. Inschakelen van deelname op elk gewenst moment `azure telemetry --enable`.

## <a name="update-the-cli"></a>De CLI bijwerken
Bijgewerkte versies van de Azure CLI regelmatig door Microsoft worden uitgegeven. De CLI met behulp van het installatieprogramma voor het besturingssysteem opnieuw installeren of uitvoeren van de meest recente Docker-container. Als u de meest recente Node.js en npm geïnstalleerd hebt, bijwerken door het volgende te typen (op Linux-distributies die u wilt gebruiken **sudo**).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Tab-Aanvulling inschakelen
Tab-aanvulling van de CLI-opdrachten wordt ondersteund voor Mac en Linux.

Inschakelen in zsh uitvoeren:

```bash
echo '. <(azure --completion)' >> .zshrc
```

Als u deze in bash, voert u het:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Volgende stappen
* [Verbinding maken met CLI met uw Azure-abonnement](xplat-cli-connect.md) maken en beheren van Azure-resources.
* Als u meer informatie over de Azure CLI, broncode downloaden, problemen melden, of bijdragen aan het project, gaat u naar de [GitHub-opslagplaats voor de Azure CLI](https://github.com/azure/azure-xplat-cli).
* Als u vragen over het gebruik van de Azure CLI of Azure hebt, gaat u naar de [Azure-Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
