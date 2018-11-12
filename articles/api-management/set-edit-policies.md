---
title: Hoe u Azure API Management-beleid instellen of bewerken | Microsoft Docs
description: In dit onderwerp laat zien hoe Azure API Management-beleid instellen of bewerken.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cflower
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: apimpm
ms.openlocfilehash: 3d1847b6001ef8e32f00a4e1cd9728d5ca0662f8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008263"
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Hoe u Azure API Management-beleid instellen of bewerken

De beleidsdefinitie is een XML-document dat een reeks instructies voor binnenkomend en uitgaand beschrijft. Het XML-bestand kan worden bewerkt rechtstreeks in het definitievenster. U kunt ook een vooraf gedefinieerd beleid selecteren in de lijst die wordt geleverd aan de rechterkant van het venster beleid. De instructies van toepassing op het huidige bereik zijn ingeschakeld en is gemarkeerd. Het juiste XML-bestand te klikken op een instructie ingeschakeld op de locatie van de cursor in het definitie-weergave worden toegevoegd. 

Zie voor gedetailleerde informatie over het beleid [beleidsregels in Azure API Management](api-management-howto-policies.md).

## <a name="set-or-edit-a-policy"></a>Instellen of bewerken van een beleid

Als u wilt instellen of bewerken van een beleid, volgt u de volgende stappen uit:

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Blader naar de APIM-instantie.
3. Klik op het tabblad **API's**.

    ![Beleid bewerken](./media/set-edit-policies/code-editor.png)

4. Selecteer een van de API's die u eerder hebt geïmporteerd.
5. Selecteer het tabblad **Ontwerpen**.
6. Selecteer een bewerking die u wilt het beleid wordt toegepast. Als u het beleid toepassen op alle bewerkingen wilt, selecteert u **alle bewerkingen**.
7. Selecteer de **</>** (code-editor)-pictogram in de **binnenkomende verwerking** of **uitgaande verwerking** sectie.
8. Plak de code van het gewenste beleid in een van de juiste blokken.
         
        <policies>
             <inbound>
                 <base />
             </inbound>
             <backend>
                 <base />
             </backend>
             <outbound>
                 <base />
             </outbound>
             <on-error>
                 <base />
             </on-error>
         </policies>
 
## <a name="configure-scope"></a>Bereik configureren

Beleidsregels kunnen worden geconfigureerd, globaal of bij het bereik van een Product, API en bewerking. Om te beginnen met een beleid te configureren, moet u eerst het bereik waarop het beleid van toepassing selecteren.

Beleid bereiken worden geëvalueerd in de volgende volgorde:

1. Globaal bereik
2. Productbereik
3. API-bereik
4. Het bewerkingsbereik

De instructies in de beleidsregels worden geëvalueerd op basis van de plaatsing van de `base` -element, indien aanwezig. Globaal beleid heeft geen bovenliggend item beleid en het gebruik van de `<base>` -element in het heeft geen effect.

Voor het beleid in het huidige bereik in de beleidseditor, klikt u op **opnieuw berekenen effectief beleid voor de geselecteerde scope**.

### <a name="global-scope"></a>Globaal bereik

Globaal bereik is geconfigureerd voor **alle API's** in de APIM-instantie.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar de APIM-instantie.
2. Klik op **alle API's**.

    ![Globaal bereik](./media/api-management-howto-policies/global-scope.png)

3. Klik op het driehoekje.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Klik op **Opslaan**. 

    De wijzigingen zijn doorgegeven aan de API Management-gateway onmiddellijk.

### <a name="product-scope"></a>Productbereik

Product scope is geconfigureerd voor het geselecteerde product.

1. Klik op **producten**.

    ![Productbereik](./media/api-management-howto-policies/product-scope.png)

2. Selecteer het product die u wilt toepassen van beleid.
3. Klik op **beleid**.
4. Beleid toevoegen of bewerken.
5. Klik op **Opslaan**. 

### <a name="api-scope"></a>API-bereik

API-bereik is geconfigureerd voor **alle bewerkingen** van de geselecteerde API.

1. Selecteer de **API** u wilt dat beleid toe te passen.

    ![API-bereik](./media/api-management-howto-policies/api-scope.png)

2. Selecteer **Alle bewerkingen**
3. Klik op het driehoekje.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Klik op **Opslaan**. 

### <a name="operation-scope"></a>Het bewerkingsbereik 

Het bewerkingsbereik is geconfigureerd voor de geselecteerde bewerking.

1. Selecteer een **API**.
2. Selecteer de bewerking die u wilt dat beleid toe te passen.

    ![Het bewerkingsbereik](./media/api-management-howto-policies/operation-scope.png)

3. Klik op het driehoekje.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Klik op **Opslaan**. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen:

+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)