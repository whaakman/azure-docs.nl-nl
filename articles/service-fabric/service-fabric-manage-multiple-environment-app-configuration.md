---
title: Beheren van toepassingen voor omgevingen met meerdere in Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric-toepassingen kunnen worden uitgevoerd op clusters die variëren in grootte van de ene machine naar duizenden computers. In sommige gevallen wordt u uw toepassing anders voor deze uiteenlopende omgevingen configureren. In dit artikel wordt beschreven hoe u verschillende groepen van toepassingsparameters per omgeving definiëren.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 15ad606578970290cef440ec4efdd967ca0c0b32
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205226"
---
# <a name="manage-applications-for-multiple-environments"></a>Toepassingen voor meerdere omgevingen beheren

Azure Service Fabric-clusters kunnen u voor het maken van clusters met behulp van een willekeurige plaats van een tot vele duizenden computers. In de meeste gevallen u merkt dat uw toepassing in meerdere configuraties van clusters te implementeren: uw lokaal ontwikkelcluster, een gedeelde ontwikkelcluster en uw productiecluster. Alle deze clusters worden beschouwd als verschillende omgevingen die uw code heeft om in te voeren. Binaire bestanden van toepassingen via deze breed scala zonder aanpassingen kunnen uitvoeren, maar vaak wilt u de toepassing anders te configureren.

Houd rekening met twee enkele eenvoudige voorbeelden:
  - uw service luistert op een gedefinieerde poort, maar moet u die poort moeten verschillen in de omgevingen
  - u moet referenties van de andere binding voor een database in de omgevingen

## <a name="specifying-configuration"></a>Configuratie opgeven

De configuratie die u opgeeft, kan worden onderverdeeld in twee categorieën worden onderverdeeld:

- Configuratie van die van toepassing is op hoe uw services worden uitgevoerd
  - Bijvoorbeeld, het poortnummer voor een eindpunt of het aantal exemplaren van een service
  - Deze configuratie is opgegeven in de toepassing of service-manifestbestand
- Configuratie van die van toepassing op uw toepassingscode
  - Bijvoorbeeld informatie over de binding voor een database
  - Deze configuratie kan worden opgegeven via-configuratiebestanden of omgevingsvariabelen

> [!NOTE]
> Niet alle kenmerken in de toepassing en service manifest Bestandsparameters voor ondersteuning.
> U hebt in deze gevallen zijn afhankelijk van het vervangen van tekenreeksen als onderdeel van uw werkstroom voor de implementatie. In Visual Studio Team Services kunt u een uitbreiding zoals Tokens vervangen: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens of in Jenkins u een taak script ter vervanging van de waarden kan uitvoeren.
>

## <a name="specifying-parameters-during-application-creation"></a>Parameters opgeven tijdens maken van de toepassing

Bij het maken van een benoemde toepassingsexemplaren in Service Fabric, hebt u de mogelijkheid om op te geven als parameters. De manier waarop u dit doen, is afhankelijk van hoe u een exemplaar van de toepassing maken.

  - In PowerShell de [ `New-ServiceFabricApplication` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet bevat de toepassingsparameters als een hashtabel.
  - Met behulp van sfctl, de [ `sfctl application create` ](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) opdracht parameters zijn vereist als een JSON-tekenreeks. Het script install.sh sfctl gebruikt.
  - Visual Studio biedt een aantal parameterbestanden in de map Parameters in het toepassingsproject. Deze parameterbestanden worden gebruikt bij het publiceren vanuit Visual Studio, met behulp van Visual Studio Team Service of Team Foundation Server. De parameterbestanden worden naar het script implementeren FabricApplication.ps1 wordt doorgegeven in Visual Studio.

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen laten zien hoe u een aantal van de concepten beschreven hier:

- [Het opgeven van omgevingsvariabelen voor services in Service Fabric](service-fabric-how-to-specify-environment-variables.md)
- [Het poortnummer van een service met behulp van de parameters in Service Fabric opgeven](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Hoe om te voorzien van configuratiebestanden](service-fabric-how-to-parameterize-configuration-files.md)

- [Variabele omgevingsverwijzing](service-fabric-environment-variables-reference.md)
