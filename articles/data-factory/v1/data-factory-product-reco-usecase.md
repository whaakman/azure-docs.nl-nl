---
title: 'Data Factory Use Case: productaanbevelingen'
description: Meer informatie over een use-case geïmplementeerd met behulp van Azure Data Factory, samen met andere services.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: a940b1d2184961d907f72ad072270dad3571e844
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229250"
---
# <a name="use-case---product-recommendations"></a>Use case: productaanbevelingen
Azure Data Factory is een van vele services die worden gebruikt voor het implementeren van de Cortana Intelligence Suite van Oplossingsverbeteringen.  Zie [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics) pagina voor meer informatie over dit pakket. In dit document wordt een gebruikelijk dat gebruikers van Azure al hebt opgelost en geïmplementeerd met behulp van Azure Data Factory en andere services van Cortana Intelligence-onderdeel beschreven.

## <a name="scenario"></a>Scenario
Onlinewinkels wilt vaak bewegen hun klanten om aan te schaffen producten door middel van deze producten zijn waarschijnlijk interessant en daarom waarschijnlijk aan te schaffen. U doet dit door moeten onlinewinkels hun online gebruikerservaring aanpassen met behulp van aangepaste productaanbevelingen voor die specifieke gebruiker. Deze persoonlijke aanbevelingen zijn om te worden gemaakt op basis van hun huidige en historische gegevens, informatie over het product, geïntroduceerde merken en product- en segmentatie-gegevens.  Ze kunnen ook de aanbevelingen voor het product van gebruikers op basis van de analyse van de algehele gebruiksgedrag van alle gecombineerde gebruikers opgeven.

Het doel van deze winkels is om te optimaliseren voor gebruikers klikken op verkoop conversies en hogere omzet te behalen.  Zij bereiken deze conversie een hoeveelheid door het leveren van contextuele, op basis van gedrag productaanbevelingen op basis van de klant interesses en acties. In dit geval gebruiken we onlinewinkels gebruiken als een voorbeeld van bedrijven die u wilt optimaliseren voor hun klanten. Deze principes gelden echter voor elk bedrijf dat wil Houd contact met de klanten over de goederen en diensten en hun klanten kopen ervaring met persoonlijke aanbevelingen te verbeteren.

## <a name="challenges"></a>Uitdagingen
Er zijn veel uitdagingen die onlinewinkels vlak bij het implementeren van dit type use case. 

Eerst gegevens van verschillende grootte en vorm moet worden opgenomen in meerdere gegevensbronnen, zowel on-premises en in de cloud. Deze gegevens omvatten productgegevens, gedrag van historische klantgegevens en gebruikersgegevens, zoals de gebruiker wordt naar de detailhandel online-site. 

Tweede, persoonlijk aangepaste productaanbevelingen moeten redelijkerwijs en nauwkeurig berekend en voorspeld. Naast het product, merk en de gegevens van de werking en de browser van de klant moeten de onlinewinkels ook om op te nemen van feedback van klanten op eerdere aankopen rekening houden met het bepalen van de beste aanbevelingen voor de gebruiker. 

Derde, moeten de aanbevelingen zijn onmiddellijk product voor de gebruiker voor een naadloze te bladeren en ervaring aanschaffen, en de meest recente en relevante aanbevelingen. 

Ten slotte moeten detailhandelaren meet de effectiviteit van hun benadering door bij te houden van de algehele up-selling en cross-selling klik naar conversie sales successen en aanpassen aan hun toekomstige aanbevelingen.

## <a name="solution-overview"></a>Overzicht van de oplossing
Deze voorbeeld-use-case is opgelost en geïmplementeerd door echte Azure-gebruikers met behulp van Azure Data Factory en andere Cortana Intelligence componentservices, waaronder [HDInsight](https://azure.microsoft.com/services/hdinsight/) en [Power BI](https://powerbi.microsoft.com/).

De online winkel maakt gebruik van een Azure Blob-opslag, een on-premises SQL-server, Azure SQL-database en een relationele datamart als hun opties voor het opslaan van gegevens in de hele werkstroom.  De blob-archief bevat informatie over de klant, gedrag klantgegevens en product-gegevens. De gegevens van het product bevat informatie over het product merk en een product catalogus lokaal opgeslagen in een SQL datawarehouse. 

Alle de gegevens worden gecombineerd en opgenomen in een systeem van de aanbeveling product persoonlijke aanbevelingen op basis van de klant interesses en acties, terwijl de gebruiker wordt naar producten in de catalogus op de website van leveren. De klanten zien ook producten die gerelateerd zijn aan het product dat ze kijkt op basis van algemene website gebruikspatronen die niet gerelateerd zijn aan elk één gebruiker.

![use-case-diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabytes van onbewerkte logboekbestanden worden dagelijks op de website van de online winkel van gegenereerd als semi-gestructureerde bestanden. De onbewerkte logboekbestanden en de catalogusgegevens klant- en is regelmatig opgenomen in een Azure Blob-opslag met behulp van Data Factory wereldwijd geïmplementeerde gegevensverplaatsing als een service. De onbewerkte logboekbestanden voor die dag worden gepartitioneerd (op basis van jaren en maanden) in blob-opslag voor langdurige opslag.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) wordt gebruikt voor het partitioneren van de onbewerkte logboekbestanden in de blob-opslag en verwerking van de opgenomen logboeken op schaal met zowel Hive als Pig-scripts. De gepartitioneerde Weblogboeken met de gegevens vervolgens om op te halen van de vereiste invoer voor een machine learning-systeem voor het genereren van de aangepaste productaanbevelingen aanbeveling is verwerkt.

De aanbeveling systeem dat wordt gebruikt voor de machine learning in dit voorbeeld is een open-source machine learning-platform van de aanbeveling van [Apache Mahout](http://mahout.apache.org/).  Alle [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) of aangepast model kan worden toegepast op het scenario.  De Mahout-model wordt gebruikt om te voorspellen van de overeenkomsten tussen items op de website op basis van algemene gebruikspatronen, en voor het genereren van de persoonlijke aanbevelingen op basis van de individuele gebruiker.

Ten slotte wordt de resultatenset van aangepaste productaanbevelingen verplaatst naar een datamart relationele gegevens voor gebruik door de website van de leverancier.  De resultatenset kan ook worden geopend rechtstreeks vanuit de blob-opslag door een andere toepassing of verplaatst naar andere archieven voor andere consumenten en use-cases.

## <a name="benefits"></a>Voordelen
De oplossing voldaan door hun product aanbeveling strategie te optimaliseren en het afstemmen van deze met de bedrijfsdoelen, de online winkel merchandising en doelstellingen marketing. Daarnaast wij kunnen operationeel maken en beheren van de werkstroom van de aanbeveling product op een efficiënte, betrouwbare en kosteneffectieve wijze. De benadering eenvoudig ze hun model bijwerken en de effectiviteit op basis van de metingen van verkoop klik naar conversie successen verfijnen. Met behulp van Azure Data Factory, wij kunnen hun tijd in beslag nemen en te duur handmatige cloudresourcemanagement afbreken en verplaatsen naar on-demand cloudresourcemanagement. Daarom wij kunnen Bespaar tijd, geld te besparen en beperk de hun implementatie van de oplossing. Afkomst gegevensweergaven en operationele servicestatus werd eenvoudig te visualiseren en problemen oplossen met de intuïtieve Data Factory-controle en beheer gebruikersinterface beschikbaar is via de Azure-portal. Hun oplossing kan nu worden gepland en worden beheerd zodat klaar gegevens op betrouwbare wijze wordt geproduceerd en die worden geleverd aan gebruikers en gegevens en verwerking van afhankelijkheden worden automatisch beheerd zonder menselijke tussenkomst.

Door deze persoonlijke winkelervaring, de online winkel gemaakt van een klant meer concurrerende, aantrekkelijke optreden en daarom verkoop en de algehele klanttevredenheid te vergroten.

