---
title: Beveiliging voor Azure Active Directory-identiteit verbinding te maken met Azure Security Center | Microsoft Docs
description: 'Meer informatie over hoe Azure Security Center integreert met Azure Active Directory: Identity Protection.'
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 7562dd5e1c303a6cb97d25bda5aa080bb5643583
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Beveiliging voor Azure Active Directory-identiteit verbinding te maken met Azure Security Center
Dit document helpt u de integratie tussen Azure Active Directory (AD) Identity Protection en Azure Security Center configureren.

## <a name="why-connect-azure-ad-identity-protection"></a>Waarom Azure AD Identity Protection verbinding?
[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) helpt bij het detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Wanneer verbinding is gemaakt, bent u Azure AD Identity Protection-waarschuwingen weergeven in Security Center. Deze integratie kunt u weergeven en onderzoeken van alle beveiligingswaarschuwingen die betrekking hebben op uw hybride cloud-werkbelastingen in Security Center correleren. 

## <a name="how-do-i-configure-this-integration"></a>Hoe kan ik deze integratie configureren?
Als uw organisatie al van Azure AD Identity Protection gebruikmaakt, volgt u onderstaande stappen voor het configureren van de integratie:

1. Open het dashboard **Security Center**.
2. Klik in het linkerdeelvenster op **beveiligingsoplossingen**. Security Center detecteert automatisch als Azure AD Identity Protection is ingeschakeld voor uw organisatie.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Klik op **CONNECT**.
4. In de **integreren Azure AD Identity Protection** pagina, schuif naar beneden en selecteer de juiste werkruimte:

    ![Werkruimte](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. Klik op **Verbinden**.

Wanneer u klaar bent met deze configuratie, de Azure AD Identity Protection-oplossing wordt weergegeven in de **beveiligingsoplossingen** pagina onder **verbonden oplossingen**. 

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure AD Identity Protection Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Microsoft Advanced Threat Analytics koppelen aan Azure Security Center](security-center-ata-integration.md)
* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) : Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van gegevens van Azure Security Center](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) — Lees het laatste nieuws van de Azure-beveiliging en de informatie.


