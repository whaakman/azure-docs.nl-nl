---
title: Toegang tot Azure Cost Management gegevens toewijzen | Microsoft Docs
description: Dit artikel helpt u al machtiging toewijzen aan Azure Cost Management-gegevens voor verschillende toegangsbereiken.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: cost-management
manager: vitavor
ms.custom: ''
ms.openlocfilehash: 3096a79737c816747f36956958f9a16f86b9715d
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582595"
---
# <a name="assign-access-to-cost-management-data"></a>Toegang tot gegevens van kostenbeheer toewijzen

Voor de meeste gebruikers definiëren een combinatie van machtigingen in de Azure portal en de Enterprise (EA)-portal het toegangsniveau van een gebruiker tot gegevens van Azure Cost Management. Dit artikel begeleidt u bij het toewijzen van toegang tot gegevens van Cost Management. Nadat de combinatie van machtigingen is toegewezen, is de gegevens van de weergaven in Cost Management op basis van het bereik dat ze toegang tot en op het bereik hebben dat ze in Azure portal selecteert.

Het adresbereik op dat een gebruiker selecteert wordt gebruikt in de gehele kostenbeheer voor gegevensconsolidatie en voor het beheren van toegang tot gegevens over de kosten. Bij het gebruik van bereiken, gebruikers geen meervoudige selectie ze. Ze selecteren in plaats daarvan een groter bereik die onderliggende bereiken tot draaien en vervolgens deze filter omlaag wat ze zoeken om weer te geven. Gegevensconsolidatie is belangrijk om te begrijpen omdat sommige mensen toegang tot een bovenliggend bereik die onderliggende bereiken tot mag niet hebt.

## <a name="cost-management-scopes"></a>Cost Management bereiken

Als u wilt weergeven van gegevens van cost, een gebruiker moet ten minste lezen hebben toegang tot een of meer van de volgende bereiken.

| **Bereik** | **Gedefinieerd op** | **Vereist toegang tot gegevens weergeven** | **Vereiste EA-instelling** | **Gegevens om te worden samengevoegd** |
| --- | --- | --- | --- | --- |
| Factureringsaccount<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Enterprise-beheerder | Geen | Alle abonnementen uit de Enterprise Overeenkomst |
| Afdeling | [https://ea.azure.com](https://ea.azure.com/) | Afdelingsbeheerder | **Kosten voor het weergeven van DA** ingeschakeld | Alle abonnementen die horen bij een inschrijvingsaccount dat is gekoppeld aan de afdeling |
| Inschrijvingsaccount<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Accounteigenaar | **Kosten weergeven die door de AO** ingeschakeld | Alle abonnementen van het inschrijvingsaccount |
| Beheergroep | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Kosten weergeven die door de AO** ingeschakeld | Alle abonnementen onder de beheergroep |
| Abonnement | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Kosten weergeven die door de AO** ingeschakeld | Alle resources/resourcegroepen in het abonnement |
| Resourcegroep | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Kosten weergeven die door de AO** ingeschakeld | Alle resources in de resourcegroep |

<sup>1</sup> de factureringsaccount ook wel de Enterprise overeenkomst of de inschrijving wordt genoemd.

<sup>2</sup> het inschrijvingsaccount wordt ook aangeduid als eigenaar van het account.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Toegang tot de kosten van de EA-portal inschakelen

Het bereik van de afdeling is vereist de **DA weergave kosten** optie **ingeschakeld** in de EA-portal. Alle andere bereiken vereisen de **kosten weergeven die door de AO** optie **ingeschakeld** in de EA-portal.

Een optie inschakelen:

1. Meld u aan bij de EA-portal op [ https://ea.azure.com ](https://ea.azure.com) met een enterprise-beheerdersaccount.
2. Selecteer **beheren** in het linkerdeelvenster.
3. Voor het kostenbeheer-die bereiken u wilt dat voor toegang tot, schakelt u de optie kosten in rekening gebracht aan **DA weergave kosten** en/of **kosten weergeven die door de AO**.  
    ![Inschrijving tabblad DA en door de AO weergeven met kosten opties voor](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Nadat de opties voor de kosten zijn ingeschakeld, moeten de meeste bereiken op basis van rollen (RBAC) machtiging configuratie voor toegangsbeheer in Azure portal.

## <a name="enterprise-administrator-role"></a>Rol van Enterprise-beheerder

Een enterprise-beheerder heeft standaard toegang tot de factureringsaccount (Enterprise Agreement/inschrijving) en alle andere bereiken, die onderliggende bereiken. Toegang tot de enterprise-beheerder toegewezen aan bereiken voor andere gebruikers. Als een best practice voor bedrijfscontinuïteit, moet u altijd twee gebruikers met een enterprise-beheerderstoegang hebben. De volgende secties zijn voorbeelden van de procedure van de enterprise-beheerder toewijzen toegang tot bereiken voor andere gebruikers.

## <a name="assign-billing-account-scope-access"></a>Facturering toegang tot het bereik toewijzen

Toegang tot de facturering accountbereik is enterprise-beheerder toestemming in de EA-portal vereist. De enterprise-beheerder heeft toegang tot de kosten voor de hele EA-inschrijving of meerdere inschrijvingen weergeven. Er is geen actie is vereist in de Azure-portal voor de facturering accountbereik.

1. Meld u aan bij de EA-portal op [ https://ea.azure.com ](https://ea.azure.com) met een enterprise-beheerdersaccount.
2. Selecteer **beheren** in het linkerdeelvenster.
3. Op de **inschrijving** tabblad, selecteert u de inschrijving die u wilt beheren.  
    ![EA-portal](./media/assign-access-acm-data/ea-portal.png)
4. Klik op **+ beheerder toevoegen**.
5. Selecteer het verificatietype en typ de e-mailadres van de gebruiker in het vak beheerder toevoegen.
6. Als de gebruiker moet alleen-lezen toegang hebben tot gegevens van de kosten en gebruik, onder **alleen-lezen**, selecteer **Ja**.  Selecteer anders **Nee**.
7. Klik op **toevoegen** om het account te maken.  
    ![Beheerder toevoegen](./media/assign-access-acm-data/add-admin.png)

Het duurt maximaal 30 minuten voordat de nieuwe gebruiker toegang heeft tot gegevens in Cost Management.

### <a name="assign-department-scope-access"></a>Afdeling bereik toegang toewijzen

Toegang tot het bereik van de afdeling is vereist afdeling beheerderstoegang (kosten voor het weergeven van DA) in de EA-portal. De beheerder van de afdeling heeft toegang tot de kosten en gebruiksgegevens die zijn gekoppeld met een afdeling of op meerdere afdelingen weergeven. Gegevens voor de afdeling bevatten alle abonnementen die behoren tot een inschrijvingsaccount die zijn gekoppeld aan de afdeling. Er is geen actie is vereist in de Azure-portal.

1. Meld u aan bij de EA-portal op [ https://ea.azure.com ](https://ea.azure.com) met een enterprise-beheerdersaccount.
2. Selecteer **beheren** in het linkerdeelvenster.
3. Op de **inschrijving** tabblad, selecteert u de inschrijving die u wilt beheren.
4. Klik op de **afdeling** tabblad en klik vervolgens op **beheerder toevoegen**.
5. In het vak afdeling beheerder toevoegen, selecteert u het verificatietype en typ vervolgens de e-mailadres van de gebruiker.
6. Als de gebruiker moet alleen-lezen toegang hebben tot gegevens van de kosten en gebruik, onder **alleen-lezen**, selecteer **Ja**.  Selecteer anders **Nee**.
7. Selecteer de afdelingen die u wilt een beheerdersmachtiging nodig voor afdeling verlenen.
8. Klik op **toevoegen** om het account te maken.  
    ![Vak voor afdeling beheerder toevoegen](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Toegang tot het bereik voor inschrijving toewijzen

Toegang tot het bereik van de account inschrijving vereist account eigenaar (kosten door de AO weergeven) toegang in de EA-portal. Eigenaar van het account kan kosten- en gebruiksgegevens die zijn gekoppeld aan de abonnementen die zijn gemaakt op basis van die apparaatregistratie-account weergeven. Er is geen actie is vereist in de Azure-portal.

1. Meld u aan bij de EA-portal op [ https://ea.azure.com ](https://ea.azure.com) met een enterprise-beheerdersaccount.
2. Selecteer **beheren** in het linkerdeelvenster.
3. Op de **inschrijving** tabblad, selecteert u de inschrijving die u wilt beheren.
4. Klik op de **Account** tabblad en klik vervolgens op **-Account toevoegen**.
5. Selecteer in het Account toevoegen de **afdeling** aan het account te koppelen of laat het als niet-toegewezen.
6. Selecteer het verificatietype en typ de naam van het account.
7. Typ de e-mailadres van de gebruiker en typt u de kostenplaats (optioneel).
8. Klik op **toevoegen** om het account te maken.  
    ![In het account toevoegen](./media/assign-access-acm-data/add-account.png)

Na het voltooien van de bovenstaande stappen het gebruikersaccount dat wordt een voor apparaatregistratie-account in de Enterprise portal en -abonnementen kunt maken. De gebruiker kan toegang tot gegevens van kosten en gebruik voor abonnementen die ze maken.

## <a name="assign-management-group-scope-access"></a>Bereik toegang tot de groep toewijzen

Toegang tot een beheerbereik van de groep moet ten minste de machtiging Cost Management Reader (of lezer). U kunt machtigingen voor een beheergroep configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor de beheergroep die u wilt toegang inschakelen voor anderen. En u moet ook zijn ingeschakeld de **kosten weergeven die door de AO** instellen in de EA-portal.

1. Meld u aan bij de Azure Portal op [http://portal.azure.com](http://portal.azure.com).
2. Selecteer **alle Services** Zoek in de zijbalk _beheergroepen_en selecteer vervolgens **beheergroepen**.
3. De beheergroep die u in de hiërarchie selecteren.
4. Naast de naam van uw beheergroep, klikt u op **Details**.
5. Selecteer **Access Control (IAM)** in het linkerdeelvenster.
6. Klik op **Add**.
7. Onder **rol**, selecteer **Cost Management lezer**.
8. Onder **toegang toewijzen aan**, selecteer **Azure AD-gebruiker, groep of toepassing**.
9. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
10. Klik op **Opslaan**.  
    ![Machtigingen toevoegen](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Abonnement bereik toegang toewijzen

Toegang tot een abonnement vereist ten minste de machtiging Cost Management Reader (of lezer). U kunt machtigingen voor een abonnement configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor het abonnement om toegang te voor anderen. En u moet ook zijn ingeschakeld de **kosten weergeven die door de AO** instellen in de EA-portal.

1. Meld u aan bij de Azure Portal op [http://portal.azure.com](http://portal.azure.com).
2. Selecteer **alle Services** Zoek in de zijbalk _abonnementen_en selecteer vervolgens **abonnementen**.
3. Selecteer uw abonnement.
4. Selecteer **Access Control (IAM)** in het linkerdeelvenster.
5. Klik op **Add**.
6. Onder **rol**, selecteer **Cost Management lezer**.
7. Onder **toegang toewijzen aan**, selecteer **Azure AD-gebruiker, groep of toepassing**.
8. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
9. Klik op **Opslaan**.

## <a name="assign-resource-group-scope-access"></a>Resource group bereik toegang toewijzen

Toegang tot een resourcegroep is vereist ten minste de machtiging Cost Management Reader (of lezer). U kunt machtigingen om een resourcegroep te configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor de resourcegroep voor het inschakelen van toegang voor anderen. En u moet ook zijn ingeschakeld de **kosten weergeven die door de AO** instellen in de EA-portal.

1. Meld u aan bij de Azure Portal op [http://portal.azure.com](http://portal.azure.com).
2. Selecteer **alle Services** Zoek in de zijbalk _resourcegroepen_en selecteer vervolgens **resourcegroepen**.
3. Selecteer de resourcegroep.
4. Selecteer **Access Control (IAM)** in het linkerdeelvenster.
5. Klik op **Add**.
6. Onder **rol**, selecteer **Cost Management lezer**.
7. Onder **toegang toewijzen aan**, selecteer **Azure AD-gebruiker, groep of toepassing**.
8. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
9. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt al de eerste snelstartgids voltooid voor kostenbeheer, lezen via [kosten analyseren](quick-acm-cost-analysis.md).
