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
ms.date: 03/20/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c34af889f65e0fa91f97655803af099dadcabc5d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Toepassingsproxy en Qlik verstand 
Azure Active Directory-toepassingsproxy en Qlik zin hebben hun samen zodat u zich eenvoudig kunt toepassingsproxy gebruiken voor externe toegang voor uw implementatie Qlik zin.  

## <a name="prerequisites"></a>Vereisten 
De rest van dit scenario wordt ervan uitgegaan dat u het volgende gedaan:
 
- Geconfigureerd [Qlik zin](https://help.qlik.com/sense/1.1/Content/Introduction.htm). 
- Een connector voor toepassingsproxy geïnstalleerd 

## <a name="install-an-application-proxy-connector"></a>Een Application Proxy connector installeren 
Als u al Application Proxy ingeschakeld hebt en hebt een connector is geïnstalleerd, kunt u deze sectie overslaan en verplaatst op naar [uw app toevoegen aan Azure AD met toepassingsproxy](application-proxy-ping-access.md). 

De connector voor toepassingsproxy is een Windows Server-service die het verkeer van uw werknemers extern naar uw gepubliceerde apps wordt verwezen. Zie voor meer installatie-instructies, [toepassingsproxy inschakelen in de Azure portal](active-directory-application-proxy-enable.md). 


1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als globale beheerder. 
2. Selecteer de Azure Active Directory > toepassingsproxy. 
3. Selecteer de Connector downloaden om de Application Proxy connector downloaden te starten. Volg de installatie-instructies. 
 
>[!NOTE]
>Downloaden van de connector moet automatisch toepassingsproxy inschakelen voor uw directory, maar als u dit niet kunt selecteren **Enable Application Proxy**. 
 
## <a name="publish-your-applications-in-azure"></a>Uw toepassingen publiceren in Azure 
Als u wilt publiceren QlikSense, moet u twee toepassingen publiceren in Azure.  

### <a name="application-1"></a>#1-toepassing: 
Volg deze stappen voor het publiceren van uw app. Voor een meer overzicht van de stappen 1-8, Zie gedetailleerde [toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md). 


1. Als dat niet in de laatste sectie aanmelden bij de Azure-portal als globale beheerder. 
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen**. 
3. Selecteer **toevoegen** boven aan de blade. 
4. Selecteer **On-premises toepassing**. 
5.       Vul de vereiste velden met informatie over uw nieuwe app. Gebruik de volgende richtlijnen voor de instellingen: 
    - **Interne URL**: deze toepassing moet een interne URL de URL van de QlikSense is. Bijvoorbeeld: **https&#58;//demo.qlikemm.com** 
    - **Methode voor verificatie vooraf**: Azure Active Directory (aanbevolen maar niet vereist) 
1.       Selecteer **toevoegen** onderaan de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend. 
2.       Selecteer in het menu Snel starten **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing. 
3.       Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker test. 
4.       (Optioneel) Selecteer op het tabblad app-beheer eenmalige aanmelding. Kies **Kerberos-beperkte overdracht** uit de vervolgkeuzelijst en vul de vereiste velden op basis van uw configuratie Qlik. Selecteer **Opslaan**. 

### <a name="application-2"></a>Toepassing #2: 
Volg dezelfde stappen als toepassing #1, met de volgende uitzonderingen: 

**Stap &#5;**: de interne URL moet nu de QlikSense-URL met de verificatiepoort gebruikt door de toepassing. De standaardwaarde is **4244** voor HTTPS en 4248 voor HTTP. Ex: **https&#58;//demo.qlik.com:4244** 
**stap #10:** niet instellen van eenmalige aanmelding en laat de **eenmalige aanmelding uitgeschakeld**
 
 
## <a name="testing"></a>Testen 
Uw toepassing is nu gereed om te testen. Toegang tot de externe URL die u hebt gebruikt voor het publiceren van QlikSense in toepassing #1 en meld u aan als een gebruiker die is toegewezen aan beide toepassingen.  

## <a name="next-steps"></a>Volgende stappen

- [Publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md)
- [Werken met toepassingsproxy connectors](active-directory-application-proxy-connectors-azure-portal.md).
