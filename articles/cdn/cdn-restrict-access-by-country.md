---
title: Azure CDN-inhoud per land beperken | Microsoft Docs
description: Informatie over het toegang te beperken door land uw Azure CDN-inhoud via de functie geo filteren.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: v-deasim
ms.openlocfilehash: 661356aeb2369bc1bbddd6caee57b256dd9e1212
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285012"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Azure CDN-inhoud per land beperken

## <a name="overview"></a>Overzicht
Wanneer een gebruiker wordt uw inhoud standaard aanvraagt, wordt de inhoud aangeboden ongeacht de locatie van de gebruiker die de aanvraag. In sommige gevallen kunt u echter toegang tot uw inhoud door land te beperken. Met de *geo filteren* functie, kunt u regels maken op specifieke paden op uw CDN-eindpunt wilt toestaan of blokkeren van inhoud in de geselecteerde landen.

> [!IMPORTANT]
> **Azure CDN Standard van Microsoft** profielen ondersteunen geen geo-filteren op basis van het pad.
> 

## <a name="standard-profiles"></a>Standaard-profielen
De procedures in deze sectie hebben betrekking op **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon** alleen profielen. 

Voor **Azure CDN Premium van Verizon** profielen, moet u de **beheren** portal activeren geo filteren. Zie voor meer informatie [Azure CDN Premium van Verizon profielen](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Pad naar de map definiëren
Voor toegang tot de functie geo filteren, selecteer uw CDN-eindpunt in de portal en vervolgens **Geo filteren** via instellingen in het menu links. 

![Standard geo filteren](./media/cdn-filtering/cdn-geo-filtering-standard.png)

Van de **pad** geeft u het relatieve pad naar de locatie waarnaar gebruikers wordt toegestaan of toegang geweigerd. 

U geo-filtering kunt toepassen voor alle bestanden met een zone voor forward schuine streep (/) of specifieke mappen selecteren door op te geven van directory-paden (bijvoorbeeld */pictures/*). U kunt ook toepassen geo filteren op één bestand (bijvoorbeeld */pictures/city.png*). Er zijn meerdere regels toegestaan; Nadat u een regel hebt ingevoerd, wordt de volgende regel geeft u op een lege rij weergegeven.

Bijvoorbeeld zijn de volgende filters voor directory-pad geldig:   
*/*                                 
*/Photos/*     
*/Photos/Straatsburg /*     
*/Photos/Strasbourg/City.PNG*

### <a name="define-the-type-of-action"></a>Definieer het type actie

Van de **actie** selecteert **toestaan** of **blok**: 

- **Toestaan dat**: alleen gebruikers van de opgegeven landen toegang hebben tot de assets die zijn opgevraagd uit het recursieve-pad.

- **Blok**: uit de opgegeven landen toegang geweigerd voor gebruikers aan de activa die zijn opgevraagd uit het recursieve-pad. Als er geen andere land-filteropties zijn geconfigureerd voor die locatie, klikt u vervolgens alle andere gebruikers mogen toegang.

Bijvoorbeeld, een geo-filterregel voor het blokkeren van het pad */foto's /Straatsburg/* filtert u de volgende bestanden:     
*http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countries"></a>Definieer de landen
Van de **LANDCODES** , selecteert u de landen die u wilt blokkeren of toestaan voor het pad. 

Nadat u klaar bent met de landen selecteren, selecteert u **opslaan** voor het activeren van de nieuwe regel voor het filteren van geo. 

![Regels geo filteren](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Resources opschonen
Voor het verwijderen van een regel selecteren uit de lijst op de **Geo filteren** pagina en kies vervolgens **verwijderen**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Azure CDN Premium van Verizon profielen
Voor **voor Azure CDN Premium van Verizon** profielen, de gebruikersinterface voor het maken van een regel voor het filteren van geo verschilt:

1. Selecteer in het menu bovenaan in uw Azure CDN-profiel **beheren**.

2. Selecteer in de portal Verizon **HTTP grote**, selecteer daarna **landen filteren**.

    ![Standard geo filteren](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Selecteer **land Filter toevoegen**.

    De **stap één:** pagina wordt weergegeven.

4. Geef het mappad op, selecteer **blok** of **toevoegen**, selecteer daarna **volgende**.

    De **stap twee:** pagina wordt weergegeven. 

5. Selecteer een of meer landen uit de lijst en vervolgens **voltooien** voor het activeren van de regel. 
    
    De nieuwe regel wordt weergegeven in de tabel op de **landen filteren** pagina.

    ![Regels geo filteren](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Resources opschonen
Selecteer het verwijderpictogram naast een regel te verwijderen of het bewerkingspictogram om deze te wijzigen in de regeltabel van land filteren.

## <a name="considerations"></a>Overwegingen
* Wijzigingen in uw configuratie voor het filteren van geo komen niet onmiddellijk van kracht:
   * Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
   * Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
   * Profielen van **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** worden normaal gesproken binnen 10 minuten doorgegeven. 
 
* Deze functie biedt geen ondersteuning voor jokertekens (bijvoorbeeld *).

* De configuratie van de geo-filteren die zijn gekoppeld aan het relatieve pad is toegepaste recursief naar het opgegeven pad.

* Slechts één regel kan worden toegepast op hetzelfde relatieve pad. Dat wil zeggen, kan niet meerdere land filters die naar hetzelfde relatieve pad verwijzen te maken. Omdat land filters recursieve, kan een map echter meerdere land filters hebben. Een submap van een eerder geconfigureerde map kan met andere woorden, een ander land filter toegewezen.

* De functie geo filteren gebruikt landcodes voor het definiëren van de landen waarvan een aanvraag wordt toegestaan of geblokkeerd voor een beveiligde map. Maar Akamai en Verizon profielen bieden ondersteuning voor de meeste van de dezelfde landcodes, zijn er een paar verschillen. Zie voor meer informatie [Azure CDN landcodes](https://msdn.microsoft.com/library/mt761717.aspx). 

