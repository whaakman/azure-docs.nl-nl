---
title: Apps publiceren met een Azure AD-toepassingsproxy | Microsoft Docs
description: Publiceer on-premises toepassingen naar de cloud met Azure AD-toepassingsproxy in de Azure portal.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 43cabb03a698dd87f12fef8e9a4dd54ee42c3ec9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Toepassingen publiceren met Azure AD-toepassingsproxy

Toepassingsproxy van Azure Active Directory (AD) kunt u ondersteuning voor externe werknemers door het publiceren van on-premises toepassingen via internet worden geopend. U kunt deze toepassingen via de Azure-portal voor beveiligde externe toegang van buiten uw netwerk publiceren.

Dit artikel begeleidt u bij de stappen voor het publiceren van een lokale app met toepassingsproxy. Nadat u dit artikel hebt voltooid, ziet u uw gebruikers die toegang tot uw app op afstand mogelijk. En u bent klaar om extra functies voor de toepassing zoals het eenmalige aanmelding, persoonlijke informatie en vereisten voor beveiliging te configureren.

Als u geen ervaring met toepassingsproxy, meer informatie over deze functie in het artikel [het verstrekken van veilige externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>Publiceren van een lokale app voor externe toegang

Volg deze stappen voor het publiceren van uw apps met Application Proxy. Als u dit nog niet hebt al gedownload en een connector voor uw organisatie hebt geconfigureerd, gaat u naar [aan de slag met Application Proxy en installeer de connector](active-directory-application-proxy-enable.md) eerste, en vervolgens uw app te publiceren.

> [!TIP]
> Als u voor de eerste keer uit toepassingsproxy test, kiest u een toepassing die ingesteld voor verificatie op basis van wachtwoorden. Toepassingsproxy biedt ondersteuning voor andere soorten authenticatie, maar apps op basis van wachtwoorden zijn het eenvoudigst te laten snel gebruiksklaar. 

1. Meld u aan als een beheerder in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen** > **nieuwe toepassing**.

  ![Een zakelijke toepassing toevoegen](./media/application-proxy-publish-azure-portal/add-app.png)

3. Selecteer **alle**, selecteer daarna **On-premises toepassing**.  

  ![Uw eigen toepassing toevoegen](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Geef de volgende informatie over de toepassing op:

   - **Naam**: de naam van de toepassing die wordt weergegeven in het deelvenster toegang en in de Azure-portal. 

   - **Interne URL**: de URL die u gebruikt voor toegang tot de toepassing uit binnen uw particuliere netwerk. U kunt voor het publiceren een specifiek pad opgeven op de back-endserver, terwijl de rest van de server ongepubliceerd blijft. U kunt op deze manier kunnen verschillende sites op dezelfde server als verschillende apps publiceren en elk een eigen naam en toegangsregels geven.

     > [!TIP]
     > Als u een pad publiceert, moet u ervoor zorgen dat dit alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing bevat. Als uw app zich bijvoorbeeld bevindt op het pad https://uwapp/app en gebruikmaakt van installatiekopieën op https://uwapp/media, moet u https://uwapp/ publiceren als het pad. Deze interne URL hoeft niet te worden van de startpagina van die uw gebruikers te zien. Zie voor meer informatie [instellen van een aangepaste startpagina van gepubliceerde apps](application-proxy-office365-app-launcher.md).

   - **Externe URL**: het adres uw gebruikers gaat naar om toegang tot de app van buiten uw netwerk. Als u niet wilt gebruiken van het standaarddomein toepassingsproxy, leest u over [aangepaste domeinen in Azure AD-toepassingsproxy](active-directory-application-proxy-custom-domains.md).
   - **Verificatie vooraf**: hoe Application Proxy gebruikers worden geverifieerd voordat ze toegang geven tot uw toepassing. 

     - Azure Active Directory: de gebruikers worden omgeleid met de toepassingsproxy zodat zij zich kunnen aanmelden met Azure AD. Hierbij worden hun machtigingen geverifieerd voor de directory en de toepassing. We raden deze optie als de standaardconfiguratie, zodat u van Azure AD-beveiligingsfuncties zoals voorwaardelijke toegang en multi-factor Authentication profiteren kunt.
     - Passthrough: Gebruikers hebben geen worden geverifieerd bij Azure Active Directory om toegang tot de toepassing. U kunt nog steeds verificatievereisten op de back-end instellen.
   - **Connector-groep**: Connectors verwerken van de externe toegang tot uw toepassing en connector groepeert u indelen connectors en apps per regio, netwerk of doel help. Als u een connector groepen die zijn gemaakt nog niet hebt, uw app is toegewezen aan **standaard**.

   ![Uw toepassing configureren](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Indien nodig, extra instellingen configureren. Voor de meeste toepassingen, moet u deze instellingen behouden de standaardwaarden hebben. 
   - **Back-end toepassing time-out**: deze waarde instelt op **lang** alleen als uw toepassing traag is om te verifiëren en verbinding maken. 
   - **URL's in de Headers vertalen**: deze waarde als behouden **Ja** tenzij uw toepassing de oorspronkelijke host-header in de verificatieaanvraag vereist.
   - **URL's in de hoofdtekst van de toepassing te vertalen**: deze waarde als behouden **Nee** tenzij u hardcoded HTML-koppelingen naar andere on-premises toepassingen hebben en geen aangepaste domeinen gebruiken. Zie voor meer informatie [koppelen vertaling met toepassingsproxy](application-proxy-link-translation.md).
   
   ![Uw toepassing configureren](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Selecteer **Toevoegen**.


## <a name="add-a-test-user"></a>Een testgebruiker toevoegen 

Als u wilt testen dat uw app correct is gepubliceerd, moet u een test-gebruikersaccount toevoegen. Controleer of dit account heeft al machtigingen voor toegang tot de app uit binnen het bedrijfsnetwerk.

1. Selecteer terug op de blade snel starten **een gebruiker toewijzen voor het testen van**.

  ![Een gebruiker toewijzen voor het testen](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Selecteer op de gebruikers en groepen blade **toevoegen**.

  ![Een gebruiker of groep toevoegen](./media/application-proxy-publish-azure-portal/add-user.png)

3. Selecteer op het tabblad van de toewijzing toevoegen **gebruikers en groepen** kies vervolgens het account dat u wilt toevoegen. 
4. Selecteer **Toewijzen**.

## <a name="test-your-published-app"></a>Uw gepubliceerde app testen

Navigeer naar de externe URL die u hebt geconfigureerd tijdens de stap voor publiceren in uw browser. U moet het startscherm Zie en kunnen aanmelden met de testaccount die u instelt.

![Uw gepubliceerde app testen](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Volgende stappen
- [Downloaden van connectors](active-directory-application-proxy-enable.md) en [connector groepen maken](active-directory-application-proxy-connectors-azure-portal.md) voor het publiceren van toepassingen op afzonderlijke netwerken en locaties.

- [Instellen van eenmalige aanmelding](application-proxy-sso-azure-portal.md) voor uw zojuist gepubliceerde app
