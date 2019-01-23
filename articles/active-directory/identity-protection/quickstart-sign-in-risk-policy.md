---
title: Snelstartgids - toegang blokkeren wanneer het risico van een sessie wordt gedetecteerd met Azure Active Directory Identity Protection | Microsoft Docs
description: In deze snelstartgids leert u hoe u een Azure Active Directory (Azure AD) Identity Protection-beleid aanmeldingsrisico voor voorwaardelijke toegang voor het blokkeren van aanmeldingen op basis van de sessie risico's kunt configureren.
services: active-directory
keywords: identiteitsbeveiliging, voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
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
ms.openlocfilehash: 7cf427ed7be91db2b243c28bb66513b25045ffc0
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54468485"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Quickstart: Toegang blokkeren als er een sessie risico's met Azure Active Directory Identity Protection wordt gedetecteerd  

Om te voorkomen dat uw omgeving beveiligd, is het raadzaam om verdachte gebruikers zich te blokkeren. Azure Active Directory (Azure AD) Identity Protection analyseert elke aanmelding en berekent de kans dat een aanmelding bij poging is niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount. De kans (laag, Gemiddeld, hoog) wordt aangegeven in de vorm van een berekende waarde met de naam van het niveau van aanmeldingsrisico. De voorwaarde voor aanmeldingsrisico instelt, kunt u beleid voor aanmeldingsrisico voorwaardelijke toegang om te reageren op specifieke aanmeldingsrisico niveaus configureren. 

Deze snelstartgids leest hoe het configureren van een beleid voor aanmeldingsrisico voorwaardelijke toegang die blokkeert een aanmelding als een gemiddeld en hoger aanmeldingsrisico niveau is gedetecteerd. 

![Beleid maken](./media/quickstart-sign-in-risk-policy/1004.png)


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.



## <a name="prerequisites"></a>Vereisten 

Voor het voltooien van het scenario in deze zelfstudie hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium P2-editie** -Azure AD Identity Protection is een functie van Azure AD Premium P2. 

- **Identity Protection** -Identity Protection wordt ingeschakeld door het scenario in deze Quick Start is vereist. Als u niet hoe u Identity Protection inschakelen weet, raadpleegt u [inschakelen van Azure Active Directory Identity Protection](../identity-protection/enable.md).

- **Tor Browser** : de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) is ontworpen om u te helpen u uw privacy online beschermen. Identity Protection detecteert een aanmelding vanuit een Tor Browser als **aanmeldingen vanaf anonieme IP-adressen**, die is voorzien van een gemiddeld risico-niveau. Zie [Risicogebeurtenissen in Azure Active Directory](../reports-monitoring/concept-risk-events.md) voor meer informatie.  

- **Een testaccount met de naam Alain Charon** : als u niet hoe ik een testaccount maakt weet, Zie [een nieuwe gebruiker toevoegen](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


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

2. Ga naar de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/Overview).
 
3. Op de **Azure AD Identity Protection** pagina, in de **configureren** sectie, klikt u op **aanmelden beleid voor gebruikersrisico's**.
 
4. Op de beleidspagina in de **toewijzingen** sectie, klikt u op **gebruikers**.

5. Op de **gebruikers** pagina, klikt u op **gebruikers selecteren**.

6. Op de **gebruikers selecteren** pagina, selecteert u **Alain Charon**, en klik vervolgens op **Selecteer**.

7. Op de **gebruikers** pagina, klikt u op **gedaan**. 

8. Op de beleidspagina in de **toewijzingen** sectie, klikt u op **voorwaarden**.

9. Op de **voorwaarden** pagina, klikt u op **aanmeldingsrisico**.

10. Op de **aanmeldingsrisico** weergeeft, schakelt **gemiddeld en hoger**, en klik vervolgens op **Selecteer**. 

11. Op de **voorwaarden** pagina, klikt u op **gedaan**.

12. Op de beleidspagina in de **besturingselementen** sectie, klikt u op **toegang**.

13. Op de **toegang** pagina, klikt u op **toegang toestaan**, selecteer **meervoudige verificatie vereisen**, en klik vervolgens op **Selecteer**.

14. Klik op de beleidspagina **opslaan**.  


## <a name="test-your-conditional-access-policy"></a>Uw voorwaardelijke toegangsbeleid testen

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alan Charon** met behulp van de Tor-Browser. Uw aanmeldingspoging moet worden geblokkeerd door uw beleid voor voorwaardelijke toegang.

![Multi-Factor Authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker, de Tor-Browser en het aanmeldingsrisico-beleid voor voorwaardelijke toegang uitschakelen:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [toevoegen of verwijderen van gebruikers](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Zie voor instructies voor het verwijderen van de Tor Browser, [verwijderen](https://tb-manual.torproject.org/uninstalling/).


