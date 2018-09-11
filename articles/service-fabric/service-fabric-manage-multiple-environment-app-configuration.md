---
title: Beheren van toepassingen voor meerdere omgevingen in Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric-toepassingen kunnen worden uitgevoerd op clusters die variëren in grootte van een machine naar duizenden machines. In sommige gevallen wilt u uw toepassing configureren voor deze verschillende omgevingen verschillend. In dit artikel wordt uitgelegd hoe u parameters voor verschillende toepassingen per omgeving definiëren.
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
ms.openlocfilehash: dac96ef6fce38a0557444e181fa6eccb649cfb9a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298332"
---
# <a name="manage-applications-for-multiple-environments"></a>Toepassingen voor meerdere omgevingen beheren

Azure Service Fabric-clusters kunnen u het maken van clusters met behulp van een willekeurige plaats van één tot vele duizenden machines. In de meeste gevallen u merkt dat uw toepassing in meerdere configuraties van clusters te implementeren: het lokale ontwikkelingscluster wijzigen, een gedeelde ontwikkelingscluster wijzigen en uw productiecluster. Al deze clusters worden beschouwd als verschillende omgevingen in die uw code is om in te voeren. Binaire waarden van toepassingen via deze breed spectrum zonder aanpassingen kunnen uitvoeren, maar vaak wilt u de toepassing anders te configureren.

Houd rekening met twee eenvoudige voorbeelden:
  - uw service op een gedefinieerde poort luistert, maar moet u die poort moeten verschillen in de omgevingen
  - u moet verschillende binding-referenties opgeven voor een database in de omgevingen

## <a name="specifying-configuration"></a>Configuratie op te geven

De configuratie die u opgeeft, kan worden onderverdeeld in twee categorieën:

- Configuratie van die van toepassing is op hoe uw services worden uitgevoerd
  - Bijvoorbeeld: het poortnummer voor een eindpunt of het aantal exemplaren van een service
  - Deze configuratie is opgegeven in de toepassing of service manifest-bestand
- Configuratie van die van toepassing op uw toepassingscode is
  - Bijvoorbeeld informatie over de binding voor een database
  - Deze configuratie kan worden opgegeven via-configuratiebestanden of omgevingsvariabelen

> [!NOTE]
> Niet alle kenmerken in de toepassing en service manifest Bestandsparameters voor ondersteuning.
> In deze gevallen moet u afhankelijk zijn van de tekenreeksen vervangen als onderdeel van de implementatiewerkstroom van uw. In Azure DevOps kunt u een extensie, zoals Tokens vervangen: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens of in Jenkins kan u een taak script ter vervanging van de waarden uit te voeren.
>

## <a name="specifying-parameters-during-application-creation"></a>Parameters op te geven tijdens het maken van de toepassing

Bij het maken van een benoemde exemplaren in Service Fabric, hebt u de mogelijkheid om door te geven in de parameters. De manier waarop u dit doen, is afhankelijk van hoe u een exemplaar van de toepassing maken.

  - In PowerShell, de [ `New-ServiceFabricApplication` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet gebruikt de parameters voor de toepassing als een hash-tabel.
  - Sfctl, met de [ `sfctl application create` ](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) opdracht parameters zijn vereist als een JSON-tekenreeks. Het script install.sh sfctl gebruikt.
  - Visual Studio biedt een reeks van parameterbestanden in de map Parameters in het toepassingsproject. Deze parameterbestanden worden gebruikt bij het publiceren vanuit Visual Studio, met behulp van Azure DevOps-Services of Team Foundation Server. De parameterbestanden in Visual Studio zijn die wordt doorgegeven aan het script Deploy-FabricApplication.ps1.

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen laten zien hoe u enkele van de concepten beschreven hier gebruiken:

- [Het opgeven van omgevingsvariabelen voor services in Service Fabric](service-fabric-how-to-specify-environment-variables.md)
- [Het opgeven van het poortnummer van een service met behulp van parameters in Service Fabric](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Hoe om te voorzien van configuratiebestanden](service-fabric-how-to-parameterize-configuration-files.md)

- [Verwijzing naar een omgeving variabele](service-fabric-environment-variables-reference.md)
