---
title: 'Zelfstudie: Slack configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory automatisch leveren en intrekken accounts van gebruikers met Slack.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: 3cb49a4abb26c34a938c963c4cf326b5ccd490de
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a>Zelfstudie: Slack configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is zodat u de stappen die u wilt uitvoeren in Slack en Azure AD aan automatisch leveren en intrekken gebruikersaccounts vanuit Azure AD met Slack. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Active directory-tenant
*   Een Slack-tenant met de [Plus plan](https://aadsyncfabric.slack.com/pricing) of beter ingeschakeld 
*   Een gebruikersaccount in Slack met beheerdersmachtigingen Team 

Opmerking: De Azure AD integratie inrichting is afhankelijk van de [Slack SCIM API](https://api.slack.com/scim) die voor toegestane teams beschikbaar is op het plusteken plannen of sneller.

## <a name="assigning-users-to-slack"></a>Toewijzen van gebruikers met Slack

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot de toegestane app vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw toegestane app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Belangrijke tips voor het toewijzen van gebruikers met Slack

*   Het is raadzaam om één Azure AD-gebruiker worden toegewezen aan de vertraging voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met Slack, moet u de **gebruiker** of 'Groep'-rol in het dialoogvenster toewijzing. De rol 'Default toegang' werkt niet voor het inrichten.


## <a name="configuring-user-provisioning-to-slack"></a>Gebruikers inrichten met Slack configureren 

Deze sectie helpt u bij uw Azure AD verbinden met Slack van gebruikersaccount inrichten API en de inrichting service maken, bijwerken en uitschakelen configureren toegewezen gebruikersaccounts in Slack op basis van gebruiker en groepstoewijzing in Azure AD.

**Tip:** u kunt ook ingeschakeld op basis van SAML eenmalige aanmelding voor de vertraging, volgt de instructies in (Azure-portal) [https://portal.azure.com]. Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.


### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Voor het configureren van automatische gebruikers account inrichten met Slack in Azure AD:


1)  In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2) Als u al vertraging voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van het zoekveld met Slack. Selecteer anders **toevoegen** en zoek naar **Slack** in de galerie met toepassingen. Vertraging in de zoekresultaten te selecteren en toe te voegen aan uw lijst met toepassingen.

3)  Selecteer uw exemplaar van Slack en selecteer vervolgens de **inrichten** tabblad.

4)  Stel de **Inrichtingsmodus** naar **automatische**.

![Vertraging inrichten](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Hiermee opent u een dialoogvenster toegestane autorisatie in een nieuw browservenster. 

6) Aanmelden bij uw Team beheerdersaccount met Slack in het nieuwe venster. Selecteer in het dialoogvenster resulterende autorisatie het toegestane team dat u inschakelen wilt voor inrichting, en selecteer vervolgens **autoriseren**. Als voltooid, terug naar de Azure-portal om het inrichtingsproces configuratie te voltooien.

![Dialoogvenster autorisatie](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw toegestane app. Als de verbinding is mislukt, controleert dat uw toegestane account Team beheerder machtigingen heeft en probeer het opnieuw de stap 'Autoriseren'.

8) Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje hieronder in.

9) Klik op **Opslaan**. 

10) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers met Slack**.

11) In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die met Slack van Azure AD worden gesynchroniseerd. Let op de kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Slack voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

12) Om de Azure AD-service voor Slack inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

13) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan de vertraging in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 10 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw toegestane app te beschrijven.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[Optioneel] Groep object inrichten met Slack configureren 

Desgewenst kunt u de inrichting van groepsobjecten van Azure AD met Slack inschakelen. Dit is anders dan 'groepen gebruikers toe te wijzen', in dat de werkelijke groep object naast de leden ervan gerepliceerd van Azure AD met Slack. Als u een groep met de naam 'Mijn groep' in Azure AD hebt, wordt bijvoorbeeld een identitical groep met de naam 'Mijn groep' gemaakt binnen de toegestane vertraging.

### <a name="to-enable-provisioning-of-group-objects"></a>Inschakelen voor het inrichten van groepsobjecten:

1) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-groepen met Slack**.

2) Stel op de blade kenmerk toewijzing Enabled in op Ja.

3) In de **kenmerktoewijzingen** sectie, moet u de kenmerken die worden gesynchroniseerd vanuit Azure AD met Slack controleren. Let op de kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de groepen in Slack voor update-bewerkingen worden gebruikt. 

4) Klik op **Opslaan**.

Deze leiden tot een groepsobjecten worden toegewezen aan de vertraging in de **gebruikers en groepen** sectie volledig van Azure AD worden gesynchroniseerd met vertraging. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw toegestane app te beschrijven.


## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
