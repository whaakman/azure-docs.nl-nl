---
title: Azure Security Center-beveiligingsbeleid kunnen worden ingesteld als onderdeel van Azure-beleid en bekeken in Security Center | Microsoft Docs
description: Dit document helpt u beleid instellen in Azure Policy of ze weergeven in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/25/2018
ms.author: rkarlin
ms.openlocfilehash: 330b66e64484417e50f39c35cf90a6fd62b1e888
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334666"
---
# <a name="view-security-policies"></a>Weergave-beveiligingsbeleid

In dit artikel wordt uitgelegd hoe beleidsregels voor veiligheid zijn geconfigureerd en hoe ze weergeven in Security Center. Azure Security Center wordt automatisch toegewezen de [ingebouwde beveiligingsbeleid](security-center-policy-definitions.md) voor elk abonnement dat is geïmplementeerd. U kunt configureren dat ze in [Azure Policy](../azure-policy/azure-policy-introduction.md), die u kunt ook beleidsregels binnen beheergroepen en voor meerdere abonnementen.

Zie voor instructies over het instellen van beleidsregels met behulp van PowerShell [Snelstartgids: een beleidstoewijzing maken om te identificeren van niet-compatibele resources met behulp van de Azure DB PowerShell-module](../azure-policy/assign-policy-definition-ps.md).



## <a name="what-are-security-policies"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Azure Policy, kunt u beleidsregels definiëren voor uw Azure-abonnementen en past u dit aan uw type workload of de vertrouwelijkheid van uw gegevens. Toepassingen met gereglementeerde gegevens, zoals persoonsgegevens, kunnen bijvoorbeeld vereisen dat een hoger beveiligingsniveau dan andere werkbelastingen. Als u wilt een beleid instellen voor abonnementen of beheergroepen, stelt u deze [Azure Policy](../azure-policy/azure-policy-introduction.md).



Uw beveiligingsbeleid de aanbevelingen voor beveiliging die in Azure Security Center u krijgt het station. Naleving van deze kunt u mogelijke beveiligingsproblemen te identificeren en bedreigingen te verhelpen, kunt u controleren. Zie voor meer informatie over hoe om te bepalen welke optie geschikt is voor u is, de lijst met [ingebouwde beveiligingsbeleid](security-center-policy-definitions.md).

### <a name="management-groups"></a>Beheergroepen
Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Azure Beheergroepen biedt een scopeniveau boven abonnementen. U organiseert abonnementen in containers, zogenaamde 'beheergroepen', en past uw governance-beleid op de beheergroepen toe. Alle abonnementen in een beheergroep nemen automatisch het beleid over dat op de beheergroep is toegepast. Elke directory krijgt één beheergroep op het hoogste niveau, de 'hoofdbeheergroep'. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. Met deze hoofdbeheergroep kunt u algemene beleidsregels en RBAC-toewijzingen op directoryniveau toepassen. Als u beheergroepen voor gebruik met Azure Security Center instelt, volg de instructies in [tenant-brede inzicht voor Azure Security Center](security-center-management-groups.md).

> [!NOTE]
> Het is belangrijk dat u de hiërarchie van beheergroepen en abonnementen begrijpt. Zie [Organize your resources with Azure Management Groups](../governance/management-groups/index.md#root-management-group-for-each-directory) (Resources organiseren met Azure Beheergroepen) voor meer informatie over beheergroepen, hoofdbeheer en toegang tot beheergroepen.
>

## <a name="how-security-policies-work"></a>Hoe beveiligingsbeleid werkt
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid in Azure-beleid voor de volgende handelingen kunt bewerken:
- Nieuwe beleidsdefinities maken.
- Beleid toepassen binnen beheergroepen en abonnementen; deze kunnen de hele organisatie of een zakelijke eenheid binnen de organisatie vertegenwoordigen.
- Beleidsnaleving bewaken.

Lees [Beleid maken en beheren om naleving af te dwingen](../azure-policy/create-manage-policy.md) voor meer informatie over Azure Policy.

De Azure-beleid bestaat uit de volgende onderdelen:

- Een **beleid** is een regel.
- Een **initiatief** is een verzameling van het beleid.
- Een **toewijzing** is de toepassing van een initiatief of een beleid voor een bepaald bereik (beheergroep, abonnement of resourcegroep).

Een resource wordt geëvalueerd op basis van de beleidsregels die eraan zijn toegewezen en krijgt een waardering op basis van het aantal beleidsregels waaraan de resource voldoet.

## <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC), waarmee u ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot ze toegang tot hebben resources. Wat betekent dat gebruikers de rol van eigenaar, bijdrager of lezer zijn toegewezen aan de groep abonnement of resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- Beveiligingslezer: heeft weergave rechten voor Security Center, waaronder aanbevelingen, waarschuwingen, beleid en de gezondheid van, maar ze niet wijzigen.
- Beveiligingsbeheerder: de dezelfde rechten weergeven als beveiligingslezer hebben, en ze kunnen ook bijwerken van het beveiligingsbeleid en aanbevelingen en waarschuwingen sluiten.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen en beheergroepen in bewerken [Azure Policy](../governance/policy/tutorials/create-and-manage.md). U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep.

Zie voor instructies over het bewerken van een beveiligingsbeleid in Azure Policy en [maken en beheren van beleidsregels voor het afdwingen van naleving](../governance/policy/tutorials/create-and-manage.md).

## <a name="view-security-policies"></a>Weergave-beveiligingsbeleid

Ga als volgt te werk als u uw beveiligingsbeleidsregels wilt weergeven in Security Center:

1. In de **Security Center** dashboard, selecteer **beveiligingsbeleid**.

    ![Het deelvenster Beleidsbeheer](./media/security-center-policies/security-center-policy-mgt.png)

  In de **beleidsbeheer** scherm ziet u het aantal beheergroepen, abonnementen en werkruimten, evenals de structuur van uw management groep.

  > [!NOTE]
  > - Het dashboard van Security Center kan een hoger aantal abonnementen onder tonen **abonnementsdekking** dan het aantal abonnementen die wordt weergegeven onder **beleidsbeheer**. Abonnementsdekking toont het aantal Standaard-, gratis en 'Niet gedekt' abonnementen. De abonnementen 'wordt niet gedekt' geen Security Center is ingeschakeld en worden niet weergegeven onder **beleidsbeheer**.
  >

  De kolommen in de tabel bevat de volgende gegevens:

 - **Toewijzing beleidsinitiatieven** – Security Center [ingebouwde beleidsregels](security-center-policy-definitions.md) en initiatieven die zijn toegewezen aan een groep abonnement of de beheergroep.
 - **Naleving** : totale nalevingsscore voor een beheergroep, het abonnement of de werkruimte. De score is het gewogen gemiddelde van de toewijzingen. Bij het gewogen gemiddelde wordt rekening gehouden met het aantal beleidsregels in één toewijzing en het aantal resources waarop de toewijzing van toepassing is.

 Als uw abonnement bijvoorbeeld twee virtuele machines heeft en een initiatief waaraan vijf beleidsregels zijn toegewezen, dan hebt u 10 beoordelingen in uw abonnement. Als een van de virtuele machines niet aan twee van de beleidsregels voldoet, is de algemene nalevingsscore van uw abonnementstoewijzing 80%.

 - **Dekking** – identificeert de prijscategorie gratis of Standard, dat de beheergroep, het abonnement of de werkruimte op wordt uitgevoerd.  Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
 - **Instellingen voor** – abonnementen hebben de koppeling **instellingen bewerken**. Selecteren **instellingen bewerken** kunt u werken uw [Security Center-instellingen](security-center-policies-overview.md) voor elke groep abonnement of de beheergroep.

2. Selecteer de groep abonnement of de beheergroep waarvan beleidsregels die u wilt weergeven.

  - De **beveiligingsbeleid** scherm weerspiegelt de actie op die door de beleidsregels die zijn toegewezen op de geselecteerde groep van abonnement of de beheergroep.
  - Gebruik de onderstaande koppelingen voor het openen van elk beleid aan de bovenkant **toewijzing** die van toepassing is op de groep abonnement of de beheergroep. Gebruik de koppelingen kunt u toegang tot de toewijzing en bewerken of uitschakelen van het beleid. Als u ziet dat de toewijzing van een bepaald beleid effectief eindpuntbeveiliging weigert, kunt u de koppeling bijvoorbeeld gebruiken voor toegang tot het beleid en bewerken of uitschakelen.
  - In de lijst met beleidsregels ziet u de effectieve toepassing van het beleid op uw abonnement of beheergroep. Dit betekent dat de instellingen die betrekking hebben op het bereik van elk beleid in aanmerking worden genomen en u u met de cumulatieve resultaten vindt van welke actie wordt ondernomen door het beleid. Bijvoorbeeld, als in één toewijzing het beleid is uitgeschakeld, maar in een andere dat is ingesteld op AuditIfNotExist, klikt u vervolgens het cumulatieve effect is van toepassing AuditIfNotExist. Het meer actieve effect heeft altijd voorrang.
  - Het beleid voor effect mag: toevoegen, controleren, AuditIfNotExists, weigeren, DeployIfNotExists, uitgeschakeld. Zie voor meer informatie over hoe effecten worden toegepast, [beleid begrijpen effecten](../governance/policy/concepts/effects.md).

   ![beleid scherm](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Wanneer u weergeven aan beleidsregels toegewezen, kunt u meerdere toewijzingen bekijken en kunt u zien hoe elke toewijzing is geconfigureerd op een eigen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Tenantbrede zichtbaarheid verkrijgen voor Azure Security Center](security-center-management-groups.md): informatie over het instellen van beheergroepen voor Azure Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

Zie [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md) voor meer informatie over Azure Policy.
