---
title: Met de naam locaties in Azure Active Directory | Microsoft Docs
description: Door te configureren met de naam locaties, kunt u voorkomen dat IP-adressen die eigendom zijn van uw organisatie genereren valse positieven voor de Impossible reis naar ongewone locaties risico gebeurtenistype.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: da437908509e40386ed23863648bd6956b308186
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Benoemde locaties in Azure Active Directory

Met de functie benoemde locaties van Azure Active Directory kunt u een vertrouwde IP-adresbereiken in uw organisaties label. In uw omgeving, kunt u met de naam locaties in de context van de detectie van [bestaat de kans dat gebeurtenissen](active-directory-reporting-risk-events.md). De functie vermindert het aantal gerapporteerde fout-positieven voor de *Impossible op reis naar ongewone locaties* gebeurtenistype risico. 

## <a name="configuration"></a>Configuratie

Voor het configureren van een benoemde locatie:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.

2. Klik in het linkerdeelvenster op **Azure Active Directory**.

    ![De Azure Active Directory-koppeling in het linkerdeelvenster](./media/active-directory-named-locations/01.png)

3. Op de **Azure Active Directory** blade in de **beveiliging** sectie, klikt u op **voorwaardelijke toegang**.

    ![De opdracht voorwaardelijke toegang](./media/active-directory-named-locations/05.png)


4. Op de **voorwaardelijke toegang** blade in de **beheren** sectie, klikt u op **locaties met de naam**.

    ![De benoemde locaties-opdracht](./media/active-directory-named-locations/06.png)


5. Op de **locaties met de naam** blade, klikt u op **nieuwe locatie**.

    ![De nieuwe locatie-opdracht](./media/active-directory-named-locations/07.png)


6. Op de **nieuw** blade het volgende doen:

    ![De nieuwe blade](./media/active-directory-named-locations/56.png)

    a. In de **naam** typt u een naam voor uw benoemde locatie.

    b. In de **IP-adresbereiken** typt u een IP-adresbereik. De IP-adresbereik moet zich in de *Classless Inter-Domain Routing* notatie (Classless).  

    c. Klik op **Create**.



## <a name="what-you-should-know"></a>Wat u moet weten

**Bulksgewijs updates**: wanneer u maken of bijwerken van de benoemde locaties, voor bulksgewijze updates, kunt u uploaden of downloaden van een CSV-bestand met de IP-adresbereiken. Een upload wordt het IP-adresbereiken in het bestand toegevoegd aan de lijst in plaats van de lijst wordt overschreven.

![De koppelingen uploaden en downloaden](./media/active-directory-named-locations/09.png)


**Beperkingen**: U kunt maximaal 60 benoemde locaties, met één IP-bereik dat is toegewezen aan elk van deze definiëren. Als u slechts één benoemde locatie geconfigureerd hebt, kunt u maximaal 500 IP-adresbereiken voor definiëren.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de risico's, [Azure Active Directory-risicogebeurtenissen](active-directory-reporting-risk-events.md).

