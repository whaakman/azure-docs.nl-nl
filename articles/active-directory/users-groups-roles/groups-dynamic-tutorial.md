---
title: Zelfstudie voor het toevoegen/verwijderen van gebruikers en dynamische groepslidmaatschappen in Azure Active Directory
description: In deze zelfstudie gebruikt u groepen met gebruikerslidmaatschapsregels voor het automatisch toevoegen of verwijderen van gebruikers
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.openlocfilehash: bf625b45fa47d4fa681521eb992bd2b07cc08946
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296242"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Zelfstudie: Automatisch groepsleden toevoegen of verwijderen

In Azure Active Directory (Azure AD) kunt u automatisch gebruikers aan beveiligingsgroepen of Office 365-groepen toevoegen of daaruit verwijderen, zodat u dit niet altijd handmatig hoeft te doen. Wanneer een of meer eigenschappen van een gebruiker of apparaat worden gewijzigd, worden alle dynamische groepsregels in uw tenant door Microsoft Azure Active Directory geëvalueerd om te zien of er leden moeten worden toegevoegd of verwijderd.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een automatisch ingevulde groep gastgebruikers van een bepaald partnerbedrijf maken
> * Licenties aan de groep toewijzen voor de partnerspecifieke functies die toegankelijk moeten zijn voor de gastgebruikers
> * Optioneel: de groep **Alle gebruikers** beveiligen door gastgebruikers te verwijderen zodat bijvoorbeeld alleen lidgebruikers toegang hebben tot interne sites

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze functie moet u, als globale beheerder van de tenant, beschikken over een Microsoft Azure Active Directory Premium-licentie. Als u nog geen licentie hebt, selecteert u in Microsoft Azure Active Directory **Licenties** > **Producten** > **Proberen/kopen**.

U bent niet verplicht licenties aan de gebruikers toe te wijzen om ze op te nemen als leden in dynamische groepen. U hebt slechts het minimum aantal beschikbare licenties voor Microsoft Azure Active Directory Premium P1 in de tenant nodig voor dergelijke gebruikers. 

## <a name="create-a-group-of-guest-users"></a>Een groep gastgebruikers maken

Eerst maakt u een groep voor uw gastgebruikers, die alle afkomstig zijn van één partnerbedrijf. Omdat zij speciale licenties nodig hebben, is het vaak efficiënter om een groep te maken voor dit doel.

1. Meld u bij Azure Portal aan (https://portal.azure.com) met het account van de globale beheerder voor de tenant.
2. Selecteer **Azure Active Directory** > **Groepen** > **Nieuwe groep**.
  ![De opdracht Nieuwe groep selecteren](./media/groups-dynamic-tutorial/new-group.png)
3. Ga als volgt te werk op de blade **Groep**:
  
  * Selecteer **Beveiliging** als het groepstype
  * Voer `Guest users Contoso` in als de naam en beschrijving voor de groep
  * Wijzig het **lidmaatschapstype** in **Dynamische gebruiker**
  * Selecteer **Dynamische query toevoegen**
  
4. Selecteer **Geavanceerde regel** en geef het volgende op in het vak **Geavanceerde regel**: `(user.userType -eq "Guest") -and (user.companyName -eq "Contoso")`
5. Selecteer **Query toevoegen** om de blade te sluiten.
6. Selecteer, op de blade **Groep**, **Maken** om de groep te maken.

## <a name="assign-licenses"></a>Licenties toewijzen

Nu u de nieuwe groep hebt gemaakt, kunt u de benodigde licenties aan deze partnergebruikers toewijzen.

1. Selecteer **Licenties** in Microsoft Azure Active Directory, selecteer een of meer licenties, en selecteer vervolgens **Toewijzen**.
2. Selecteer **Gebruikers en groepen**, selecteer de groep **Gastgebruikers Contoso** en sla uw wijzigingen op.
3. Bij **Toewijzingsopties** kunt u de abonnementen bij de geselecteerde licenties in- en uitschakelen. Wanneer u een wijziging aanbrengt, moet u op **OK** klikken uw wijzigingen op te slaan.
4. Voltooi de toewijzing door onder aan het deelvenster **Licentie toewijzen** op **Toewijzen** te klikken.

## <a name="remove-guests-from-all-users-group"></a>Gastgebruikers verwijderen uit de groep Alle gebruikers

Mogelijk wilt u als beheerder uiteindelijk alle gastgebruikers per bedrijf toewijzen aan een afzonderlijke groep. U kunt echter nu ook de groep **Alle gebruikers** zo wijzigen, dat deze is voorbehouden aan lidgebruikers in uw tenant. Vervolgens kunt u aan deze groep apps en licenties toewijzen die specifiek zijn voor uw organisatie.

   ![De groep Alle gebruikers zo wijzigen dat er alleen leden overblijven](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Resources opschonen

**De groep Gastgebruikers verwijderen**

1. Meld u bij [Azure Portal](https://portal.azure.com) aan met het account van de globale beheerder voor uw tenant.
2. Selecteer **Azure Active Directory** > **Groepen**. Selecteer de groep **Gastgebruikers Contoso**, selecteer het weglatingsteken (...) en selecteer vervolgens **Verwijderen**. Wanneer u de groep verwijdert, worden alle toegewezen licenties verwijderd.

**De groep Alle gebruikers herstellen**
1. Selecteer **Azure Active Directory** > **Groepen**. Selecteer de naam van de groep **Alle gebruikers** om de groep te openen.
1. Selecteer **Dynamisch-lidmaatschapregels**, verwijder alle tekst in de regel en selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]
> * Een groep gastgebruikers maken
> * Licenties toewijzen aan uw nieuwe groep
> * De groep Alle gebruikers zo wijzigen dat er alleen leden overblijven

Ga naar het volgende artikel voor meer informatie over de basisprincipes van groepslicenties
> [!div class="nextstepaction"]
> [Basisprincipes van groepslicenties](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



