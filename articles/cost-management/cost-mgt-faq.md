---
title: Veelgestelde vragen over Azure kosten Management | Microsoft Docs
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Azure kosten Management.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/14/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: f62e5a224c2fb33714a80bc47b98238208b787e5
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Veelgestelde vragen over Azure kosten Management

In dit artikel komen enkele veelgestelde vragen over Azure kosten Management (ook wel bekend als Cloudyn). Als u vragen over het beheer van kosten hebt, kunt u ze op vragen [Veelgestelde vragen over Azure kosten Management Cloudyn](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Hoe kan ik de algemene indirecte enterprise installatieproblemen oplossen?

Wanneer u de portal Cloudyn voor het eerst gebruikt, ziet u mogelijk de volgende berichten te zien als u een Enterprise Agreement of Cloud Solution Provider (CSP) gebruiker:

- 'De opgegeven API-sleutel is niet een sleutel van het hoogste niveau inschrijving' weergegeven in de **instellen van Azure kosten Management** wizard.
- 'Directe inschrijving – niet' weergegeven in de Enterprise Agreement-portal.
- 'Geen gebruiksgegevens gevonden voor de afgelopen 30 dagen. Neem contact op met uw leverancier om te controleren of de opmaak is ingeschakeld voor uw Azure-account' weergegeven in de portal Cloudyn.

De voorgaande berichten geven aan dat u een Enterprise Agreement voor Azure via een wederverkoper of CSP hebt aangeschaft. Uw wederverkoper of CSP moet inschakelen _markup_ voor uw Azure-account zodat u uw gegevens kunt bekijken in Cloudyn.

Ga als volgt de problemen op te lossen:

1. Uw reseller nodig heeft om in te schakelen _markup_ voor uw account. Voor instructies raadpleegt u de [indirecte Onboarding-handleiding voor klant](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. U de Enterprise Agreement voor Azure-sleutel voor gebruik met Cloudyn genereren. Zie voor instructies [uw Azure-EA toe te voegen](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) of [vinden uw EA inschrijving-ID en API-sleutel](https://youtu.be/u_phLs_udig).

Alleen een beheerder van de Azure-service kunt kostenbeheer inschakelen. U hebt onvoldoende beheerdersmachtigingen voor CO.

Voordat u de Azure Enterprise Agreement API-sleutel voor het instellen van Cloudyn genereren kunt, moet u de Azure-facturering API inschakelen door de instructies op:

- [Overzicht van de rapportage-API's voor Enterprise-klanten](../billing/billing-enterprise-api.md)
- [Microsoft Azure enterprise portal rapportage-API](https://ea.azure.com/helpdocs/reportingAPI) onder **gegevenstoegang tot de API inschakelen**


U moet wellicht ook geven afdeling beheerders, account eigenaars en enterprise-administrators machtigingen voor _kosten weergeven_ met de API voor facturering.

## <a name="why-dont-i-see-optimizer-recommendations"></a>Waarom zie ik niet optimaliseren aanbevelingen?

Aanbeveling informatie is alleen beschikbaar voor accounts die zijn geactiveerd. Worden er geen informatie aanbeveling in **optimaliseren** categorieën voor accounts die zijn gerapporteerd *niet-geactiveerde*, waaronder:

- Optimalisatie Manager
- Sizing optimalisatie
- Inefficiëntie

Als u niet alle gegevens van de aanbeveling optimaliseren weergeven, waarschijnlijk, hebt u accounts die niet-geactiveerde zijn. Voor het activeren van een account dat u wilt registreren met uw Azure-referenties.

Een account activeren:

1.  Klik in de portal Cloudyn **instellingen** in de linkerbovenhoek rechts en selecteer **Accounts in de Cloud**.
2.  Zoek op het tabblad Microsoft Azure-Accounts voor accounts die u hebt een **niet-geactiveerde** abonnement.
3.  Aan de rechterkant van een niet-geactiveerde account, klikt u op de **bewerken** symbool dat lijkt op een pen.
4.  Uw tenant-ID en tarief-ID wordt automatisch gedetecteerd. Klik op **Volgende**.
5.  U wordt omgeleid naar de Azure-portal. Aanmelden bij de portal en autoriseren van de Cloudyn Collector toegang tot uw Azure-gegevens.
6.  Vervolgens wordt u doorgestuurd naar de pagina Cloudyn Accounts beheren en uw abonnement is bijgewerkt met **active** accountstatus. Er wordt een symbool groen vinkje weergegeven.
7.  Als er geen een groen vinkje voor een of meer van de abonnementen, betekent dit dat u bent niet gemachtigd om een reader-app (CloudynCollector) voor het abonnement te maken. Herhaal stap 3 en 4 moet een gebruiker met hogere machtigingen voor het abonnement.  

Nadat u de voorgaande stappen hebt voltooid, kunt u optimaliseren aanbevelingen weergeven binnen een tot twee dagen. Het kan echter maximaal vijf dagen voordat de gegevens volledig optimalisatie beschikbaar duren.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Hoe kan ik onderbroken of vergrendelde gebruikers inschakelen?

Als u een waarschuwing met een verzoek om toegang te verlenen voor een gebruiker ontvangt, moet u het gebruikersaccount activeren.

Het gebruikersaccount activeren:

1. Aanmelden bij Cloudyn met behulp van de gebruiker met beheerdersrechten Azure-account dat u gebruikt voor het instellen van Cloudyn. Of meld u aan met een gebruikersaccount met administrator-toegang is verleend.

2. Selecteer het symbool tandwielpictogram in de rechterbovenhoek en selecteer **Gebruikersbeheer**.

3. De gebruiker niet vinden, selecteer het symbool pen en bewerk vervolgens de gebruiker.

4. Onder **gebruikersstatus**, wijzig de status van **onderbroken** naar **Active**.

Gebruikersaccounts Cloudyn verbinding maken met behulp van eenmalige aanmelding van Azure. Als een gebruiker mistypes hun wachtwoord, ze mogelijk ophalen vergrendeld Cloudyn, zelfs als ze steeds toegang Azure tot nog.

Als u uw e-mailadres in Cloudyn van het standaard-adres in Azure wijzigt, kan uw account krijgen vergrendeld. Deze mogelijk weer 'status initiallySuspended'. Als uw gebruikersaccount is vergrendeld, contact op met een alternatieve beheerder uw account opnieuw instelt.

U wordt aangeraden dat u ten minste twee Cloudyn administrator-accounts maken als een van de accounts is vergrendeld.

Als u niet kunt bij de portal Cloudyn aanmelden, zorg ervoor dat u de juiste URL van Azure kosten Management aan te melden bij Cloudyn. Gebruik [https://azure.cloudyn.com](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade).

Vermijd het gebruik van de Cloudyn directe URL https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Hoe kan ik de niet-geactiveerde accounts met Azure-referenties activeren?

Als uw Azure-accounts worden gedetecteerd door Cloudyn, wordt kostengegevens onmiddellijk beschikbaar in rapporten op basis van kosten. Voor Cloudyn om gegevens voor gebruik en prestaties te leveren, moet u echter uw Azure-referenties voor de accounts te registreren. Zie voor instructies [toevoegen Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager).

Als Azure-referenties voor een account in de portal Cloudyn, schakelt u het symbool bewerken rechts van de accountnaam, niet het abonnement.

Als uw Azure-referenties aan Cloudyn zijn toegevoegd, het account wordt weergegeven als _niet geactiveerde_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Hoe voeg ik meerdere accounts en entiteiten aan een bestaand abonnement?

Als u meer entiteiten worden gebruikt om aanvullende Enterprise overeenkomsten toevoegen aan een abonnement Cloudyn. De volgende koppelingen beschrijven hoe extra entiteiten toevoegen:

- [Toevoegen van een entiteit](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) artikel
- [Het definiëren van uw hiërarchie met kosten entiteiten](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) video

Voor CSP's:

Selecteer om extra CSP-accounts toe aan een entiteit **MSP toegang** in plaats van **Enterprise** wanneer u de nieuwe entiteit maken. Als uw account is geregistreerd als een Enterprise Agreement, en u wilt toevoegen, CSP-referenties, mogelijk Cloudyn ondersteunend personeel om de accountinstellingen van uw te wijzigen. Als u een betaald Azure-abonnee bent, kunt u een nieuw ondersteuningsverzoek maken in de Azure portal. Selecteer **Help + ondersteuning**, en selecteer vervolgens **nieuw ondersteuningsverzoek**.

## <a name="currency-symbols-in-cloudyn-reports"></a>Valutasymbolen in Cloudyn rapporten

U mogelijk meerdere Azure-accounts met behulp van verschillende valuta's. Kostenrapporten in Cloudyn worden echter niet meer dan één Valutatype per rapport weergegeven.

Als u meerdere abonnementen met verschillende valuta's hebt, een bovenliggende entiteit en de onderliggende entiteit valuta's worden weergegeven met de  **$**  symbool. Onze voorgestelde aanbevolen procedure is om te voorkomen dat met behulp van verschillende valuta's in de entiteitshiërarchie met dezelfde. Met andere woorden, moeten uw abonnementen die zijn ingedeeld in de structuur van een entiteit dezelfde valuta gebruiken.

Cloudyn automatisch detecteert de Enterprise Agreement abonnementsvaluta en goed geeft in rapporten.  Echter, Cloudyn alleen wordt de  **$**  symbool voor de CSP en web-directe Azure-accounts.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Wat zijn de gegevens Cloudyn tijdlijnen vernieuwen?

Cloudyn heeft de volgende gegevens vernieuwen tijdlijnen:

- **Initiële**: na het instellen van deze kostengegevens weergeven in Cloudyn tot 24 uur kan duren. Het kan maximaal 10 dagen voor Cloudyn voor het verzamelen van voldoende gegevens om weer te geven sizing aanbevelingen ook duren.
- **Dagelijkse**: van de tien dagen aan het einde van elke maand Cloudyn moet de gegevens worden weergegeven up-to-date te houden van de vorige dag na over de UTC + 3 de volgende dag.
- **Maandelijkse**: vanaf de eerste dag op de tien dag van elke maand Cloudyn uw gegevens alleen tot aan het einde van de vorige maand kan weergeven.

Gegevens verwerkt Cloudyn voor de vorige dag als volledige gegevens van de vorige dag beschikbaar is. De vorige dag gegevens zijn gewoonlijk beschikbaar in de Cloudyn door over UTC + 3 elke dag. Sommige gegevens, zoals tags, kan verwerken nog 24 uur duren.

Gegevens voor de huidige maand zijn niet beschikbaar voor de verzameling aan het begin van elke maand. Serviceproviders voltooien tijdens de periode hun facturering voor de vorige maand. De vorige maand gegevens weergegeven in Cloudyn 5 tot 10 dagen na het begin van elke maand. Gedurende deze tijd ziet u mogelijk alleen afgeschreven kosten van de vorige maand. U ziet misschien niet dagelijks facturering of gebruik van gegevens. Wanneer de gegevens beschikbaar, verwerkt Cloudyn het met terugwerkende kracht. Na de verwerking wordt de maandelijkse gegevens tussen de vijfde dag en de tien dag van elke maand weergegeven.

Als er een vertraging optreden bij het verzenden van gegevens van Azure naar Cloudyn, is nog steeds gegevens geregistreerd in Azure. De gegevens worden overgedragen naar Cloudyn wanneer de verbinding is hersteld.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Hoe kan een directe CSP Cloudyn toegang configureert voor indirecte CSP klanten of partners

Zie voor instructies [indirecte CSP toegang configureren in Cloudyn](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Wat de oorzaak van het optimalisatie van menu-item moet worden weergegeven?

Nadat u hebt toegevoegd Azure Resource Manager-toegang en gegevens worden verzameld, ziet u de **optimaliseren** optie. Zie voor het activeren van Azure Resource Manager toegang [hoe activeren niet-geactiveerde accounts met Azure-referenties?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cost-managementcloudyn-agent-based"></a>Is agent kosten Management/Cloudyn gebaseerd?

Nee. Agents worden niet gebruikt. Metrische gegevens van de virtuele machine van Azure voor virtuele machines zijn verzameld vanaf de inzicht API van Microsoft. Als u verzamelen van metrische gegevens van Azure Virtual machines wilt, moeten ze diagnostische instellingen is ingeschakeld.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Kunnen Cloudyn rapporten meer dan één AD-tenant per rapport weergeven?

Ja. U kunt [een overeenkomstige entiteit voor de cloud-account maken](tutorial-user-access.md#create-entities) voor elk AD-tenant die u hebt. U kunt vervolgens alle van de gegevens van uw Azure AD-tenant en andere platform cloudproviders zoals Amazon Web Services en Google Cloud Platform weergeven.
