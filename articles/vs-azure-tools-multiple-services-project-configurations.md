---
title: Configureren van uw Azure-project met behulp van serviceconfiguraties met meerdere | Microsoft Docs
description: Informatie over het configureren van een Azure-cloud service-project met wijzigen van de bestanden ServiceDefinition.csdef en ServiceConfiguration.cscfg.
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
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: deb69101e855bcad56b9212736c52ace72631f0a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Uw Azure-Project met behulp van serviceconfiguraties met meerdere configureren
Een Azure-cloud service-project bevat twee configuratiebestanden: ServiceDefinition.csdef en ServiceConfiguration.cscfg. Deze bestanden zijn verpakt met uw Azure-cloud service-toepassing en geïmplementeerd in Azure.

* De **ServiceDefinition.csdef** bestand bevat de metagegevens die is door de Azure-omgeving voor de vereisten van uw servicetoepassing cloud vereist, met inbegrip van welke rollen deze bevat. Dit bestand bevat ook configuratieinstellingen die voor alle exemplaren gelden. Deze configuratie-instellingen kunnen worden gelezen tijdens runtime met de Azure-Service die als host fungeert Runtime-API. Dit bestand kan niet worden bijgewerkt terwijl uw service wordt uitgevoerd in Azure.
* De **ServiceConfiguration.cscfg** bestand waarden ingesteld voor de configuratie-instellingen gedefinieerd in het servicedefinitiebestand en geeft het aantal exemplaren worden uitgevoerd voor elke rol. Dit bestand kan worden bijgewerkt terwijl uw cloudservice wordt uitgevoerd in Azure.

De Azure-hulpprogramma's voor Microsoft Visual Studio bevatten eigenschappenpagina's die u gebruiken kunt om in te stellen van configuratie-instellingen opgeslagen in de volgende bestanden. Voor toegang tot de eigenschappenpagina's, dubbelklik op de rol verwijzing onder het Azure-cloud service-project in Solution Explorer met de rechtermuisknop op de rol-verwijzing en kies of **eigenschappen**, zoals wordt weergegeven in de volgende afbeelding.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Zie voor informatie over de onderliggende schema's voor de servicedefinitie en de configuratiebestanden van de service, de [schemaverwijzing](https://msdn.microsoft.com/library/azure/dd179398.aspx). Zie voor meer informatie over de serviceconfiguratie van de [Cloud-Services configureren hoe](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Eigenschappen van de rol configureren
De eigenschappenpagina's voor een Webrol en een werkrol zijn vergelijkbaar, maar er een paar verschillen zijn, uiteengezet in de volgende secties.

Van de **opslaan in cache** pagina kunt u de Azure caching-services configureren.

### <a name="configuration-page"></a>Configuratiepagina
Op de **configuratie** pagina kunt u deze eigenschappen instellen:

**Exemplaren**

Stel de **exemplaar** eigenschap om het aantal exemplaren voor deze rol moet worden uitgevoerd door de service te tellen.

Stel de **VM-grootte** eigenschap **Extra klein**, **kleine**, **gemiddeld**, **grote**, of **Extra groot**.  Zie voor meer informatie [grootten voor Cloudservices](cloud-services/cloud-services-sizes-specs.md).

**Opstartactie** (rol alleen Web)

Stel deze eigenschap om op te geven dat een webbrowser voor de HTTP-eindpunten of de HTTPS-eindpunten of beide in Visual Studio moet worden gestart als u foutopsporing start.

De HTTPS-eindpunt-optie is alleen beschikbaar als u al een HTTPS-eindpunt voor de rol hebt gedefinieerd. U kunt een HTTPS-eindpunt definiëren voor de **eindpunten** eigenschappenpagina.

Als u al een HTTPS-eindpunt hebt toegevoegd, de HTTPS-eindpunt-optie is standaard ingeschakeld en Visual Studio een browser voor dit eindpunt wordt gestart bij het starten van foutopsporing, naast een browser voor uw HTTP-eindpunt. Hierbij wordt ervan uitgegaan dat beide opstartopties zijn ingeschakeld.

**Diagnostics**

Diagnostische gegevens is standaard ingeschakeld voor de Webrol. Het account Azure-cloud service-project en opslag worden ingesteld op de emulator van de lokale opslag gebruiken. Wanneer u klaar om te implementeren naar Azure bent, kunt u de knop builder (**...** ) bijwerken van het opslagaccount voor het gebruik van Azure-opslag in de cloud. U kunt de diagnostics-gegevens overbrengen naar het storage-account op verzoek of regelmatig automatisch. Zie voor meer informatie over Azure diagnostics [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Pagina met instellingen
Op de **instellingen** pagina kunt u de configuratie-instellingen voor uw service toevoegen. Configuratie-instellingen zijn naam / waarde-paren. Code die wordt uitgevoerd in de rol vindt de waarden van uw configuratie-instellingen bij uitvoering met klassen die worden geleverd door de [Azure beheerde bibliotheek](http://go.microsoft.com/fwlink?LinkID=171026). In het bijzonder de [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) methode retourneert de waarde van een benoemde configuratie-instelling tijdens runtime.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Een verbindingsreeks voor een opslagaccount configureren
Een verbindingsreeks is een configuratie-instelling waarmee verbinding en verificatiegegevens voor de opslagemulator of voor een Azure storage-account. Wanneer uw code toegang moet hebben tot Azure storage services gegevens – dat wil zeggen, blob, wachtrij of tabelgegevens van de code die wordt uitgevoerd in een rol, moet u Definieer een verbindingsreeks voor dat opslagaccount.

Een verbindingsreeks die naar een Azure storage-account verwijst moet een gedefinieerde indeling gebruiken. Zie voor meer informatie over het maken van verbindingsreeksen [Azure Storage-verbindingsreeksen configureren](storage/common/storage-configure-connection-string.md).

Wanneer u klaar bent voor het testen van uw service op basis van de Azure storage-services, of wanneer u klaar bent voor uw cloudservice implementeren in Azure, kunt u de waarde van eventuele verbindingstekenreeksen om te verwijzen naar uw Azure storage-account. Selecteer (**...** ), selecteer **opslagaccountreferenties Voer**. Voer uw accountgegevens die uw accountnaam en accountsleutel bevat. In de **Storage Account Connection String** in het dialoogvenster kunt u ook aangeven of u wilt gebruiken de standaard HTTPS-eindpunten (standaardoptie), de standaard HTTP-eindpunten of aangepaste eindpunten. U kunt aangepaste eindpunten gebruiken als u een aangepaste domeinnaam voor uw service geregistreerd zoals beschreven in [een aangepaste domeinnaam configureren voor blob-gegevens in Azure storage-account](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> De verbindingsreeksen om te verwijzen naar een Azure storage-account voordat u uw service implementeert, moet u wijzigen. Om dit te doen mislukken, kan dit ertoe leiden dat uw rol bij het starten of doorloopt u de statussen initialiseren bezet en stoppen.
> 
> 

## <a name="endpoints-page"></a>Pagina eindpunten
Een werkrol kan een onbeperkt aantal HTTP-, HTTPS- of TCP-eindpunten hebben. Eindpunten kunnen invoereindpunten naar externe clients beschikbaar zijn, of interne eindpunten die beschikbaar zijn voor andere functies die worden uitgevoerd in de service zijn.

* Wijzigen van het eindpunttype voor het invoeren van een HTTP-eindpunt om beschikbaar te maken naar externe clients en webbrowsers, en geef een naam en een openbare-poortnummer.
* Als u een HTTPS-eindpunt naar externe clients en webbrowsers, wijzig het eindpunttype **invoer**, en geef een naam, een openbare-poortnummer en de naam van een management-certificaat.
  
    Houd er rekening mee dat voordat u een beheercertificaat opgeven kunt, moet u het certificaat op de **certificaten** eigenschappenpagina.
* Als u een eindpunt beschikbaar voor interne toegang door andere functies in de cloudservice, het wijzigen van het eindpunttype met interne en geef een naam en mogelijke particuliere poorten voor dit eindpunt.

## <a name="local-storage-page"></a>Lokale opslag-pagina
U kunt de **lokale opslag** eigenschappenpagina te reserveren van een of meer lokale opslagresources voor een rol. Een lokale opslagbron is een gereserveerde map in het bestandssysteem van de virtuele machine van Azure waarop een exemplaar van een rol wordt uitgevoerd.

## <a name="certificates-page"></a>De pagina Certificaten
Op de **certificaten** pagina u certificaten kunt koppelen aan uw rol. De certificaten die u toevoegt, kunnen worden gebruikt voor het HTTPS-eindpunten configureren op de **eindpunten** eigenschappenpagina.

De **certificaten** eigenschappenpagina gegevens over uw certificaten worden toegevoegd aan de configuratie van uw service. Houd er rekening mee dat uw certificaten niet zijn verpakt met uw service; u moet uw certificaten afzonderlijk uploaden naar Azure via de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).

Als u wilt verbinden met een certificaat met de rol, Geef een naam voor het certificaat. Gebruikt u deze naam om te verwijzen naar het certificaat, wanneer u een HTTPS-eindpunt configureren op de **eindpunten** eigenschappenpagina. Vervolgens opgeven of het certificaatarchief **lokale Machine** of **huidige gebruiker** en de naam van het archief. Voer ten slotte de vingerafdruk van het certificaat. Als het certificaat zich in de huidige User\Personal () winkel, kunt u de vingerafdruk van het certificaat door het certificaat van een ingevulde lijst te selecteren. Als deze zich op een andere locatie, voert u de vingerafdrukwaarde handmatig in.

Wanneer u een certificaat uit het certificaatarchief toevoegt, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten ook worden geüpload naar Azure om uw service voor SSL de juiste wijze configureren.

Van beheercertificaten die u aan uw service koppelen met uw service alleen toegepast wanneer deze wordt uitgevoerd in de cloud. Wanneer uw service wordt uitgevoerd in de lokale ontwikkelingsomgeving, gebruikt een standaardcertificaat dat wordt beheerd door de rekenemulator.

## <a name="configuring-the-azure-cloud-service-project"></a>Het Azure-cloudserviceproject configureren
U eerst het snelmenu voor dat projectknooppunt openen voor het configureren van instellingen die voor een volledige Azure-cloud service-project gelden en kies Eigenschappen te openen van de eigenschappenvensters. De volgende tabel ziet de eigenschappenpagina's.

| Eigenschappenpagina | Beschrijving |
| --- | --- |
| Toepassing |Op deze pagina kunt u informatie over de versie van Azure-hulpprogramma's die deze cloudserviceproject gebruikt en u kunt upgraden naar de huidige versie van de hulpprogramma's weergeven. |
| Gebeurtenissen maken |Op deze pagina kunt u gebeurtenissen vóór het samenstellen en na het samenstellen instellen. |
| Ontwikkeling |Op deze pagina kunt u configuratie-instructies build en de voorwaarden waaronder alle gebeurtenissen na build worden uitgevoerd. |
| Web |Op deze pagina kunt u de instellingen die betrekking op de webserver hebben configureren. |

