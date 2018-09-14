---
title: Snelstartgids - toegang blokkeren wanneer het risico van een sessie wordt gedetecteerd met Azure Active Directory Identity Protection | Microsoft Docs
description: In deze snelstartgids leert u hoe u een Azure Active Directory (Azure AD) Identity Protection-beleid aanmeldingsrisico voor voorwaardelijke toegang voor het blokkeren van aanmeldingen op basis van de sessie risico's kunt configureren.
services: active-directory
keywords: identiteitsbeveiliging, voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: identity-protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: cadcc806b9aaeea4f2fc68c911e09c7e35926623
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45552405"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Snelstartgids: Toegang blokkeren als er een sessie risico's met Azure Active Directory Identity Protection wordt gedetecteerd  

Om te voorkomen dat uw omgeving beveiligd, is het raadzaam om verdachte gebruikers zich te blokkeren. Azure Active Directory (Azure AD) Identity Protection analyseert elke aanmelding en berekent de kans dat een aanmelding bij poging is niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount. De kans (laag, Gemiddeld, hoog) wordt aangegeven in de vorm van een berekende waarde met de naam van het niveau van aanmeldingsrisico. De voorwaarde voor aanmeldingsrisico instelt, kunt u beleid voor aanmeldingsrisico voorwaardelijke toegang om te reageren op specifieke aanmeldingsrisico niveaus configureren. 

Deze snelstartgids leest hoe het configureren van een beleid voor aanmeldingsrisico voorwaardelijke toegang die blokkeert een aanmelding als een gemiddeld en hoger aanmeldingsrisico niveau is gedetecteerd. 

![Beleid maken](./media/quickstart-sign-in-risk-policy/1003.png)


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="prerequisites"></a>Vereisten 

Als u wilt het scenario in deze zelfstudie hebt voltooid, hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium P2-editie** -Azure AD Identity Protection is een functie van Azure AD Premium P2. 

- **Identity Protection** -Identity Protection wordt ingeschakeld door het scenario in deze Quick Start is vereist. Als u niet hoe u Identity Protection inschakelen weet, raadpleegt u [inschakelen van Azure Active Directory Identity Protection](../identity-protection/enable.md).

- **Tor Browser** : de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) is ontworpen om u te helpen u uw privacy online beschermen. Identity Protection detecteert een aanmelding vanuit een Tor Browser als **aanmeldingen vanaf anonieme IP-adressen**, die is voorzien van een gemiddeld risico-niveau. Zie [Risicogebeurtenissen in Azure Active Directory](../reports-monitoring/concept-risk-events.md) voor meer informatie.  

- **Een testaccount met de naam Alain Charon** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Test de aanmelding 

Het doel van deze stap is om ervoor te zorgen dat uw testaccount toegang heeft tot uw tenant met behulp van de Tor-Browser.

**Voor het testen van de aanmelding:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alain Charon**.

2. Meld u af. 


## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken 

Het scenario in deze snelstartgids wordt een aanmelding vanuit een Tor-Browser gebruikt voor het genereren van een gedetecteerde **aanmeldingen vanaf anonieme IP-adressen** risicogebeurtenis. Het risiconiveau van deze risicogebeurtenis is normaal. Om te reageren op deze risicogebeurtenis, kunt u de voorwaarde voor aanmeldingsrisico instellen op Gemiddeld. 

Deze sectie wordt beschreven hoe u de vereiste aanmeldingsrisico-beleid voor voorwaardelijke toegang maken. Stel in het beleid:

|Instelling |Waarde|
|---     | --- |
| Gebruikers  | Alain Charon  |
| Voorwaarden | Aanmeldingsrisico, gemiddeld en hoger |
| Besturingselementen | Toegang blokkeren |
 

![Beleid maken](./media/quickstart-sign-in-risk-policy/201.png)

 


**Uw beleid voor voorwaardelijke toegang configureren:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als globale beheerder.

2. Klik in de Azure-portal op de navigatiebalk links op **alle services**. 

4. In de **Filter** tekstvak, type **identiteitsbeveiliging**.

5. Klik op **Azure AD Identity Protection**.   
 
6. Op de **Azure AD Identity Protection** pagina, in de **configureren** sectie, klikt u op **aanmelden beleid voor gebruikersrisico's**.
 
5. Op de beleidspagina in de **toewijzingen** sectie, klikt u op **gebruikers**.

6. Op de **gebruikers** pagina, klikt u op **gebruikers selecteren**.

7. Op de **gebruikers selecteren** pagina, selecteert u **Alain Charon**, en klik vervolgens op **Selecteer**.

8. Op de **gebruikers** pagina, klikt u op **gedaan**. 

9. Op de beleidspagina in de **toewijzingen** sectie, klikt u op **voorwaarden**.

10. Op de **voorwaarden** pagina, klikt u op **aanmeldingsrisico**.

11. Op de **aanmeldingsrisico** weergeeft, schakelt **gemiddeld en hoger**, en klik vervolgens op **Selecteer**. 

12. Op de **voorwaarden** pagina, klikt u op **gedaan**.

13. Op de beleidspagina in de **besturingselementen** sectie, klikt u op **toegang**.

14. Op de **toegang** pagina, klikt u op **toegang toestaan**, selecteer **meervoudige verificatie vereisen**, en klik vervolgens op **Selecteer**.

15. Klik op de beleidspagina **opslaan**.  


## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alan Charon** met behulp van de Tor-Browser. Uw aanmeldingspoging moet worden geblokkeerd door uw beleid voor voorwaardelijke toegang.

![Multi-Factor Authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker, de Tor-Browser en het aanmeldingsrisico-beleid voor voorwaardelijke toegang uitschakelen:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Zie voor instructies voor het verwijderen van de Tor Browser, [verwijderen](https://tb-manual.torproject.org/en-US/uninstalling.html).


