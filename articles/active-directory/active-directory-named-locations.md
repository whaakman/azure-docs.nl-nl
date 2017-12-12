---
title: Met de naam locaties in Azure Active Directory | Microsoft Docs
description: Meer informatie over wat met de naam locaties zijn en voor het configureren ervan.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 231255d9a119c404c0c947c00414572aaab82719
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Benoemde locaties in Azure Active Directory

Met benoemde locaties, kunt u een vertrouwde IP-adresbereiken label in uw organisatie. Azure Active Directory maakt gebruik van benoemde locaties in de context van:

- De detectie van [bestaat de kans dat gebeurtenissen](active-directory-reporting-risk-events.md) het aantal gemelde fout-positieven te verminderen.  

- [Voorwaardelijke toegang op basis van locatie](active-directory-conditional-access-azure-portal.md#locations).


Dit artikel wordt uitgelegd, hoe u kunt configureren met de naam locaties in uw omgeving.


## <a name="entry-points"></a>Toegangspunten

U toegang hebt tot de configuratiepagina van de benoemde locatie in de **beveiliging** sectie van de pagina Azure Active Directory door te klikken op:

![Toegangspunten](./media/active-directory-named-locations/34.png)

- **Voorwaardelijke toegang:**

    - In de **beheren** sectie, klikt u op **locaties met de naam**.
    
        ![De benoemde locaties-opdracht](./media/active-directory-named-locations/06.png)

- **Riskant aanmeldingen:**

    - Klik in de werkbalk bovenaan op **toevoegen bekende IP-adresbereiken**.

       ![De benoemde locaties-opdracht](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Configuratievoorbeeld van de

**Voor het configureren van een benoemde locatie:**

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.

2. Klik in het linkerdeelvenster op **Azure Active Directory**.

    ![De Azure Active Directory-koppeling in het linkerdeelvenster](./media/active-directory-named-locations/01.png)

3. Op de **Azure Active Directory** pagina in de **beveiliging** sectie, klikt u op **voorwaardelijke toegang**.

    ![De opdracht voorwaardelijke toegang](./media/active-directory-named-locations/05.png)


4. Op de **voorwaardelijke toegang** pagina in de **beheren** sectie, klikt u op **locaties met de naam**.

    ![De benoemde locaties-opdracht](./media/active-directory-named-locations/06.png)


5. Op de **locaties met de naam** pagina, klikt u op **nieuwe locatie**.

    ![De nieuwe locatie-opdracht](./media/active-directory-named-locations/07.png)


6. Op de **nieuw** pagina, de volgende handelingen uit:

    ![De nieuwe blade](./media/active-directory-named-locations/56.png)

    a. In de **naam** typt u een naam voor uw benoemde locatie.

    b. In de **IP-adresbereiken** typt u een IP-adresbereik. De IP-adresbereik moet zich in de *Classless Inter-Domain Routing* notatie (Classless).  

    c. Klik op **Create**.



## <a name="what-you-should-know"></a>Wat u moet weten

**Bulksgewijs updates**: wanneer u maken of bijwerken van de benoemde locaties, voor bulksgewijze updates, kunt u uploaden of downloaden van een CSV-bestand met de IP-adresbereiken. Een upload wordt het IP-adresbereiken in het bestand toegevoegd aan de lijst in plaats van de lijst wordt overschreven.

![De koppelingen uploaden en downloaden](./media/active-directory-named-locations/09.png)


**Beperkingen**: U kunt maximaal 60 benoemde locaties, met één IP-bereik dat is toegewezen aan elk van deze definiëren. Als u slechts één benoemde locatie geconfigureerd hebt, kunt u maximaal 500 IP-adresbereiken voor definiëren.


## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over:

- **Het risico van gebeurtenissen**, Zie [Azure Active Directory-risicogebeurtenissen](active-directory-reporting-risk-events.md).

- **Voorwaardelijke toegang**, Zie [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- **Rapporten van riskante aanmeldingen**, Zie [riskant aanmeldingen rapport in de Azure Active Directory-portal](active-directory-reporting-security-risky-sign-ins.md).  
