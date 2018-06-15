---
title: Azure Service Fabric image store verbinding tekenreeks | Microsoft Docs
description: Inzicht in de verbindingsreeks van de image store
services: service-fabric
documentationcenter: .net
author: alexwun
manager: timlt
editor: ''
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2018
ms.author: alexwun
ms.openlocfilehash: 7d164fea62afac83c4fe2216c56a9980d9279f3a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207125"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>De instelling ImageStoreConnectionString begrijpen

In sommige documentatie vermeld we kort in de aanwezigheid van een parameter 'ImageStoreConnectionString' zonder het met een beschrijving van wat het betekent. En na het doorlopen van een artikel zoals [implementeren en verwijderen van toepassingen via PowerShell][10], het lijkt erop dat u hoeft kopiëren en plakken is de waarde weergegeven in het clustermanifest van het doelcluster. Daarom de instelling moet worden geconfigureerd per cluster, maar bij het maken van een cluster via de [Azure-portal][11]en er is geen optie voor het configureren van deze instelling is altijd 'fabric: Installatiekopieopslag'. Wat is het doel van deze instelling vervolgens?

![Clustermanifest][img_cm]

Service Fabric gestart uit als een platform voor intern Microsoft-verbruik door veel verschillende teams, zodat sommige aspecten van deze zeer aanpasbare zijn-'Image Store' een dergelijke aspect is. De Image Store is in wezen een pluggable opslagplaats voor het opslaan van toepassingspakketten. Wanneer uw toepassing wordt geïmplementeerd naar een knooppunt in het cluster, downloadt u de inhoud van het toepassingspakket met dat knooppunt van de Image Store. De ImageStoreConnectionString is een instelling van de benodigde informatie voor clients en knooppunten aan het archief van de juiste installatiekopie voor een bepaald cluster niet vinden.

Er zijn momenteel drie mogelijke soorten Image Store-providers en hun bijbehorende verbindingsreeksen zijn als volgt:

1. Image Store-Service: "fabric: Installatiekopieopslag'

2. Bestandssysteem: "file:[file system path]"

3. Azure Storage: ' xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Container = [...] "

Het type provider dat wordt gebruikt in productie is de Image Store-Service een stateful persistente systeemservice die u in Service Fabric Explorer kunt zien. 

![Image Store-Service][img_is]

Hosting-archief van de installatiekopie in een systeemservice binnen het cluster zelf, hoeft u externe afhankelijkheden voor de opslagplaats van het pakket en geeft ons meer controle over de plaats van opslag. Toekomstige verbeteringen rond de Image Store, zullen de Image Store-provider eerst als dit niet uitsluitend als doel. De verbindingsreeks voor de provider Image Store-Service geen unieke informatie omdat de client al met het doelcluster verbonden is. De client moet alleen te weten dat de protocollen die gericht is op de service moeten worden gebruikt.

De File System-provider wordt gebruikt in plaats van de Image Store-Service voor lokale 1-box-clusters tijdens het ontwikkelen voor het cluster iets sneller bootstrap. Het verschil is normaal gesproken een kleine, maar het is een nuttig optimalisatie voor de meeste mensen tijdens de ontwikkeling. Het is mogelijk een lokaal cluster een vak met de andere provider opslagtypen ook implementeren, maar meestal is er geen reden om dit te doen omdat u de werkstroom ontwikkelen en testen blijft hetzelfde, ongeacht de provider. De Azure Storage-provider is alleen beschikbaar voor de ondersteuning van de oude clusters geïmplementeerd voordat de Image Store-Service provider werd geïntroduceerd.

Bovendien noch de provider bestandssysteem noch de Azure Storage-provider moet worden gebruikt als een methode voor het delen van een Image Store tussen clusters met meerdere - dit leidt tot beschadiging van de clusterconfiguratiegegevens als elk cluster conflicterende gegevens naar schrijven kunt de Image Store. Als u wilt ingerichte toepassingspakketten tussen clusters met meerdere delen, gebruik [sfpkg] [ 12] bestanden in plaats daarvan die kunnen worden geüpload naar een externe winkel met het downloaden van een URI.

Dus terwijl de ImageStoreConnectionString kan worden geconfigureerd, wordt meestal NET gebruikt de standaardinstelling. Bij het publiceren naar Azure met Visual Studio, wordt de parameter automatisch voor u ingesteld dienovereenkomstig. De verbindingsreeks is voor programmatische implementatie voor clusters die worden gehost in Azure, altijd 'fabric: Installatiekopieopslag'. Hoewel bij twijfel kan de waarde altijd worden gecontroleerd door bij het ophalen van het clustermanifest door [PowerShell](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), of [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Zowel on-premises test en productieclusters moeten altijd worden geconfigureerd voor gebruik van de Image Store-Service-provider.

### <a name="next-steps"></a>Volgende stappen
[Implementeren en verwijderen van toepassingen met behulp van PowerShell][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
