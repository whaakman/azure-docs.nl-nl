---
title: 'Zelfstudie: Configureer LinkedIn met verhoogde bevoegdheden voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met LinkedIn met verhoogde bevoegdheden.
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
ms.date: 01/28/2018
ms.author: asmalser-msft
ms.openlocfilehash: 1dcc198c1a1cc798e991f489e6897d4b930c0593
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348495"
---
# <a name="tutorial-configure-linkedin-elevate-for-automatic-user-provisioning"></a>Zelfstudie: LinkedIn, met verhoogde bevoegdheden configureren voor het automatisch inrichten van gebruikers


Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in de LinkedIn met verhoogde bevoegdheden en Azure AD wilt voor het automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar LinkedIn met verhoogde bevoegdheden. 

## <a name="prerequisites"></a>Vereiste onderdelen

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active Directory-tenant
*   Een tenant LinkedIn met verhoogde bevoegdheden uitvoeren 
*   Een administrator-account in LinkedIn verhogen met toegang tot het Accountcentrum LinkedIn

> [!NOTE]
> Azure Active Directory kan worden geïntegreerd met het gebruik van LinkedIn met verhoogde bevoegdheden de [SCIM](http://www.simplecloud.info/) protocol.

## <a name="assigning-users-to-linkedin-elevate"></a>Gebruikers toewijzen aan LinkedIn met verhoogde bevoegdheden uitvoeren

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot LinkedIn met verhoogde bevoegdheden. Als besloten, kunt u deze gebruikers toewijzen aan LinkedIn met verhoogde bevoegdheden en volg de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a>Belangrijke tips voor het toewijzen van gebruikers om te verhogen voor LinkedIn

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan LinkedIn met verhoogde bevoegdheden voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker op LinkedIn met verhoogde bevoegdheden, moet u de **gebruiker** rol in het dialoogvenster toewijzing. De rol 'standaardtoegang' werkt niet voor het inrichten.


## <a name="configuring-user-provisioning-to-linkedin-elevate"></a>Configureren met het inrichten van gebruikers naar LinkedIn met verhoogde bevoegdheden uitvoeren

Deze sectie helpt u bij uw Azure AD verbinden met LinkedIn met verhoogde bevoegdheden uitvoeren van SCIM gebruikersaccount Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen toegewezen gebruikersaccounts in LinkedIn met verhoogde bevoegdheden op basis van gebruiker en groepstoewijzing in Azure AD.

**Tip:** u kunt ook ingeschakeld SAML gebaseerde eenmalige aanmelding voor LinkedIn met verhoogde bevoegdheden, op. instructies hiervoor vindt [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a>Het configureren van automatische inrichten van gebruikersaccounts naar LinkedIn met verhoogde bevoegdheden in Azure AD:


De eerste stap is om op te halen van uw LinkedIn-toegangstoken. Als u een Enterprise-beheerder bent, kunt u zelf een toegangstoken inrichten. In het accountcentrum, gaat u naar **instellingen &gt; globale instellingen** en open de **SCIM Setup** deelvenster.

> [!NOTE]
> Als u het accountcentrum rechtstreeks in plaats van via een koppeling opent, kunt u bereiken met behulp van de volgende stappen uit.

1)  Aanmelden bij Account Center.

2)  Selecteer **Admin &gt; beheerdersinstellingen** .

3)  Klik op **geavanceerde integraties** op de linkerzijbalk. U bent omgeleid naar het accountcentrum.

4)  Klik op **+ toevoegen nieuwe SCIM configuratie** en volg de procedure door in elk veld te vullen.

> Wanneer autoassign licenties niet is ingeschakeld, betekent dit dat alleen de gegevens van de gebruiker is gesynchroniseerd.

![LinkedIn met verhoogde bevoegdheden ingericht](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate1.PNG)

> Wanneer autolicense toewijzing is ingeschakeld, moet u de toepassingsexemplaar en het licentietype. Licenties zijn toegewezen op een eerst komt, eerst dienen uit te voeren totdat alle licenties zijn genomen.

![LinkedIn met verhoogde bevoegdheden ingericht](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  Klik op **token genereren**. U ziet nu de toegang tot token wordt weergegeven onder de **toegangstoken** veld.

6)  Sla uw toegangstoken op het Klembord of de computer voordat u de pagina verlaat.

7) Vervolgens maakt u zich aanmeldt bij de [Azure-portal](https://portal.azure.com), en blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

8) Als u al hebt geconfigureerd LinkedIn met verhoogde bevoegdheden voor eenmalige aanmelding, zoeken naar uw exemplaar van LinkedIn met verhoogde bevoegdheden uitvoeren met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **LinkedIn met verhoogde bevoegdheden** in de toepassingengalerie. Selecteer LinkedIn met verhoogde bevoegdheden in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

9)  Selecteer uw exemplaar van LinkedIn met verhoogde bevoegdheden en selecteer vervolgens de **Provisioning** tabblad.

10) Stel de **Inrichtingsmodus** naar **automatische**.

![LinkedIn met verhoogde bevoegdheden ingericht](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  Vul de volgende velden onder **beheerdersreferenties** :

* In de **Tenant-URL** veld https://api.linkedin.com.

* In de **geheim Token** veld, voer het toegangstoken die u in stap 1 in en klikt u op **testverbinding** .

* U ziet een melding succes upperright aan van de portal.

12) Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het onderstaande selectievakje in.

13) Klik op **Opslaan**. 

14) In de **kenmerktoewijzingen** sectie, controleert u de gebruikers- en groepskenmerken die van Azure AD worden gesynchroniseerd met LinkedIn met verhoogde bevoegdheden. Houd er rekening mee dat de kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen wordt gebruikt zodat deze overeenkomen met de gebruikersaccounts en groepen in LinkedIn met verhoogde bevoegdheden voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

![LinkedIn met verhoogde bevoegdheden ingericht](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) Als u wilt inschakelen in de Azure AD-inrichtingsservice voor LinkedIn met verhoogde bevoegdheden, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

16) Klik op **Opslaan**. 

Hiermee start u de initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan LinkedIn met verhoogde bevoegdheden in de sectie gebruikers en groepen. Houd er rekening mee dat de eerste synchronisatie langer dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden duurt als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op uw app LinkedIn met verhoogde bevoegdheden worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
