---
title: bestand opnemen
description: bestand opnemen
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 05/24/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: e431e7c45dc2cb41db5b9fa762abc908cc76b375
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665279"
---
## <a name="create-a-new-cdn-profile"></a>Nieuwe CDN-profielen maken

Een CDN-profiel is een container voor CDN-eindpunten waarmee een prijscategorie wordt opgegeven.

1. Selecteer linksboven in Azure Portal **Een resource maken**. 
    
    Het deelvenster **Nieuw** verschijnt.
   
2. Selecteer **Web en mobiel** en vervolgens **CDN**.
   
    ![CDN-resource selecteren](./media/cdn-create-profile/cdn-new-resource.png)

    Het deelvenster **CDN-profiel** verschijnt.

3. Gebruik voor de CDN-profielinstellingen de waarden die in de volgende tabel zijn opgegeven:
   
    | Instelling  | Waarde |
    | -------- | ----- |
    | **Naam** | Voer *my-cdn-profile-123* in als profielnaam. Deze naam moet wereldwijd uniek zijn. Als deze al in gebruik is, kunt u een andere naam invoeren. |
    | **Abonnement** | Kies een Azure-abonnement in de vervolgkeuzelijst. |
    | **Resourcegroep** | Selecteer **Nieuwe maken** en voer *my-resource-group-123* in als naam voor de resourcegroep. Deze naam moet wereldwijd uniek zijn. Als deze al in gebruik is, kunt u een andere naam invoeren of **Bestaande gebruiken** selecteren en **my-resource-group-123** in de vervolgkeuzelijst selecteren. | 
    | **Resourcegroeplocatie** | Selecteer **VS Centraal** in de vervolgkeuzelijst. |
    | **Prijscategorie** | Selecteer **Standard - Verizon** in de vervolgkeuzelijst. |
    | **Nu een nieuw CDN-eindpunt maken** | Laat het selectievakje uitgeschakeld. |  
   
    ![Nieuw CDN-profiel](./media/cdn-create-profile/cdn-new-profile.png)

4. Selecteer **Aan dashboard vastmaken** om het profiel in het dashboard op te slaan nadat het is gemaakt.
    
5. Selecteer **Maken** om het profiel te maken. 

    Voor alleen **Azure CDN Standard van Microsoft**-profielen duurt het voltooien van het profiel meestal twee uur. 

