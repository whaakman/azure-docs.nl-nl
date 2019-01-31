---
title: Benoemde locaties configureren in Azure Active Directory | Microsoft Docs
description: Leren hoe benoemde locaties te configureren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7921e35158a78eca246932e9b3a1af46d4db8834
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190021"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Snelstart: Benoemde locaties configureren in Azure Active Directory

Met benoemde locaties, kunt u vertrouwde IP-adresbereiken in uw organisatie een label geven. Azure AD maakt gebruik van benoemde locaties om:
- Fout-positieven in [risicogebeurtenissen](concept-risk-events.md) te detecteren. Aanmelden van een vertrouwde benoemde locaties verlaagt het aanmeldingsrisico van een gebruiker.   
- [Voorwaardelijke toegang op basis van locatie](../conditional-access/location-condition.md) configureren.

In deze snelstartgids leert u hoe benoemde locaties in uw omgeving te configureren.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

* Een Azure AD-tenant. Meld u aan voor een [gratis proefversie](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Een gebruiker die een globale administrator is voor de tenant.
* Een IP-adresbereik dat is tot stand gebracht en betrouwbaar in uw organisatie. Het IP-adresbereik moet in **Classless Interdomain routering (CIDR)** indeling zijn.

## <a name="configure-named-locations"></a>Benoemde locaties configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het linkerdeelvenster **Azure Active Directory**en selecteer vervolgens **voorwaardelijke toegang** uit de **Security** sectie.

    ![Tabblad Voorwaardelijke toegang](./media/quickstart-configure-named-locations/entrypoint.png)

3. Op de pagina **Voorwaardelijke toegang**, selecteer **Benoemde locaties** en selecteer **Nieuwe locatie**.

    ![Benoemde locatie](./media/quickstart-configure-named-locations/namedlocation.png)

6. Vul het formulier in op de nieuwe pagina. 

    * In het vak **Naam** typt u een naam voor uw benoemde locatie.
    * In de **IP-adresbereiken** typt u het IP-adresbereik in CIDR-indeling.  
    * Klik op **Create**.
    
    ![De nieuwe blade](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

- [Voorwaardelijke toegang voor Azure AD](../active-directory-conditional-access-azure-portal.md).
- [Locatievoorwaarden in Azure AD voor voorwaardelijke toegang](../conditional-access/location-condition.md)
- [Rapport voor riskante aanmeldingen](concept-risky-sign-ins.md).  
