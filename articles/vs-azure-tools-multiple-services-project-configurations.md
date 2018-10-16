---
title: Configureren van uw Azure-project met behulp van serviceconfiguraties met meerdere | Microsoft Docs
description: Informatie over het configureren van een Azure-cloud service-project door de bestanden ServiceDefinition.csdef en ServiceConfiguration.Local.cscfg ServiceConfiguration.Cloud.cscfg wijzigen.
services: visual-studio-online
author: ghogen
manager: douge
assetId: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 1ec587f4a4519f86efceb7cefa0acb372035a9a9
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318211"
---
# <a name="configuring-your-azure-project-in-visual-studio-to-use-multiple-service-configurations"></a>Uw Azure-project configureren in Visual Studio te gebruiken serviceconfiguraties met meerdere

Een Azure-cloud service-project in Visual Studio bevat drie configuratiebestanden: `ServiceDefinition.csdef`, `ServiceConfiguration.Local.cscfg`, en `ServiceConfiguration.Cloud.cscfg`:

- `ServiceDefinition.csdef` is geïmplementeerd in Azure om te beschrijven de vereisten van de cloudservice en de bijbehorende rollen en instellingen die van toepassing op alle exemplaren zijn opgeven. Instellingen kunnen worden gelezen tijdens runtime met de Azure-Service die als host fungeert Runtime-API. Dit bestand kan worden bijgewerkt in Azure, alleen wanneer de cloudservice is gestopt.
- `ServiceConfiguration.Local.cscfg` en `ServiceConfiguration.Cloud.cscfg` waarden opgeven voor de instellingen in de definitie van het bestand en geef het aantal exemplaren om uit te voeren voor elke rol. Het "Local" bestand bevat waarden die worden gebruikt in het lokale foutopsporing; het bestand 'Cloud' is geïmplementeerd in Azure als `ServiceConfiguration.cscfg` en vindt u instellingen voor de server-omgeving. Dit bestand kan worden bijgewerkt terwijl uw cloudservice wordt uitgevoerd in Azure.

Configuratie-instellingen worden beheerd en gewijzigd in Visual Studio met eigenschappenpagina's voor de rol die van toepassing (met de rechtermuisknop op de rol en selecteer **eigenschappen**, dubbelklikt u op de rol). Wijzigingen kunnen worden toegewezen aan de configuratie is geselecteerd in de **serviceconfiguratie** vervolgkeuzelijst. De eigenschappen voor web-en werkrollen zijn vergelijkbaar, behalve wanneer in de volgende secties wordt beschreven.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Zie voor meer informatie over de onderliggende schema's voor de servicedefinitie en de configuratiebestanden van de service de [.csdef XML-Schema](cloud-services/schema-csdef-file.md) en [.cscfg XML-Schema](cloud-services/schema-cscfg-file.md) artikelen. Zie voor meer informatie over de configuratie van de service, [over het configureren van Cloud Services](cloud-services/cloud-services-how-to-configure-portal.md).


## <a name="configuration-page"></a>Pagina-configuratie

### <a name="service-configuration"></a>Configuratie van de service

Selecteert die `ServiceConfiguration.*.cscfg` bestand wordt beïnvloed door wijzigingen. Standaard worden lokale en Cloud-varianten en kunt u de **beheren...**  opdracht uit om te kopiëren, wijzigen en verwijderen van de configuratiebestanden. Deze bestanden worden toegevoegd aan uw cloudserviceproject en worden weergegeven in **Solution Explorer**. Echter, hernoemen of verwijderen van configuraties worden uitgevoerd alleen vanaf dit besturingselement.

### <a name="instances"></a>Exemplaren

Stel de **exemplaar** aan het aantal exemplaren van de service moet worden uitgevoerd voor deze rol de eigenschap count.

Stel de **VM-grootte** eigenschap **Extra kleine**, **kleine**, **gemiddeld**, **grote**, of **Extra grote**.  Zie voor meer informatie, [groottes voor Cloud Services](cloud-services/cloud-services-sizes-specs.md).

### <a name="startup-action-web-role-only"></a>Opstartactie (alleen Webrol)

Stel deze eigenschap om op te geven dat Visual Studio een webbrowser voor de HTTP-eindpunten of de HTTPS-eindpunten of beide starten moet wanneer u voor de foutopsporing starten.

De **HTTPS-eindpunt** optie is alleen beschikbaar als u al een HTTPS-eindpunt voor uw rol hebt gedefinieerd. U kunt een HTTPS-eindpunt definiëren voor de **eindpunten** eigenschappenpagina.

Als u al een HTTPS-eindpunt hebt toegevoegd, de HTTPS-eindpunt-optie is standaard ingeschakeld en Visual Studio een browser voor dit eindpunt wordt gestart wanneer u voor de foutopsporing, naast een browser voor uw HTTP-eindpunt starten, ervan uitgaande dat beide opties voor opstarten zijn ingeschakeld.

### <a name="diagnostics"></a>Diagnostiek

Diagnostische gegevens zijn standaard ingeschakeld voor de Webrol. Het Azure-cloud-project en storage-serviceaccount zijn ingesteld op de lokale opslag-emulator. Wanneer u klaar om te implementeren naar Azure bent, kunt u de knop builder (**...** ) in plaats daarvan gebruik van Azure storage. U kunt de diagnostische gegevens overbrengen naar het opslagaccount op aanvraag of automatisch geplande intervallen. Zie voor meer informatie over Azure diagnostics [diagnostische gegevens inschakelen in Azure Cloud Services en Virtual Machines](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>De pagina Instellingen

Op de **instellingen** pagina, kunt u instellingen toevoegen aan een configuratie als naam / waarde-paren. Code die wordt uitgevoerd in de rol vindt u de waarden van uw configuratie-instellingen tijdens runtime met behulp van klassen die worden geleverd door de [Azure Beheerbibliotheek](http://go.microsoft.com/fwlink?LinkID=171026), in het bijzonder, de [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) methode.

### <a name="configuring-a-connection-string-for-a-storage-account"></a>Een verbindingsreeks voor een storage-account configureren

Een verbindingsreeks is een instelling waarmee verbinding en de verificatiegegevens voor de opslagemulator of voor een Azure storage-account. Wanneer de code in een rol toegang heeft tot Azure storage (blobs, wachtrijen en tabellen), heeft een verbindingsreeks nodig.

> [!Note]
> Een verbindingsreeks voor Azure storage-account moet een gedefinieerde indeling gebruiken (Zie [Azure Storage-verbindingsreeksen configureren](storage/common/storage-configure-connection-string.md)).

De cloudservice, kunt u de verbindingsreeks voor het gebruik van lokale opslag naar behoefte, klikt u vervolgens ingesteld op een Azure storage-account wanneer u de toepassing implementeert instellen. Instellen van de verbindingsreeks correct is mislukt, kan ertoe leiden dat uw rol bij het starten of doorloopt u de Staten initialiseren, bezet en stoppen.

Voor het maken van een verbindingsreeks selecteert **instelling toevoegen** en stel **Type** naar "Connection String".

Selecteer voor nieuwe of bestaande met verbindingsreeksen, **...** * aan de rechterkant van de **waarde** veld om te openen de **Create Storage Connection String** dialoogvenster:

1. Onder **verbinding maken met behulp van**, kiest u de **uw abonnement** optie voor het selecteren van een storage-account van een abonnement. Visual Studio haalt vervolgens de referenties van het storage-account automatisch uit de `.publishsettings` bestand.
1. Selecteren **referenties handmatig worden ingevoerd** kunt u de accountnaam opgeven en de sleutel rechtstreeks met behulp van gegevens vanuit Azure portal. De accountsleutel kopiëren:
    1. Ga naar het opslagaccount in de Azure portal en selecteer **sleutels beheren**.
    1. Als u wilt kopiëren van de accountsleutel, gaat u naar het opslagaccount in Azure portal, selecteer **instellingen > toegangssleutels**, gebruikt u de knop kopiëren aan de primaire toegangssleutel naar het Klembord kopiëren.
1. Selecteer een van de opties voor de verbinding. **Geef aangepaste eindpunten** vraagt u om op te geven specifieke URL's voor blobs, tabellen en wachtrijen. Aangepaste eindpunten kunt u gebruikmaken van [aangepaste domeinen](storage/blobs/storage-custom-domain-name.md) en voor het beheren van toegang tot meer exact. Zie [Azure Storage-verbindingsreeksen configureren](./storage/common/storage-configure-connection-string.md).
1. Selecteer **OK**, klikt u vervolgens **bestand > Opslaan** configuratie bijwerken met de nieuwe verbindingsreeks.

Wanneer u uw toepassing in Azure publiceert, kies opnieuw de configuratie van de service met de Azure storage-account voor de verbindingsreeks. Nadat uw toepassing is gepubliceerd, moet u controleren of de toepassing werkt zoals verwacht op basis van de Azure storage-services.

Zie de sectie voor meer informatie over het bijwerken van configuraties [verbindingsreeksen voor storage-accounts beheren](vs-azure-tools-configure-roles-for-cloud-service.md#manage-connection-strings-for-storage-accounts).

## <a name="endpoints-page"></a>Pagina eindpunten

Een Webrol heeft meestal één HTTP-eindpunt op poort 80. Een werkrol, daarentegen, kan een onbeperkt aantal HTTP, HTTPS of TCP-eindpunten hebben. Eindpunten kunnen zich invoereindpunten die beschikbaar is voor externe clients, of interne eindpunten, die beschikbaar zijn voor andere functies die worden uitgevoerd in de service.

- Wijzig het eindpunttype voor het invoeren van een HTTP-eindpunt om beschikbaar te maken naar externe clients en webbrowsers, en geef een naam en een openbare-poortnummer.
- Als u wilt een HTTPS-eindpunt beschikbaar maken voor externe clients en webbrowsers, wijzigt u de eindpunt-adrestype in **invoer**, en geef een naam, een openbare-poortnummer en de naam van een management-certificaat. U moet het certificaat ook definiëren op de **certificaten** eigenschappenpagina voordat u een beheercertificaat kunt opgeven. 
- Als u wilt een eindpunt beschikbaar maken voor interne toegang door andere rollen in de cloudservice, het eindpunt van het type wijzigen naar interne en geef een naam en mogelijk persoonlijke poorten voor dit eindpunt.

## <a name="local-storage-page"></a>Lokale opslag-pagina

U kunt de **lokale opslag** eigenschappenvenster voor het reserveren van resources voor een of meer lokale opslag voor een rol. De resource van een lokale opslag is een gereserveerde map in het bestandssysteem van de Azure-machine waarop een exemplaar van een rol wordt uitgevoerd.

## <a name="certificates-page"></a>De pagina Certificaten

De **certificaten** eigenschappenpagina gegevens over uw certificaten worden toegevoegd aan de configuratie van uw service. Houd er rekening mee dat de certificaten niet zijn verpakt met de service; u moet uw certificaten afzonderlijk uploaden naar Azure via de [Azure-portal](http://portal.azure.com).

Hier een certificaat toe te voegen, wordt informatie over uw certificaten toegevoegd aan de configuratie van uw service. Certificaten zijn niet verpakt met de service; u moet uw certificaten afzonderlijk via Azure portal uploaden.

Geef een naam voor het certificaat voor het koppelen van een certificaat met de rol. U gebruikt deze naam om te verwijzen naar het certificaat wanneer u een HTTPS-eindpunt configureren op de **eindpunten** pagina. Daarna geeft u of het certificaatarchief is **lokale Machine** of **huidige gebruiker** en de naam van de store. Voer ten slotte de vingerafdruk van het certificaat. Als het certificaat in de huidige User\Personal (My) store is, kunt u de vingerafdruk van het certificaat door het selecteren van het certificaat van een gevulde lijst met invoeren. Als deze zich in een andere locatie bevindt, voert u de vingerafdrukwaarde handmatig in.

Wanneer u een certificaat uit het certificaatarchief toevoegt, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Bovendien moeten deze tussenliggende certificaten worden geüpload naar Azure correct configureren van uw service voor SSL.

Van beheercertificaten die u aan uw service koppelt van toepassing op uw service alleen wanneer deze wordt uitgevoerd in de cloud. Wanneer de service wordt uitgevoerd in de lokale ontwikkelomgeving, gebruikt een standaard-certificaat dat wordt beheerd door de rekenemulator.
