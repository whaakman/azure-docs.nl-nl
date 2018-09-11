---
title: 'Zelfstudie: Slack configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory automatisch inrichten en ongedaan maken inrichting accounts van gebruikers naar Slack.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: 7d6056987ee05f68eecf026e954327a2f62cf886
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348902"
---
# <a name="tutorial-configure-slack-for-automatic-user-provisioning"></a>Zelfstudie: Slack configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is om weer te geven u de stappen die u wilt uitvoeren in Slack en Azure AD voor het automatisch inrichten en ongedaan maken inrichting gebruikersaccounts uit Azure AD naar Slack. 

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een Slack-tenant met de [Plus plan](https://aadsyncfabric.slack.com/pricing) of beter ingeschakeld 
*   Een gebruikersaccount in Slack met Team beheerdersmachtigingen 

Opmerking: De integratie wordt ingericht op Azure AD is afhankelijk van de [Slack SCIM API](https://api.slack.com/scim) die voor Slack teams beschikbaar is op het plusteken plannen of hoger.

## <a name="assigning-users-to-slack"></a>Gebruikers toewijzen aan Slack

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw app in Slack. Als besloten, kunt u deze gebruikers toewijzen aan uw Slack-app door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Belangrijke tips voor het toewijzen van gebruikers naar Slack

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan Slack voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer u een gebruiker toewijst aan Slack, moet u de **gebruiker** of rol 'Groep' in het dialoogvenster toewijzing. De rol 'standaardtoegang' werkt niet voor het inrichten.


## <a name="configuring-user-provisioning-to-slack"></a>Configureren met het inrichten van gebruikers naar Slack 

Deze sectie helpt u bij uw Azure AD verbinden met de Slack-gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen toegewezen gebruikersaccounts in Slack op basis van gebruiker en groepstoewijzing in Azure AD.

**Tip:** u kunt ook ingeschakeld SAML gebaseerde eenmalige aanmelding voor Slack, op. instructies hiervoor vindt in (Azure portal) [https://portal.azure.com]. Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.


### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts aan Slack in Azure AD:


1)  In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2) Als u Slack al hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van het zoekveld met Slack. Selecteer anders **toevoegen** en zoek naar de **Slack** in de toepassingengalerie. Selecteer Slack in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3)  Selecteer uw exemplaar van Slack, en selecteer vervolgens de **Provisioning** tabblad.

4)  Stel de **Inrichtingsmodus** naar **automatische**.

![Slack inrichten](./media/slack-provisioning-tutorial/Slack1.PNG)

5)  Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Hiermee opent u een dialoogvenster Slack autorisatie in een nieuw browservenster. 

6) Meld u in Slack met uw Team-beheerdersaccount in het nieuwe venster. Selecteer in het dialoogvenster resulterende autorisatie het Slack-team dat u inschakelen wilt voor inrichting, en selecteer vervolgens **autoriseren**. Als voltooid, terug naar de Azure-portal om de configuratie van de inrichting te voltooien.

![Autorisatie-dialoogvenster](./media/slack-provisioning-tutorial/Slack3.PNG)

7) Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app in Slack. Als de verbinding is mislukt, zorg ervoor dat uw Slack-account Team beheerdersmachtigingen heeft en probeer het opnieuw de stap 'Autoriseren'.

8) Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

9) Klik op **Opslaan**. 

10) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Slack**.

11) In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Slack. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen wordt gebruikt zodat deze overeenkomen met de gebruikersaccounts in Slack voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

12) Als wilt inschakelen in de Azure AD-inrichtingsservice voor Slack, wijzigt de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

13) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Slack in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 10 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenrapporten, waarin alle acties die worden uitgevoerd door de provisioning-service in uw Slack-app worden beschreven.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[Optioneel] Configureren van de groep object inrichten naar Slack 

Desgewenst kunt u de inrichting van groepsobjecten van Azure AD naar Slack. Dit wijkt af van het 'groepen gebruikers toe te wijzen', in dat de werkelijke groep object naast de leden ervan gerepliceerd van Azure AD naar Slack. Als u een groep met de naam 'Mijn groeperen' in Azure AD hebt, wordt bijvoorbeeld een identitical-groep met de naam 'Mijn groeperen' gemaakt in Slack.

### <a name="to-enable-provisioning-of-group-objects"></a>Om in te schakelen voor het inrichten van een groepsobjecten worden weergegeven:

1) Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-groepen naar Slack**.

2) Ingesteld op de blade kenmerk toewijzing ingeschakeld op Yes.

3) In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van die van Azure AD worden gesynchroniseerd naar Slack. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen wordt gebruikt zodat deze overeenkomen met de groepen in Slack voor update-bewerkingen. 

4) Klik op **Opslaan**.

Dit resultaat in een groepsobjecten die zijn toegewezen aan Slack in de **gebruikers en groepen** sectie volledig uit Azure AD worden gesynchroniseerd met Slack. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service in uw Slack-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
