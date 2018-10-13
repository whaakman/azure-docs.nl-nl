---
title: Het oplossen van fouten van de Docker-client op Windows met behulp van Visual Studio | Microsoft Docs
description: Oplossen van problemen die kunnen optreden bij het gebruik van Visual Studio maken en implementeren van web-apps aan Docker op Windows met behulp van Visual Studio 2017.
services: azure-container-service
documentationcenter: na
author: devinb
manager: douge
editor: ''
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/13/2017
ms.author: devinb
ms.openlocfilehash: cd88dec2ad79ad9f4b4c004866060be86b777cd9
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311181"
---
# <a name="troubleshoot-visual-studio-2017-development-with-docker"></a>Visual Studio 2017-ontwikkeling met Docker oplossen

Wanneer u met Visual Studio Tools voor Docker werkt, kunnen problemen optreden tijdens het bouwen of foutopsporing van uw toepassing. Hieronder worden enkele algemene stappen voor probleemoplossing.

## <a name="volume-sharing-is-not-enabled-enable-volume-sharing-in-the-docker-ce-for-windows-settings--linux-containers-only"></a>Volume delen is niet ingeschakeld. Volume delen in de instellingen voor Docker CE voor Windows (alleen Linux-containers) inschakelen

Dit probleem oplossen:

1. Met de rechtermuisknop op **Docker voor Windows** in het systeemvak te plaatsen en selecteer vervolgens **instellingen**.
1. Selecteer **gedeelde stations** en delen van het systeemstation, samen met het station waar het project is opgeslagen.

> [!NOTE]
> Als bestanden worden gedeeld weergegeven, moet u mogelijk nog steeds de koppeling 'Opnieuw instellen van referenties...' aan de onderkant van het dialoogvenster klikt u op als u wilt opnieuw inschakelen volume delen. Mogelijk moet u Visual Studio opnieuw om door te gaan nadat u de referenties opnieuw instellen.

![gedeelde stations](./media/vs-azure-tools-docker-troubleshooting-docker-errors/shareddrives.png)

## <a name="mounts-denied"></a>Koppelingen geweigerd

Wanneer met behulp van Docker voor macOS, kunt u een fout die verwijst naar de map /usr/local/share/dotnet/sdk/NuGetFallbackFolder kunt tegenkomen. De map toevoegen op het tabblad delen van bestanden in Docker.

## <a name="unable-to-start-debugging"></a>Kan niet worden gestart met het opsporen van fouten

Eén reden kan zijn gerelateerd aan met verouderde foutopsporing onderdelen in uw map gebruiker profiel. De volgende opdrachten voor het verwijderen van deze mappen, zodat de meest recente foutopsporing onderdelen worden gedownload op de volgende foutopsporingssessie uitvoeren.

- DEL %userprofile%\vsdbg
- del %userprofile%\onecoremsvsmon

## <a name="errors-specific-to-networking-when-debugging-your-application"></a>Fouten bij het netwerk bij het opsporen van fouten in uw toepassing

Voer het script kunt downloaden van [opschonen Container Host netwerken](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking), die de netwerk-gerelateerde onderdelen op de hostcomputer wordt vernieuwd.


## <a name="microsoftdockertools-github-repo"></a>Microsoft/DockerTools GitHub-opslagplaats

Zie voor een andere problemen die optreden [Microsoft/DockerTools](https://github.com/microsoft/dockertools/issues) problemen.