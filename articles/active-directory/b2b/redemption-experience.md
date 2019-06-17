---
title: 'Uitnodiging inwisselen in B2B-samenwerking: Azure Active Directory | Microsoft Docs'
description: Hierin wordt beschreven in de Azure AD B2B-samenwerking uitnodiging inwisselen ervaring voor eindgebruikers, met inbegrip van de overeenkomst met privacyvoorwaarden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: a80eaa134130195fce00ee6a4d68851e478c4532
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052472"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B-samenwerking uitnodiging inwisselen

Dit artikel beschrijft de manieren gastgebruikers toegang krijgen tot uw resources en de toestemming proces die ze tegenkomt. Als u een uitnodiging via e-mail naar de Gast verzenden, de uitnodiging bevat een koppeling die kan worden ingewisseld door de Gast om toegang te krijgen uw app of -portal. De uitnodiging per e-mail is slechts een van de manieren gasten toegang tot uw resources krijgen kunnen. Als alternatief kunt u gasten toevoegen aan uw directory en geef ze een directe koppeling naar de portal of de app die u wilt delen. Ongeacht welke methode die ze gebruiken, worden de gasten door een eerst toestemming geleid. Dit proces zorgt ervoor dat uw gasten privacyvoorwaarden accepteren en alle accepteren [gebruiksvoorwaarden](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) u hebt ingesteld.

Wanneer u een gastgebruiker aan uw directory toevoegen, het gastgebruikersaccount toestemming status heeft (zichtbaar zijn in PowerShell) die in eerste instantie ingesteld op **PendingAcceptance**. Deze instelling blijft totdat de Gast de uitnodiging accepteert en gaat akkoord met het privacybeleid en de gebruiksvoorwaarden. Hierna, verandert de status toestemming in **geaccepteerde**, en de toestemming van pagina's worden niet meer weergegeven op de Gast.

## <a name="redemption-through-the-invitation-email"></a>Inschrijving via de uitnodiging per e-mail

Wanneer u een gastgebruiker toevoegen aan uw directory door [met behulp van de Azure-portal](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal), een uitnodiging via e-mail is verzonden naar de gast op in het proces. U kunt er ook voor kiezen om te uitnodigingen per e-mail verzenden wanneer u bent [met behulp van PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) gastgebruikers toevoegen aan uw directory. Hier volgt een beschrijving van de ervaring van de Gast wanneer ze de link in het e-mailbericht inwisselen.

1. De Gast ontvangt een [uitnodigingse-mail](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) die wordt verzonden vanuit **Microsoft Invitations**.
2. De Gast selecteert **aan de slag** in het e-mailbericht.
3. Als de Gast geen een Azure AD-account, een Microsoft-Account (MSA) of een e-mailaccount in een organisatie voor federatieve, ze wordt gevraagd om u te maken van een beheerde Serviceaccounts (tenzij de [eenmalige wachtwoordcode](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) functie is ingeschakeld, waarvoor een MSA niet nodig ).
4. De Gast geleid door de [toestemming geven ervaring](#consent-experience-for-the-guest) die hieronder worden beschreven.

## <a name="redemption-through-a-direct-link"></a>Inschrijving via een directe koppeling

Als alternatief voor de uitnodiging per e-mail, kunt u geven een gast een directe koppeling naar uw app of -portal. U moet eerst de gastgebruiker toevoegen aan uw directory via de [Azure-portal](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) of [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell). Vervolgens kunt u een van de [aanpasbare manieren voor het implementeren van toepassingen voor gebruikers](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences), met inbegrip van directe aanmelding koppelingen. Wanneer een gast gebruikmaakt van een directe koppeling in plaats van de uitnodiging per e-mail, moeten ze nog steeds via de toestemming van de eerste keer worden geleid.

> [!IMPORTANT]
> De directe koppeling moet de tenant-specifieke. Met andere woorden, een tenant-ID moet bevatten of geverifieerd domein zodat de Gast kan worden geverifieerd in uw tenant, waar de gedeelde app zich bevindt. Een algemene URL, zoals https://myapps.microsoft.com voor een gast werkt niet omdat deze wordt u doorgestuurd naar de starttenant voor verificatie. Hier volgen enkele voorbeelden van directe koppelingen met tenant-context:
 > - Toegangsvenster Apps: https://myapps.microsoft.com/?tenantid=&lt; tenant-id&gt; 
 > - Toegangsvenster van Apps voor een geverifieerd domein: https://myapps.microsoft.com/&lt; geverifieerde domein&gt;
 > - Azure-portal: https://portal.azure.com/&lt; tenant-id&gt;
 > - Afzonderlijke Apps: Zie hoe u een [directe koppeling voor eenmalige aanmelding](../manage-apps/end-user-experiences.md#direct-sign-on-links)

Er zijn enkele gevallen waarbij de uitnodiging per e-mail geniet de voorkeur boven een directe koppeling. Als deze speciale gevallen belangrijk voor uw organisatie zijn, raden we u gebruikers uitnodigen met behulp van methoden die nog steeds de uitnodiging per e-mail verzenden:
 - De gebruiker beschikt niet over een Azure AD-account, een MSA of een e-mailaccount in een organisatie voor federatieve. Tenzij u de functie voor eenmalige wachtwoordcode gebruikt, moet de Gast de uitnodiging per e-mail om te worden geleid door de stappen voor het maken van een MSA inwisselen.
 - Het object van de uitgenodigde gebruiker kan soms niet een e-mailadres hebben vanwege een conflict met een verbindingsobject (bijvoorbeeld een Outlook contact-object). In dit geval de gebruiker klikt op de URL voor inschrijving in de uitnodiging per e-mail.
 - De gebruiker kan zich aanmelden met een alias van het e-mailadres dat is uitgenodigd. (Een alias is een aanvullende e-mailadres dat is gekoppeld aan een e-mailaccount.) In dit geval de gebruiker klikt op de URL voor inschrijving in de uitnodiging per e-mail.

## <a name="consent-experience-for-the-guest"></a>Toestemming ervaring voor de Gast

Wanneer een gast zich aanmeldt voor toegang tot resources in een partnerorganisatie voor de eerste keer, zijn ze door de volgende pagina's geleid. 

1. De beoordelingen van de Gast de **machtigingen controleren** pagina met een beschrijving van de privacyverklaring van de uitnodigende organisatie. Een gebruiker moet **accepteren** het gebruik van de informatie in overeenstemming met het privacybeleid van de uitnodigende organisatie om door te gaan.

   ![Schermopname van de pagina van de machtigingen controleren](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Zie voor meer informatie over hoe u als tenantbeheerder aan de privacyverklaring van uw organisatie koppelen kunt [procedures: Privacy-informatie van uw organisatie toevoegen in Azure Active Directory](https://aka.ms/adprivacystatement).

2. Als gebruiksvoorwaarden zijn geconfigureerd, de Gast wordt geopend en controleert u de gebruiksvoorwaarden en selecteert vervolgens **accepteren**. 

   ![Schermafbeelding van de nieuwe gebruiksvoorwaarden](media/redemption-experience/terms-of-use-accept.png) 

   > [!NOTE]
   > U kunt configureren dat Zie [gebruiksvoorwaarden](../governance/active-directory-tou.md) in **beheren** > **organisatie relaties** > **gebruiksvoorwaarden**.

3. Tenzij anders vermeld, wordt de Gast omgeleid naar het toegangsvenster van Apps, die hier worden de toepassingen die toegang heeft tot de Gast.

   ![Schermopname van het toegangsvenster van Apps](media/redemption-experience/myapps.png) 

In uw directory, de Gast van **uitnodiging geaccepteerd** waarde wordt gewijzigd in **Ja**. Als een MSA is gemaakt, van de Gast **bron** bevat **Microsoft-Account**. Zie voor meer informatie over de eigenschappen van gebruikersaccount Gast [eigenschappen van de gebruiker van een Azure AD B2B-samenwerking](user-properties.md). 

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Gebruikers van Azure Active Directory B2B-samenwerking toevoegen in Azure portal](add-users-administrator.md)
- [Hoe voeg informatiemedewerkers gebruikers van B2B-samenwerking toe aan Azure Active Directory?](add-users-information-worker.md)
- [Gebruikers van Azure Active Directory B2B-samenwerking toevoegen met behulp van PowerShell](customize-invitation-api.md#powershell)
- [Een organisatie als gastgebruiker verlaten](leave-the-organization.md)
