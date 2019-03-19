---
title: Web Analytics | Microsoft Docs
description: .
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 89cc8c4bffe910de0861d7f44925a10df3811fdb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58097471"
---
<a name="web-analytics"></a>Web Analytics
=============

Dit artikel vindt u instructies voor het leren en te gebruiken van Web Analytics beste Breid uw bedrijf. Op dit tabblad Insights is momenteel beschikbaar voor alle AppSource-aanbiedingen.

Nu dat u hebt gemaakt en gepubliceerd van uw aanbieding, het volgende deel van uw reis bij te houden en te meten is de\' geslaagd. Met **Web Analytics**, hebben we de mogelijkheid om te zien precies hoe goed presteert elk van uw aanbiedingen op marketplace toegevoegd. Voor het starten van uw reis, gaat u naar de pagina met inzichten aan de linkerkant van de Cloud Partner-Portal om te zien van het nieuwe tabblad analytische gegevens.

![WebAnalytics pagina](./media/si-getting-started/WebAnalytics1.png)

Ziet u een uitgebreid dashboard toegevoegd voor de uitgevers-ID die is gemaakt met Microsoft Power BI en kunt u elk van uw aanbiedingen\' gegevens die dagelijks wordt vernieuwd.

<a name="microsoft-campaigns"></a>**Microsoft Campaigns**
-----------------------

Om te groeien van uw aanbiedingen en bijhouden van de groei van uw aanbiedingen, is voorzien van de mogelijkheid om te gebruiken **Microsoft campagnes** in de Cloud Partner-Portal. Campagnes zijn een nieuwe functie voor de marketplace die zorgen u dat ervoor voor het bijhouden van de verschillende kanalen die klanten naar de pagina met details van uw app verzenden.

### <a name="how-to-make-a-campaign"></a>**Het maken van een campagne**

De eenvoudigste manier om te beschrijven van campagnes is dat u toevoegt een aangepaste word/term naar de URL die terechtkomt op de detailpagina van uw app in de marketplace. Google, Bing, LinkedIn en veel andere sites raden u aan te maken van een aankondiging, het koppelen van hun site in uw gewenste site.

In het algemeen worden station nieuwe om klanten te helpen uw product en is het essentieel is voor het meten van het succes van hoe elk van uw kanalen presteert. Dit is de herkomst van campagnes.

Er zijn twee manieren om uw eigen campagne te genereren.

1. Toevoegen aan uw URL de queryparameter **mktcmpid** waarin wordt beschreven wat de campagne is en welke pagina/event deze klanten afkomstig zijn uit.

U kunt bijvoorbeeld het volgende gebruiken: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign>

1. (Geavanceerd): Gebruik een van onze campagne ondersteunde, algemene id's in de URL. We willen laten met extra ref-labels die u gebruiken, wilt zodat we bieden ondersteuning voor de overeenkomst automatisch herkent deze andere labels worden werken:
    
    1. **UTM\_campagne**
    2. **utm\_source**
    3. **ref**
    4. **src**

U kunt bijvoorbeeld het volgende gebruiken: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=NewCampaign>

U kunt ervoor kiezen een combinatie van meerdere van deze campagne-ID's om meerdere bronnen verkeer voor de campagne, zoals waar de klant afkomstig uit (e-mailbericht, blog, sociale media-bron, enzovoort zijn) te stimuleren nader te identificeren.

Bijvoorbeeld:

1. Nieuwsbrief verwijzende site:  <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=newsletter>
2. LinkedIn referrer:  <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=LinkedIn>

### <a name="ensuring-campaigns-pass-through-all-your-pages"></a>**Ervoor zorgen dat campagnes doorgeven aan alle pagina 's**

Er is mogelijk een scenario waarbij uw campagnes een tussenliggende pagina die u verkeer naar die vervolgens gaat deze hebt naar de klanten te verzenden naar de marketplace. Het is belangrijk om door te geven tot en met uw eerste campagne-ID's in de laatste-URL die u naar de marketplace verzendt.

Hier volgt een voorbeeld:

1. Marketing werknemer koopt advertenties van Google drive-verkeer naar het bedrijf\'s landingspagina <https://contoso.com>. Deze landingspagina heeft een \"probeer mijn product\" koppeling naar <https://appsource.com>.
2. Een gebruiker klikt op de ad en over zijn bedrijf belandt\'landingspagina s.
    1.  Referentie-URL = google.com
    2.  URL van openingspagina = <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
3. De gebruiker klikt op de \"probeer mijn product\" koppelen en gaat in AppSource.
    1. Referentie-URL =  <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
    2. Landing Page URL (**Ensure that this URL has utm\_campaign and  utm\_source added to this URL**) =  [https://appsource.microsoft.com/en-us/product/dynamics-365/contoso.offername?**utm\_campaign=MyCampaignAdName&utm\_source=MySourceAdName**](https://appsource.microsoft.com/en-us/product/dynamics-365/contoso.offername?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName)

<a name="how-to-evaluate-the-success-of-a-campaign"></a>Het evalueren van het succes van een campagne
-----------------------------------------

### <a name="page-visits-by-campaign"></a>**Pagina bezoeken per campagne**

![WebAnalytics2](./media/si-getting-started/WebAnalytics2.png)

Dit is de uitsplitsing van elk van uw dagelijkse paginabezoeken door de campagne die ze afkomstig zijn uit.

### <a name="conversion-rate-by-campaign"></a>**Conversieverhouding per campagne**

![WebAnalytics3](./media/si-getting-started/WebAnalytics3.png)

Vergelijkbaar met hoe we de conversieverhouding van uw gehele aanbod, in dit diagram ziet u de verdeling weergegeven van de manier waarop uw verschillende campagnes doen. In deze grafiek kunt u om te bepalen waar de hogere conversie tarief campagnes afkomstig zijn.

### <a name="distribution-by-campaign"></a>**Distributie per campagne**

![WebAnalytics4](./media/si-getting-started/WebAnalytics4.png)

In deze grafiek is net als bij hoe we kijken naar de domeinen van uw klanten, kunt u de distributie van uw gegevens per campagne die de gebruikers worden toegevoegd aan de marketplace onder zien. \_NoCampaign betekent dat de klant geen een campagne-ID in de url heeft wanneer ze de marketplace genavigeerd.

<a name="next-steps"></a>**Volgende stappen**
--------------

Nu dat u de mogelijkheid om bij te houden van uw aanbiedingen is geslaagd hebt, willen we raden u aan uw eigen campagnes maken.

Als u vragen/functieaanvragen hebt, kunt u deze via Feedback, zich in de rechterbovenhoek delen.

![Feedback in de Cloud Partner-Portal](./media/si-getting-started/WebAnalytics5.png)
