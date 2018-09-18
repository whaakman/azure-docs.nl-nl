---
title: 'Uitnodiging inwisselen in B2B-samenwerking: Azure Active Directory | Microsoft Docs'
description: Hierin wordt beschreven in de Azure AD B2B-samenwerking uitnodiging inwisselen ervaring voor eindgebruikers, met inbegrip van de overeenkomst met privacyvoorwaarden.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 7819ed3e18092e8b7bdf52225e7025b4b6d8146a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981878"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B-samenwerking uitnodiging inwisselen

Als u wilt samenwerken met gebruikers van partnerorganisaties via Azure Active Directory (Azure AD) B2B-samenwerking, kunt u gastgebruikers ook kunnen voor toegang tot gedeelde apps uitnodigen. Nadat een gastgebruiker is toegevoegd aan de map via de gebruikersinterface of de gebruiker is uitgenodigd via PowerShell, gastgebruikers moeten gaan via een waar zij akkoord met toestemming van de eerste keer [privacyvoorwaarden](#privacy-policy-agreement). Dit proces wordt uitgevoerd op een van de volgende manieren:

- De gastuitnodiging verzendt een directe koppeling naar een gedeelde app. De genodigde klikt op de koppeling aan te melden, accepteert de voorwaarden van de privacyverklaring en naadloos toegang heeft tot de gedeelde bron. (De gastgebruiker ontvangt nog steeds een uitnodiging via e-mail met een URL voor inschrijving, maar dan enkele bijzondere gevallen, het heeft niet langer vereist voor de uitnodiging per e-mail gebruiken.)  
- De gastgebruiker ontvangt een uitnodiging via e-mail en klikt op de URL voor inwisselen. Als onderdeel van de eerste keer aanmelden, wordt ze gevraagd om de privacyvoorwaarden te accepteren.

## <a name="redemption-through-a-direct-link"></a>Inschrijving via een directe koppeling

Een gastuitnodiging kan uitnodigingen versturen van een gastgebruiker door het verzenden van een directe koppeling naar een gedeelde app. De code-ervaring is voor de gastgebruiker net zo gemakkelijk als aanmelden bij de app die met hen is gedeeld. Ze kunnen klikt u op een koppeling naar de app, controleren en de privacyvoorwaarden accepteren en vervolgens naadloos toegang krijgen tot de app. In de meeste gevallen moeten de gastgebruikers ook kunnen niet meer op de URL voor inschrijving in een uitnodiging via e-mail.

Als u uitgenodigd gastgebruikers ook kunnen via de gebruikersinterface of u hebt gekozen voor het verzenden van de uitnodiging per e-mail als onderdeel van de PowerShell-ervaring voor inschrijving, ontvangt de uitgenodigde gebruiker nog steeds een uitnodiging via e-mail. Dit e-mailbericht is handig voor de volgende speciale gevallen:

- De gebruiker beschikt niet over een Azure AD-account of een Microsoft-account (MSA). In dit geval moet de gebruiker een MSA maken voordat ze op de koppeling klikken, of ze de inschrijving-URL in de uitnodiging per e-mail gebruiken kunnen. Het proces inwisselen is automatisch gevraagd of de gebruiker een MSA maken.
- Het object van de uitgenodigde gebruiker kan soms niet een e-mailadres hebben vanwege een conflict met een verbindingsobject (bijvoorbeeld een Outlook contact-object). In dit geval de gebruiker klikt op de URL voor inschrijving in de uitnodiging per e-mail.
- De gebruiker kan zich aanmelden met een alias van het e-mailadres dat is uitgenodigd. (Een alias is een aanvullende e-mailadres dat is gekoppeld aan een e-mailaccount.) In dit geval de gebruiker klikt op de URL voor inschrijving in de uitnodiging per e-mail.

Als deze speciale gevallen belangrijk voor uw organisatie zijn, raden wij u gebruikers uitnodigen met behulp van methoden die nog steeds de uitnodiging per e-mail verzenden. Ook als een gebruiker niet onder een van deze speciale gevallen valt, ze kunnen nog steeds klikt u op de URL in een uitnodiging via e-mail om toegang te krijgen.

## <a name="redemption-through-the-invitation-email"></a>Inschrijving via de uitnodiging per e-mail

Als u uitgenodigd via een methode die een uitnodiging via e-mail verzendt, kunnen gebruikers ook een uitnodiging via e-mail met de uitnodiging inwisselen. Een van de uitgenodigde gebruiker kunt klikt u op de URL van de inschrijving in het e-mailbericht, en vervolgens bekijken en de privacyvoorwaarden accepteren. Het proces wordt in meer detail hier beschreven:

1.  Nadat u wordt uitgenodigd, ontvangt de genodigde een uitnodiging via e-mail wordt verzonden uit **Microsoft Invitations**.
2.  De genodigde selecteert **aan de slag** in het e-mailbericht.
3.  Als de genodigde niet beschikt over een Azure AD-account of een MSA, bent u wordt gevraagd om te maken van een beheerde Serviceaccounts.
4.  De genodigde wordt omgeleid naar de **machtigingen controleren** scherm, waar ze de privacyverklaring van de uitnodigende organisatie en de voorwaarden accepteren.

## <a name="privacy-policy-agreement"></a>Privacybeleid

Nadat een gastgebruiker zich heeft aangemeld voor toegang tot resources in een partnerorganisatie voor de eerste keer, zien ze een **machtigingen controleren** scherm. Hier kunnen ze de privacyverklaring van de uitnodigende organisatie bekijken. Een gebruiker moet het gebruik van hun eigen informatie accepteren in overeenstemming met het privacybeleid van de uitnodigende organisatie om door te gaan.

![Schermopname van gebruikersinstellingen in Toegangsvenster](media/redemption-experience/ConsentScreen.png) 

Zie voor meer informatie over hoe u als tenantbeheerder aan de privacyverklaring van uw organisatie koppelen kunt [instructies: het toevoegen van privacy-informatie van uw organisatie in Azure Active Directory](https://aka.ms/adprivacystatement).

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Gebruikers van Azure Active Directory B2B-samenwerking toevoegen in Azure portal](add-users-administrator.md)
- [Hoe voeg informatiemedewerkers gebruikers van B2B-samenwerking toe aan Azure Active Directory?](add-users-information-worker.md)
- [Gebruikers van Azure Active Directory B2B-samenwerking toevoegen met behulp van PowerShell](customize-invitation-api.md#powershell)
- [Een organisatie als gastgebruiker verlaten](leave-the-organization.md)