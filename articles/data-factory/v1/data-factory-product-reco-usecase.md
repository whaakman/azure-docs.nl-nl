---
title: Data Factory gebruiksvoorbeeld - aanbevelingen
description: "Meer informatie over een gebruiksvoorbeeld geïmplementeerd met behulp van Azure Data Factory samen met andere services."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 04504d1e32243f752e488a24e04ec5ba73fbadc1
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="use-case---product-recommendations"></a>Use case: productaanbevelingen
Azure Data Factory is een van de vele services die worden gebruikt voor het implementeren van de Cortana Intelligence Suite accelerators oplossing.  Zie [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) pagina voor meer informatie over dit pakket. Een algemene gebruiksvoorbeeld die Azure-gebruikers hebben al opgelost en geïmplementeerd met behulp van Azure Data Factory en andere Cortana Intelligence-onderdeel-services worden beschreven in dit document.

## <a name="scenario"></a>Scenario
Onlinewinkels wilt vaak Lok hun klanten te kopen in de vorm van deze met producten die ze zijn waarschijnlijk is geïnteresseerd in en daarom waarschijnlijk aan te schaffen. Hiervoor moeten onlinewinkels hun online gebruikerservaring aanpassen met behulp van aangepaste aanbevelingen voor die specifieke gebruiker. Deze persoonlijke aanbevelingen moeten worden gemaakt op basis van hun huidige en historische winkelen gedragsgegevens, informatie over het product, nieuwe merken en product en klant segmentatie-gegevens.  Daarnaast kunnen ze de gebruiker aanbevelingen op basis van de analyse van gedrag voor algemene informatie over het gebruik van hun gebruikers die zijn gecombineerd bieden.

Het doel van deze winkels is te optimaliseren voor gebruiker klik naar verkoop conversies en hogere omzet behalen.  Zij bereiken deze conversie door het leveren van contextuele, op basis van gedrag aanbevelingen op basis van de klant interesses en acties. Dit geval gebruiken we onlinewinkels gebruiken als een voorbeeld van bedrijven die u wilt optimaliseren voor hun klanten. Deze principes zijn echter van toepassing op elk bedrijf dat wil benaderen van zijn klanten rond de goederen en diensten en verbeter hun klanten kopen ervaring met persoonlijke aanbevelingen.

## <a name="challenges"></a>Uitdagingen
Er zijn veel uitdagingen die face onlinewinkels bij het implementeren van dit type gebruiksvoorbeeld. 

Gegevens van verschillende grootte en vormen moet eerst worden ingenomen uit meerdere gegevensbronnen, zowel on-premises en in de cloud. Deze gegevens omvatten productgegevens en gedrag van historische klantgegevens gebruikersgegevens als de gebruiker op de site online retail gezocht. 

Tweede, persoonlijke productaanbevelingen moeten worden redelijkerwijs en nauwkeurig berekend en voorspeld. Naast de product-, merk- en gedrag en browser klantgegevens moeten onlinewinkels ook opnemen feedback van klanten over eerdere aankopen van de bepaling van de beste aanbevelingen voor de gebruiker. 

Ten slotte moet de aanbevelingen onmiddellijk product aan de gebruiker een naadloze bladeren en ervaring aanschaffen, en bieden de meest recente en relevante aanbevelingen. 

Ten slotte moeten retailers meten van de effectiviteit van hun keuze algehele Upsell kunt bijhouden en cross-verkopen klikken voor conversie verkoop successen en aanpassen aan hun toekomstige aanbevelingen.

## <a name="solution-overview"></a>Overzicht van de oplossing
In dit voorbeeld gebruiksvoorbeeld is opgelost en geïmplementeerd door de echte Azure-gebruikers met behulp van Azure Data Factory en andere Cortana Intelligence componentservices, waaronder [HDInsight](https://azure.microsoft.com/services/hdinsight/) en [Power BI](https://powerbi.microsoft.com/).

De online winkel maakt gebruik van een Azure Blob-opslag, een lokale SQL server, Azure SQL-database en een relationele datamart als hun opties voor het opslaan van gegevens in de hele werkstroom.  Het bloblarchief bevat klantgegevens, klantgegevens gedrag en informatie van productgegevens. De informatie Productgegevens omvat merk productinformatie en een product catalogus lokaal opgeslagen in een SQL datawarehouse. 

Alle gegevens wordt gecombineerd en ingevoerd in een product aanbeveling systeem persoonlijke aanbevelingen op basis van de klant interesses en acties, terwijl de gebruiker op producten in de catalogus op de website gezocht leveren. De klanten wordt ook weergegeven producten die gerelateerd zijn aan het product dat ze kijken op basis van algemene website gebruikspatronen die niet zijn gerelateerd aan een één gebruiker.

![use case-diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabytes van onbewerkte web logboekbestanden worden dagelijks van de website van de fabrikant van de online gegenereerd als semi-gestructureerde bestanden. De onbewerkte web-logboekbestanden en de catalogusgegevens van de klant en product wordt ingenomen regelmatig in een Azure Blob-opslag met behulp van de Data Factory globaal geïmplementeerde gegevensverplaatsing als een service. De onbewerkte logboekbestanden voor de dag worden gepartitioneerd (door jaar en maand) in de blobopslag voor langdurige opslag.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) wordt gebruikt voor het partitioneren van de onbewerkte logboekbestanden in de blob-opslag en de logboeken van de opgenomen op grote schaal met behulp van zowel Hive en Pig-scripts worden verwerkt. De gepartitioneerde weblogboeken gegevens wordt vervolgens verwerkt om op te halen van de benodigde ingangen voor een machine learning system aanbeveling om de persoonlijke productaanbevelingen te genereren.

De aanbeveling systeem dat wordt gebruikt voor de machine learning-in dit voorbeeld is een open-source machine learning-aanbeveling platform van [Apache Mahout](http://mahout.apache.org/).  Alle [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) of aangepaste model kan worden toegepast op het scenario.  Het model Mahout wordt gebruikt om te voorspellen van de overeenkomsten tussen items op de website op basis van algemene gebruikspatronen en voor het genereren van de persoonlijke aanbevelingen op basis van de individuele gebruiker.

Ten slotte wordt de resultatenset van persoonlijke productaanbevelingen verplaatst naar een relationele datamart voor verbruik door de detailhandel-website.  De resultatenset kan ook worden geopend rechtstreeks vanuit de blob-opslag door een andere toepassing of verplaatst naar extra winkels voor andere consumenten en gebruiksvoorbeelden.

## <a name="benefits"></a>Voordelen
De oplossing voldaan door hun product aanbeveling strategie te optimaliseren en het afstemmen met doelen, de online winkel aanbevelingen en doelstellingen marketing. Bovendien kunnen ze operationeel maken en beheren van de werkstroom van de aanbeveling product op een efficiënte, betrouwbare en rendabele manier. De aanpak gemaakt dat ze hun model bijwerken en de effectiviteit op basis van de metingen van verkoop klikken voor conversie successen stemmen. Met behulp van Azure Data Factory, kunnen ze hun tijdrovend en kostbaar handmatige cloud Resourcemanagement afbreken en verplaatsen naar op aanvraag cloud resourcemanagement. Daarom konden ze tijd, geld besparen en hun tijd tot oplossing implementatie. Afkomst gegevensweergaven en operationele servicestatus werd gemakkelijk te visualiseren en problemen oplossen met de intuïtieve Data Factory-bewaking en beheer gebruikersinterface die beschikbaar is via de Azure-portal. De oplossing kan nu worden gepland en beheerd zodat voltooide data betrouwbaar is gemaakt en geleverd aan gebruikers en gegevens en afhankelijkheden van de verwerking wordt automatisch beheerd zonder menselijke tussenkomst.

Dankzij deze persoonlijke ervaring, de online winkel gemaakt van een klant meer concurrerende, aantrekkelijke optreden en daarom verkoop en de algehele klanttevredenheid te verhogen.

