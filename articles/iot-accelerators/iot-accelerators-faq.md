---
title: IoT-oplossingsversnellers FAQ - Azure | Microsoft Docs
description: Veelgestelde vragen over IoT-oplossingsversnellers
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 59271a96c5ad1a92483ca585fc30f1e9de0ed4f7
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608746"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Veelgestelde vragen over IoT-oplossingsversnellers

Zie ook de [verbonden Factory-specifieke Veelgestelde vragen over](iot-accelerators-faq-cf.md) en de [Veelgestelde vragen over externe bewaking-specifieke](iot-accelerators-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Waar vind ik de broncode voor de oplossingsversnellers?

De broncode wordt opgeslagen in de volgende GitHub-opslagplaatsen:

* [Oplossingsverbetering voor externe bewaking (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Oplossingsverbetering voor externe bewaking (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Oplossingsverbetering voor predictief onderhoud](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Verbonden Factory-oplossingsversnellers](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Welke SDK's kan ik gebruiken voor het ontwikkelen van apparaatclients voor de oplossingsversnellers?

Vindt u koppelingen naar de andere taal (C, .NET, Java, Node.js, Python) IoT device SDK's in de [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub-opslagplaatsen.

Als u het apparaat DevKit, vindt u bronnen en voorbeelden uit de [IoT DevKit SDK](https://github.com/Microsoft/devkit-sdk) GitHub-opslagplaats.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Is de nieuwe architectuur met microservices beschikbaar voor alle drie de oplossingsversnellers?

Op dit moment alleen de oplossing voor externe controle maakt gebruik van de architectuur met microservices omdat hierin de meest uiteenlopende scenario.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welke voordelen biedt de nieuwe open source op basis van microservices-architectuur in de nieuwe update?

Architectuur van de afgelopen twee jaar aanzienlijk zich heeft ontwikkeld. Microservices hebben naar voren gekomen als een geweldige patroon om de schaal en flexibiliteit, zonder dat dit ten koste gaat ontwikkelingssnelheid bereiken. Dit architectuurpatroon wordt gebruikt in verschillende Microsoft-services die intern met geweldige betrouwbaarheid en schaalbaarheid resultaten. Microsoft is deze geleerde lessen in de praktijk in de oplossingsversnellers plaatsen, zodat klanten van deze profiteren.

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-azure-ad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil wijzigen van de directory-toewijzing tussen mijn abonnement en een specifieke Azure AD-tenant. Hoe ik deze taak voltooid?

Zie [aan een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Ik wil een servicebeheerder of CO-beheerder wanneer u bent aangemeld met een organisatie-account wijzigen

Zie het ondersteuningsartikel [wijzigen van servicebeheerder en Medebeheerder wanneer u bent aangemeld met een organisatieaccount](https://azure.microsoft.com/support/changing-service-admin-and-co-admin).

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom krijg ik deze fout te zien? 'Uw account beschikt niet over de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account."

Bekijk het volgende diagram voor richtlijnen:

![Stroomdiagram van machtigingen](media/iot-accelerators-faq/flowchart.png)

> [!NOTE]
> Als u een foutbericht weergegeven na de validatie blijft u een globale beheerder van de Azure AD-tenant en een CO-beheerder van het abonnement zijn, de beheerder van uw account de gebruiker te verwijderen en opnieuw toewijzen van benodigde machtiging in deze volgorde. Eerst de gebruiker als een globale beheerder toevoegen en voeg deze gebruiker als een CO-beheerder van de Azure-abonnement. Als de problemen zich blijven voordoen, neem dan contact op met [Help en ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Waarom krijg ik deze fout zien wanneer ik heb een Azure-abonnement? "Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefaccount maken in een paar minuten."

Als u zeker weet dat u hebt een Azure-abonnement, de tenanttoewijzing voor uw abonnement te valideren en controleer of de juiste tenant is geselecteerd in de vervolgkeuzelijst. Als u hebt gevalideerd de tenant juist is, volgt u de voorgaande diagram en valideren van de toewijzing van uw abonnement en deze Azure AD-tenant.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Waar vind ik informatie over de vorige versie van de oplossing voor externe controle

De vorige versie van de oplossingsverbetering voor externe controle was bekend als de IoT Suite vooraf geconfigureerde oplossing voor externe controle. U vindt de gearchiveerde documentatie op [ https://docs.microsoft.com/previous-versions/azure/iot-suite/ ](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Nieuwe solution accelerator is beschikbaar in dezelfde geografische regio als de bestaande oplossing?

Ja, de nieuwe externe controle is beschikbaar in dezelfde geografische regio's.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in Azure portal en te klikken op verwijderen bij een oplossingsversnellers op azureiotsuite.com?

* Als u de oplossingsversneller in verwijdert [azureiotsuite.com](https://www.azureiotsolutions.com/), verwijdert u alle resources die zijn geïmplementeerd tijdens het maken van de solution accelerator. Als u aanvullende resources toegevoegd aan de resourcegroep, worden deze resources worden ook verwijderd.
* Als u verwijdert de resourcegroep in de [Azure-portal](https://portal.azure.com), verwijdert u alleen de resources in die resourcegroep. U moet ook de Azure Active Directory-toepassing die is gekoppeld aan de solution accelerator verwijderen.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Kan ik dan blijven om mijn bestaande investeringen in Azure IoT-oplossingsversnellers?

Ja. Een oplossing die vandaag de dag blijft werken in uw Azure-abonnement en de broncode blijft beschikbaar is in GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van de IoT Hub kan ik inrichten in een abonnement?

Standaard kunt u inrichten [10 IoT-hubs per abonnement](../azure-subscription-service-limits.md#iot-hub-limits). U kunt maken een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om deze limiet te verhogen. Als gevolg hiervan, kunt aangezien elke oplossing accelerator levert een nieuwe IoT-Hub, u alleen inrichten tot 10 oplossingsversnellers in een bepaald abonnement.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van Azure Cosmos DB kan ik inrichten in een abonnement?

Vijftig. U kunt maken een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om deze limiet te verhogen, maar standaard kunt u alleen 50 Cosmos DB-exemplaren per abonnement inrichten.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise-abonnementen maken in een Azure-abonnement. De oplossing voor externe controle is standaard ingericht met het interne transacties Level-1-plan. Als gevolg hiervan kunt u alleen maximaal twee externe controle-oplossingen in een abonnement zonder wijzigingen inrichten.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een oplossingsversnellers maken als ik Microsoft Azure voor DreamSpark heb?

> [!NOTE]
> Microsoft Azure voor DreamSpark is nu bekend als Microsoft Imagine voor studenten.

U kunt geen op dit moment een oplossingsversnellers met maken een [Microsoft Azure voor DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) account. U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) maken in een paar minuten waarmee u kunt een oplossingsversnellers.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Hoe verwijder ik een Azure AD-tenant?

Zie het blogbericht van Eric Golpe van [overzicht van het verwijderen van een Azure AD-Tenant](https://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de IoT-oplossingsversnellers bekijken:

* [Verken de mogelijkheden van de oplossingsverbetering voor externe controle](quickstart-remote-monitoring-deploy.md)
* [Een overzicht van de oplossingsversneller voor Predictief onderhoud](iot-accelerators-predictive-overview.md)
* [Verbonden Factory oplossingsversnellers implementeren](quickstart-connected-factory-deploy.md)
* [Fundamentele IoT-beveiliging](/azure/iot-fundamentals/iot-security-ground-up)
