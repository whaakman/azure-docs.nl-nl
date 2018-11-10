---
title: Azure Active Directory Identity Protection koppelen aan Azure Security Center | Microsoft Docs
description: Meer informatie over hoe Azure Security Center kan worden geïntegreerd met Azure Active Directory Identity Protection.
services: security-center
documentationcenter: na
author: terrylan
manager: MBaldwin
editor: ''
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2018
ms.author: yurid
ms.openlocfilehash: 9c13bd671efee5bc07885320cbaa0bd090cc1390
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51226357"
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Azure Active Directory Identity Protection koppelen aan Azure Security Center
Dit document helpt u bij het configureren van de integratie tussen Azure Active Directory (AD) Identity Protection en Azure Security Center.

## <a name="why-connect-azure-ad-identity-protection"></a>Waarom moet ik Azure AD Identity Protection koppelen?
[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) helpt mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie detecteren. Wanneer de verbinding is gemaakt, bent u Azure AD Identity Protection-waarschuwingen weergeven in Security Center. Deze integratie kunt u bekijken, correleren en onderzoeken van alle beveiligingswaarschuwingen die betrekking hebben op uw hybride cloudworkloads in Security Center. 

## <a name="how-do-i-configure-this-integration"></a>Hoe configureer ik deze integratie?
Als uw organisatie maakt al gebruik van Azure AD Identity Protection, volgt u onderstaande stappen voor het configureren van de integratie:

1. Open het dashboard **Security Center**.
2. Klik in het linkerdeelvenster op **beveiligingsoplossingen**. Security Center detecteert automatisch of Azure AD Identity Protection is ingeschakeld voor uw organisatie.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Klik op **CONNECT**.
4. In de **integreren van Azure AD Identity Protection** pagina, schuif naar beneden en selecteer de juiste werkruimte:

    ![werkruimte](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. Klik op **Verbinden**.

Wanneer u klaar bent met deze configuratie, de Azure AD Identity Protection-oplossing wordt weergegeven in de **beveiligingsoplossingen** pagina onder **verbonden oplossingen**. 

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure AD Identity Protection met Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Microsoft Advanced Threat Analytics koppelen aan Azure Security Center](security-center-ata-integration.md)
* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) : Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : ophalen van de meest recente Azure-beveiliging-nieuws en informatie.


