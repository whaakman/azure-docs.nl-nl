---
title: Syslog-gegevens verbinden met Azure Sentinel preview | Microsoft Docs
description: Meer informatie over het verbinden van syslog-gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2019
ms.author: rkarlin
ms.openlocfilehash: fef9fa128d2ebb84fb82579f254735fdb9aa7ee2
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881069"
---
# <a name="connect-your-external-solution-using-syslog"></a>Verbinding maken met uw externe oplossing met behulp van syslog

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt elk on-premises apparaat dat syslog ondersteunt, verbinden met Azure Sentinel. Dit wordt gedaan met behulp van een agent die is gebaseerd op een Linux-machine tussen het apparaat en de Azure-Sentinel. Als uw Linux-machine zich in azure bevindt, kunt u de logboeken van uw apparaat of toepassing streamen naar een toegewezen werk ruimte die u in azure maakt en er verbinding mee maken. Als uw Linux-machine zich niet in azure bevindt, kunt u de logboeken van uw apparaat streamen naar een specifieke on-premises VM of computer waarop u de agent voor Linux installeert. 

> [!NOTE]
> Als uw apparaat syslog CEF ondersteunt, is de verbinding meer voltooid en moet u deze optie kiezen en de instructies volgen bij het [verbinden van gegevens van CEF](connect-common-event-format.md).

## <a name="how-it-works"></a>Hoe werkt het?

Syslog is een protocol voor het vastleggen van gebeurtenis die geldt voor Linux. Toepassingen wordt berichten die kunnen worden opgeslagen op de lokale computer of worden geleverd met een Syslog-collector verzonden. Wanneer de Log Analytics-agent voor Linux is geïnstalleerd, wordt de lokale syslog-daemon geconfigureerd voor het door sturen van berichten naar de agent. De agent verzendt het bericht vervolgens naar Azure Monitor waar een corresponderende record wordt gemaakt.

Zie [syslog-gegevens bronnen in azure monitor](../azure-monitor/platform/data-sources-syslog.md)voor meer informatie.

> [!NOTE]
> De agent kan Logboeken van meerdere bronnen verzamelen, maar moet op een specifieke proxy computer worden geïnstalleerd.

## <a name="connect-your-syslog-appliance"></a>Uw syslog-apparaat aansluiten

1. Selecteer in de Azure-Sentinel-Portal de optie **gegevens connectors** en selecteer de **syslog** -lijn in de tabel en klik in het deel venster syslog rechts op de **pagina connector openen**.
2. Als uw Linux-machine zich in azure bevindt, selecteert u **agent downloaden en installeren op virtuele machine van Azure Linux**. Selecteer in het venster virtuele machines de computers waarop u de agent wilt installeren en klik bovenaan op **verbinding maken** .
1. Als uw Linux-machine niet in Azure is, selecteert u **agent op Linux niet-Azure-machine downloaden en installeren**. In het venster **direct agent** kopieert u de opdracht onder **down load and agent voor Linux** en voert u deze uit op de computer. 
1. Volg de instructies onder **Configureer de logboeken die moeten worden verbonden** in het venster Setup van syslog-connector:
    1. Klik op de koppeling om **de configuratie van geavanceerde instellingen voor uw werk ruimte te openen**. 
    1. Selecteer **gegevens**, gevolgd door **syslog**.
    1. Vervolgens stelt u in de tabel in welke faciliteiten u syslog wilt laten verzamelen. U moet de faciliteiten toevoegen of selecteren die door uw syslog-apparaat in de logboek headers worden opgenomen. U kunt deze configuratie bekijken in uw syslog-apparaat in syslog-d in de map:/etc/rsyslog.d/Security-config-omsagent.conf en in r-syslog onder/etc/syslog-ng/Security-config-omsagent.conf. 
       > [!NOTE]
       > Als u het selectie vakje voor de **onderstaande configuratie op mijn machines**inschakelt, wordt deze configuratie toegepast op alle Linux-machines die zijn verbonden met deze werk ruimte. U kunt deze configuratie bekijken in uw syslog-machine onder 
1. Klik op **Druk hier om de Blade configuratie te openen**.
1. Selecteer **gegevens** en vervolgens **syslog**.
   - Zorg ervoor dat elke faciliteit die u verstuurt door syslog in de tabel staat. Stel een ernst in voor elke faciliteit die u gaat bewaken. Klik op **Toepassen**.
1. Zorg ervoor dat u in uw syslog-machine deze faciliteiten verzendt. 

1. Als u het relevante schema in Log Analytics voor de syslog-logboeken wilt gebruiken, zoekt u naar **syslog**.
1. U kunt de functie Kusto die wordt beschreven in [functies gebruiken in azure monitor-logboek query's](../azure-monitor/log-query/functions.md) gebruiken om uw syslog-berichten te parseren en deze vervolgens op te slaan als een nieuwe log Analytics-functie en vervolgens de functie als een nieuw gegevens type te gebruiken.




## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u op on-premises syslog-apparaten verbindt met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
