---
title: 'Azure Active Directory Domain Services: Instellingen voor meldingen | Microsoft Docs'
description: Instellingen voor meldingen voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: ergreenl
ms.openlocfilehash: c6d827629850de88940f41febeeca61b812d692e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958522"
---
# <a name="notification-settings-in-azure-ad-domain-services"></a>Instellingen voor meldingen in Azure AD Domain Services

Meldingen voor Azure AD Domain Services kunt u, zodra een statuswaarschuwing in een wordt gedetecteerd op uw beheerde domein worden bijgewerkt.  

Deze functie is alleen beschikbaar voor beheerde domeinen die zich niet op klassieke virtuele netwerken.


## <a name="how-to-check-your-azure-ad-domain-services-email-notification-settings"></a>Het controleren van de meldingsinstellingen van uw Azure AD Domain Services-e-mailadres

1. Navigeer naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal
2. Kies uw beheerde domein uit de tabel
3. Kies in de navigatiebalk links **instellingen voor meldingen**

Op de pagina geeft alle geadresseerden voor e-mailmeldingen voor Azure AD Domain Services.

## <a name="what-does-an-email-notification-look-like"></a>Wat een e-mailmelding eruit?

De volgende afbeelding is een voorbeeld van een e-mailmelding:

![Voorbeeld van de e-mailmelding](./media/active-directory-domain-services-alerts/email-alert.png)

Het e-mailbericht Hiermee geeft u het beheerde domein die de waarschuwing is aanwezig op, evenals waardoor de tijd van detectie en een koppeling naar de health-pagina van de Azure AD Domain Services in Azure portal.

> [!WARNING]
> Zorg ervoor dat altijd dat de e-mail afkomstig is van een geverifieerde afzender van Microsoft voordat u de koppelingen in uw e-mailberichten op. De e-mailberichten die altijd afkomstig zijn van het e-mailbericht azure-noreply@microsoft.com
>


## <a name="why-would-i-receive-email-notifications"></a>Waarom zou ik e-mailmeldingen ontvangen?

Azure AD Domain Services verzonden e-mailmeldingen voor belangrijke updates over uw domein.  Deze meldingen zijn alleen voor dringende zaken die heeft gevolgen voor uw service en moeten onmiddellijk worden opgelost. Elk e-mailmelding wordt geactiveerd door een waarschuwing op uw beheerde domein. Deze waarschuwingen worden ook weergegeven in de Azure-portal en kunnen worden weergegeven op de [health-pagina van Azure AD Domain Services](active-directory-ds-check-health.md).

Azure AD Domain Services verzendt geen e-mailberichten naar deze lijst voor de aankondiging, -updates of verkoop doeleinden.

## <a name="when-will-i-receive-email-notifications"></a>Wanneer worden e-mailmeldingen worden ontvangen?

Een melding wordt direct verzonden wanneer een [nieuwe waarschuwing](active-directory-ds-troubleshoot-alerts.md) is gevonden op uw beheerde domein. Als de waarschuwing niet opgelost is, wordt een e-mailmelding verzonden als een herinnering vier dagen.

## <a name="who-should-receive-the-email-notifications"></a>Wie de e-mailmeldingen moeten ontvangen?


 U wordt aangeraden de lijst met e-mailontvangers voor Azure AD Domain Services worden samengesteld met mensen die kunnen worden gebruikt voor het beheren en wijzigingen aanbrengen in het beheerde domein. Deze lijst met e-mailbericht moet worden beschouwd als 'eerste beantwoorders' voor een probleem gevonden. Hebt u meer dan vijf extra e-mails die u wilt toevoegen, wordt u aangeraden een distributielijst om toe te voegen aan de meldingenlijst met in plaats daarvan maken.

Er kunnen maximaal vijf extra e-mailberichten voor meldingen met betrekking tot Azure AD Domain Services toevoegen. Bovendien kunt u ook alle globale beheerders van uw directory hebt en elk lid van de groep 'AAD DC Administrators' Azure AD Domain Services-e-mailmeldingen ontvangen. Azure AD Domain Services worden alleen meldingen verzenden naar maximaal 100 e-mailadressen, met inbegrip van de lijst met globale beheerders en AAD DC Administrators.


## <a name="how-to-add-an-additional-email-recipient"></a>Een extra e-mailontvanger toevoegen

> [!WARNING]
> Bij het wijzigen van de instellingen voor meldingen, wijzigt u de instellingen voor meldingen voor de volledig beheerde domein, niet alleen voor uzelf.

1. Navigeer naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal.
2. Klik op uw beheerde domein.
3. Klik op de navigatiebalk links **meldingsinstellingen**.
4. Als u wilt een e-mailadres toevoegen, typt u in het e-mailadres in de tabel extra ontvangers.
5. Klik op 'Opslaan' op de navigatiebalk boven hand.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

#### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>Ik heb een e-mailmelding voor een waarschuwing ontvangen, maar er geen waarschuwing wanneer ik aangemeld bij de Azure-portal. Wat is er gebeurd?

Als een waarschuwing is opgelost, wordt de waarschuwing verdwijnt vanuit Azure portal. De meest waarschijnlijke reden hiervoor is dat iemand anders die e-mailmeldingen ontvangt de waarschuwing op uw beheerde domein is opgelost, of er automatisch opgelost door Azure AD Domain Services.


#### <a name="why-can-i-not-edit-the-notification-settings"></a>Waarom kan ik de meldingsinstellingen niet bewerken?

Als u geen toegang tot de instellingenpagina van de melding in Azure portal, hoeft u niet de machtigingen voor het bewerken van Azure AD Domain Services. U moet contact opnemen met uw globale beheerder om toegang te krijgen machtigingen voor het bewerken van Azure AD Domain Services-resources of worden verwijderd uit de lijst met geadresseerden.

#### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>Kan ik lijken geen te worden ontvangen van e-mailmeldingen zelfs als ik mijn e-mailadres hebt opgegeven. Hoe komt dat?

Controleer uw map spam of ongewenste e-mail in uw e-mailadres voor de melding en controleer of aan lijst met geaccepteerde de afzender (azure-noreply@microsoft.com).

## <a name="next-steps"></a>Volgende stappen
- [Los waarschuwingen op uw beheerde domein](active-directory-ds-troubleshoot-alerts.md)
- [Lees meer over Azure AD Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).
