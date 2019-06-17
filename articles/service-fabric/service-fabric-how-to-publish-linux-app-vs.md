---
title: Meer informatie over het maken en publiceren van .net Core-toepassingen naar een externe Azure Service Fabric Linux-cluster | Microsoft Docs
description: Maken en publiceren van .net Core-apps die zijn gericht op een externe Linux-cluster op basis van Visual Studio
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: 46d76edbe8cede12e8c7811f43c28a65c1ebaed0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078662"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Visual Studio gebruiken om te maken en publiceren van .net Core-toepassingen die zijn gericht op een externe Linux Service Fabric-cluster
Met Visual Studio-hulpmiddelen u kunt ontwikkelen en publiceren van Service Fabric .net Core-toepassingen die zijn gericht op een Linux Service Fabric-cluster. De SDK-versie moet 3.4 of hoger implementeren van een .net Core-toepassing die gericht is op Linux Service Fabric-clusters vanuit Visual Studio.

> [!Note]
> Visual Studio biedt geen ondersteuning voor foutopsporing Service Fabric-toepassingen die gericht op Linux.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>Maken van een Service Fabric-toepassing die gericht is op .net Core
1. Start Visual Studio als **beheerder**.
2. Maak een project met **File -> Nieuw Project ->** .
3. In de **nieuw Project** dialoogvenster kiezen **Cloud > Service Fabric-toepassing**.
![toepassing maken]
4. Noem de toepassing en klikt u op **Ok**.
5. Op de **New Service Fabric Service** pagina, selecteert u het type van de service die u maken wilt onder de **.Net Core-sectie**.
![create-service]

## <a name="deploy-to-a-remote-linux-cluster"></a>Op een externe Linux-cluster implementeren
1. In de solution explorer, klik met de rechtermuisknop op de toepassing en selecteer **bouwen**.
![Build-toepassing]
2. Nadat het bouwproces voor de toepassing is voltooid, klik met de rechtermuisknop op de service en selecteer Bewerken de **csproj-bestand**.
![edit-csproj]
3. Bewerk de eigenschap UpdateServiceFabricManifestEnabled van de waarde True **False** als de service is een **actor projecttype**. Als uw toepassing heeft geen een actor-service, gaat u naar stap 4.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> UpdateServiceFabricManifestEnabled is ingesteld op false, worden updates voor de ServiceManifest.xml tijdens een build uitgeschakeld. Eventuele wijzigingen van dergelijke wordt zoals toevoegen, verwijderen of naar de service Hernoemen niet worden doorgevoerd in de ServiceManifest.xml. Als er wijzigingen zijn aangebracht u moet het bestand servicemanifest.XML handmatig of tijdelijk ingesteld UpdateServiceFabricManifestEnabled true en de service die de ServiceManifest.xml bijwerken en vervolgens terugkeren bouwt werken back-ups maken op false.
>

4. Werk de RuntimeIndetifier van win7 x64 naar het doelplatform in het service-project.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. Update het programma entrypoint .exe verwijderen in het ServiceManifest. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. Klik in Solution Explorer met de rechtermuisknop op de toepassing en selecteer **publiceren**. Het dialoogvenster **Publiceren** wordt weergegeven.
7. In **verbindingseindpunt**, selecteert u het eindpunt voor de externe Linux Service Fabric-cluster die u wilt richten.
![publish-application]

<!--Image references-->
[toepassing maken]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[create-service]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[Build-toepassing]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[publish-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [aan de slag met Service Fabric met .net Core](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/)
