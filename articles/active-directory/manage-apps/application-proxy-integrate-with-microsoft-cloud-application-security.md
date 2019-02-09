---
title: On-premises apps integreren met Cloud App Security - Azure Active Directory | Microsoft Docs
description: Configureer een on-premises toepassing in Azure Active Directory om te werken met Microsoft Cloud App Security (MCAS). De MCAS voorwaardelijke toegang tot App-beheer gebruiken voor het bewaken van en controle-sessies in realtime op basis van beleid voor voorwaardelijke toegang. U kunt deze beleidsregels toepassen op de on-premises toepassingen die gebruikmaken van Application Proxy in Azure Active Directory (Azure AD).
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: fe35f91d9b27d9ac5ab511de99fc2bf1dc74bdce
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55958819"
---
# <a name="configure-real-time-application-access-monitoring-with-microsoft-cloud-app-security-and-azure-active-directory"></a>Realtime toegang toepassingsbewaking configureren met Microsoft Cloud App Security en Azure Active Directory
Een on-premises toepassing in Azure Active Directory (Azure AD) naar Microsoft Cloud App Security (MCAS) gebruiken voor realtime-controle configureren. MCAS maakt gebruik van App-beheer voor voorwaardelijke toegang voor het bewaken van en controle-sessies in realtime op basis van beleid voor voorwaardelijke toegang. U kunt deze beleidsregels toepassen op de on-premises toepassingen die gebruikmaken van Application Proxy in Azure Active Directory (Azure AD).

Hier volgen enkele voorbeelden van de typen beleidsregels die u met MCAS maken kunt:

- Blokkeren of het downloaden van gevoelige documenten op niet-beheerde apparaten te beveiligen.
- Monitor als met een hoog risico gebruikers bij toepassingen aanmelden en meld u vervolgens de acties op basis van binnen de sessie. Met deze informatie kunt kunt u gedrag van gebruikers om te bepalen hoe om toe te passen sessiebeleid analyseren.
- Clientcertificaten of het nalevingsbeleid voor apparaten gebruiken voor toegang tot specifieke toepassingen blokkeren van niet-beheerde apparaten.
- Gebruikerssessies voorkomen dat niet-bedrijfsnetwerken. U kunt beperkte toegang verlenen aan gebruikers met toegang tot een toepassing buiten uw bedrijfsnetwerk. Deze beperkte toegang kan bijvoorbeeld worden geblokkeerd dat de gebruiker van gevoelige documenten downloaden.

Zie voor meer informatie, [apps beveiligen met Microsoft Cloud App Security-App beheer voor voorwaardelijke toegang](/cloud-app-security/proxy-intro-aad).

## <a name="requirements"></a>Vereisten

Licentie:

- EMS E5-licentie, of 
- Azure Active Directory Premium P1 en MCAS zelfstandige.

On-premises toepassing:

- De on-premises toepassing moet Kerberos-beperkte delegatie (KCD) gebruiken

Toepassingsproxy configureren:

- Azure AD voor het gebruik van Application Proxy, met inbegrip van uw omgeving voorbereiden en het installeren van de Application Proxy-connector configureren. Zie voor een zelfstudie [toevoegen van een on-premises toepassingen voor externe toegang via Application Proxy in Azure AD](application-proxy-add-on-premises-application.md). 

## <a name="add-on-premises-application-to-azure-ad"></a>On-premises toepassing naar Azure AD toevoegen

Toevoegen van een on-premises toepassing naar Azure AD. Zie voor een snelstartgids [een on-premises app toevoegen aan Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). Bij het toevoegen van de toepassing, moet u de volgende twee instellingen instellen in de **toevoegen van uw on-premises toepassing** blade:

- **Verificatie vooraf**: Enter **Azure Active Directory**.
- **URL's vertalen in de hoofdtekst van de toepassing**: Kies **Ja**.

Deze twee instellingen zijn vereist voor de toepassing om te werken met MCAS.

## <a name="test-the-on-premises-application"></a>De on-premises toepassing testen

Na het toevoegen van uw toepassing naar Azure AD, gebruikt u de stappen in [de toepassing testen](application-proxy-add-on-premises-application.md#test-the-application) aan een gebruiker toevoegen voor het testen en test de aanmelding. 

## <a name="deploy-conditional-access-app-control"></a>App-beheer voor voorwaardelijke toegang implementeren

Volg de instructies in voor het configureren van uw toepassing met de Toepassingsbeheer voor voorwaardelijke toegang, [Toepassingsbeheer voor voorwaardelijke toegang voor Azure AD-apps implementeren](/cloud-app-security/proxy-deployment-aad).


## <a name="test-conditional-access-app-control"></a>App-beheer voor voorwaardelijke toegang van test

Als u wilt testen van de implementatie van Azure AD-toepassingen met beheer van de toepassing voor voorwaardelijke toegang, volg de instructies in [testen van de implementatie voor Azure AD-apps](/cloud-app-security/proxy-deployment-aad).





