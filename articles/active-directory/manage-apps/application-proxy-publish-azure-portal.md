---
title: Apps publiceren met een Azure AD-toepassingsproxy | Microsoft Docs
description: Publiceer on-premises toepassingen naar de cloud met Azure AD-toepassingsproxy in de Azure-portal.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1224642bb7e6fc0c51b3f839a78449132db5b4bb
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364254"
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Toepassingen publiceren met Azure AD-toepassingsproxy

Azure Active Directory (AD)-toepassingsproxy helpt u externe werknemers te ondersteunen door het publiceren van on-premises toepassingen kunnen worden benaderd via internet. U kunt deze toepassingen via de Azure-portal voor beveiligde externe toegang van buiten uw netwerk.

In dit artikel leidt u door de stappen voor het publiceren van een on-premises app met Application Proxy. Nadat u dit artikel hebt voltooid, ziet u uw gebruikers die toegang tot uw app op afstand mogelijk. En u bent er klaar voor om extra functies voor de toepassing, zoals eenmalige aanmelding, persoonlijke informatie en vereisten voor beveiliging te configureren.

Als u geen ervaring met Application Proxy, meer informatie over deze functie in het artikel [het bieden van veilige externe toegang tot on-premises toepassingen](application-proxy.md).

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd en geregistreerd, een connector. Als u nog steeds moet deze stappen uitvoeren, Zie [aan de slag met Application Proxy en de Connector installeert](application-proxy-enable.md).

## <a name="publish-an-on-premises-app-for-remote-access"></a>Publiceren van een on-premises app voor externe toegang

Volg deze stappen voor het publiceren van uw apps met Application Proxy. Als u dit nog niet hebt al gedownload en een connector hebt geconfigureerd voor uw organisatie, gaat u naar [aan de slag met Application Proxy en de connector installeert](application-proxy-enable.md) eerst, en vervolgens uw app publiceren.

> [!TIP]
> Als u uit Application Proxy voor de eerste keer test, kiest u een toepassing die ingesteld voor verificatie op basis van wachtwoorden. Application Proxy biedt ondersteuning voor andere typen verificatie, maar apps op basis van wachtwoorden zijn het eenvoudigst aan de slag kunt en snel. 

1. Meld u aan als beheerder aan in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen** > **nieuwe toepassing**.

  ![Een zakelijke toepassing toevoegen](./media/application-proxy-publish-azure-portal/add-app.png)

3. Selecteer **alle**en selecteer vervolgens **On-premises toepassing**.  

  ![Uw eigen toepassing toevoegen](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Geef de volgende informatie over de toepassing op:

   - **Naam**: de naam van de toepassing die wordt weergegeven in het toegangsvenster en in de Azure-portal. 

   - **Interne URL**: de URL die u gebruiken voor toegang tot de toepassing uit binnen uw particuliere netwerk. U kunt voor het publiceren een specifiek pad opgeven op de back-endserver, terwijl de rest van de server ongepubliceerd blijft. Op deze manier kunt u verschillende sites op dezelfde server als de verschillende apps publiceren en elk een eigen naam en toegangsregels geven.

     > [!TIP]
     > Als u een pad publiceert, moet u ervoor zorgen dat dit alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing bevat. Bijvoorbeeld, als uw app zich bevindt https://yourapp/app en gebruikmaakt van installatiekopieën te vinden op https://yourapp/media, en vervolgens moet u publiceren https://yourapp/ als het pad. Deze interne URL hoeft te zijn van de landingspagina van die uw gebruikers te zien. Zie voor meer informatie, [instellen van een aangepaste startpagina voor gepubliceerde apps](application-proxy-configure-custom-home-page.md).

   - **Externe URL**: het adres uw gebruikers wordt omgeleid naar voor toegang tot de app van buiten uw netwerk. Als u niet wilt gebruiken van het standaarddomein voor de Application Proxy, meer informatie over [aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md).
   - **Verificatie vooraf**: hoe Application Proxy gebruikers worden geverifieerd voordat ze toegang krijgen tot uw toepassing. 

     - Azure Active Directory: de gebruikers worden omgeleid met de toepassingsproxy zodat zij zich kunnen aanmelden met Azure AD. Hierbij worden hun machtigingen geverifieerd voor de directory en de toepassing. We raden deze optie als de standaard, zodat u van Azure AD-beveiligingsfuncties, zoals voorwaardelijke toegang en multi-factor Authentication profiteren kunt.
     - Passthrough: Gebruikers hoeven te worden geverifieerd bij Azure Active Directory voor toegang tot de toepassing. U kunt nog steeds verificatievereisten op de back-end instellen.
   - **Connectorgroep**: Connectors verwerken van de externe toegang tot uw toepassing en connector groepen help u indelen connectors en per regio, netwerk of doel-apps. Als u geen connectorgroepen nog gemaakt hebt, uw app is toegewezen aan **standaard**.

>[!NOTE]
>Als uw toepassing gebruikmaakt van websockets om verbinding te maken, gebruikt of dat u connectorversies 1.5.612.0 hebt of hoger met websocket-ondersteuning en de toegewezen Connectorgroep alleen deze connectors.

   ![Uw toepassing configureren](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Indien nodig, kunt u aanvullende instellingen configureren. Voor de meeste toepassingen, moet u deze instellingen behouden de standaardwaarden hebben. 
   - **Back-end toepassing time-out**: deze waarde instelt op **lang** alleen als uw toepassing langzaam om te verifiëren en verbinding maken. 
   - **URL's vertalen in Headers**: deze waarde als houden **Ja** , tenzij uw toepassing de oorspronkelijke host-header in de verificatieaanvraag vereist.
   - **URL's vertalen in de hoofdtekst van de toepassing**: deze waarde als houden **Nee** , tenzij u vastgelegd HTML-koppelingen naar andere on-premises toepassingen hebt en aangepaste domeinen niet gebruiken. Zie voor meer informatie, [vertaling met Application Proxy koppelen](application-proxy-configure-hard-coded-link-translation.md).
   
   ![Uw toepassing configureren](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Selecteer **Toevoegen**.


## <a name="add-a-test-user"></a>Een testgebruiker toevoegen 

Als u wilt testen dat uw app correct is gepubliceerd, moet u een test-gebruikersaccount toevoegen. Controleer of dit account heeft al machtigingen voor toegang tot de app uit in het bedrijfsnetwerk bevinden.

1. Selecteer terug op de blade snel starten **een gebruiker toewijzen voor het testen van**.

  ![Een gebruiker toewijzen voor het testen](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Selecteer op de gebruikers en groepen blade **toevoegen**.

  ![Een gebruiker of groep toevoegen](./media/application-proxy-publish-azure-portal/add-user.png)

3. Selecteer op het tabblad van de toewijzing toevoegen **gebruikers en groepen** vervolgens kiezen welk account u wilt toevoegen. 
4. Selecteer **Toewijzen**.

## <a name="test-your-published-app"></a>De gepubliceerde app testen

Navigeer in uw browser naar de externe URL die u hebt geconfigureerd tijdens de stap publiceren. U moet het startscherm zien en kunnen zich aanmelden met het instellen van test.

![De gepubliceerde app testen](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Volgende stappen
- [Connectors downloaden](application-proxy-enable.md) en [connectorgroepen maken](application-proxy-connector-groups.md) voor het publiceren van toepassingen op afzonderlijke netwerken en locaties.

- [Instellen van eenmalige aanmelding](application-proxy-configure-single-sign-on-password-vaulting.md) voor uw zojuist gepubliceerde app
