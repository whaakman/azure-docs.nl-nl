---
title: Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory | Microsoft Docs
description: 'Meer informatie over fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory '
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 31447d597296bac57481dc2acb4a95ee3a104161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory
Wanneer een toepassing op basis van SAML-integratie foutopsporing, is het vaak nuttig zijn voor het gebruik van een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) om te zien van de SAML-aanvraag, de SAML-reactie en de werkelijke SAML-token dat is uitgegeven aan de toepassing. Door in het SAML-token, kunt u ervoor zorgen dat alle vereiste kenmerken, de gebruikersnaam in het SAML-onderwerp en de URI van de verlener afkomstig via zoals verwacht.

![][1]

Het antwoord van Azure AD met het SAML-token is doorgaans een die plaatsvindt na een HTTP 302-omleiding van https://login.windows.net en wordt verzonden naar de geconfigureerde **antwoord-URL** van de toepassing. 

U kunt het SAML-token weergeven door deze regel te selecteren en vervolgens te klikken op de **Inspectors > webformulieren** tabblad in het rechterpaneel. Van daaruit met de rechtermuisknop op de **SAMLResponse** waarde en selecteert u **verzenden naar TextWizard**. Selecteer vervolgens **van Base64** van de **transformeren** menu voor het decoderen van het token en de inhoud weergeven.

**Opmerking**: overzicht van de inhoud van deze HTTP-aanvraag Fiddler u mogelijk gevraagd te configureren van de ontsleuteling van HTTPS-verkeer, wat u moet doen.

## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](../active-directory-saas-custom-apps.md) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
* [Het aanpassen van uitgegeven Claims in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png