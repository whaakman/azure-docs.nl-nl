---
title: 'Zelfstudie: Velpic configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met Velpic.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 059ce3a23a9bdacfb978ccad775c7da853772e3f
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344716"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Zelfstudie: Velpic configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in Velpic en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar Velpic ongedaan maken. 

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant Velpic met de [Enterprise-plan](https://www.velpic.com/pricing.html) of beter ingeschakeld 
*   Een gebruikersaccount in Velpic met beheerdersmachtigingen 

## <a name="assigning-users-to-velpic"></a>Gebruikers toewijzen aan Velpic

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app Velpic vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app Velpic door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Belangrijke tips voor het toewijzen van gebruikers aan Velpic

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan Velpic voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Velpic toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. Houd er rekening mee dat de **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-velpic"></a>Inrichten van gebruikers naar Velpic configureren 

Deze sectie helpt u bij uw Azure AD verbinden met de Velpic gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen toegewezen gebruikersaccounts in Velpic op basis van gebruiker en groepstoewijzing in Azure AD.

>[!TIP]
>U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Velpic, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.


### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts aan Velpic in Azure AD:

1.  In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al Velpic hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Velpic met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Velpic** in de toepassingengalerie. Selecteer Velpic in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3.  Selecteer uw exemplaar van Velpic en selecteer vervolgens de **Provisioning** tabblad.

4.  Stel de **Inrichtingsmodus** naar **automatische**.

    ![Velpic inrichten](./media/velpic-provisioning-tutorial/Velpic1.png)

5.  Onder de **beheerdersreferenties** sectie, voer de **Tenant-URL en geheim Token** van Velpic. () U kunt deze waarden vinden onder uw account Velpic: **Beheren** > **integratie** > **invoegtoepassing** > **SCIM**)

    ![Autorisatiewaarden](./media/velpic-provisioning-tutorial/Velpic2.png)

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app Velpic. Als de verbinding is mislukt, zorg ervoor dat uw account Velpic beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Velpic**.

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd met Velpic. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen wordt gebruikt zodat deze overeenkomen met de gebruikersaccounts in Velpic voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

11. Wijzigen zodat de Azure AD-inrichtingsservice voor Velpic de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Velpic in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenrapporten, waarin alle acties die worden uitgevoerd door de inrichtingsservice worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)