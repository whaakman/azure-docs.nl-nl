---
title: Er worden geen gebruikers ingericht voor een Azure AD-galerie toepassing | Microsoft Docs
description: Veelvoorkomende problemen oplossen die zich voordoen wanneer u geen gebruikers ziet die worden weer gegeven in een Azure AD Gallery-toepassing die u hebt geconfigureerd voor het inrichten van gebruikers met Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: b80539181e6614283b6170b9cd9d4db85f812a5f
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879898"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Er worden geen gebruikers ingericht voor een Azure AD-galerie toepassing
Nadat automatische inrichting is geconfigureerd voor een toepassing (waaronder controleren of de referenties van de app die zijn opgegeven voor Azure AD om verbinding te maken met de app geldig zijn), worden gebruikers en/of groepen ingericht voor de app. Inrichting wordt bepaald door de volgende zaken:

-   Welke gebruikers en groepen zijn **toegewezen** aan de toepassing. Houd er rekening mee dat het inrichten van geneste groepen of Office 365-groepen niet wordt ondersteund. Zie [een gebruiker of groep toewijzen aan een bedrijfs-app in azure Active Directory](assign-user-or-group-access-portal.md)voor meer informatie over de toewijzing.
-   Of **kenmerk toewijzingen** zijn ingeschakeld, en geconfigureerd om geldige kenmerken van Azure ad te synchroniseren met de app. Zie voor meer informatie over kenmerk toewijzingen [aanpassen van kenmerk toewijzingen van gebruikers inrichting voor SaaS-toepassingen in azure Active Directory](customize-application-attributes.md).
-   Hiermee wordt aangegeven of er een **bereik filter** aanwezig is waarmee gebruikers worden gefilterd op basis van specifieke kenmerk waarden. Zie voor meer informatie over het bereik van filters [kenmerk toepassing inrichten met bereik filters](define-conditional-rules-for-provisioning-user-accounts.md).
  
Als u ziet dat gebruikers niet worden ingericht, raadpleegt u de audit Logboeken in azure AD. Zoeken naar logboek vermeldingen voor een specifieke gebruiker.

De inrichtings controle logboeken kunnen worden geopend in de Azure Portal, op het tabblad  **&gt; Azure Active Directory toepassings\] naam &gt; &gt; van de bedrijfs-apps \[** . Filter de logboeken op de **account** inrichtings categorie om alleen de inrichtings gebeurtenissen voor die app te bekijken. U kunt zoeken naar gebruikers op basis van de ' overeenkomende ID ' die voor hen is geconfigureerd in de kenmerk toewijzingen. Als u bijvoorbeeld het ' User Principal Name ' of ' e-mail adres ' hebt geconfigureerd als het overeenkomende kenmerk op de Azure AD-kant en de gebruiker die niet wordt ingericht, de waarde 'audrey@contoso.com' heeft, zoekt u in de audit logboeken naar 'audrey@contoso.com' en bekijkt u de vermeldingen geretourneerd.

De inrichtings controle logboeken registreren alle bewerkingen die worden uitgevoerd door de inrichtings service, inclusief het opvragen van Azure AD voor toegewezen gebruikers die binnen het bereik van de inrichting vallen, het opvragen van de doel-app voor het bestaan van deze gebruikers, het vergelijken van de gebruikers objecten tussen het systeem. Vervolgens kunt u het gebruikers account in het doel systeem toevoegen, bijwerken of uitschakelen op basis van de vergelijking.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleem gebieden met het inrichten om te overwegen
Hieronder vindt u een lijst met algemene probleem gebieden die u kunt inzoomen als u een idee hebt van waar u moet beginnen.

- [De inrichtings service lijkt niet te worden gestart](#provisioning-service-does-not-appear-to-start)
- [Controle logboeken: gebruikers worden overgeslagen en niet ingericht, zelfs als ze zijn toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>De inrichtings service lijkt niet te worden gestart
Als u instelt dat de inrichtings **status** moet worden **ingeschakeld** in de sectie  **&gt; Azure Active Directory bedrijfs\] apps &gt; \[toepassings naam &gt;inrichten** van de Azure Portal . Er worden echter geen andere status gegevens op die pagina weer gegeven na de volgende keer opnieuw laden. de service wordt waarschijnlijk uitgevoerd, maar is nog niet klaar met het volt ooien van de initiële synchronisatie. Controleer de hierboven beschreven **controle logboeken** om te bepalen welke bewerkingen de service uitvoert en of er fouten zijn.

>[!NOTE]
>Een initiële synchronisatie kan 20 minuten tot enkele uren duren, afhankelijk van de grootte van de Azure AD-adres lijst en het aantal gebruikers in het bereik dat moet worden ingericht. Volgende synchronisaties na de initiële synchronisatie zijn sneller, omdat er door de inrichtings service water merken worden opgeslagen die de status van beide systemen na de initiële synchronisatie vertegenwoordigen. De initiële synchronisatie verbetert de prestaties van volgende synchronisaties.
>


## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Controle logboeken zeggen dat gebruikers worden overgeslagen en niet ingericht, zelfs als ze worden toegewezen

Wanneer een gebruiker wordt weer gegeven als ' overgeslagen ' in de audit logboeken, is het belang rijk om de uitgebreide details in het logboek bericht te lezen om de reden te bepalen. Hieronder vindt u enkele veelvoorkomende redenen en oplossingen:

- **Er is een bereik filter geconfigureerd** **waarmee de gebruiker wordt gefilterd op basis van een kenmerk waarde**. Zie [bereik filters](define-conditional-rules-for-provisioning-user-accounts.md)voor meer informatie over het bereik van filters.
- **De gebruiker is niet effectief.** Als u dit specifieke fout bericht ziet, is er een probleem met de gebruikers toewijzings record die is opgeslagen in azure AD. U kunt dit probleem oplossen door de toewijzing van de gebruiker (of groep) uit de app ongedaan te verwijderen en opnieuw toe te wijzen. Zie [toegang toewijzen aan gebruikers of groepen](assign-user-or-group-access-portal.md)voor meer informatie over de toewijzing.
- **Een vereist kenmerk ontbreekt of is niet ingevuld voor een gebruiker.** Het is belang rijk om rekening mee te houden bij het instellen van inrichting door de kenmerk toewijzingen en werk stromen te controleren en te configureren die bepalen welke gebruikers-(of groeps) eigenschappen stromen van Azure AD naar de toepassing. Deze configuratie omvat het instellen van de ' overeenkomende eigenschap ' die wordt gebruikt om gebruikers/groepen tussen de twee systemen uniek te identificeren en te vergelijken. Zie voor meer informatie over dit belang rijke proces de [toewijzing van kenmerk toewijzingen van gebruikers aanpassen voor SaaS-toepassingen in azure Active Directory](customize-application-attributes.md).
- **Kenmerk toewijzingen voor groepen:** Het inrichten van de groeps naam en groeps gegevens, naast de leden, als deze worden ondersteund voor sommige toepassingen. U kunt deze functie in-of uitschakelen door de **toewijzing** voor groeps objecten die op het tabblad **inrichten** wordt weer gegeven in of uit te scha kelen. Als inrichtings groepen is ingeschakeld, moet u de kenmerk toewijzingen controleren om ervoor te zorgen dat er een geschikt veld wordt gebruikt voor de ' overeenkomende ID '. De overeenkomende ID kan de weergave naam of e-mail alias zijn. De groep en de bijbehorende leden worden niet ingericht als de overeenkomende eigenschap leeg is of niet is ingevuld voor een groep in azure AD.

## <a name="next-steps"></a>Volgende stappen

[Azure AD Connect-synchronisatie: Inzicht in declaratieve inrichting](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
