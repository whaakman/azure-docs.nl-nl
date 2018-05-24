---
title: Beveiligingsoplossingen integreren in Azure Security Center | Microsoft Docs
description: Leer hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging van uw Azure-resources te verbeteren.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2018
ms.author: terrylan
ms.openlocfilehash: ac1e49400f32194ada319f23b5233cc81de02a02
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Beveiligingsoplossingen integreren in Azure Security Center
Dit document helpt u bij het beheren van beveiligingsoplossingen die al zijn gekoppeld aan Azure Security Center en bij het toevoegen van nieuwe oplossingen.

## <a name="integrated-azure-security-solutions"></a>Geïntegreerde Azure-beveiligingsoplossingen
Met Security Center kunt u gemakkelijk geïntegreerde beveiligingsoplossingen in Azure inschakelen. Voordelen zijn:

- **Vereenvoudigde implementatie**: Security Center biedt gestroomlijnde inrichting van geïntegreerde partneroplossingen. Met Security Center kan op virtuele machines de benodigde agent worden ingericht voor oplossingen zoals het beoordelen van antimalware en beveiligingsproblemen. Voor firewallapparaten kan in Security Center een groot gedeelte van de vereiste netwerkconfiguratie worden uitgevoerd.
- **Geïntegreerde detectie**: beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als waarschuwingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde statuscontrole en beheer**: klanten kunnen geïntegreerde statusgebeurtenissen gebruiken om alle partneroplossingen in één oogopslag te controleren. Er zijn basisbeheermogelijkheden beschikbaar, met eenvoudige toegang tot geavanceerde installatie met behulp van de partneroplossing.

Geïntegreerde beveiligingsoplossingen omvatten momenteel:

- Eindpuntbescherming ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, McAfee, Windows Defender en System Center Endpoint Protection (SCEP))
- Firewall voor webtoepassingen ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) en [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewall van de volgende generatie ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) en [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))
- Evaluatie van beveiligingsproblemen ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

De integratie van de eindpuntbescherming kan per oplossing verschillen. De volgende tabel bevat meer informatie over de integratie-ervaring van elke oplossing:

| Eindpuntbeveiliging               | Platformen                             | Security Center-installatie | Security Center Discovery |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft Antimalware)                  | Windows Server 2016                   | Nee, ingebouwd in besturingssysteem           | Ja                       |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 | Via extensie                | Ja                       |
| Trend Micro: alle versies         | Windows Server-familie                 | Via extensie                | Ja                       |
| Symantec v12.1.1100+              | Windows Server-familie                 | Nee                           | Ja                       |
| McAfee v10+                       | Windows Server-familie                 | Nee                           | Ja                       |
| Kaspersky                         | Windows Server-familie                 | Nee                           | Nee                        |
| Sophos                            | Windows Server-familie                 | Nee                           | Nee                        |



## <a name="how-security-solutions-are-integrated"></a>Beveiligingsoplossingen integreren
Azure-beveiligingsoplossingen die zijn geïmplementeerd vanuit Security Center, zijn automatisch verbonden. U kunt ook verbinding maken met andere gegevensbronnen voor beveiliging, waaronder:

- Azure AD-identiteitsbeveiliging
- Computers die on-premises of in andere clouds worden uitgevoerd
- Een beveiligingsoplossing die ondersteuning biedt voor CEF (Common Event Format)
- Microsoft Advanced Threat Analytics

![Integratie van partneroplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Geïntegreerde Azure-beveiligingsoplossingen en andere gegevensbronnen beheren

1. Meld u aan bij [Azure Portal](https://azure.microsoft.com/features/azure-portal/).

2. Ga naar het menu van **Microsoft Azure** en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

  ![Overzicht van Security Center](./media/security-center-partner-integration/overview.png)

3. Selecteer bij **Overzicht** de optie **Beveiligingsoplossingen**.

Bij **beveiligingsoplossingen** kunt u informatie bekijken over de status van de geïntegreerde Azure-beveiligingsoplossingen en algemene beheertaken uitvoeren. U kunt ook andere typen gegevensbronnen voor beveiliging verbinden, zoals Azure Active Directory Identity Protection-waarschuwingen en firewall-logboeken in CEF (Common Event Format).

### <a name="connected-solutions"></a>Verbonden oplossingen

Het gedeelte **Verbonden oplossingen** bevat beveiligingsoplossingen die momenteel zijn verbonden met Security Center en informatie over de status van elke oplossing.  

![Verbonden oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Raadpleeg [Managing connected partner solutions](security-center-partner-solutions.md) (Verbonden partneroplossingen beheren) voor meer informatie.

### <a name="discovered-solutions"></a>Gedetecteerde oplossingen

Security Center detecteert automatisch beveiligingsoplossingen die worden uitgevoerd in Azure maar niet zijn verbonden met Security Center, en geeft deze oplossingen weer in het gedeelte **Gedetecteerde oplossingen**. Hierbij gaat het zowel om Azure-oplossingen, zoals [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), als om partneroplossingen.

> [!NOTE]
> De Standard-laag van Security Center is op abonnementsniveau vereist voor de functie Gedetecteerde oplossingen. Ga naar [deze pagina](security-center-pricing.md) voor meer informatie over de prijzen van Security Center.
>
>

Selecteer **CONNECT** onder een oplossing om met Security Center te integreren en meldingen over beveiligingswaarschuwingen te ontvangen.

![Gedetecteerde oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center detecteert ook oplossingen die worden uitgevoerd in het abonnement en die Common Event Format-logboeken (CEF) kunnen doorsturen. Informatie over hoe u [een beveiligingsoplossing met Security Center verbindt](quick-security-solutions.md) die CEF-logboeken gebruikt.

### <a name="add-data-sources"></a>Gegevensbronnen toevoegen

Het gedeelte **Gegevensbronnen toevoegen** bevat andere beschikbare gegevensbronnen die kunnen worden verbonden. Klik op **TOEVOEGEN** voor instructies over het toevoegen van gegevens uit een van deze bronnen.

![Gegevensbronnen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u kunnen lezen hoe u partneroplossingen integreert in Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Microsoft Advanced Threat Analytics koppelen aan Azure Security Center](security-center-ata-integration.md)
* [Azure Active Directory Identity Protection koppelen aan Azure Security Center](security-center-aadip-integration.md)
* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Krijg antwoorden op veelgestelde vragen over het gebruik van Security Center.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.
