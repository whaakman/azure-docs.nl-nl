---
title: AppSource-pakket voorbereiden | Microsoft Docs
description: Explanaion in hoe u voorbereidt en bouwen van AppSource-pakketten.
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
ms.openlocfilehash: cc06ddf2fce93bef27e7aaee85b47179608e192b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57837028"
---
# <a name="appsource-package-preparation"></a>AppSource-pakket voorbereiden

Naast een solution.zip-bestand, moet u een **AppSource pakket**. Dit is een ZIP-bestand dat alle activa die nodig zijn voor het automatiseren van uw oplossing implementeren in de klanten CRM-omgeving bevat. De **AppSource pakket** heeft de volgende onderdelen

* Pakket voor Package Deployer
* **Content_Types.XML** bestand met de assets die u gebruikt
* XML-bestand met de gegevens van uw App-specifiek
* logo van 32 x 32 die wordt weergegeven met uw aanbieding in het beheercentrum
* Licentievoorwaarden, privacybeleid

De onderstaande stappen kunt u uw AppSource-pakket maken.

## <a name="a-create-a-package-for-the-package-deployer"></a>a. Een pakket maken voor Package Deployer

Het pakket voor Package Deployer maakt deel uit van het pakket met AppSource.

Voor het maken van een pakket voor Package Deployer, gebruik de volgende instructies: [ https://msdn.microsoft.com/library/dn688182.aspx ](https://msdn.microsoft.com/library/dn688182.aspx). Als u klaar bent, wordt uw pakket bestaan uit de volgende elementen:

1. Pakketmap: alle oplossingen, configuratiegegevens, platte bestanden en de inhoud van het pakket bevat. _Opmerking: In het volgende voorbeeld wordt ervan uitgegaan dat de pakketmap heet "PkgFolder"_
2. DLL-bestand: De assembly bevat de aangepaste code voor het pakket. _Opmerking: In het volgende voorbeeld wordt ervan uitgegaan dat dit bestand is met de naam "MicrosoftSample.dll."_

U moet nu om te maken van een bestand met de naam '**Content_Types.xml**"dit bestand wordt een lijst alle activa-extensies die deel van uw pakket uitmaken. Hier volgt een voorbeeld van code voor het bestand.

    <?xml version="1.0" encoding="utf-8"?>
        <Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
        <Default Extension="xml" ContentType="application/octet-stream" />
        <Default Extension="xaml" ContentType="application/octet-stream" />
        <Default Extension="dll" ContentType="application/octet-stream" />
        <Default Extension="zip" ContentType="application/octet-stream" />
        <Default Extension="jpg" ContentType="application/octet-stream" />
        <Default Extension="gif" ContentType="application/octet-stream" />
        <Default Extension="png" ContentType="application/octet-stream" />
        <Default Extension="htm" ContentType="application/octet-stream" />
        <Default Extension="html" ContentType="application/octet-stream" />
        <Default Extension="db" ContentType="application/octet-stream" />
        <Default Extension="css" ContentType="application/octet-stream" />
    </Types>

De laatste stap is het zip-het volgende tot één bestand. Deze aanroepen **package.zip**. Deze bevat

1. PkgFolder (met inbegrip van alles wat in de map)
2. DLL-bestand
3. **Content_Types.xml**

Stappen voor het maken van package.zip:

1. Plaats de pakketmap **Content_Types.xml** bestands- en PackageName.dll in één map.

![CRMScreenShot2](media/CRMScreenShot2.png)

1. Selecteer alle items in de map met de rechtermuisknop en verzenden naar gecomprimeerde (zip) map kiezen

![CRMScreenShot3](media/CRMScreenShot3.png)

1. Wijzig de naam in package.zip

![CRMScreenShot4](media/CRMScreenShot4.png)

## <a name="b-create-an-appsource-package"></a>b. Maken van een pakket met AppSource

Het AppSource-pakket moet een paar extra bestanden.

1. JPG (resolutie van 32 x 32)
2. HTML-code (HTML opgemaakt bestand)
3. **Content_Types.XML** (hetzelfde als hierboven)
4. xml

Hier volgt een voorbeeld van code voor is input.xml. Zie definities in de onderstaande tabel.

    <PvsPackageData>
        <ProviderName>Microsoft</ProviderName>
        <PackageFile>package.zip</PackageFile>
        <SolutionAnchorName>SampleSolution.zip</SolutionAnchorName>
        <StartDate>01/01/2016</StartDate>
        <EndDate>01/01/2021</EndDate>
        <SupportedCountries>US,CA</SupportedCountries>
        <LearnMoreLink>https://www.microsoft.com</LearnMoreLink>
        <Locales>
        <PackageLocale Code="1033" IsDefault="true">
        <Logo>logo32x32.png</Logo>
        <Terms>
        <PackageTerm File="TermsOfUse.html" />
        </Terms>
        </PackageLocale>
        </Locales>
    </PvsPackageData>
 
**Waar:**

|Veld|Details|
|---|---|
|ProviderName|Wie is de oplossing die afkomstig zijn van. Als een Microsoft-team, moet dit Microsoft.|
|PackageFile |Deployer-activa, samen met een inhoud ingepakte pakket\_types.xml-bestand. Dit zip-bestand moet bevatten de assembly van de implementatie van pakket en de map met de implementatie van pakket assets. Dat wil zeggen, package.zip|
|SolutionAnchorName |De naam van het zip-bestand van de oplossing in package deployer die wordt gebruikt voor de weergavenaam en beschrijving van de activa van de oplossing.|
| StartDate| Dit is de datum waarop het oplossingspakket beschikbaar. De indeling is MM/DD/JJJJ|
|Einddatum|Dit is de datum dat het pakket wordt beëindigd beschikbaar worden gesteld. De indeling is MM/DD/JJJJ |
|SupportedCountries |Dit is een door komma's gescheiden lijst van landen die dit pakket moeten zien. Neem contact op met online services voor een lijst van alle huidige landcodes. Op het moment van schrijven van dit de lijst is: AE, AL, AM, DOOR DE AO, AR, AT, AUSTRALIË, AZ, BA, BB, BD, WORDEN, BG, BH, BM, BN, BO, BRAZILIË, DOOR CA, CH, CI, CL, CM, CO, CR, CV, GEWICHT, CY, CZ, DE, DK, DOEN, DZ, EG, EE, BIJVOORBEELD, ES, FI, FR, GB, GE, GH, GR, GT, HK , HN, HR, HU, -ID, INTERNET EXPLORER, IL, INDIA, IQ, IS, HET, JM, JO, JP, KE, KG, KN, KOREA, KW, KY, KZ, LB, LK, LT, LU, LV, LY, MA, MC, MD, MIJ, MK, MN, MAAND, MT, MU, MX, MIJN, NG, NI, NL, NEE, NZ, OPERATIONS MANAGER, PA, PE, PH , PK, PL, PULL-AANVRAAG, PS, PT, PY, QA, RO, RS, RU, RW, SA, SE, AG, SI, SK, SN, SV, E, TM, TN, TR, TT, TW, UA, VERENIGDE STATEN, UY, UZ, VE, VI, VN, ZA, ZW |
|LearnMoreLink | URL naar de pagina voor meer info voor dit pakket. |
|Landinstellingen|Een exemplaar van dit knooppunt voor elke taal UX die u wilt ondersteunen in de oplossing Preferred UX. Dit knooppunt bevat onderliggende items die worden beschreven van de landinstellingen, logo en voorwaarden voor elke taal|
|Landinstellingen: PackageLocale.Code|LCID van de voor dit knooppunt. Voorbeeld: Amerikaans-Engels is 1033|
|Landinstellingen: PackageLocale.IsDefault|Geeft aan dat dit de standaardtaal is. Dit wordt gebruikt als de herfst back-ups taal als de UX-taal die is gekozen door de klant niet beschikbaar is.|
|Landinstellingen: Logo|Dit als het logo dat u wilt gebruiken voor dit pakket. Grootte van het pictogram is 32 x 32. Bestandsindelingen toegestaan zijn PNG en JPG|
|Landinstellingen: voorwaarden: PackageTerm.File|Dit is de bestandsnaam van het HTML-document met de licentievoorwaarden.|

Dit is waar het logo wordt weergegeven:

![CRMScreenShot5](media/CRMScreenShot5.png)

De laatste stap is het zip-het volgende tot één bestand.

1. ZIP (eerder hebt gemaakt)
2. **Content_Types.xml**
3. xml
4. PNG
5. html

![CRMScreenShot6](media/CRMScreenShot6.png)

Naam van het bestand die geschikt zijn voor uw app. We de voorkeur geeft aan dat u uw bedrijfsnaam en app-naam bevatten. Bijvoorbeeld: **_Microsoft_SamplePackage.zip**.
