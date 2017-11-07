---
title: Het oplossen van fouten van Docker-client op Windows met behulp van Visual Studio | Microsoft Docs
description: Problemen met het oplossen van problemen die kunnen optreden bij gebruik van Visual Studio maken en implementeren van web-apps op Docker in Windows met behulp van Visual Studio 2017.
services: azure-container-service
documentationcenter: na
author: devinb
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/13/2017
ms.author: devinb
ms.openlocfilehash: 90dd5df4a607568e2f3a60791da2948af7ce4e50
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="troubleshoot-visual-studio-2017-development-with-docker"></a>Ontwikkeling van een Visual Studio 2017 met Docker oplossen

Wanneer u met Visual Studio Tools voor Docker werkt, kunnen problemen optreden tijdens het bouwen van of foutopsporing van uw toepassing. Hieronder worden enkele algemene stappen voor probleemoplossing.

## <a name="volume-sharing-is-not-enabled-enable-volume-sharing-in-the-docker-ce-for-windows-settings--linux-containers-only"></a>Delen van het volume is niet ingeschakeld. Volume delen in de Docker CE voor Windows-instellingen (alleen voor Linux-containers) inschakelen

Dit probleem oplossen:

1. Met de rechtermuisknop op **Docker voor Windows** in het systeemvak en selecteer vervolgens **instellingen**.
1. Selecteer **gedeelde stations** en delen van het systeemstation samen met het station waarop het project zich bevindt.

> [!NOTE]
> Als de bestanden worden gedeeld weergegeven, moet u mogelijk nog steeds klikt u op de koppeling 'Opnieuw instellen van referenties...' aan de onderkant van het dialoogvenster om het opnieuw inschakelen volume delen.

![gedeelde stations](./media/vs-azure-tools-docker-troubleshooting-docker-errors/shareddrives.png)

## <a name="unable-to-start-debugging"></a>Kan niet worden gestart met het opsporen van fouten

Een reden kan zijn gerelateerd aan het oude foutopsporing onderdelen in de profielmap hebben. Voer de volgende opdrachten uit deze mappen verwijderen zodat de meest recente foutopsporing onderdelen worden gedownload op de volgende foutopsporingssessie.

- DEL %userprofile%\vsdbg
- DEL %userprofile%\onecoremsvsmon

## <a name="errors-specific-to-networking-when-debugging-your-application"></a>Fouten die specifiek zijn voor netwerken wanneer foutopsporing van uw toepassing

Probeer het uitvoeren van script kunt downloaden van [opschonen Container Host Networking](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking), die de netwerk-gerelateerde onderdelen op uw hostmachine wordt vernieuwd.


## <a name="microsoftdockertools-github-repo"></a>Microsoft/DockerTools GitHub-repo-

Zie voor andere problemen u tegenkomt, [Microsoft/DockerTools](https://github.com/microsoft/dockertools/issues) problemen.