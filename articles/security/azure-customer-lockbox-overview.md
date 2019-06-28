---
title: Klanten-Lockbox voor Microsoft Azure
description: Technisch overzicht van klanten-Lockbox voor Microsoft Azure, waarmee u controle over de cloud-provider toegang als Microsoft mogelijk toegang hebben tot klantgegevens.
author: cabailey
ms.service: security
ms.topic: article
ms.author: cabailey
manager: barbkess
ms.date: 06/20/2019
ms.openlocfilehash: 0ee2dde5a941d069f5b745eafb35df780f657a47
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312626"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Klanten-Lockbox voor Microsoft Azure

> [!NOTE]
> Deze functie wilt gebruiken, uw organisatie moet beschikken over een [Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/) met een minimumniveau aan **Developer**.

Klanten-Lockbox voor Microsoft Azure biedt een interface voor klanten om te controleren en goedkeuren of afwijzen van aanvragen voor de klant. Het wordt gebruikt in gevallen waarin een Microsoft-technicus moet toegang hebben tot klantgegevens tijdens een ondersteuningsaanvraag.

In dit artikel bevat informatie over hoe klanten-Lockbox aanvragen worden geïnitieerd, bijgehouden en opgeslagen voor latere beoordelingen en controles.

Klanten-Lockbox is nu algemeen beschikbaar en is momenteel ingeschakeld voor externe bureaublad toegang tot virtuele machines.

## <a name="workflow"></a>Werkstroom

De volgende stappen beschrijven een gebruikelijke werkstroom voor een klant Lockbox-aanvraag.

1. Iemand in een organisatie heeft een probleem met hun Azure-workload.

2. Nadat deze persoon Hiermee lost u het probleem, maar niet worden opgelost, ze opent u een ondersteuningsticket vanuit de [Azure-portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). Het ticket is toegewezen aan een Customer Support Engineer van Azure.

3. Een Azure-ondersteuningstechnicus controleert de serviceaanvraag en bepaalt van de volgende stappen uit om het probleem te verhelpen.

4. Als de ondersteuningstechnicus kan niet het probleem oplossen met behulp van hulpprogramma's voor standard en telemetrie, wordt de volgende stap is het verhoogde machtigingen aanvragen met behulp van een access-service van JIT (Just in time). Deze aanvraag kan afkomstig zijn uit de oorspronkelijke ondersteuningstechnicus. Of het kan zijn van een andere engineer omdat het probleem is geëscaleerd naar het Azure DevOps-team.

5. Nadat de toegang tot de aanvraag is verzonden door de Azure-Engineer, Just-In-Time-service beoordeelt wat de aanvraag houdend met account factoren zoals:
    - Het bereik van de resource
    - Of de aanvrager is een identiteit die is geïsoleerd of met multi-factor authentication
    - Machtigingsniveaus
    
    Op basis van de JIT-regel, kan deze aanvraag ook een goedkeuring van interne Microsoft goedkeurders. De fiatteur zijn mogelijk de potentiële klant ondersteuning of de DevOps-Manager.

6. Wanneer de aanvraag nodig heeft rechtstreeks toegang tot gegevens van de klant, wordt een klant Lockbox-aanvraag wordt geïnitieerd. Bijvoorbeeld, externe bureaublad toegang tot een klant virtuele machine.
    
    De aanvraag is nu in een **klant op de hoogte gesteld** staat, wachten op goedkeuring van de klant voordat u toegang verleent.

7. Op de organisatie van de klant, de gebruiker die heeft de [rol van eigenaar](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles) voor de Azure abonnement ontvangt een e-mailbericht van Microsoft, waarmee u ze hierover te informeren over de aanvraag in behandeling zijnde. Voor aanvragen van klanten-Lockbox is deze persoon de aangewezen fiatteur.
    
    Voorbeeld van de e-mailadres:
    
    ![Azure klanten-Lockbox - e-mailmelding](./media/azure-customer-lockbox/customer-lockbox-email-notification.png)

8. Het e-mailbericht bevat een koppeling naar de **klanten-Lockbox** blade in Azure portal. Via deze koppeling, zich de aangewezen fiatteur aanmeldt bij Azure portal om een in behandeling zijnde aanvragen die de organisatie voor klanten-Lockbox heeft weer te geven:
    
    ![Azure klanten-Lockbox - landingspagina](./media/azure-customer-lockbox/customer-lockbox-landing-page.png)
    
   De aanvraag blijft in de wachtrij van de klant van vier dagen. Na deze tijd de aanvraag voor toegang tot automatisch verlopen en geen toegang te krijgen tot Microsoft-technici.

9. Als u de details van de aanvraag in behandeling, de aangewezen fiatteur de aanvraag voor lockbox kunt selecteren **in behandeling genomen aanvragen**:
    
    ![Azure klanten-Lockbox - de aanvraag in behandeling weergeven](./media/azure-customer-lockbox/customer-lockbox-pending-requests.png)

10. De aangewezen fiatteur kunt ook selecteren de **SERVICEAANVRAAG-ID** om de aanvraag voor het ticket van ondersteuning die is gemaakt door de oorspronkelijke gebruiker weer te geven. Deze informatie geeft context voor waarom Microsoft Support is geboeid en de geschiedenis van het gerapporteerde probleem. Bijvoorbeeld:
    
    ![Azure klanten-Lockbox - de ondersteuningsaanvraag ticket weergeven](./media/azure-customer-lockbox/customer-lockbox-support-ticket.png)

11. Bekijk de aanvraag en selecteert u de aangewezen fiatteur **goedkeuren** of **weigeren**:
    
    ![Azure-klant Lockbox - goedkeuren of weigeren](./media/azure-customer-lockbox/customer-lockbox-approval.png)
    
    Als gevolg van de selectie van:
    - **Goedkeuren**:  Toegang te krijgen tot de Microsoft-technicus. De toegang is verleend aan een standaardperiode van acht uur.
    - **Weigeren**: De aanvraag voor toegang met verhoogde bevoegdheid op de Microsoft-technicus wordt afgewezen en er geen verdere actie wordt uitgevoerd.

Voor controledoeleinden, de acties die in deze werkstroom worden vastgelegd in [klanten-Lockbox aanvraag logboeken](#auditing-logs).

## <a name="auditing-logs"></a>Er is logboekcontrole

Klanten-Lockbox logboeken worden opgeslagen in de activiteitenlogboeken. Selecteer in de Azure portal, **activiteitenlogboeken** om controle-informatie met betrekking tot aanvragen van klanten-Lockbox weer te geven. U kunt filteren op specifieke acties, zoals:
- **Lockbox aanvraag weigeren**
- **Lockbox-aanvraag maken**
- **Lockbox aanvraag goedkeuren**
- **Lockbox aanvraag verlopen**

Als u een voorbeeld:

![Azure klanten-Lockbox - activiteitenlogboeken](./media/azure-customer-lockbox/customer-lockbox-activitylogs.png)

## <a name="supported-services-and-scenarios"></a>Ondersteunde services en scenario 's

De volgende scenario's en services zijn momenteel algemeen beschikbaar voor klanten-Lockbox.

### <a name="remote-desktop-access-to-virtual-machines"></a>Externe bureaublad toegang tot virtuele machines

Klanten-Lockbox is momenteel ingeschakeld voor toegang tot extern bureaublad-aanvragen voor virtuele machines. De volgende werkbelastingen worden ondersteund:
- Platform als een service (PaaS) - Azure Cloud Services (Webrol en werkrol)
- Infrastructuur als een dienst (IaaS) - Windows- en Linux (alleen voor Azure Resource Manager)
- Virtuele-machineschaalset - Windows- en Linux

> [!NOTE]
> Klassieke versie van IaaS-exemplaren worden niet ondersteund door klanten-Lockbox. Als u workloads die worden uitgevoerd op IaaS Classic-exemplaren hebt, raden wij dat u deze migreren van klassiek naar Resource Manager-implementatiemodel. Zie voor instructies [Platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Gedetailleerde auditlogboeken

Voor scenario's voor extern bureaublad-toegang, kunt u Windows-gebeurtenislogboeken gebruiken om te controleren van de acties die door de Microsoft-technicus. Overweeg het gebruik van Azure Security Center voor het verzamelen van gebeurtenislogboeken en kopieer de gegevens naar uw werkruimte voor analyse. Zie voor meer informatie, [verzamelen van gegevens in Azure Security Center](../security-center/security-center-enable-data-collection.md).

## <a name="exclusions"></a>Uitzonderingen

Aanvragen van klanten-Lockbox niet worden geactiveerd in de volgende technische ondersteuning voor scenario's:

- Een Microsoft-technicus moet doen van een activiteit die buiten standaardwerkwijzen valt. Als u bijvoorbeeld wilt herstellen of herstellen van services in scenario's onverwachte of onvoorspelbaar.

- Een Microsoft-technicus toegang heeft tot het Azure-platform als onderdeel van het oplossen van problemen en per ongeluk heeft toegang tot klantgegevens. Het Azure-netwerk-Team voert bijvoorbeeld problemen die in een pakketopname op een netwerkapparaat resulteert. Als de klant de gegevens worden versleuteld terwijl deze tijdens de overdracht was, kan de engineer echter de gegevens lezen.

## <a name="next-steps"></a>Volgende stappen

Klanten-Lockbox is automatisch beschikbaar voor alle klanten met een [Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/) met een minimumniveau aan **Developer**.

Wanneer u een in aanmerking komende ondersteuningsplan hebt, is door u geen actie vereist om in te schakelen van klanten-Lockbox. Aanvragen van klanten-Lockbox zijn gestart door een Microsoft-technicus, als deze actie is vereist voor de voortgang van een ondersteuningsticket die is ingediend door iemand binnen uw organisatie.
