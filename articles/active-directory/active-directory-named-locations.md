---
title: Benoemde locaties configureren in Azure Active Directory | Microsoft Docs
description: Informatie over het configureren van benoemde locaties.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.component: protection
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f739917b201d5255716d22930d7c4bd9e6602f37
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224514"
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Benoemde locaties configureren in Azure Active Directory

Met benoemde locaties, kunt u vertrouwde IP-adresbereiken in uw organisatie een label. Azure Active Directory maakt gebruik van benoemde locaties in de context van:

- De detectie van [risicogebeurtenissen](active-directory-reporting-risk-events.md) het aantal gerapporteerde fout-positieven te verminderen.  

- [Voorwaardelijke toegang op basis van locatie](active-directory-conditional-access-locations.md).


Dit artikel wordt uitgelegd, hoe u kunt configureren met de naam locaties in uw omgeving.


## <a name="entry-points"></a>Invoerpunten

U kunt toegang tot de configuratiepagina van de benoemde locatie in de **Security** sectie van de Azure Active Directory-pagina door te klikken op:

![Invoerpunten](./media/active-directory-named-locations/34.png)

- **Voorwaardelijke toegang:**

    - In de **beheren** sectie, klikt u op **benoemde locaties**.
    
        ![De benoemde locaties-opdracht](./media/active-directory-named-locations/06.png)

- **Riskante aanmeldingen:**

    - Klik in de werkbalk bovenaan op **toevoegen bekende IP-adresbereiken**.

       ![De benoemde locaties-opdracht](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Configuratievoorbeeld van de

**Het configureren van een benoemde locatie:**

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.

2. Klik in het linkerdeelvenster op **Azure Active Directory**.

    ![De Azure Active Directory-koppeling in het linkerdeelvenster](./media/active-directory-named-locations/01.png)

3. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

    ![De opdracht voor voorwaardelijke toegang](./media/active-directory-named-locations/05.png)


4. Op de **voorwaardelijke toegang** pagina, in de **beheren** sectie, klikt u op **benoemde locaties**.

    ![De benoemde locaties-opdracht](./media/active-directory-named-locations/06.png)


5. Op de **benoemde locaties** pagina, klikt u op **nieuwe locatie**.

    ![De nieuwe locatie-opdracht](./media/active-directory-named-locations/07.png)


6. Op de **nieuw** pagina, de volgende handelingen uit:

    ![De nieuwe blade](./media/active-directory-named-locations/61.png)

    a. In de **naam** typt u een naam voor uw locatie met de naam.

    b. In de **IP-adresbereiken** typt u een IP-adresbereik. De IP-adresbereik moet zich in de *Classless Inter-Domain Routing* (CIDR)-indeling.  

    c. Klik op **Create**.



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

- [Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- [Locatievoorwaarden in Azure Active Directory voor voorwaardelijke toegang](active-directory-conditional-access-locations.md)

- [Azure Active Directory-risicogebeurtenissen](active-directory-reporting-risk-events.md).

- [Rapport riskante aanmeldingen in de Azure Active Directory-portal](active-directory-reporting-security-risky-sign-ins.md).  
