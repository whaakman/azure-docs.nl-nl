---
title: Het instellen of bewerken van Azure API Management-beleid | Microsoft Docs
description: Dit onderwerp leest hoe u kunt instellen of bewerken van Azure API Management-beleidsregels.
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 409069cbc382610a48139df75f0f64b1682d8ee6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Het instellen of bewerken van Azure API Management-beleidsregels

De beleidsdefinitie is een XML-document dat een reeks instructies voor binnenkomend en uitgaand beschrijft. Het XML-bestand kan worden bewerkt rechtstreeks in het definitievenster. U kunt ook een vooraf gedefinieerd beleid selecteren uit de lijst die wordt geleverd aan de rechterkant van het venster beleid. De instructies van toepassing op het huidige bereik zijn ingeschakeld en gemarkeerd. Te klikken op een instructie ingeschakeld, wordt het juiste XML-bestand op de locatie van de cursor in de weergave definitie toegevoegd. 

Zie voor gedetailleerde informatie over het beleid [-beleid in Azure API Management](api-management-howto-policies.md).

## <a name="set-or-edit-a-policy"></a>Instellen of bewerken van een beleid

Als u wilt instellen of bewerken van een beleid, volg de volgende stappen uit:

1. Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).
2. Blader naar uw APIM-exemplaar.
3. Klik op de **API's** tabblad.
4. Selecteer een van de API's die u eerder hebt geïmporteerd.
5. Selecteer de **ontwerp** tabblad.
6. Selecteer een bewerking die u wilt het beleid wordt toegepast. Als u het beleid toepassen op alle bewerkingen wilt, selecteert u **alle bewerkingen**.
7. Klik op de driehoek naast de **inkomende** of **uitgaande** potloden.
8. Selecteer de **Code-editor** item.

    ![Beleid bewerken](./media/set-edit-policies/set-edit-policies01.png)

9. Plak de code van het gewenste beleid in een van de juiste blokken.
         
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
 
## <a name="configure-scope"></a>Scope configureren

Beleidsregels kunnen worden geconfigureerd globaal of in het bereik van een Product, API of bewerking. Om te beginnen met een beleid te configureren, moet u eerst het bereik waarvoor het beleid moet toepassen selecteren.

Beleid scopes worden geëvalueerd in de volgende volgorde:

1. Globaal bereik
2. Product-bereik
3. API-bereik
4. Bewerkingsbereik

De instructies in de beleidsregels worden geëvalueerd op basis van de plaatsing van de `base` element, indien aanwezig. Globaal beleid heeft geen bovenliggend element beleid en het gebruik van de `<base>` -element in het heeft geen effect.

Klik op een overzicht van het beleid in het huidige bereik in de beleidseditor **herberekenen effectief beleid voor de geselecteerde scope**.

### <a name="global-scope"></a>Globaal bereik

Globaal bereik is geconfigureerd voor **alle API's** in uw exemplaar APIM.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw APIM-exemplaar.
2. Klik op **alle API's**.

    ![Globaal bereik](./media/api-management-howto-policies/global-scope.png)

3. Klik op het pictogram driehoek.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Druk op **opslaan**. 

    De wijzigingen zijn onmiddellijk doorgegeven aan de API Management-gateway.

### <a name="product-scope"></a>Product-bereik

Product-scope is geconfigureerd voor het geselecteerde product.

1. Klik op **producten**.

    ![Product-bereik](./media/api-management-howto-policies/product-scope.png)

2. Selecteer het product waarnaar u wilt toepassen van beleid.
3. Klik op **beleid**.
4. Beleid toevoegen of bewerken.
5. Druk op **opslaan**. 

### <a name="api-scope"></a>API-bereik

API-scope is geconfigureerd voor **alle bewerkingen** van de geselecteerde API.

1. Selecteer de **API** u wilt toepassen van beleidsregels.

    ![API-bereik](./media/api-management-howto-policies/api-scope.png)

2. Selecteer **alle bewerkingen**
3. Klik op het pictogram driehoek.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Druk op **opslaan**. 

### <a name="operation-scope"></a>Bewerkingsbereik 

Bewerking scope is geconfigureerd voor de geselecteerde bewerking.

1. Selecteer een **API**.
2. Selecteer de bewerking die u wilt toepassen van beleidsregels.

    ![Bewerkingsbereik](./media/api-management-howto-policies/operation-scope.png)

3. Klik op het pictogram driehoek.
4. Selecteer **Code-editor**.
5. Beleid toevoegen of bewerken.
6. Druk op **opslaan**. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen:

+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)