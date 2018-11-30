---
title: Problemen met uitgaande SMTP-verbinding in Azure oplossen | Microsoft Docs
description: Informatie over het oplossen van problemen van de uitgaande SMTP-verbinding in Azure.
services: virtual-network
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 34d42f9987303c1381584ae4b2991a8f30a67ed5
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618956"
---
# <a name="troubleshoot-outbound-smtp-connectivity-issues-in-azure"></a>Uitgaande SMTP-verbindingsproblemen in Azure oplossen

Vanaf 15 November 2017, zijn uitgaande e-mailberichten die worden verzonden rechtstreeks naar externe domeinen (zoals outlook.com en gmail.com) van een virtuele machine (VM) alleen beschikbaar gesteld aan bepaalde abonnementstypen in Microsoft Azure. Uitgaande SMTP-verbindingen die gebruikmaken van TCP-poort 25 is geblokkeerd. (Poort 25 wordt voornamelijk gebruikt voor niet-geverifieerde e-maillevering.)

Deze wijziging in gedrag geldt alleen voor nieuwe abonnementen en nieuwe implementaties na 15 November 2017.

## <a name="recommended-method-of-sending-email"></a>Aanbevolen methode voor het verzenden van e-mail
U wordt aangeraden dat u geverifieerde SMTP-relayservices (die doorgaans verbinding maken via TCP-poort 587 of 443 maar andere poorten te ondersteunen) gebruiken om u te e-mailbericht verzenden vanuit Azure-VM's of Azure App Services. Deze services worden gebruikt voor het onderhouden van de reputatie van de IP-adres of -domein om te voorkomen dat externe e-mailproviders het bericht wordt geweigerd. Dergelijke SMTP-relayservices omvatten, maar niet beperkt tot [SendGrid](http://sendgrid.com/partners/azure/). Het is ook mogelijk dat u hebt een veilige SMTP-relay-service, die wordt uitgevoerd on-premises die u kunt gebruiken.

Met behulp van deze services voor het leveren van e-mailbericht is niet beperkt in Azure, ongeacht het abonnementstype.

## <a name="enterprise-agreement"></a>Enterprise Agreement
Er is geen wijziging in de technische mogelijkheden voor het verzenden van e-mailbericht zonder via een geverifieerde relay voor Azure Enterprise Agreement-gebruikers. Nieuwe en bestaande Enterprise Agreement-gebruikers kunnen uitgaande e-mail delivery van Azure VM's rechtstreeks naar externe e-mailproviders zonder beperkingen van het Azure-platform uitproberen. Hoewel er geen garantie dat e-mailproviders inkomende e-mail van een bepaalde gebruiker accepteert, bezorgingspogingen niet geblokkeerd door de Azure-platform voor virtuele machines in een Enterprise Agreement-abonnementen. Hebt u rechtstreeks met e-mailproviders oplossen van eventuele bezorging van berichten of filters gebruiken om problemen met betrekking specifieke providers tot SPAM.

## <a name="pay-as-you-go"></a>Betalen per gebruik
Als u zich hebt geregistreerd vóór 15 November 2017 voor het betalen per gebruik of Microsoft Partner Network-abonnement biedt, er zijn geen wijzigingen in de technische mogelijkheden om te proberen uitgaande e-maillevering. U gaat verder met het mogelijk om te proberen uitgaande e-maillevering van Azure VM's binnen deze abonnementen rechtstreeks naar externe e-mailproviders zonder beperkingen van het Azure-platform. Nogmaals, er geen garantie dat e-mailproviders inkomende e-mailbericht van een bepaalde gebruiker accepteert, en gebruikers hebben rechtstreeks met e-mailproviders oplossen van eventuele bezorging van berichten of filters gebruiken om problemen met betrekking specifieke providers tot SPAM.

Voor betalen per gebruik of Microsoft Partner Network-abonnementen die zijn gemaakt na 15 November 2017 zullen er technische beperkingen die voorkomen dat e-mail die rechtstreeks vanuit virtuele machines in deze abonnementen worden verzonden. Als u wilt dat de mogelijkheid om e-mail te verzenden van Azure VM's rechtstreeks naar externe e-mailproviders (niet via een geverifieerde SMTP-relay), kunt u een aanvraag voor het verwijderen van de beperking. Aanvragen worden bekeken en goedgekeurd goeddunken van Microsoft, en ze wordt verleend nadat extra ter voorkoming van fraude wordt gecontroleerd. Als u een aanvraag, kunt u een ondersteuningsaanvraag openen met behulp van de volgende probleemtype: **technische** > **Virtueelnetwerk** > **connectiviteit**  >  **Kunnen geen e-mail (SMTP/poort 25) verzenden**. Zorg ervoor dat u meer informatie over waarom uw implementatie heeft voor het verzenden van e-mail rechtstreeks naar e-mailproviders in plaats van via een geverifieerde relay toevoegt.

Wanneer u een abonnement met betalen per gebruik of Microsoft Partner Network is uitgesloten, wordt virtuele machines binnen dat abonnement alleen vrijgesteld voortaan.

> [!NOTE]
> Microsoft behoudt zich het recht om in te trekken van deze uitzondering als wordt vastgesteld dat een schending van de servicevoorwaarden heeft plaatsgevonden.

## <a name="msdn-azure-pass-azure-in-open-education-bizspark-and-free-trial"></a>MSDN, Azure Pass, Azure in Open, Education, BizSpark en gratis proefversie
Als u een MSDN, Azure Pass, Azure in Open, Education, BizSpark, Azure Sponsorship, Azure Student, gratis proefversie of een Visual Studio-abonnement na 15 November 2017 hebt gemaakt hebt u technische beperkingen dat e-blok dat wordt verzonden door virtuele machines in deze abonnementen rechtstreeks naar e-providers. De beperkingen worden uitgevoerd om te voorkomen dat misbruik. Er zijn geen aanvragen voor het verwijderen van deze beperking wordt verleend.

Als u deze abonnementstypen, bent u aangeraden om te gebruiken SMTP-relayservices, zoals eerder in dit artikel wordt beschreven.

## <a name="cloud-service-provider-csp"></a>Cloud serviceprovider (CSP)

Als u Azure-resources via CSP, om te maken van een ondersteuningsverzoek in via uw keuze van CSP kunt u en u kunt aanvragen de CSP een aanvraag voor opheffen van blokkeringen namens maken als een beveiligde SMTP-relay kan niet worden gebruikt.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
