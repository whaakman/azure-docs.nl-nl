---
title: De ontwikkelaarsportal aanpassen in Azure API Management | Microsoft Docs
description: Informatie over het aanpassen van de ontwikkelaarsportal in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 13431648e87d085161ad278dc991d49f7872be34
ms.openlocfilehash: 60213f885020a5ba36d6ada0812f755e06b3c48b


---
# <a name="customize-the-developer-portal-in-azure-api-management"></a>De ontwikkelaarsportal aanpassen in Azure API Management.
In deze handleiding wordt getoond hoe u het uiterlijk van de ontwikkelaarsportal kunt wijzigen in Azure API Management om dit aan te passen aan uw merk.

## <a name="change-page-headers"> </a>De tekst of het logo in de paginaheader wijzigen
Een van de belangrijkste aspecten van portalaanpassing is het vervangen van de tekst boven aan alle pagina's door uw bedrijfsnaam of logo.

Inhoud in de ontwikkelaarsportal wordt gewijzigd via de publicatieportal, die u kunt openen vanuit de Azure Portal. U opent deze door vanaf de servicewerkbalk van het API Management-exemplaar op **Publicatieportal** te klikken.

![Publicatieportal][api-management-management-console]

De ontwikkelaarsportal is gebaseerd op een inhoudsbeheersysteem (CMS, content management system). De header die op elke pagina wordt weergegeven, is een speciaal type inhoud dat een widget wordt genoemd. Als u de inhoud van deze widget wilt bewerken, klikt u op **Widgets** in het menu **Ontwikkelaarsportal** aan de linkerkant en selecteert u vervolgens de widget **Koptekst** in de lijst.

![Widget Koptekst][api-management-widgets-header]

De inhoud van de header kan worden bewerkt vanuit het veld **Hoofdtekst**. Wijzig de tekst in 'Fabrikam-ontwikkelaarsportal' en klik vervolgens op **Opslaan** onder aan de pagina.

U zou nu de nieuwe header op elke pagina moeten zien binnen de ontwikkelaarsportal.

> Als u de ontwikkelaarsportal wilt openen terwijl u zich in de publicatieportal bevindt, klikt u op **Ontwikkelaarsportal** in de bovenste balk.
> 
> 

## <a name="change-headers-styling"> </a>De stijl van de headers wijzigen
De kleuren, lettertypen, grootten, afstanden en andere stijlelementen van pagina's in de portal worden gedefinieerd met stijlregels. Als u de stijlen wilt bewerken, opent u in de **Ontwikkelaarsportal** aan de linkerkant de werkbalk Aanpassen. Dit doet u door uw muis over het pictogram Aanpassen te bewegen en in de werkbalk Stijlen te selecteren.

![Werkbalkknop Aanpassen][api-management-customization-toolbar-button]

Er zijn twee hoofdmanieren om stijlregels te bewerken. U kunt de lijst die standaard wordt weergegeven, bekijken met alle stijlen die overal worden gebruikt en de gewenste stijl wijzigen. U kunt ook **Een element op de pagina selecteren** kiezen en vervolgens op een willekeurige plek op de pagina klikken om alleen de stijlen voor dat element weer te geven.

In deze sectie willen we alleen de stijl van de headers wijzigen. Klik op de optie **Een element op de pagina selecteren** in de stijleditorwerkbalk. 

![Werkbalk voor aanpassing][api-management-customization-toolbar]

Elementen worden nu gemarkeerd als u de muisaanwijzer er overheen beweegt om aan te geven welke stijlen van het element u zou gaan bewerken als u erop klikt. Beweeg de muisaanwijzer over de tekst die de bedrijfsnaam in de header aangeeft ('Fabrikam-ontwikkelaarsportal' als u de instructies in de vorige sectie hebt gevolgd) en klik erop. Een set benoemde en gecategoriseerde stijlregels wordt weergegeven in de stijleditor. Elke regel geeft een stijleigenschap van het geselecteerde element aan. Voor de header die hierboven is geselecteerd, staat de tekengrootte bijvoorbeeld in @font-size-h1 terwijl de naam van het lettertype met alternatieven in @headings-font-family staat.

> Als u bekend bent met [bootstrap][bootstrap]: deze regels zijn in feite [LESS-variabelen][LESS variables] binnen het bootstrapthema dat wordt gebruikt voor de ontwikkelaarsportal.
> 
> 

Laten we de kleur van de koptekst wijzigen. Selecteer de invoer in het veld **@headings-color** en typ **#000000**. Dit is de hexadecimale code voor de kleur zwart. Terwijl u dit doet, ziet u dat er een vierkante kleurenindicator aan het einde van het tekstvak wordt weergegeven. Als u op deze indicator klikt, kunt u met een kleurenkiezer een kleur kiezen.

![Kleurenkiezer][api-management-customization-toolbar-color-picker]

Wijzigingen worden weergegeven in realtime terwijl u ze toepast, maar zijn alleen zichtbaar voor beheerders. Als u deze wijzigingen zichtbaar wilt maken voor iedereen, klikt u op de knop **Publiceren** in de stijleditor en bevestigt u de wijzigingen.

![Het menu Publiceren][api-management-customization-toolbar-publish-form]

> Als u de stijlregels wilt wijzigen die van toepassing zijn op andere elementen op de pagina, volgt u dezelfde procedure als voor de header. Klik op **Element kiezen** in de stijleditor, selecteer het betreffende element en begin met het wijzigen van de waarden van de stijlregels die op het scherm worden weergegeven.
> 
> 

## <a name="edit-page-contents"> </a>De inhoud van een pagina bewerken
De ontwikkelaarsportal bestaat uit automatisch gegenereerde pagina's zoals API's, Producten, Toepassingen, Problemen en handmatig geschreven inhoud. Omdat de portal is gebaseerd op een inhoudsbeheersysteem, kunt u dergelijke inhoud naar behoefte maken.

Als u de lijst met alle bestaande inhoudspagina's wilt zien, klikt u op **Inhoud** in het menu **Ontwikkelaarsportal** in de publicatieportal.

![Inhoud beheren][api-management-customization-manage-content]

Klik op de pagina **Welkom** om te bewerken wat wordt weergegeven op de startpagina van de ontwikkelaarsportal. Breng de gewenste wijzigingen aan, bekijk er indien nodig een voorbeeld van en klik vervolgens op **Nu publiceren** om ze voor iedereen zichtbaar te maken.

> Op de startpagina wordt een speciale indeling gebruikt waardoor bovenaan een banner kan worden weergegeven. Deze banner kan niet worden bewerkt in de sectie **Inhoud**. Als u deze banner wilt bewerken, klikt u op **Widgets** in het menu **Ontwikkelaarsportal**, selecteert u **Startpagina** in de vervolgkeuzelijst **Huidige laag** en opent u het item **Banner** onder de sectie **Aanbevolen**. De inhoud van deze widget kan net zoals andere pagina's worden bewerkt.
> 
> 

## <a name="next-steps"> </a>Volgende stappen
* Meer informatie over het aanpassen van de inhoud van ontwikkelaarsportalpagina's met [Sjablonen voor ontwikkelaarsportals](api-management-developer-portal-templates.md).

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-portal/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/



<!--HONumber=Feb17_HO1-->


