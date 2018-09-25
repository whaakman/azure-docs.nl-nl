---
title: Veelgestelde vragen over Cloudyn in Azure | Microsoft Docs
description: Vindt u antwoorden op enkele veelgestelde vragen over Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: troubleshooting
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 2126875a18d4a6581629ea1c8362236242a666a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961022"
---
# <a name="frequently-asked-questions-for-cloudyn"></a>Veelgestelde vragen over Cloudyn

In dit artikel komen enkele veelgestelde vragen over Cloudyn. Als u vragen over Cloudyn hebt, kunt u vragen deze [Veelgestelde vragen over de Cloudyn](https://social.msdn.microsoft.com/Forums/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Hoe kan ik problemen met het algemene indirecte enterprise instellen oplossen?

Wanneer u de Cloudyn-portal voor het eerst gebruikt, ziet u mogelijk de volgende berichten als u een Enterprise Agreement of een Cloud Solution Provider (CSP) gebruiker:

- 'De opgegeven API-sleutel is niet een sleutel van het bovenste niveau inschrijving' weergegeven in de **Cloudyn ingesteld** wizard.
- 'Directe inschrijving: geen' weergegeven in de Enterprise Agreement-portal.
- "Er zijn geen gebruiksgegevens is gevonden voor de afgelopen 30 dagen. Neem contact op met uw leverancier om te controleren of de opmaak is ingeschakeld voor uw Azure-account' weergegeven in de Cloudyn-portal.

De voorgaande berichten geven aan dat u een Azure Enterprise Agreement via een wederverkoper of CSP hebt aangeschaft. Uw wederverkoper of CSP nodig heeft om in te schakelen _markup_ voor uw Azure-account zodat u uw gegevens kunt weergeven in Cloudyn.

Los de problemen als volgt op:

1. Uw wederverkoper moet _markeren_ voor uw account inschakelen. Raadpleeg de [Onboarding-handleiding voor indirecte klanten](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide) voor meer instructies.

2. U de Azure Enterprise overeenkomst-sleutel voor gebruik met Cloudyn genereren. Zie voor instructies [toevoegen van uw Azure EA](https://support.cloudyn.com/hc/articles/210429585-Adding-Your-AZURE-EA) of [vinden Your EA Enrollment ID en API-sleutel](https://youtu.be/u_phLs_udig).

Alleen een Azure-service-beheerder kunt Cloudyn inschakelen. U hebt onvoldoende co-beheerdersmachtigingen.

Voordat u de Azure Enterprise overeenkomst-API-sleutel voor het instellen van Cloudyn genereren kunt, moet u de Azure Billing API inschakelen door de instructies op:

- [Overzicht van Rapportage-API's voor Enterprise-klanten](../billing/billing-enterprise-api.md)
- [Microsoft Azure enterprise portal rapportage-API](https://ea.azure.com/helpdocs/reportingAPI) onder **Gegevenstoegang tot de API inschakelen**


U moet wellicht ook afdelingsbeheerders, accounteigenaars en enterprise-administrators machtigingen geven voor _Kosten weergeven_ met de facturerings-API.

## <a name="why-dont-i-see-optimizer-recommendations"></a>Waarom zie ik geen aanbevelingen optimaliseren?

Aanbeveling informatie is alleen beschikbaar voor accounts die worden geactiveerd. Ziet u niet alle informatie aanbeveling in **Optimizer** categorieën voor accounts die zijn gerapporteerd *unactivated*, waaronder:

- Optimalisatie van Manager
- Sizing Optimization
- Inefficiëntie

Als u niet alle gegevens van de aanbeveling Optimizer weergeven, klikt u vervolgens hebt waarschijnlijk u accounts die unactivated zijn. Voor het activeren van een account dat u wilt registreren bij uw Azure-referenties.

Een account te activeren:

1.  Klik in de rechterbovenhoek van de portal van Cloudyn op **Settings** en selecteer **Cloud Accounts**.
2.  Zoek op het tabblad Microsoft Azure-Accounts accounts waarvoor een **unactivated** abonnement.
3.  Aan de rechterkant van een unactivated account, klikt u op de **bewerken** symbool dat lijkt op een potlood.
4.  Uw tenant-ID en tarief-ID wordt automatisch gedetecteerd. Klik op **Volgende**.
5.  U bent omgeleid naar de Azure-portal. Aanmelden bij de portal en autoriseren van Cloudyn Collector voor toegang tot uw Azure-gegevens.
6.  Vervolgens wordt u doorgestuurd naar de pagina van de Cloudyn-Accounts beheren en uw abonnement wordt bijgewerkt met **active** accountstatus. U ziet een groen vinkje symbool.
7.  Als u niet een groen vinkje voor een of meer van de abonnementen ziet, betekent dit dat u bent niet gemachtigd om een reader-app (de CloudynCollector) voor het abonnement te maken. Herhaal stap 3 en 4 moet een gebruiker met hogere machtigingen voor het abonnement.  

Nadat u de voorgaande stappen hebt voltooid, kunt u optimaliseren aanbevelingen weergeven binnen één tot twee dagen. Echter, duurt het tot vijf dagen voordat de optimalisatie van de volledige gegevens beschikbaar is.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Hoe schakel ik onderbroken of uitgesloten gebruikers?

Eerst laten we bekijken met de meest gebruikte scenario dat ervoor zorgt dat de accounts voor gebruikers om op te halen *initiallySuspended*.

> Admin1 mogelijk Microsoft Cloud Solution Provider- of Enterprise Agreement-gebruiker. De organisatie is klaar om te beginnen met behulp van Cloudyn.  Hij wordt geregistreerd via Azure portal en zich bij de Cloudyn-portal aanmeldt. Als de persoon die de Cloudyn-service en zich bij de Cloudyn-portal registreert, wordt hij de *primaire beheerder*. Admin1 maakt niet alle gebruikersaccounts. Echter met behulp van de Cloudyn-portal, hij maakt Azure-accounts en stelt u een hiërarchie voor entiteitskosten. Admin1 informeert Admin2, tenantbeheerder die hij nodig heeft om te registreren bij Cloudyn en meld u aan bij de Cloudyn-portal.

> Admin2 wordt geregistreerd via Azure portal. Echter wanneer hij wil zich aanmelden bij de Cloudyn-portal, hij krijgt een foutmelding met de mededeling zijn account is **onderbroken**. De primaire Admin1,-beheerder is op de hoogte gesteld van de account-onderbreking. Admin1 moet Admin2 van account activeren en verleen *entiteit beheerderstoegang* voor de juiste entiteiten en kunt u toegang tot de gebruiker en actieve account van de gebruiker.


Als u een waarschuwing met een verzoek om toegang te verlenen voor een gebruiker ontvangt, moet u het gebruikersaccount activeren.

Het gebruikersaccount activeren:

1. Meld u aan Cloudyn met behulp van het gebruikersaccount voor Azure met beheerdersrechten die u hebt gebruikt voor het instellen van Cloudyn. Of meld u aan met een gebruikersaccount dat toegang heeft gekregen.
2. Selecteer het tandwielsymbool in de rechterbovenhoek en selecteer **Gebruikersbeheer**.
3. De gebruiker niet vinden, selecteert u het potloodsymbool en bewerk vervolgens de gebruiker.
4. Onder **gebruikersstatus**, wijzig de status van **onderbroken** naar **Active**.

Gebruikersaccounts van Cloudyn verbinding maken met behulp van eenmalige aanmelding van Azure. Als een gebruiker het wachtwoord mistypes, ze mogelijk toegang meer hebt tot Cloudyn, zelfs als ze nog steeds toegang Azure tot.

Als u uw e-mailadres in Cloudyn van het standaard-adres in Azure wijzigt, kan uw account ophalen vergrendeld. Deze kan worden 'status initiallySuspended.' weergeven Als uw account is vergrendeld, neem dan contact op met een andere beheerder als u uw account opnieuw wilt.

U wordt aangeraden dat u ten minste twee Cloudyn administrator-accounts maken als een van de accounts is vergrendeld.

Als u niet bij de Cloudyn-portal aanmelden, zorgt u ervoor dat u de correcte URL gebruikt voor aanmelding bij Cloudyn. Gebruik [ https://azure.cloudyn.com ](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade).

Vermijd het gebruik van de directe URL Cloudyn https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Hoe kan ik unactivated accounts met Azure-referenties activeren?

Als uw Azure-accounts worden gedetecteerd door Cloudyn, gegevens van cost onmiddellijk vindt u in rapporten op basis van kosten. Voor Cloudyn om gegevens van gebruik en de prestaties te bieden, moet u echter uw Azure-referenties voor de accounts registreren. Zie voor instructies [toevoegen Azure Resource Manager](https://support.cloudyn.com/hc/articles/212784085-Adding-Azure-Resource-Manager).

Als u wilt toevoegen voor een account, Azure-referenties in de Cloudyn-portal, selecteert u het symbool bewerken aan de rechterkant van de accountnaam, niet het abonnement.

Als uw Azure-referenties aan Cloudyn zijn toegevoegd, het account wordt weergegeven als _niet geactiveerd_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Hoe ik meerdere accounts en entiteiten toevoegen aan een bestaand abonnement?

Als u meer entiteiten worden gebruikt om aanvullende Enterprise overeenkomsten toevoegen aan een Cloudyn-abonnement. De volgende koppelingen beschrijven aanvullende entiteiten toevoegen:

- [Toevoegen van een entiteit](https://support.cloudyn.com/hc/articles/212016145-Adding-an-Entity) artikel
- [Uw hiërarchie met Kostenentiteiten definieert](https://support.cloudyn.com/hc/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) video

Voor CSP's:

Selecteer aanvullende CSP-accounts toevoegen aan een entiteit, **MSP toegang** in plaats van **Enterprise** wanneer u de nieuwe entiteit maakt. Als uw account is geregistreerd als een Enterprise Agreement, en u wilt toevoegen van de CSP-referenties, mogelijk ondersteunend personeel van Cloudyn om de accountinstellingen van uw te wijzigen. Als u een betaald Azure-abonnee bent, kunt u een nieuwe ondersteuningsaanvraag maken in Azure portal. Selecteer **Help en ondersteuning**, en selecteer vervolgens **nieuwe ondersteuningsaanvraag**.

## <a name="currency-symbols-in-cloudyn-reports"></a>Valutasymbolen in Cloudyn-rapporten

Mogelijk hebt u meerdere Azure-accounts met behulp van verschillende valuta's. Kostenrapporten in Cloudyn worden echter niet meer dan één Valutatype per rapport weergegeven.

Als u meerdere abonnementen met behulp van verschillende valuta's hebt, een bovenliggende entiteit en de onderliggende entiteit valuta's worden weergegeven in USD **$**. Onze voorgestelde aanbevolen procedure is om te voorkomen dat met behulp van verschillende valuta's in de entiteitshiërarchie van dezelfde. Al uw abonnementen ingedeeld in de entiteitsstructuur van een moeten met andere woorden, gebruikt u dezelfde valuta.

Cloudyn automatisch detecteert de valuta van uw Enterprise Agreement-abonnement en geeft deze juist in rapporten.  Cloudyn wordt echter alleen USD **$** voor CSP en Azure web direct-accounts.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Wat zijn de gegevens Cloudyn tijdlijnen vernieuwen?

Cloudyn heeft de volgende gegevens vernieuwen tijdlijnen:

- **Eerste**: nadat u ingesteld, duurt het tot 24 uur om gegevens van cost in Cloudyn weer te geven. Het kan ook voor Cloudyn voor het verzamelen van voldoende gegevens om schaling aanbevelingen weer te geven tot tien dagen duren.
- **Dagelijkse**: van de tiende aan het einde van elke maand, Cloudyn moet worden weergegeven uw gegevens actueel te houden van de vorige dag na over UTC + 3 de volgende dag.
- **Maandelijkse**: van de eerste dag tot de tiende dag van elke maand, Cloudyn uw gegevens alleen via het einde van de vorige maand kan weergeven.

Cloudyn verwerkt de gegevens voor de vorige dag waarop de volledige gegevens uit de vorige dag beschikbaar is. Gegevens van de vorige dag zijn gewoonlijk beschikbaar in Cloudyn door over UTC + 3 elke dag. Sommige gegevens, zoals tags, kan een extra 24 uur voor het verwerken van duren.

Gegevens voor de huidige maand niet beschikbaar is voor de verzameling aan het begin van elke maand. Serviceproviders voltooien tijdens de periode, de facturering voor de vorige maand. Gegevens van de vorige maand wordt weergegeven in Cloudyn 5 tot 10 dagen na het begin van elke maand. Gedurende deze tijd ziet u mogelijk alleen de afgeschreven kosten van de vorige maand. U ziet mogelijk niet dagelijks facturering of gebruik gegevens. Wanneer de gegevens beschikbaar komt, verwerkt Cloudyn het met terugwerkende kracht. Na de verwerking, wordt de maandelijkse gegevens tussen de vijfde dag en de tiende dag van elke maand weergegeven.

Als er een vertraging optreden bij het verzenden van gegevens van Azure naar Cloudyn, is nog steeds gegevens geregistreerd in Azure. De gegevens worden overgedragen aan Cloudyn wanneer de verbinding wordt hersteld.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Kosten schommelingen in Kostenrapporten van Cloudyn

Kostenrapporten kunnen fluctuaties in de kosten weergeven wanneer cloudserviceproviders bijgewerkte facturering bestanden verzenden. Fluctuerend kosten optreden wanneer er nieuwe bestanden worden ontvangen van een cloudserviceprovider buiten de normale dagelijks of maandelijks schema reporting. Kosten wijzigingen niet worden veroorzaakt door Cloudyn herberekening.

Gedurende de maand zijn alle facturering bestanden die zijn verzonden door uw cloudserviceprovider een schatting van uw dagelijkse kosten. Soms gegevens regelmatig wordt bijgewerkt, soms meerdere keren per dag. Er zijn updates vaker met AWS dan Azure. Totaal blijft stabiel wanneer het berekenen van facturering voor de vorige maand voltooid is en de uiteindelijke facturering bestand wordt ontvangen. Normaal gesproken door de 10e van de maand.

Wijzigingen optreden wanneer u aanpassingen van de kosten van uw cloudserviceprovider ontvangt. Tegoed ontvangen is een voorbeeld. Wijzigingen kunnen maanden optreden nadat de desbetreffende maand is gesloten. Wijzigingen worden weergegeven wanneer een herberekening wordt gemaakt door uw cloudserviceprovider. Cloudyn-updates de historische gegevens om ervoor te zorgen dat alle aanpassingen worden herberekend. Hij controleert ook dat kosten nauwkeurig worden weergegeven in het rapport.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Hoe kan een directe CSP Cloudyn-toegang configureren voor indirecte CSP-klanten of partners?

Zie voor instructies [indirecte CSP-toegang in Cloudyn configureren](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Wat de oorzaak van het optimalisatieprogramma menu-item wordt weergegeven?

Nadat u Azure Resource Manager-toegang en gegevens hebt toegevoegd, wordt verzameld, ziet u de **Optimizer** optie. Zie voor het activeren van Azure Resource Manager-toegang, [hoe activeren unactivated accounts met Azure-referenties?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cloudyn-agent-based"></a>Is de Cloudyn-agent gebaseerd?

Nee. Agents worden niet gebruikt. Metrische gegevens van de virtuele machine van Azure voor virtuele machines zijn verzameld uit de Microsoft Insights-API. Als u verzamelen van metrische gegevens van virtuele Azure-machines wilt, moeten deze diagnostische instellingen zijn ingeschakeld.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Kunnen de Cloudyn-rapporten weergeven per rapport meer dan één AD-tenant?

Ja. U kunt [maken van een bijbehorende cloud-accountentiteit](tutorial-user-access.md#create-and-manage-entities) voor elk AD-tenant die u hebt. U kunt vervolgens alle gegevens van uw Azure AD-tenant en andere cloudproviders platform, waaronder Amazon-webservices en Google Cloud Platform weergeven.
