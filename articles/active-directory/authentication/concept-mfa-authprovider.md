---
title: Wanneer en hoe u Azure multi-factor Authentication-Provider?
description: Wanneer moet u een verificatieprovider gebruiken met Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 4a6ce07bfe641d9efdbe0eac841bb4f27f468b34
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161461"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Wanneer u een Azure multi-factor Authentication-Provider

Verificatie in twee stappen is standaard beschikbaar voor globale beheerders van Azure Active Directory- en Office 365-gebruikers. Als u echter wilt profiteren van [geavanceerde functies](howto-mfa-mfasettings.md), moet u de volledige versie van Azure Multi-Factor Authentication (MFA) aanschaffen.

Een Azure Multi-Factor Authentication-provider wordt gebruikt om te kunnen profiteren van de functies van de volledige versie van Azure MFA. Het is bedoeld voor gebruikers die **geen licenties via Azure MFA, Azure AD Premium of Azure AD Premium of Azure MFA-bundels**. Azure MFA en Azure AD Premium bevatten standaard de volledige versie van Azure MFA.

Als u licenties die betrekking hebben op alle gebruikers in uw organisatie hebt, klikt u vervolgens hoeft niet u Azure multi-factor Authentication-Provider. Maak een Azure multi-factor Authentication-Provider alleen als u moet ook opgeven de verificatie in twee stappen voor gebruikers die geen licenties.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Waarschuwingen met betrekking tot de Azure MFA-SDK

Een Azure Multi-Factor Authentication-provider is vereist als u de SDK wilt downloaden. Houd er rekening mee de SDK is afgeschaft en wordt niet meer ondersteund voor nieuwe klanten en alleen blijven werken tot en met 14 November 2018. Na deze datum kan de SDK niet meer worden aangeroepen.

Voor het downloaden van de SDK een Azure multi-factor Authentication-Provider te maken, zelfs als u Azure MFA, AAD Premium of andere gebundelde licenties hebt. Als u een Azure Multi-Factor Authentication-provider voor dit doeleinde maakt en al licenties hebt, moet u ervoor zorgen dat u de provider maakt met het model **Per ingeschakelde gebruiker**. Vervolgens de Provider een koppeling naar de map waarin de Azure MFA, Azure AD Premium of andere gebundelde licenties. Deze configuratie zorgt ervoor dat u alleen een rekening ontvangt als u meer unieke gebruikers hebt die verificatie in twee stappen gebruiken dan het aantal licenties dat u in uw bezit hebt.

## <a name="what-is-an-mfa-provider"></a>Wat is een MFA-provider?

Als u geen licenties voor Azure multi-factor Authentication hebt, kunt u een verificatieprovider om te vereisen van verificatie in twee stappen voor uw gebruikers maken.

Er zijn twee soorten verificatieproviders en is het onderscheid te maken over hoe uw Azure-abonnement wordt in rekening gebracht. De optie Per verificatie berekent het aantal verificaties die in een maand op uw tenant worden uitgevoerd. Deze optie wordt aanbevolen als u een aantal gebruikers hebt die slechts sporadisch verifiëren. De optie Per gebruiker berekent het aantal personen in uw tenant die in een maand verificatie in twee stappen uitvoeren. Deze optie wordt aanbevolen als u een aantal gebruikers met licenties hebt, maar MFA wilt uitbreiden naar meer gebruikers buiten de restricties van uw licentie.

## <a name="create-an-mfa-provider"></a>Een MFA-provider maken

Voer de volgende stappen uit om een Azure Multi-Factor Authentication-provider te maken in Azure Portal:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **MFA-server** > **Providers**.

   ![Providers][Providers]

3. Selecteer **Toevoegen**.
4. Vul de volgende velden in en selecteer **Toevoegen**:
   - **Naam**: de naam van de provider.
   - **Gebruiksmodel**: kies een van de volgende twee opties:
      * Per verificatie – koopmodel waarin per verificatie wordt betaald. Wordt doorgaans gebruikt in scenario's die gebruikmaken van Azure Multi-Factor Authentication in een toepassing waarmee consumenten te maken krijgen.
      * Per ingeschakelde gebruiker – koopmodel waarin voor elke ingeschakelde gebruiker wordt betaald. Wordt doorgaans gebruikt om werknemers toegang te geven tot toepassingen zoals Office 365. Kies deze optie als sommige gebruikers al een licentie voor Azure MFA hebben.
   - **Abonnement**: het Azure-abonnement waaraan de kosten voor de activiteit van verificatie in twee stappen via de provider in rekening wordt gebracht.
   - **Adreslijst**: de Azure Active Directory-tenant waaraan de provider is gekoppeld.
      * U hebt geen Azure AD-adreslijst nodig om een provider te maken. Laat dit vak leeg als u alleen van plan bent de Azure Multi-Factor Authentication-server te downloaden.
      * De provider moet worden gekoppeld aan een Azure AD-adreslijst om te kunnen profiteren van de geavanceerde functies.
      * Er kan slechts één provider worden gekoppeld aan één Azure AD-adreslijst.

## <a name="manage-your-mfa-provider"></a>Uw MFA-provider beheren

U kunt het gebruiksmodel (per ingeschakelde gebruiker of per verificatie) niet wijzigen nadat een MFA-provider is gemaakt. U kunt de MFA-provider echter verwijderen en vervolgens een nieuwe maken met een ander gebruiksmodel.

Als de huidige Multi-Factor Authentication-provider is gekoppeld aan een Azure AD-adreslijst (ook wel een Azure AD-tenant genoemd), kunt u de MFA-provider gerust verwijderen en er een maken die is gekoppeld aan dezelfde Azure AD-tenant. U kunt ook als u onvoldoende MFA, Azure AD Premium of bundels met Azure AD Premium of Azure MFA softwarelicenties voor alle gebruikers die zijn ingeschakeld voor MFA hebt aangeschaft, kunt u verwijderen de MFA-provider kan worden overgeslagen.

Als de MFA-provider niet is gekoppeld aan een Azure AD-tenant, of als u de nieuwe MFA-provider koppelt aan een andere Azure AD-tenant, worden de gebruikersinstellingen en configuratieopties niet overgedragen. Bestaande Azure MFA-servers moeten ook opnieuw worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de nieuwe MFA-provider. Opnieuw activeren van de MFA-Servers om ze te koppelen aan de nieuwe MFA-Provider niet van invloed op telefonische oproepen en tekstberichtverificatie, maar de mobiele app-meldingen werken voor alle gebruikers niet meer totdat ze de mobiele app activeren.

## <a name="next-steps"></a>Volgende stappen

[Multi-Factor Authentication-instellingen configureren](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "MFA-providers toevoegen"
