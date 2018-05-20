---
title: Uitnodiging inwisseling in B2B-samenwerking - Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven in de Azure AD B2B-samenwerking uitnodiging inwisseling ervaring voor eindgebruikers, inclusief de overeenkomst privacy voorwaarden.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0a0c900bbfbb2778d8fabcbb71e339fd3dabcf47
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B-samenwerking uitnodiging inwisseling

Als u wilt samenwerken met partners via Azure Active Directory (Azure AD) B2B-samenwerking gebruikers, kunt u gastgebruikers toegang tot gedeelde apps uitnodigen. Nadat een gastgebruiker is toegevoegd aan de directory via de gebruikersinterface of de gebruiker wordt verzocht via PowerShell, gastgebruikers moeten gaan via een eerst toestemming proces waar zij akkoord met [privacy voorwaarden](#privacy-policy-agreement). Dit proces wordt uitgevoerd op een van de volgende manieren:

- De afzender van de Gast uitnodiging verzendt een directe koppeling naar een gedeelde-app. De helper klikt op de koppeling aan te melden, accepteert de voorwaarden van de privacy en naadloos toegang heeft tot de gedeelde bron. (De gastgebruiker ontvangt nog steeds een uitnodiging voor een e-mailbericht met een URL inwisseling, maar dan enkele bijzondere gevallen, dit is niet langer vereist voor de uitnodiging voor een e-mailadres gebruiken.)  
- De gastgebruiker een uitnodiging voor een e-mailbericht ontvangt, en klikt op de inwisselcode-URL. Als onderdeel van de eerste keer aanmelden, wordt ze gevraagd om de privacy-voorwaarden te accepteren.

## <a name="redemption-through-a-direct-link"></a>Inwisseling via een directe koppeling

Een uitnodiging Gast antwoorden kunt gastgebruiker uitnodigen door het verzenden van een directe koppeling naar een gedeelde-app. De ervaring inwisseling is voor de gastgebruiker net zo eenvoudig als het aanmelden bij de app die met hen is gedeeld. Ze kunnen Klik op een koppeling naar de app controleren en akkoord gaat met de privacy en vervolgens naadloos toegang krijgen tot de app. In de meeste gevallen moeten gastgebruikers niet meer op de URL van de inwisselcode in een uitnodiging voor een e-mailbericht.

Als u bent uitgenodigd gastgebruikers via de gebruikersinterface of gekozen de uitnodiging om e-mail te verzenden als onderdeel van de ervaring van de uitnodiging PowerShell, wordt in de uitgenodigde gebruiker nog steeds een uitnodiging voor een e-mailbericht ontvangt. Dit e-mailadres is nuttig voor de volgende speciale gevallen:

- De gebruiker beschikt niet over een Azure AD-account of een Microsoft-account (MSA). In dit geval moet de gebruiker een MSA maken voordat ze op de koppeling klikken, of ze de inwisselcode-URL in de uitnodiging via e-mail gebruiken kunnen. Het proces inwisselcode wordt automatisch de gebruiker voor het maken van een beheerde Serviceaccounts.
- Soms de uitgenodigde gebruikersobject mogelijk geen een e-mailadres vanwege een conflict met een contact (bijvoorbeeld een Outlook contact op met object)-object. In dit geval de gebruiker klikt op de URL van de inwisselcode in de e-mail uitnodiging.
- De gebruiker kan zich aanmelden met een alias van de e-mailadres dat is uitgenodigd. (Een alias is een extra e-mailadres dat is gekoppeld aan een e-mailaccount.) In dit geval de gebruiker klikt op de URL van de inwisselcode in de e-mail uitnodiging.

Als deze speciale gevallen belangrijk voor uw organisatie zijn, raden wij u gebruikers uitnodigen met behulp van methoden die nog steeds de uitnodiging via e-mail te verzenden. Ook als een gebruiker niet onder een van deze bijzondere gevallen valt, ze kunnen nog steeds klikt u op de URL in een uitnodiging voor een e-mailbericht om toegang te krijgen.

## <a name="redemption-through-the-invitation-email"></a>Inwisseling via de uitnodiging via e-mail

Als uitgenodigd via een methode die een uitnodiging voor een e-mailbericht verzendt, kunnen gebruikers ook een uitnodiging via de uitnodigingsmail inwisselen. Een gebruiker uitgenodigde kan klikt u op de URL van de inwisselcode in de e-mail en vervolgens te controleren en akkoord gaat met de privacy. Het proces wordt in meer detail hier beschreven:

1.  Nadat u bent uitgenodigd, ontvangt de genodigden een uitnodiging via e-mailbericht dat wordt verzonden vanuit **Microsoft Invitations**.
2.  Hiermee selecteert u de helper **aan de slag** in het e-mailbericht.
3.  Als de helper is geen Azure AD-account of een MSA, wordt ze gevraagd voor het maken van een beheerd Serviceaccount.
4.  De helper wordt omgeleid naar de **Controleer machtigingen** scherm, waar ze kunnen Lees de privacyverklaring van de uitnodiging organisatie en de voorwaarden accepteren.

## <a name="privacy-policy-agreement"></a>Privacybeleid

Nadat een gastgebruiker zich aanmeldt bij de toegang tot bronnen in een partnerorganisatie voor de eerste keer, zien ze een **Controleer machtigingen** scherm. Hier kunnen ze de privacyverklaring van de uitnodiging organisatie bekijken. Een gebruiker moet het gebruik van hun gegevens accepteren overeenkomstig het privacybeleid van de organisatie van de uitnodigen om door te gaan.

![Schermopname van gebruikersinstellingen in het deelvenster toegang](media/redemption-experience/ConsentScreen.png) 

Zie voor meer informatie over hoe u als tenantbeheerder aan de privacyverklaring van uw organisatie koppelen kunt [instructies: het toevoegen van privacy-informatie van uw organisatie in Azure Active Directory](https://aka.ms/adprivacystatement).

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Azure Active Directory B2B-samenwerking gebruikers toevoegen in de Azure portal](add-users-administrator.md)
- [Hoe voeg informatiemedewerkers B2B-samenwerking gebruikers toe aan Azure Active Directory](add-users-information-worker.md)
- [Azure Active Directory B2B-samenwerking gebruikers toevoegen met behulp van PowerShell](customize-invitation-api.md#powershell)
- [Een organisatie als gastgebruiker verlaten](leave-the-organization.md)