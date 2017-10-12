---
title: Stijlen aanpassen in de ontwikkelaarsportal in Azure API Management | Microsoft Docs
description: Informatie over het wijzigen van de stijlen die worden gebruikt voor pagina's via de ontwikkelaarsportal in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: 89baf60d0204a1701e93309f09b90bc94c4ca57b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-the-styling-of-the-developer-portal-in-azure-api-management"></a>De stijl van de ontwikkelaarsportal aanpassen in Azure API Management
Er zijn die manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:

* [De inhoud van statische pagina's en pagina-indelingselementen bewerken][modify-content-layout]
* [De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal][customize-styles] (uitgelegd in deze handleiding)
* [De sjablonen bewerken die worden gebruikt voor pagina's die worden gegenereerd door de portal][portal-templates] (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enz.)

## <a name="change-headers-styling"> </a>De stijl van pagina-elementen aanpassen

De kleuren, lettertypen, grootten, afstanden en andere stijlelementen van pagina's in de portal worden gedefinieerd met stijlregels. 

U kunt stijlregels bewerken via de **ontwikkelaarsportal** terwijl u bent aangemeld als beheerder. Open daarvoor Azure Portal en klik op **Uitgeversportal** via de servicewerkbalk van uw API Management-exemplaar.

![Publicatieportal][api-management-management-console]

Klik daarna in de rechterbovenhoek op **Ontwikkelaarsportal**. 

![Koppeling naar de ontwikkelaarsportal in de uitgeversportal][api-management-pp-dp-link]

Als u de werkbalk voor aanpassing wilt openen, gaat u met de muis naar het aanpassingspictogram (of selecteer het pictogram) en klik op Stijlen op de werkbalk.

![Werkbalkknop Aanpassen][api-management-customization-toolbar-button]

Er zijn twee hoofdmanieren om stijlregels te bewerken. U kunt de lijst die standaard wordt weergegeven, bekijken met alle stijlen die overal worden gebruikt en de gewenste stijl wijzigen. U kunt ook **Een element op de pagina selecteren** kiezen en vervolgens op een willekeurige plek op de pagina klikken om alleen de stijlen voor dat element weer te geven.

![Werkbalk voor aanpassing][api-management-customization-toolbar]

Klik op de optie **Een element op de pagina selecteren** voor dit voorbeeld.  Elementen worden nu gemarkeerd als u de muisaanwijzer er overheen beweegt om aan te geven welke stijlen van het element u zou gaan bewerken als u erop klikt. Ga met de muisaanwijzer naar de tekst in de kop (normaal gesproken de bedrijfsnaam) en klik erop. Een set benoemde en gecategoriseerde stijlregels wordt weergegeven in de stijleditor. Elke regel geeft een stijleigenschap van het geselecteerde element aan. Voor de header die hierboven is geselecteerd, staat de tekengrootte bijvoorbeeld in @font-size-h1 terwijl de naam van het lettertype met alternatieven in @headings-font-family staat.

> Als u bekend bent met [bootstrap][bootstrap]: deze regels zijn in feite [LESS-variabelen][LESS variables] binnen het bootstrapthema dat wordt gebruikt voor de ontwikkelaarsportal.
> 
> 

Laten we de kleur van de koptekst wijzigen. Selecteer de invoer in het veld **@headings-color** en typ **#000000**. Dit is de hexadecimale code voor de kleur zwart. Terwijl u dit doet, ziet u dat er een vierkante kleurenindicator aan het einde van het tekstvak wordt weergegeven. Als u op deze indicator klikt, kunt u met een kleurenkiezer een kleur kiezen.

![Kleurenkiezer][api-management-customization-toolbar-color-picker]

Wijzigingen worden weergegeven in realtime terwijl u ze toepast, maar zijn alleen zichtbaar voor beheerders. Als u deze wijzigingen zichtbaar wilt maken voor iedereen, klikt u op de knop **Publiceren** in de stijleditor en bevestigt u de wijzigingen.

![Het menu Publiceren][api-management-customization-toolbar-publish-form]

> Als u de stijlregels wilt wijzigen die van toepassing zijn op andere elementen op de pagina, volgt u dezelfde procedure als voor de header. Klik op **Een element op de pagina kiezen** in de stijleditor, selecteer het betreffende element en begin met het wijzigen van de waarden van de stijlregels die op het scherm worden weergegeven.
> 
> 


## <a name="next-steps"> </a>Volgende stappen
* Meer informatie over het aanpassen van de inhoud van ontwikkelaarsportalpagina's met [Sjablonen voor ontwikkelaarsportals](api-management-developer-portal-templates.md).

[Change the styling of the headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/
