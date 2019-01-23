---
title: Hoe-vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang van Azure Active Directory te beheren | Microsoft Docs
description: Informatie over het configureren van Azure Active Directory (Azure AD)-beleid voor voorwaardelijke toegang op basis van apparaten die beheerde apparaten voor toegang tot cloud-Apps vereisen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2df2d716516cb8e451c054fce3e184e1b4bf3f9b
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447595"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Procedures: Beheerde apparaten vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang

In een wereld mobiliteit, cloud-first kunt Azure Active Directory (Azure AD) single sign-on bij apps en services vanaf elke locatie. Geautoriseerde gebruikers kunnen toegang krijgen tot uw cloud-apps uit een breed scala aan apparaten, waaronder mobiele en ook op persoonlijke apparaten. Veel omgevingen hebben echter ten minste een aantal apps dat alleen moeten worden geopend vanaf apparaten die voldoen aan uw normen voor beveiliging en naleving. Deze apparaten zijn ook wel bekend als beheerde apparaten. 

In dit artikel wordt uitgelegd hoe u beleid voor voorwaardelijke toegang die vereisen dat beheerde apparaten toegang krijgen tot bepaalde cloud-apps in uw omgeving kunt configureren. 


## <a name="prerequisites"></a>Vereisten

Beheerde apparaten vereisen voor cloud-app toegang ties **voorwaardelijke toegang voor Azure AD** en **Azure AD-Apparaatbeheer** samen. Als u nog niet bekend bent met een van deze gebieden, moet u eerst de volgende onderwerpen lezen:

- **[Voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md)**  -in dit artikel biedt u een conceptueel overzicht van voorwaardelijke toegang en de bijbehorende terminologie.

- **[Inleiding tot Apparaatbeheer in Azure Active Directory](../devices/overview.md)**  -in dit artikel geeft een overzicht van de verschillende opties die u kunt apparaten in organisatie-beheer. 


## <a name="scenario-description"></a>Scenariobeschrijving

Onder de knie krijgen de balans tussen beveiliging en productiviteit is een uitdaging. De toename van ondersteunde apparaten voor toegang tot uw cloudresources kunt u de productiviteit van uw gebruikers verbeteren. Aan de andere kant wilt u waarschijnlijk geen bepaalde resources in uw omgeving om te worden geopend vanaf apparaten met een onbekende beveiligingsniveau. Voor de betrokken resources, moet u vereisen dat gebruikers hebben alleen toegang met behulp van een beheerd apparaat. 

Met Azure AD voor voorwaardelijke toegang, kunt u deze vereiste met één beleid waarmee toegang wordt verleend adres:

- Aan de geselecteerde cloud-apps

- Voor de geselecteerde gebruikers en groepen

- Een beheerd apparaat vereisen


## <a name="managed-devices"></a>Beheerde apparaten  

Simpel gezegd, beheerde apparaten zijn apparaten die onder *sommige sorteren* van organisatie-besturingselement. In Azure AD is de vereiste voor een beheerd apparaat dat is het geregistreerd bij Azure AD. Registreren van een apparaat, maakt u een identiteit voor het apparaat in de vorm van een apparaatobject. Dit object wordt gebruikt door Azure voor het bijhouden van statusinformatie over een apparaat. U kunt dit object aan in-/ uitschakelen (in-of uitschakelen) de status van een apparaat al gebruiken als een Azure AD-beheerder.
  
![Voorwaarden op basis van apparaat](./media/require-managed-devices/32.png)

Als u een apparaat is geregistreerd bij Azure AD, hebt u drie opties:

- **[Azure AD ingeschreven apparaten](../devices/overview.md#azure-ad-registered-devices)**  : als u wilt ophalen van een persoonlijk apparaat is geregistreerd bij Azure AD

- **[Azure AD gekoppelde apparaten](../devices/overview.md#azure-ad-joined-devices)**  : als u wilt ophalen van een organisatie-Windows 10-apparaat die niet is gekoppeld aan een on-premises AD geregistreerd bij Azure AD. 

- **[Hybride Azure AD gekoppelde apparaten](../devices/overview.md#hybrid-azure-ad-joined-devices)**  : als u wilt ophalen van een Windows 10 of een ondersteunde downlevel-apparaten die lid van een on-premises is AD geregistreerd bij Azure AD.

Als u wilt worden van een beheerd apparaat, een geregistreerd apparaat moet een **hybride Azure AD toegevoegde apparaat** of een **apparaat dat is gemarkeerd als compatibel**.  

![Voorwaarden op basis van apparaat](./media/require-managed-devices/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Vereisen van hybride Azure AD gekoppelde apparaten

In het beleid voor voorwaardelijke toegang, kunt u **vereisen dat het apparaat is toegevoegd aan Hybrid Azure AD** om aan te geven dat de geselecteerde cloud-apps alleen kunnen worden geopend met behulp van een beheerd apparaat. 

![Voorwaarden op basis van apparaat](./media/require-managed-devices/10.png)

Deze instelling is alleen van toepassing op Windows 10 of downlevel-apparaten, zoals Windows 7 of Windows 8 die zijn gekoppeld aan een on-premises AD. U kunt deze apparaten alleen registreren met Azure AD met behulp van een hybride Azure AD-join, dit is een [geautomatiseerd](../devices/hybrid-azuread-join-plan.md) om op te halen van een Windows 10-apparaat dat is geregistreerd. 

![Voorwaarden op basis van apparaat](./media/require-managed-devices/45.png)

Wat is een hybride Azure AD toegevoegde apparaat een beheerd apparaat?  Voor apparaten die zijn gekoppeld aan een on-premises AD, wordt ervan uitgegaan dat de controle over deze apparaten wordt afgedwongen met behulp van de oplossingen zoals **System Center Configuration Manager (SCCM)** of **Groepsbeleid (GP)** om deze te beheren. Omdat er geen methode voor Azure AD om te bepalen of een van deze methoden is toegepast op een apparaat, een hybride Azure AD gekoppelde apparaat dat is een relatief zwakke mechanisme voor het vereisen van een beheerd apparaat. Het is aan u als beheerder om te beoordelen of de methoden die worden toegepast op uw on-premises domein apparaten zijn sterk genoeg is om deel uitmaken van een beheerd apparaat als een dergelijk apparaat ook een hybride Azure AD gekoppelde apparaat is.


## <a name="require-device-to-be-marked-as-compliant"></a>Vereisen dat het apparaat moet worden gemarkeerd als compatibel

De optie voor het *vereisen dat een apparaat moet worden gemarkeerd als compatibel* is de krachtigste om aan te vragen van een beheerd apparaat.

![Voorwaarden op basis van apparaat](./media/require-managed-devices/11.png)

Deze optie vereist dat een apparaat worden geregistreerd bij Azure AD en moet worden gemarkeerd als compatibel met:
         
- Intune.
- Een management (MDM)-systeem voor mobiele apparaten van derden waarmee Windows 10-apparaten via Azure AD-integratie worden beheerd. MDM-systemen van derden voor apparaattypen besturingssysteem dan Windows 10 worden niet ondersteund.
 
![Voorwaarden op basis van apparaat](./media/require-managed-devices/46.png)



Voor een apparaat dat is gemarkeerd als compatibel, kunt u ervan uitgaan dat: 

- De mobiele apparaten die uw werknemers worden gebruikt voor toegang tot bedrijfsgegevens worden beheerd
- Mobiele apps die uw werknemers worden beheerd
- Gegevens van uw bedrijf wordt beveiligd door het helpt bij het bepalen hoe uw werknemers toegang heeft tot en deze delen
- Het apparaat en de apps die compatibel zijn met de beveiligingsvereisten bedrijf




## <a name="next-steps"></a>Volgende stappen

Voordat u beleid voor voorwaardelijke toegang op basis van het apparaat configureert in uw omgeving, moet u Kijk eens de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md).

