---
title: Configureren van uw Azure-project met behulp van serviceconfiguraties met meerdere | Microsoft Docs
description: Informatie over het configureren van een Azure-cloud service-project door de bestanden ServiceDefinition.csdef ServiceConfiguration.Local.cscfg en ServiceConfiguration.Cloud.cscfg wijzigen.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: kraigb
ms.openlocfilehash: 8125ef36f730b745d63c39493f48d14a5a33d76c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="configuring-your-azure-project-in-visual-studio-to-use-multiple-service-configurations"></a>Configureren van uw Azure-project in Visual Studio te gebruiken serviceconfiguraties met meerdere

Een Azure-cloud service-project in Visual Studio omvat drie configuratiebestanden: `ServiceDefinition.csdef`, `ServiceConfiguration.Local.cscfg`, en `ServiceConfiguration.Cloud.cscfg`:

- `ServiceDefinition.csdef`wordt geïmplementeerd naar Azure te beschrijven de vereisten van de cloudservice en de bijbehorende rollen en instellingen opgeven die van toepassing op alle exemplaren. Instellingen kunnen worden gelezen tijdens runtime met de Azure-Service die als host fungeert Runtime-API. Dit bestand kan alleen worden bijgewerkt in Azure wanneer de cloudservice is gestopt.
- `ServiceConfiguration.Local.cscfg`en `ServiceConfiguration.Cloud.cscfg` waarden opgeven voor de instellingen in de definitie van het bestand en geef het aantal exemplaren worden uitgevoerd voor elke rol. Het "Local" bestand bevat waarden die worden gebruikt bij de lokale foutopsporing; het bestand 'Cloud' wordt geïmplementeerd op Azure als `ServiceConfiguration.cscfg` en vindt u instellingen voor de server-omgeving. Dit bestand kan worden bijgewerkt terwijl uw cloudservice wordt uitgevoerd in Azure.

Configuratie-instellingen worden beheerd en gewijzigd in Visual Studio met eigenschappenpagina's voor de rol van toepassing (met de rechtermuisknop op de rol en selecteer **eigenschappen**, dubbelklikt u op de rol). Wijzigingen kunnen worden binnen het bereik van de configuratie wordt gekozen de **serviceconfiguratie** vervolgkeuzelijst. De eigenschappen voor web-en werkrollen zijn vergelijkbaar, behalve wanneer in de volgende secties beschreven.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Zie voor informatie over de onderliggende schema's voor de servicedefinitie en de configuratiebestanden van de service, de [csdef XML-Schema](cloud-services/schema-csdef-file.md) en [.cscfg XML-Schema](cloud-services/schema-cscfg-file.md) artikelen. Zie voor meer informatie over de serviceconfiguratie van de [Cloud-Services configureren hoe](cloud-services/cloud-services-how-to-configure-portal.md).


## <a name="configuration-page"></a>Configuratiepagina

### <a name="service-configuration"></a>Configuratie van de service

Selecteert op welke `ServiceConfiguration.*.cscfg` bestand is beïnvloed door wijzigingen. Standaard worden lokale en Cloud varianten en u kunt de **beheren...**  opdracht uit om te kopiëren, wijzigen en verwijderen van de configuratiebestanden. Deze bestanden worden toegevoegd aan uw cloudserviceproject en worden weergegeven in **Solution Explorer**. Echter, hernoemen of verwijderen van configuraties worden uitgevoerd alleen vanaf dit besturingselement.

### <a name="instances"></a>Exemplaren

Stel de **exemplaar** eigenschap om het aantal exemplaren voor deze rol moet worden uitgevoerd door de service te tellen.

Stel de **VM-grootte** eigenschap **Extra klein**, **kleine**, **gemiddeld**, **grote**, of **Extra groot**.  Zie voor meer informatie [grootten voor Cloudservices](cloud-services/cloud-services-sizes-specs.md).

### <a name="startup-action-web-role-only"></a>Opstartactie (Webrol)

Stel deze eigenschap om op te geven dat een webbrowser voor de HTTP-eindpunten of de HTTPS-eindpunten of beide in Visual Studio moet worden gestart als u foutopsporing start.

De **HTTPS-eindpunt** optie is alleen beschikbaar als u al een HTTPS-eindpunt voor de rol hebt gedefinieerd. U kunt een HTTPS-eindpunt definiëren voor de **eindpunten** eigenschappenpagina.

Als u al een HTTPS-eindpunt hebt toegevoegd, de HTTPS-eindpunt-optie is standaard ingeschakeld en Visual Studio een browser voor dit eindpunt wordt gestart wanneer u foutopsporing, naast een browser voor uw HTTP-eindpunt starten, ervan uitgaande dat beide opstartopties zijn ingeschakeld.

### <a name="diagnostics"></a>Diagnostiek

Diagnostische gegevens zijn standaard ingeschakeld voor de Webrol. Het account Azure-cloud service-project en opslag worden ingesteld op de emulator van de lokale opslag gebruiken. Wanneer u klaar om te implementeren naar Azure bent, kunt u de knop builder (**...** ) in plaats daarvan Azure storage gebruiken. U kunt de diagnostics-gegevens overbrengen naar het storage-account op verzoek of regelmatig automatisch. Zie voor meer informatie over Azure diagnostics [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Pagina met instellingen

Op de **instellingen** pagina kunt u instellingen toevoegen aan een configuratie als naam / waarde-paren. Code die wordt uitgevoerd in de rol vindt de waarden van uw configuratie-instellingen bij uitvoering met klassen die worden geleverd door de [Azure beheerde bibliotheek](http://go.microsoft.com/fwlink?LinkID=171026), met name de [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) methode.

### <a name="configuring-a-connection-string-for-a-storage-account"></a>Een verbindingsreeks voor een opslagaccount configureren

Een verbindingsreeks is een instelling waarmee verbinding en verificatiegegevens voor de opslagemulator of voor een Azure storage-account. Als de code in een rol toegang krijgt tot Azure-opslag (blobs, wachtrijen of tabellen), moet deze een verbindingsreeks.

> [!Note]
> Een verbindingsreeks voor Azure storage-account moet een gedefinieerde indeling gebruiken (Zie [Azure Storage-verbindingsreeksen configureren](storage/common/storage-configure-connection-string.md)).

De cloudservice, kunt u de verbindingsreeks met het gebruik van lokale opslag, indien nodig, vervolgens ingesteld op een Azure storage-account wanneer u de toepassing implementeren instellen. De verbindingsreeks juist instellen is mislukt, kan ertoe leiden dat uw rol bij het starten of doorloopt u de statussen initialiseren bezet en stoppen.

Selecteer voor het maken van een verbindingsreeks **instelling toevoegen** en stel **Type** naar "Connection String".

Selecteer voor nieuwe of bestaande verbindingsreeksen **...** * aan de rechterkant van de **waarde** veld openen de **Create Storage Connection String** dialoogvenster:

1. Onder **verbinding maken via**, kies de **uw abonnement** optie voor het selecteren van een opslagaccount van een abonnement. Visual Studio wordt vervolgens opgehaald voor de opslagaccountreferenties automatisch uit de `.publishsettings` bestand.
1. Selecteren **referenties handmatig worden ingevoerd** kunt u de accountnaam opgeven en sleutel rechtstreeks met behulp van informatie van de Azure-portal. De accountsleutel wilt kopiëren: een. Navigeer naar het opslagaccount in de Azure portal en selecteert u een **sleutels beheren**.
    2. Om te kopiëren van de accountsleutel, gaat u naar het storage-account op de Azure portal, selecteer **instellingen > toegangssleutels**, gebruikt u de knop kopiëren aan de primaire toegangssleutel naar het Klembord kopiëren.
1. Selecteer een van de opties voor de verbinding. **Geef aangepaste eindpunten** vraagt u om op te geven van specifieke URL's voor blobs, tabellen en wachtrijen. Aangepaste eindpunten kunnen u gebruiken [aangepaste domeinen](storage/blobs/storage-custom-domain-name.md) en toegangsbeheer duidelijker. Zie [Azure Storage-verbindingsreeksen configureren](./storage/common/storage-configure-connection-string.md).
1. Selecteer **OK**, klikt u vervolgens **bestand > Opslaan** configuratie bijwerken met de nieuwe verbindingsreeks.

Wanneer u uw toepassing in Azure publiceert, kies opnieuw, de configuratie van de service met de Azure storage-account voor de verbindingsreeks. Nadat uw toepassing wordt gepubliceerd, moet u controleren dat de toepassing werkt zoals verwacht ten opzichte van de Azure storage-services.

Zie de sectie voor meer informatie over het bijwerken van configuraties [tekenreeksen voor databaseverbindingen voor storage-accounts beheren](vs-azure-tools-configure-roles-for-cloud-service.md#manage-connection-strings-for-storage-accounts).

## <a name="endpoints-page"></a>Pagina eindpunten

Een Webrol heeft doorgaans een enkelvoudig HTTP-eindpunt op poort 80. Een werkrol aan de andere kant kan een onbeperkt aantal HTTP-, HTTPS- of TCP-eindpunten hebben. Eindpunten kunnen invoereindpunten naar externe clients beschikbaar zijn, of interne eindpunten die beschikbaar zijn voor andere functies die worden uitgevoerd in de service zijn.

- Wijzigen van het eindpunttype voor het invoeren van een HTTP-eindpunt om beschikbaar te maken naar externe clients en webbrowsers, en geef een naam en een openbare-poortnummer.
- Als u een HTTPS-eindpunt naar externe clients en webbrowsers, wijzig het eindpunttype **invoer**, en geef een naam, een openbare-poortnummer en de naam van een management-certificaat. U moet het certificaat ook definiëren op de **certificaten** eigenschappenpagina voordat u een beheercertificaat kunt opgeven. 
- Als u een eindpunt beschikbaar voor interne toegang door andere functies in de cloudservice, het wijzigen van het eindpunttype met interne en geef een naam en mogelijke particuliere poorten voor dit eindpunt.

## <a name="local-storage-page"></a>Lokale opslag-pagina

U kunt de **lokale opslag** eigenschappenpagina te reserveren van een of meer lokale opslagresources voor een rol. Een lokale opslagbron is een gereserveerde map in het bestandssysteem van de virtuele machine van Azure waarop een exemplaar van een rol wordt uitgevoerd.

## <a name="certificates-page"></a>De pagina Certificaten

De **certificaten** eigenschappenpagina gegevens over uw certificaten worden toegevoegd aan de configuratie van uw service. Houd er rekening mee dat uw certificaten niet zijn verpakt met uw service; u moet uw certificaten afzonderlijk uploaden naar Azure via de [Azure-portal](http://portal.azure.com).

Hier een certificaat toe te voegen, wordt informatie over uw certificaten toegevoegd aan de configuratie van uw service. Certificaten zijn niet verpakt met de service; u moet uw certificaten afzonderlijk via de Azure portal uploaden.

Als u wilt verbinden met een certificaat met de rol, Geef een naam voor het certificaat. Gebruikt u deze naam om te verwijzen naar het certificaat, wanneer u een HTTPS-eindpunt configureren op de **eindpunten** pagina. Vervolgens opgeven of het certificaatarchief **lokale Machine** of **huidige gebruiker** en de naam van het archief. Voer ten slotte de vingerafdruk van het certificaat. Als het certificaat zich in de huidige User\Personal () winkel, kunt u de vingerafdruk van het certificaat door het certificaat van een ingevulde lijst te selecteren. Als deze zich op een andere locatie, voert u de vingerafdrukwaarde handmatig in.

Wanneer u een certificaat uit het certificaatarchief toevoegt, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten bovendien worden geüpload naar Azure correct configureren van uw service voor SSL.

Van beheercertificaten die u aan uw service koppelen met uw service alleen toegepast wanneer deze wordt uitgevoerd in de cloud. Wanneer uw service wordt uitgevoerd in de lokale ontwikkelingsomgeving, gebruikt een standaardcertificaat dat wordt beheerd door de rekenemulator.
