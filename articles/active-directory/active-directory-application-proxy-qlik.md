---
title: Toepassingsproxy van Azure AD en Qlik verstand | Microsoft Docs
description: Toepassingsproxy inschakelen in de Azure-portal en installeer de Connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/03/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7e4731e150c2d6428fa8c752765780f827c33741
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Toepassingsproxy en Qlik verstand 
Azure Active Directory-toepassingsproxy en Qlik zin hebben hun samen zodat u zich eenvoudig kunt toepassingsproxy gebruiken voor externe toegang voor uw implementatie Qlik zin.  

## <a name="prerequisites"></a>Vereisten 
De rest van dit scenario wordt ervan uitgegaan dat u het volgende gedaan:
 
- Geconfigureerd [Qlik zin](https://community.qlik.com/docs/DOC-19822). 
- [Een connector voor toepassingsproxy geïnstalleerd](manage-apps/application-proxy-enable.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uw toepassingen publiceren in Azure 
Als u wilt publiceren QlikSense, moet u twee toepassingen publiceren in Azure.  

### <a name="application-1"></a>#1-toepassing: 
Volg deze stappen voor het publiceren van uw app. Voor een meer overzicht van de stappen 1-8, Zie gedetailleerde [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md). 


1. Meld u aan bij de Azure portal als globale beheerder. 
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen**. 
3. Selecteer **toevoegen** boven aan de blade. 
4. Selecteer **On-premises toepassing**. 
5.       Vul de vereiste velden met informatie over uw nieuwe app. Gebruik de volgende richtlijnen voor de instellingen: 
    - **Interne URL**: deze toepassing moet een interne URL de URL van de QlikSense is. Bijvoorbeeld: **https&#58;//demo.qlikemm.com:4244** 
    - **Methode voor verificatie vooraf**: Azure Active Directory (aanbevolen maar niet vereist) 
1.       Selecteer **toevoegen** onderaan de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend. 
2.       Selecteer in het menu Snel starten **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing. 
3.       Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker test. 
4.       (Optioneel) Selecteer op het tabblad app-beheer eenmalige aanmelding. Kies **Kerberos-beperkte overdracht** uit de vervolgkeuzelijst en vul de vereiste velden op basis van uw configuratie Qlik. Selecteer **Opslaan**. 

### <a name="application-2"></a>Toepassing #2: 
Volg dezelfde stappen als toepassing #1, met de volgende uitzonderingen: 

**Stap 5 #**: de interne URL moet nu de QlikSense-URL met de verificatiepoort gebruikt door de toepassing. De standaardwaarde is **4244** voor HTTPS en 4248 voor HTTP. Ex: **https&#58;//demo.qlik.com:4244**</br></br> 
**Stap 10 #:** niet instellen van eenmalige aanmelding en laat de **eenmalige aanmelding uitgeschakeld**
 
 
## <a name="testing"></a>Testen 
Uw toepassing is nu gereed om te testen. Toegang tot de externe URL die u hebt gebruikt voor het publiceren van QlikSense in toepassing #1 en meld u aan als een gebruiker die is toegewezen aan beide toepassingen.  

## <a name="next-steps"></a>Volgende stappen

- [Publiceren van toepassingen met toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md)
- [Werken met toepassingsproxy connectors](active-directory-application-proxy-connectors-azure-portal.md).
