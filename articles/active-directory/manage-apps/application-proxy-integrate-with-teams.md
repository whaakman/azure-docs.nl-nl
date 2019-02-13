---
title: Toegang tot Azure AD-toepassingsproxy-apps in Teams | Microsoft Docs
description: Azure AD-toepassingsproxy gebruiken voor toegang tot uw on-premises toepassing via Microsoft Teams.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/05/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 660d05146e82ab85136cac715b56cb1ee89b002f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210948"
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Toegang tot uw on-premises toepassingen via Microsoft Teams

Azure Active Directory-toepassingsproxy biedt eenmalige aanmelding tot on-premises toepassingen waar u ook bent. Microsoft Teams wordt uw gezamenlijke inspanningen op één plek gestroomlijnd. Integratie van de twee samen betekent dat uw gebruikers productief zijn met hun collega's in een situatie. 

Uw gebruikers kunnen cloud-apps toevoegen aan hun Teams-kanalen [met tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), maar hoe zit het SharePoint-sites of planning die on-premises gehost? Application Proxy is de oplossing. Deze kunnen apps die zijn gepubliceerd via toepassingsproxy aan hun kanalen met behulp van de dezelfde externe URL's die ze altijd gebruiken voor toegang tot hun apps op afstand toevoegen. En omdat Application Proxy worden geverifieerd via Azure Active Directory, uw gebruikers een ervaring voor eenmalige aanmelding.


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>De Application Proxy-connector installeert en uw app publiceren

Als u dat nog niet gedaan hebt, [Application Proxy configureren voor uw tenant en het installeren van de connector](application-proxy-add-on-premises-application.md). Vervolgens [publiceren van uw on-premises toepassing](application-proxy-add-on-premises-application.md) voor externe toegang. Wanneer u de app publiceert, noteer de externe URL omdat deze wordt gebruikt om de app toevoegen aan Teams.

Als u al uw Apps zijn gepubliceerd, maar niet meer de externe URL's weet, controleren of ze de [Azure-portal](https://portal.azure.com). Meld u aan en navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** > Selecteer uw app >  **Toepassingsproxy**.

## <a name="add-your-app-to-teams"></a>Uw app toevoegen aan Teams

Nadat u de app via toepassingsproxy publiceert, kunt u uw gebruikers weten dat ze deze als een tabblad rechtstreeks in hun Teams-kanalen toevoegen kunnen en vervolgens de app is beschikbaar voor iedereen in het team om te gebruiken. Hebben ze deze drie stappen:

1. Navigeer naar de Teams-kanaal waar u deze app en selecteer **+** een tabblad toevoegen.

   ![Selecteer een tabblad toevoegen](./media/application-proxy-integrate-with-teams/add-tab.png)

2. Selecteer **Website** van de opties op het tabblad.

   ![Een website toevoegen](./media/application-proxy-integrate-with-teams/website.png)

3. Geef een naam op het tabblad en de URL ingesteld op de externe URL van de Application Proxy. 

   ![Tabbladnaam en URL configureren](./media/application-proxy-integrate-with-teams/tab-name-url.png)

Wanneer een lid van een team wordt het tabblad toegevoegd, wordt deze weergegeven voor iedereen in het kanaal. Gebruikers die toegang tot de app hebben krijgen toegang voor eenmalige aanmelding met de referenties die ze voor Microsoft Teams gebruiken. Alle gebruikers die geen toegang tot de app naar het tabblad in Teams kunnen zien, maar is geblokkeerd totdat u ze machtigingen tot de on-premises-app en de Azure portal gepubliceerde versie van de app zodat. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [on-premises SharePoint-sites publiceren](application-proxy-integrate-with-sharepoint-server.md) met Application Proxy.
- Configureren van uw apps gebruik [aangepaste domeinen](application-proxy-configure-custom-domain.md) voor de externe URL. 
