---
title: Hoe-vereisen beheerde apparaten voor toegang tot cloud-Apps met voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Informatie over het configureren van Azure Active Directory (Azure AD)-beleidsregels voor voorwaardelijke toegang op basis van apparaten die beheerde apparaten voor toegang tot de cloud-Apps vereisen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 066d25e8953a2be4bd64cdd1af79b7f2a25dd5f9
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035907"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Procedure: Vereisen beheerde apparaten voor toegang tot cloud-Apps met voorwaardelijke toegang

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory (Azure AD) eenmalige aanmelding voor apps en services vanaf elke locatie. Gemachtigde gebruikers hebben toegang tot uw cloud-apps van een breed scala aan apparaten, zoals mobiele en ook op persoonlijke apparaten. Veel omgevingen hebben echter ten minste enkele apps die alleen door apparaten die voldoen aan uw standaarden voor beveiliging en naleving moeten worden benaderd. Deze apparaten zijn ook wel bekend als beheerde apparaten. 

Dit artikel wordt uitgelegd hoe u beleidsregels voor voorwaardelijke toegang waarvoor beheerde apparaten toegang tot bepaalde cloud-apps in uw omgeving kunt configureren. 


## <a name="prerequisites"></a>Vereisten

Beheerde apparaten vereisen voor cloud app toegang ties **voorwaardelijke toegang van Azure AD** en **Azure AD-Apparaatbeheer** samen. Als u nog niet bekend bent met een van deze gebieden, moet u eerst de volgende onderwerpen lezen:

- **[Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -dit artikel vindt u een conceptueel overzicht van voorwaardelijke toegang en de verwante terminologie.

- **[Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)**  -in dit artikel geeft een overzicht van de verschillende opties hebt tot het registreren van apparaten onder het organisatie-beheer. 


## <a name="scenario-description"></a>Scenariobeschrijving

De balans tussen beveiliging en productiviteit mastering is moeilijk. De komst van ondersteunde apparaten toegang krijgen tot uw cloudresources helpt bij het verbeteren van de productiviteit van uw gebruikers. De zijde spiegelen wilt u waarschijnlijk niet bepaalde bronnen in uw omgeving worden geopend door apparaten met een onbekende beveiligingsniveau. Voor de betrokken resources, moet u instellen dat gebruikers alleen toegang tot deze met een beheerd apparaat. 

U kunt deze vereiste met een enkele beleidsregel die toegang verleent oplossen met voorwaardelijke toegang van Azure AD:

- Aan de geselecteerde cloud-apps

- Voor de geselecteerde gebruikers en groepen

- Het vereisen van een beheerd apparaat


## <a name="managed-devices"></a>Beheerde apparaten  

Beheerde apparaten zijn in termen van eenvoudige, worden apparaten zijn onder *sommige sorteren* van organisatie-besturingselement. In Azure AD is de vereiste voor een beheerd apparaat dat is geregistreerd met Azure AD. Een apparaat wordt geregistreerd, wordt een identiteit voor het apparaat in de vorm van een apparaatobject gemaakt. Dit object wordt gebruikt door Azure om bij te houden statusinformatie over een apparaat. U kunt dit object met in-of uitschakelen (in-of uitschakelen) de status van een apparaat al gebruiken als een Azure AD-beheerder.
  
![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Als u een apparaat dat is geregistreerd in Azure AD, hebt u drie opties:

- **[Azure AD apparaten geregistreerd](device-management-introduction.md#azure-ad-registered-devices)**  : ophalen van een persoonlijk apparaat geregistreerd bij Azure AD

- **[Azure AD die lid zijn van de apparaten](device-management-introduction.md#azure-ad-joined-devices)**  : ophalen van een organisatie Windows 10-apparaten die niet is gekoppeld aan een on-premises AD geregistreerd in Azure AD. 

- **[Hybride Azure AD die lid zijn van de apparaten](device-management-introduction.md#hybrid-azure-ad-joined-devices)**  : ophalen van een Windows 10-apparaat dat lid is van een on-premises AD geregistreerd in Azure AD.

Als een beheerd apparaat, een geregistreerd apparaat moet ofwel een **hybride Azure AD toegevoegd apparaat** of een **apparaat dat is gemarkeerd als compatibel**.  

![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Vereisen hybride Azure AD die lid zijn van apparaten

In uw beleid voor voorwaardelijke toegang, kunt u **gekoppelde hybride Azure AD-apparaat vereist** naar status dat de geselecteerde cloud-apps alleen kunnen worden geopend met behulp van een beheerd apparaat. 

![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/10.png)

Deze instelling geldt alleen voor Windows 10-apparaten die lid zijn van een on-premises AD. U kunt alleen deze apparaten registreren met Azure AD dat gebruikmaakt van een hybride Azure AD join, dit is een [geautomatiseerd proces](device-management-hybrid-azuread-joined-devices-setup.md) ophalen van een Windows 10-apparaat is geregistreerd. 

![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/45.png)

Wat een hybride Azure AD maakt toegevoegd apparaat een beheerd apparaat?  Voor apparaten die lid zijn van een on-premises AD, wordt ervan uitgegaan dat de controle over deze apparaten wordt afgedwongen met oplossingen voor het beheer, zoals **System Center Configuration Manager (SCCM)** of **Groepsbeleid (GP)** om deze te beheren. Omdat er geen methode voor Azure AD om te bepalen is of een van deze methoden is toegepast op een apparaat, is het vereisen van een apparaat aan een hybride Azure AD een relatief zwakke mechanisme voor het vereisen van een beheerd apparaat. Het is aan u als beheerder om te beoordelen of de methoden die worden toegepast op uw on-premises domein apparaten sterk genoeg is om deel uitmaken van een beheerd apparaat als een dergelijk apparaat ook een apparaat aan een hybride Azure AD is zijn.


## <a name="require-device-to-be-marked-as-compliant"></a>Vereisen dat het apparaat moet worden gemarkeerd als compatibel

De optie voor het *vereisen dat een apparaat worden gemarkeerd als compatibel* vormt de krachtigste om aan te vragen van een beheerd apparaat.

![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/11.png)

Deze optie vereist dat een apparaat moet worden geregistreerd met Azure AD en zijn gemarkeerd als compatibel met:
         
- Intune.
- Een management (MDM)-systeem voor mobiele apparaten van derden waarmee Windows 10-apparaten via Azure AD-integratie worden beheerd. MDM-systemen van derden voor apparaattypen besturingssysteem dan Windows 10 worden niet ondersteund.
 
![Voorwaarden op basis van apparaten](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Voor een apparaat dat is gemarkeerd als compatibel, kunt u ervan uitgaan dat: 

- De mobiele apparaten die uw werknemers gebruikt voor toegang tot bedrijfsgegevens worden beheerd
- Mobiele apps die maakt gebruik van uw werknemers worden beheerd
- Gegevens van uw bedrijf wordt te helpen bepalen hoe uw werknemers worden gebruikt en deelt deze beveiligd
- Het apparaat en de apps die compatibel zijn met de beveiligingsvereisten bedrijf




## <a name="next-steps"></a>Volgende stappen

Voordat u configureert een beleid voor voorwaardelijke toegang op basis van apparaten in uw omgeving, dient u te kijken hoe de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md).

