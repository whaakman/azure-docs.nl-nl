---
title: Toegang tot apps van Azure AD-toepassingsproxy in Teams | Microsoft Docs
description: Azure AD-toepassingsproxy gebruiken voor toegang tot uw on-premises toepassing via Microsoft-Teams.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a8dca307bb97898f229b39710a3b566ee913fcca
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Toegang tot uw on-premises toepassingen via Microsoft-Teams

Azure Active Directory-toepassingsproxy hebt u eenmalige aanmelding tot on-premises toepassingen ongeacht waar u zich bevindt. Microsoft-Teams stroomlijnt uw gezamenlijke inspanningen op één plek. De twee samen integreren betekent dat uw gebruikers productiever te maken met hun teamleden in een situatie. 

Uw gebruikers cloud-apps kunnen toevoegen aan hun kanalen Teams [tabbladen](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), maar hoe zit het SharePoint-sites of de planning van de software die zijn gehost lokale? Toepassingsproxy is de oplossing. Ze kunnen apps die zijn gepubliceerd via toepassingsproxy in hun kanalen met de dezelfde externe URL's die ze altijd gebruiken toegang tot hun apps op afstand toevoegen. En omdat Application Proxy worden geverifieerd via Azure Active Directory, uw gebruikers een ervaring voor eenmalige aanmelding.


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>De Application Proxy connector installeert en uw app publiceren

Als u dat nog niet gedaan hebt, [Application Proxy configureren voor uw tenant en de connector installeert](active-directory-application-proxy-enable.md). Vervolgens [publiceren van uw on-premises toepassing](application-proxy-publish-azure-portal.md) voor externe toegang. Wanneer u de app publiceren wilt: Noteer de externe URL omdat deze wordt gebruikt om de app toevoegen aan Teams.

Als u al uw Apps die zijn gepubliceerd, maar niet meer de externe URL's weet, opzoeken ze de [Azure-portal](https://portal.azure.com). Meld u aan en navigeert u naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** > uw app selecteren >  **Toepassingsproxy**.

## <a name="add-your-app-to-teams"></a>Uw app toevoegen aan Teams

Zodra u de app via toepassingsproxy publiceert, uw gebruikers laten weten dat ze als een tabblad rechtstreeks in hun kanalen Teams toevoegen kunnen, en de app beschikbaar is voor iedereen in het team om te gebruiken. Hebben ze deze drie stappen:

1. Navigeer naar de Teams kanaal waar u deze app toevoegen en selecteer  **+**  een tabblad toevoegen.

   ![Selecteer een tabblad toevoegen](./media/application-proxy-teams/add-tab.png)

2. Selecteer **Website** van het tabbladopties.

   ![Een website toevoegen](./media/application-proxy-teams/website.png)

3. Geef een naam op het tabblad en de URL ingesteld op de externe URL Application Proxy. 

   ![Tabbladnaam en URL configureren](./media/application-proxy-teams/tab-name-url.png)

Zodra de tabblad van een lid van een team wordt toegevoegd, wordt deze weergegeven voor iedereen in het kanaal. Alle gebruikers die toegang tot de app hebben worden geopend voor één aanmelding met de referenties die ze voor Microsoft-Teams gebruiken. Gebruikers die u geen toegang tot de app hebt het tabblad in de Teams kunnen zien, maar worden geblokkeerd totdat u ze machtigingen voor de lokale-app en de Azure portal gepubliceerde versie van de app geven. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [lokale SharePoint-sites publiceren](application-proxy-enable-remote-access-sharepoint.md) met toepassingsproxy.
- Configureren van uw apps te gebruiken [aangepaste domeinen](active-directory-application-proxy-custom-domains.md) voor de externe URL. 
