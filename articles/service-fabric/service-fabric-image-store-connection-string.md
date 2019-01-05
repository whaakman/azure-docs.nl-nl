---
title: Azure Service Fabric image store-verbindingsreeks | Microsoft Docs
description: Inzicht in de verbindingsreeks van de installatiekopie-store
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
ms.openlocfilehash: 5f69de9ba9a3226209a339c6f6fa778d9ded9755
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050981"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>Inzicht in de instelling ImageStoreConnectionString

In sommige van onze documentatie vermeld we kort sprake is van een parameter 'ImageStoreConnectionString' zonder waarin wordt beschreven wat het betekent. En na het doorlopen van een artikel, zoals [implementeren en remove-toepassingen met behulp van PowerShell][10], ziet u hoeft kopiëren/plakken is de waarde zoals wordt weergegeven in het clustermanifest van het doelcluster. Zodat de instelling moet worden geconfigureerd per cluster zijn, maar bij het maken van een cluster via de [Azure-portal][11], is er geen optie om deze instelling te configureren en het is altijd 'fabric: ImageStore'. Wat is het doel van deze instelling vervolgens?

![Clustermanifest][img_cm]

Service Fabric gestart uit als een platform voor intern verbruik van Microsoft door veel verschillende teams, zodat sommige aspecten van het hoge mate aanpasbaar zijn: 'Installatiekopie Store' slechts één dergelijke aspect is. De installatiekopie-Store is in wezen een pluggable opslagplaats voor het opslaan van toepassingspakketten. Wanneer uw toepassing wordt geïmplementeerd op een knooppunt in het cluster, wordt dat knooppunt de inhoud van het toepassingspakket gedownload uit de Store-installatiekopie. De ImageStoreConnectionString is een instelling die alle benodigde informatie voor clients en knooppunten, de juiste installatiekopie Store vinden voor een bepaald cluster bevat.

Er zijn momenteel drie soorten mogelijk installatiekopie Store-providers en hun bijbehorende verbindingsreeksen zijn als volgt:

1. Afbeelding Store-Service: 'fabric: ImageStore"

2. Bestandssysteem: "file:[file systeempad"]

3. Azure Storage: "xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Container [...] = "

Het providertype gebruikt in productie is de Image Store-Service, dit is een stateful persistente systeemservice die u in Service Fabric Explorer kunt zien. 

![Afbeelding Store-Service][img_is]

Die als host fungeert voor de Store-installatiekopie in een systeemservice binnen het cluster zelf wordt voorkomen dat externe afhankelijkheden voor de opslagplaats van het pakket en geeft ons voor meer controle over de plaats van opslag. Toekomstige verbeteringen rond de afbeelding Store waarschijnlijk gericht op de installatiekopie van Store-provider eerst, indien niet exclusief. De verbindingsreeks voor de installatiekopie van Store-provider heeft unieke gegevens geen omdat de client al met het doelcluster verbonden is. De client moet alleen te weten dat de protocollen die gericht is op de service moeten worden gebruikt.

De File System provider wordt gebruikt in plaats van de installatiekopie van Store-Service voor lokale-in-één-clusters tijdens het ontwikkelen van iets sneller opstarten van het cluster. Het verschil is meestal klein, maar het is een nuttig optimalisatie voor de meeste mensen die tijdens de ontwikkeling. Het is mogelijk een lokale-in-één cluster met de andere provider opslagtypen ook te implementeren, maar meestal is er geen reden om dit te doen omdat de werkstroom ontwikkelen en testen hetzelfde, ongeacht de provider blijft. De provider voor Azure Storage is alleen beschikbaar voor ondersteuning van de oude clusters geïmplementeerd voordat de installatiekopie van Store-provider is geïntroduceerd.

Bovendien niet de bestandssysteem-provider of de Azure Storage-provider moet worden gebruikt als een methode voor het delen van een installatiekopie Store tussen clusters met meerdere - dit leidt tot beschadiging van de cluster-configuratiegegevens van elk cluster veroorzaakt een conflict kan schrijven naar de installatiekopie Store. Als u wilt delen ingerichte toepassingspakketten tussen meerdere clusters, gebruikt u [sfpkg] [ 12] bestanden in plaats daarvan, die kunnen worden geüpload naar een externe opslag met een download-URI.

Terwijl de ImageStoreConnectionString kan worden geconfigureerd, u alleen gebruiken de standaardinstelling. Bij het publiceren naar Azure met Visual Studio, is de parameter automatisch voor u ingesteld dienovereenkomstig. Voor clusters die worden gehost in Azure programmatische implementatie, is de verbindingsreeks altijd 'fabric: ImageStore'. Hoewel bij twijfel kan de waarde altijd worden gecontroleerd door bij het ophalen van het clustermanifest door [PowerShell](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), of [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Zowel on-premises testen en productieclusters moeten altijd worden geconfigureerd voor het gebruik van de installatiekopie Store Service-provider.

### <a name="next-steps"></a>Volgende stappen
[Implementeren en verwijderen van toepassingen met behulp van PowerShell][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
