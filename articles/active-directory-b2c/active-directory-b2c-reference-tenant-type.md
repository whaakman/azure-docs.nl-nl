---
title: 'Azure Active Directory B2C: Regio beschikbaarheid & gegevens hand vestigingsplaats | Microsoft Docs'
description: Een onderwerp over de verschillende typen tenants van Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/10/2017
ms.author: davidmu
ms.openlocfilehash: b982a6b6e0068370ef7b5cb30ea689829e90d494
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Regio beschikbaarheid & gegevens hand vestigingsplaats
Beschikbaarheid in regio's en de hand van gegevens vestigingsplaats zijn twee heel andere concepten die anders van toepassing op Azure AD B2C van de rest van Azure. In dit artikel wordt uitgelegd van de verschillen tussen deze twee concepten en vergelijken hoe ze van toepassing op Azure ten opzichte van Azure AD B2C.

## <a name="summary"></a>Samenvatting
Azure AD B2C wordt **algemeen beschikbaar wereldwijd** met de optie voor **hand vestigingsplaats gegevens in de Verenigde Staten of Europa**.

## <a name="concepts"></a>Concepten
* **Beschikbaarheid in regio's** verwijst naar waar een service beschikbaar voor gebruik is.
* **De hand van gegevens vestigingsplaats** verwijst naar waar de gebruikersgegevens zijn opgeslagen.

## <a name="region-availability"></a>Beschikbaarheid in regio’s
Azure AD B2C is wereldwijd beschikbaar via de openbare Azure-cloud. 

Dit verschilt van het model meeste andere Azure-services volgen die Combineer beschikbaarheid aan de hand van gegevens vestigingsplaats. Ziet u voorbeelden van deze in beide Azure [beschikbare producten door regio](https://azure.microsoft.com/regions/services/) pagina en de [Active Directory B2C prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Gegevensresidentie
Azure AD B2C slaat gebruikersgegevens in de Verenigde Staten of Europa.

De hand van gegevens vestigingsplaats wordt bepaald op basis van welke land/regio is geselecteerd als [maken van een Azure AD B2C-tenant](active-directory-b2c-get-started.md).

![Schermopname van een tenant preview](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Gegevens bevinden zich in de Verenigde Staten voor de volgende landen/regio's:

> Verenigde Staten, Canada, Costa Rica, Dominicaanse Republiek, El Salvador, Guatemala, Mexico, Panama, Portorico en Trinidad en Tobago

Voor de volgende landen/regio's, bevindt zich in Europa gegevens:

> Algerije, Oostenrijk, Azerbeidzjan, Bahrein, Belarus, België, Bulgarije, Kroatië, Cyprus, Tsjechië, Denemarken, Egypte, Estland, Finland, Frankrijk, Duitsland, Griekenland, Hongarije, IJsland, Ierland, Israël, Italië, Jordanië, Kazachstan, Kenia, Koeweit, Letland, Libanon, Liechtenstein, NNNNNN, Luxemburg, Macedonië Macedonisch, Malta, Montenegro, Marokko, Nederland, Nigeria, Noorwegen, Oman, Pakistan, Polen, Portugal, Qatar, Roemenië, Rusland, Saudi-Arabië, Servië, Slowakije, Slovenië, Zuid-Afrika, Spanje, Zweden, Zwitserland, Tunesië, Turkije, Oekraïne, VAE en Verenigd Koninkrijk.

De overige landen/regio's zijn bezig met worden toegevoegd aan de lijst.  Op dit moment kunt u Azure AD B2C door een van de landen/regio's bovenstaande verzamelen.

> Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.

## <a name="preview-tenant"></a>Preview-tenant
Als u een B2C-tenant had gemaakt in Azure AD B2C preview-periode, is het waarschijnlijk dat uw **Tenant-type** zegt **Preview tenant**. Als dit het geval is, moet u uw tenant alleen voor ontwikkelings- en testdoeleinden en niet voor productie-apps gebruiken.

> [!IMPORTANT]
> Er is geen migratiepad van een preview B2C-tenant in een productie-scale B2C-tenant. Let op: Er zijn bekende problemen wanneer u een voorbeeld B2C-tenant verwijderen en opnieuw maken van een productie-scale B2C-tenant met dezelfde domeinnaam. U moet een productie-scale B2C-tenant maken met een andere domeinnaam.


![Schermopname van een tenant preview](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
