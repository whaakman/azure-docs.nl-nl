---
title: Het gebruik van de Azure Mobile Apps SDK voor Android | Microsoft Docs
description: Het gebruik van de Azure Mobile Apps SDK voor Android
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/16/2017
ms.author: crdun
ms.openlocfilehash: c0e6aa34b80389689e49ac6ad3566a3a109a96e1
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158159"
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a>Het gebruik van de Azure Mobile Apps SDK voor Android

Deze handleiding wordt beschreven hoe u de Android SDK voor Mobile Apps-client gebruikt voor het implementeren van algemene scenario's, zoals:

* Opvragen van gegevens (invoegen, bijwerken en verwijderen).
* -Verificatie.
* Fouten afhandelen.
* Aanpassing van de client.

Deze handleiding is gericht op de client-side Android SDK.  Zie voor meer informatie over de server-side SDK's voor Mobile Apps, [werken met .NET-backend SDK] [ 10] of [over het gebruik van de back-end-Node.js SDK][11].

## <a name="reference-documentation"></a>Referentiedocumentatie voor

U vindt de [Javadocs API-verwijzing] [ 12] voor de Android-client-bibliotheek op GitHub.

## <a name="supported-platforms"></a>Ondersteunde Platforms

De Azure Mobile Apps SDK voor Android biedt ondersteuning voor API-niveaus 19 tot 24 uur per dag (KitKat via Nougat) voor telefoons en tablets vormfactoren.  Verificatie in het bijzonder maakt gebruik van een benadering van de algemene framework voor het verzamelen van referenties.  Werkstroom voor Server-verificatie werkt niet met een klein formulier factor apparaten zoals Watch.

## <a name="setup-and-prerequisites"></a>Installatie en vereisten

Voltooi de [Mobile Apps quickstart](app-service-mobile-android-get-started.md) zelfstudie.  Deze taak zorgt ervoor dat alle vereisten voor het ontwikkelen van Azure Mobile Apps is voldaan.  De Quick Start helpt u bij het configureren van uw account en maak uw eerste back-end van Mobile App.

Als u besluit niet aan de Quick Start-zelfstudie hebt voltooid, moet u de volgende taken uitvoeren:

* [Maak een back-end van Mobile App] [ 13] voor gebruik met uw Android-app.
* In Android Studio [update de Gradle bouwen bestanden](#gradle-build).
* [Inschakelen toestemming internet](#enable-internet).

### <a name="gradle-build"></a>De Gradle-buildbestand bijwerken

Beide wijzigen **build.gradle** bestanden:

1. Deze code toevoegen aan de *Project* niveau **build.gradle** bestand in de *buildscript* tag:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Deze code toevoegen aan de *Module app* niveau **build.gradle** bestand in de *afhankelijkheden* tag:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    ```

    De meest recente versie is momenteel 3.4.0. De ondersteunde versies zijn vermeld [op bintray][14].

### <a name="enable-internet"></a>Internet-machtiging inschakelen

Voor toegang tot Azure, moet uw app in de INTERNET-machtiging ingeschakeld hebben. Als deze nog niet ingeschakeld, voeg de volgende regel code naar uw **AndroidManifest.xml** bestand:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>Maak een clientverbinding

Azure Mobile Apps biedt vier functies voor uw mobiele App:

* Toegang tot gegevens en Offline synchronisatie met een Azure Mobile Apps-Service.
* Aangepaste API's die zijn geschreven met de Azure Mobile Apps Server SDK aanroepen.
* Verificatie met Azure App Service-verificatie en autorisatie.
* Registratie bij Notification Hubs voor pushmeldingen.

Elk van deze functies eerst is vereist dat u maakt een `MobileServiceClient` object.  Slechts één `MobileServiceClient` object moet worden gemaakt in uw mobiele client (dat wil zeggen, er moet een Singleton-patroon).  Maakt een `MobileServiceClient` object:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

De `<MobileAppUrl>` is een tekenreeks of een URL-object dat naar uw mobiele back-end verwijst.  Als u van Azure App Service gebruikmaakt voor het hosten van uw mobiele back-end, klikt u vervolgens Zorg ervoor dat u de beveiligde `https://` versie van de URL.

De client is ook vereist voor toegang tot de activiteit of het Context - de `this` parameter in het voorbeeld.  De bouw MobileServiceClient moet gebeuren in de `onCreate()` methode van de activiteit waarnaar wordt verwezen in de `AndroidManifest.xml` bestand.

Als een best practice, moet u communicatie tussen de server in een eigen (singleton-patroon)-klasse abstract.  In dit geval moet u de activiteit in de constructor voor het configureren van de service op de juiste wijze doorgeven.  Bijvoorbeeld:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public class AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

U kunt nu aanroepen `AzureServiceAdapter.Initialize(this);` in de `onCreate()` -methode van uw belangrijkste activiteit.  Een andere manier toegang tot het gebruik van de client hoeven `AzureServiceAdapter.getInstance();` verkrijgen van een verwijzing naar de service-adapter.

## <a name="data-operations"></a>Gegevensbewerkingen

De kern van de Azure Mobile Apps SDK is voor toegang tot gegevens die zijn opgeslagen in SQL Azure op de back-end van de mobiele App.  U kunt toegang tot deze gegevens met behulp van sterk getypeerde klassen (bij voorkeur) of getypeerde query's (niet aanbevolen).  Het grootste deel van deze sectie behandelt met behulp van sterk getypeerde klassen.

### <a name="define-client-data-classes"></a>Definiëren van gegevensklassen client

Voor toegang tot gegevens uit SQL Azure-tabellen, client gegevensklassen die overeenkomen met de tabellen in de mobiele App back-end te definiëren. Voorbeelden in dit onderwerp wordt ervan uitgegaan dat een tabel met de naam **MyDataTable**, heeft de volgende kolommen:

* id
* tekst
* Voltooien

Het bijbehorende getypte client-side-object zich bevindt in een bestand met de naam **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Methoden voor elk veld dat u toevoegt getter en setter toevoegen.  Als uw SQL Azure-tabel meer kolommen bevat, zou u de bijbehorende velden toevoegen aan deze klasse.  Bijvoorbeeld, als het DTO (data transfer object) was een kolom met gehele getallen prioriteit, en vervolgens kunt u dit veld, samen met de methoden getter en setter toevoegen:

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

Zie voor meer informatie over het maken van aanvullende tabellen in uw back-end van Mobile Apps, [het: Een controller voor de tabel definieert] [ 15] (.NET-back-end) of [definiëren tabellen met behulp van een dynamische Schema] [ 16] (Node.js-back-end).

Een Azure Mobile Apps-back-end-tabel definieert vijf speciale velden, vier die beschikbaar voor clients zijn:

* `String id`: De unieke ID voor de record.  Als een best practice, Controleer de id de tekenreeksweergave van een [UUID] [ 17] object.
* `DateTimeOffset updatedAt`: De datum/tijd van de laatste update.  Het veld updatedAt is ingesteld door de server en nooit door uw clientcode moet worden ingesteld.
* `DateTimeOffset createdAt`: De datum/tijd die het object is gemaakt.  Het veld createdAt is ingesteld door de server en nooit door uw clientcode moet worden ingesteld.
* `byte[] version`: Normaal gesproken weergegeven als een tekenreeks, is de versie ook ingesteld door de server.
* `boolean deleted`: Geeft aan dat de record is verwijderd, maar nog niet is verwijderd.  Gebruik geen `deleted` als een eigenschap in uw klasse.

Het veld `id` is vereist.  De `updatedAt` veld en `version` veld worden gebruikt voor offline synchronisatie (voor incrementele synchronisatie en een conflict resolutie respectievelijk).  De `createdAt` veld is een veld en wordt niet gebruikt door de client.  De namen zijn 'in-the-wire' namen van de eigenschappen en zijn niet aanpasbaar.  U kunt echter een toewijzing maken tussen uw object en de namen van de 'in-the-wire' met behulp van de [gson] [ 3] bibliotheek.  Bijvoorbeeld:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getCreatedAt() { return mCreatedAt; }
    protected void setCreatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected void setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getVersion() { return mVersion; }
    public final void setVersion(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>Maken van een tabelverwijzing

Voor toegang tot een tabel, maakt u eerst een [MobileServiceTable] [ 8] object door het aanroepen van de **getTable** methode voor het [MobileServiceClient] [9].  Deze methode heeft twee overloads:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

In de volgende code **ook mClient** is een verwijzing naar uw MobileServiceClient-object.  De eerste overbelasting wordt gebruikt voor naam van de klasse en de naam van de tabel hetzelfde zijn, waarbij de in de Quick Start wordt gebruikt:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

De tweede overbelasting wordt gebruikt wanneer de tabelnaam af van de naam van de klasse wijkt: de eerste parameter is de naam van de tabel.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Query uitvoeren op een back-end-tabel

Eerst moet u een tabelverwijzing.  Voer een query op de tabelverwijzing.  Een query is een combinatie van:

* Een `.where()` [filtercomponent](#filtering).
* Een `.orderBy()` [bestellen component](#sorting).
* Een `.select()` [veld selectiecomponent](#selection).
* Een `.skip()` en `.top()` voor [wisselbaar geheugen: resultaten](#paging).

De componenten moeten worden weergegeven in de voorgaande bewerkingsvolgorde.

### <a name="filter"></a> Filteren van resultaten

De algemene vorm van een query is:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

Het vorige voorbeeld retourneert alle resultaten (maximaal de maximale paginagrootte ingesteld door de server).  De `.execute()` methode voert u de query op de back-end.  De query wordt geconverteerd naar een [OData v3] [ 19] query voor verzending naar de back-end van Mobile Apps.  Bij ontvangst zet de back-end van Mobile Apps de query in een SQL-instructie voordat deze wordt uitgevoerd op de SQL Azure-instantie.  Sinds de netwerkactiviteit duurt enige tijd, de `.execute()` methode retourneert een [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Geretourneerde gegevens filteren

De uitvoering van de volgende query retourneert alle items uit de **ToDoItem** waar tabel **voltooid** gelijk is aan **false**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** is de verwijzing naar de tabel van de mobiele service die we eerder hebben gemaakt.

Definieer een filter met de **waar** methodeaanroep in de tabelverwijzing. De **waar** methode wordt gevolgd door een **veld** methode gevolgd door een methode die Hiermee geeft u het logische-predicaat. Mogelijke predicaat methoden zijn onder meer **eq** (gelijk aan), **ne** (niet gelijk aan), **gt** (groter dan), **ge** (groter dan of gelijk aan), **lt** (minder dan,) **le** (kleiner dan of gelijk aan). Deze methoden kunnen u het aantal en de tekenreeks velden om specifieke waarden te vergelijken.

U kunt filteren op datums. De volgende methoden kunnen u de volledige datumveld of delen van de datum vergelijken: **jaar**, **maand**, **dag**, **uur**,  **minuut**, en **tweede**. Het volgende voorbeeld wordt een filter voor artikelen waarvan *vervaldatum* gelijk is aan 2013.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

De volgende methoden ondersteuning voor complexe filters op tekenreeksvelden: **startsWith**, **endsWith**, **concat**, **subtekenreeks**, **indexOf**, **vervangen**, **toLower**, **toUpper**, **trim**, en **lengte** . Het volgende voorbeeld van de filters voor de tabel rijen waar de *tekst* kolom begint met "PRI0."

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

De volgende methoden voor de operator op numerieke velden worden ondersteund: **toevoegen**, **sub**, **mul**, **div**, **mod**, **floor**, **maximum**, en **afronden**. Het volgende voorbeeld van de filters voor de tabel rijen waar de **duur** een even getal is.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

U kunt predikaten met deze logische methoden combineren: **en**, **of** en **niet**. Het volgende voorbeeld combineert twee van de voorgaande voorbeelden.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Groep en geneste logische operators:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Zie voor meer informatie en voorbeelden van filteren, [verkennen de rijkdom van het model van de query voor Android-client][20].

### <a name="sorting"></a>Geretourneerde gegevens sorteren

De volgende code retourneert alle items uit een tabel met **ToDoItems** door oplopend gesorteerd de *tekst* veld. *mToDoTable* is de verwijzing naar de back endtabel die u eerder hebt gemaakt:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

De eerste parameter van de **orderBy** methode is een tekenreeks die gelijk is aan de naam van het veld waarop u wilt sorteren. De tweede parameter gebruikt de **QueryOrder** inventarisatie kunt u opgeven of oplopend of Aflopend sorteren.  Als u filtert met behulp van de ***waar*** methode, de ***waar*** methode moet worden aangeroepen voordat de ***orderBy*** methode.

### <a name="selection"></a>Specifieke kolommen selecteren

De volgende code laat zien hoe om te retourneren van alle items uit een tabel met **ToDoItems**, maar worden alleen de **voltooid** en **tekst** velden. **mToDoTable** is de verwijzing naar de back endtabel die we eerder hebben gemaakt.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

De parameters voor de select-functie zijn de tekenreeksnamen van de van de tabel kolommen die u wilt terugkeren.  De **Selecteer** methode moet volgen methoden, zoals **waar** en **orderBy**. Het kan worden gevolgd door wisselgeheugengebruik methoden, zoals **overslaan** en **boven**.

### <a name="paging"></a>Als resultaat de gegevens op pagina 's

Gegevens zijn **altijd** geretourneerd op pagina's.  Het maximum aantal records dat wordt geretourneerd is door de server ingesteld.  Als de client meer records vraagt, retourneert de server het maximum aantal records.  De maximale grootte op de server is standaard 50 records.

Het eerste voorbeeld laat zien hoe de top vijf items selecteren uit een tabel. De query retourneert de items van een tabel van **ToDoItems**. **mToDoTable** is de verwijzing naar de back endtabel die u eerder hebt gemaakt:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Hier volgt een query die de eerste vijf items wordt overgeslagen en retourneert vervolgens de volgende vijf:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Als u ophalen van alle records in een tabel wilt, moet u code om te herhalen op alle pagina's implementeert:

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

Een aanvraag voor alle records met deze methode maakt een minimum van twee aanvragen naar de back-end van Mobile Apps.

> [!TIP]
> Het formaat van de juiste keuze is een balans tussen geheugengebruik terwijl de aanvraag plaatsvindt, bandbreedtegebruik en vertragingen in het ontvangen van de gegevens volledig.  De standaardwaarde (50 records) is geschikt voor alle apparaten.  Als u werkt met uitsluitend op grotere geheugenapparaten, verhoogt u maximaal 500.  We hebben vastgesteld dat verhoging van de grootte van meer dan 500 records leidt tot onaanvaardbare vertragingen en problemen met grote geheugens.

### <a name="chaining"></a>Procedures: Methoden voor query's samenvoegen

De methoden die worden gebruikt in het back-end-tabellen kunnen worden samengevoegd. Querymethoden-koppeling, kunt u specifieke kolommen van gefilterde rijen gesorteerd en wisselbaar geheugen: selecteren. U kunt complexe logische filters maken.  Elke querymethode retourneert een Query-object. Aanroepen voor het einde van de reeks methoden en daadwerkelijk uitvoeren van de query, de **uitvoeren** methode. Bijvoorbeeld:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

De querymethoden gekoppelde moeten als volgt worden gerangschikt:

1. Filteren (**waar**) methoden.
2. Sorteren (**orderBy**) methoden.
3. Selectie (**Selecteer**) methoden.
4. voor het wisselbestand (**overslaan** en **boven**) methoden.

## <a name="binding"></a>Gegevens binden aan de gebruikersinterface

Gegevensbinding bestaat uit drie onderdelen:

* De gegevensbron
* De indeling van het startscherm
* De adapter die de twee gebundeld.

In ons voorbeeld van code, wordt de gegevens uit de mobiele Apps SQL Azure-tabel geretourneerd **ToDoItem** in een matrix. Deze activiteit wordt een algemeen patroon voor data-toepassingen.  Database-query's retourneren vaak een verzameling rijen die de client in een lijst of een matrix ophaalt. In dit voorbeeld is de matrix de gegevensbron.  De code wordt een schermindeling die definieert de weergave van de gegevens die wordt weergegeven op het apparaat.  Afhankelijk van de twee samen met een adapter, die in deze code een uitbreiding is van de **ArrayAdapter&lt;ToDoItem&gt;**  klasse.

#### <a name="layout"></a>De indeling definiëren

De lay-out is gedefinieerd door enkele codefragmenten van XML-code. Een bestaande indeling wordt opgegeven, de volgende code geeft de **ListView** we wilt vullen met gegevens van onze server.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

In de vorige code de *listitem* kenmerk geeft de id van de indeling voor een afzonderlijke rij in de lijst. Deze code geeft een selectievakje in en de bijbehorende tekst en eenmaal voor elk item in de lijst wordt geïnstantieerd. Deze lay-out niet wordt weergegeven de **id** veld en een complexere indeling aanvullende velden in de weergave wilt opgeven. Deze code is in de **row_list_to_do.xml** bestand.

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="adapter"></a>De adapter definiëren
Omdat de gegevensbron van de weergave een matrix van is **ToDoItem**, we subklasse onze-adapter van een **ArrayAdapter&lt;ToDoItem&gt;**  klasse. Deze subcategorie produceert een weergave voor elke **ToDoItem** met behulp van de **row_list_to_do** lay-out.  In de code, definiëren we de volgende klasse die is een uitbreiding van de **ArrayAdapter&lt;E&gt;**  klasse:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Onderdrukking van de adapters **getView** methode. Bijvoorbeeld:

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

We maken een exemplaar van deze klasse in onze activiteit als volgt:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

De tweede parameter aan de constructor ToDoItemAdapter is een verwijzing naar de lay-out. We kunnen nu exemplaar maken van de **ListView** en toewijzen van de adapter op de **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Gebruik van de Adapter te binden aan de gebruikersinterface

U bent nu klaar voor gebruik van gegevensbinding. De volgende code laat zien hoe u items in de tabel ophalen en op de lokale adapter met de geretourneerde items.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

De adapter aanroepen telkens wanneer u wijzigt de **ToDoItem** tabel. Omdat wijzigingen worden uitgevoerd op basis van een door een andere record, kunt u een enkele rij in plaats van een verzameling verwerken. Wanneer u een item invoegt, roept de **toevoegen** methode op de adapter; wanneer te verwijderen, roept de **verwijderen** methode.

U vindt een compleet voorbeeld in de [Android Quick Start-Project][21].

## <a name="inserting"></a>Gegevens invoegen in de back-end

Een instantie van de *ToDoItem* klasse en de eigenschappen instellen.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Gebruik vervolgens **insert()** een object in te voegen:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

De geretourneerde entiteit komt overeen met de gegevens ingevoegd in de endtabel back-van de ID en andere waarden opgenomen (zoals de `createdAt`, `updatedAt`, en `version` velden) instellen op de back-end.

Mobile Apps-tabellen moet een primaire-sleutelkolom met de naam **id**. Deze kolom moet een tekenreeks zijn. De standaardwaarde van de kolom-ID is een GUID.  U kunt andere unieke waarden, zoals e-mailadressen of gebruikersnamen opgeven. Als een tekenreeks-ID-waarde niet is opgegeven voor een record ingevoegd, wordt een nieuwe GUID gegenereerd door de back-end.

Tekenreeks-ID's bieden de volgende voordelen:

* Id's kunnen worden gegenereerd zonder een retour naar de database.
* Records zijn gemakkelijker te samenvoegen uit andere tabellen of databases.
* Id-waarden integratie betere met de logica van een toepassing.

Tekenreeks-ID-waarden zijn **vereist** voor ondersteuning voor offlinesynchronisatie.  U kunt een Id niet wijzigen nadat deze is opgeslagen in de back enddatabase.

## <a name="updating"></a>Gegevens in een mobiele app bijwerken

Voor het bijwerken van gegevens in een tabel, geeft u het nieuwe object aan de **update()** methode.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

In dit voorbeeld *item* is een verwijzing naar een rij in de *ToDoItem* het aantal wijzigingen aangebracht in deze heeft tabel.  De rij met dezelfde **id** wordt bijgewerkt.

## <a name="deleting"></a>Verwijderen van gegevens in een mobiele app

De volgende code toont hoe u gegevens uit een tabel verwijderen door het object op te geven.

```java
mToDoTable
    .delete(item);
```

U kunt ook een item verwijderen door op te geven de **id** veld van de rij te verwijderen.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Een specifiek item door-Id opzoeken

Opzoeken van een item met een specifieke **id** veld met de **lookUp()** methode:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Procedures: Werken met niet-getypeerde gegevens

De niet-getypeerde programmeermodel hebt u exacte controle over de JSON-serialisatie.  Er zijn enkele algemene scenario's waarin u kunt desgewenst een niet-getypeerde programmeermodel gebruiken. Bijvoorbeeld, als uw back endtabel veel kolommen bevat en u hoeft alleen te verwijzen naar een subset van de kolommen.  De getypte model, moet u de kolommen die zijn gedefinieerd in de back-end van Mobile Apps in uw gegevensklasse definiëren.  De meeste van de API-aanroepen voor toegang tot gegevens zijn vergelijkbaar met de getypte programming aanroepen. Het belangrijkste verschil is dat in het model niet-getypeerde u methoden aanroepen op de **MobileServiceJsonTable** -object, in plaats van de **MobileServiceTable** object.

### <a name="json_instance"></a>Maak een instantie van een niet-getypeerde tabel

Net als bij de getypte model, u beginnen met het ophalen van een tabelverwijzing, maar in dit geval is het een **MobileServicesJsonTable** object. De verwijzing verkrijgen door het aanroepen van de **getTable** methode op een exemplaar van de client:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Nadat u hebt gemaakt dat een exemplaar van de **MobileServiceJsonTable**, is vrijwel dezelfde API beschikbaar als met de getypte programmeermodel. In sommige gevallen kan maken de methoden gebruik van een niet-getypeerde parameter in plaats van een type parameter.

### <a name="json_insert"></a>In een niet-getypeerde tabel invoegen
De volgende code toont hoe u een insert doet. De eerste stap is het maken van een [JsonObject][1], die deel uitmaakt van de [gson] [ 3] bibliotheek.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Vervolgens gebruikt u **insert()** de niet-getypeerde object in de tabel invoegen.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Als u nodig hebt om op te halen van de ID van het object ingevoegd, gebruikt u de **getAsJsonPrimitive()** methode.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Verwijderen uit een niet-getypeerde tabel
De volgende code laat zien hoe u verwijderen van een instantie, in dit geval hetzelfde exemplaar van een **JsonObject** die is gemaakt in de voorafgaande *invoegen* voorbeeld. De code is hetzelfde als bij de ingevoerde aanvraag, maar de methode heeft een andere handtekening omdat deze verwijst naar een **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

U kunt ook een exemplaar rechtstreeks door met de bijbehorende id. verwijderen:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Retourneert alle rijen uit een niet-getypeerde tabel
De volgende code toont hoe u kunt een hele tabel ophalen. Omdat u van een JSON-tabel gebruikmaakt, kunt u slechts enkele van de tabel kolommen selectief ophalen.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

De dezelfde set filteren, filteren en pagineren methoden die beschikbaar voor de getypte model zijn zijn beschikbaar voor de niet-getypeerde model.

## <a name="offline-sync"></a>Offlinesynchronisatie implementeren

Offline synchronisatie van gegevens van implementeert de Client-SDK van Azure Mobile Apps ook met behulp van een SQLite-database voor het opslaan van een kopie van de servergegevens lokaal.  Bewerkingen die worden uitgevoerd op een offline-tabel vereisen geen mobiele connectiviteit te werken.  Offline synchronisatie op hardwareniveau, bij de flexibiliteit en prestaties ten koste van de complexere logica voor conflictoplossing.  De Client-SDK van Azure Mobile Apps implementeert de volgende functies:

* Incrementele synchronisatie: Alleen worden bijgewerkt en nieuwe records gedownload verbruik van bandbreedte en geheugen.
* Optimistische gelijktijdigheid: Bewerkingen wordt aangenomen dat te voltooien.  Conflictoplossing is uitgesteld totdat de updates worden uitgevoerd op de server.
* Conflictoplossing: De SDK detecteert wanneer een conflicterende wijziging is aangebracht op de server en hooks biedt voor de gebruiker waarschuwen.
* Voorlopig verwijderen: Verwijderde records zijn gemarkeerd als verwijderd, zodat andere apparaten hun offline configuratiecache bij te werken.

### <a name="initialize-offline-sync"></a>Offline synchronisatie initialiseren

Elke tabel offline moet worden gedefinieerd in de offline cache vóór gebruik.  Definitie van de tabel wordt gewoonlijk gedaan onmiddellijk na het maken van de client:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-to-the-offline-cache-table"></a>Een verwijzing naar de tabel Offline Cache ophalen

Voor een online-tabel, gebruikt u `.getTable()`.  Voor een offline-tabel, gebruikt u `.getSyncTable()`:

```java
MobileServiceSyncTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

De methoden die beschikbaar voor online-tabellen zijn (inclusief filteren, sorteren, paging, gegevens invoegen, gegevens bijwerken en verwijderen van gegevens) even goed werken op online en offline-tabellen.

### <a name="synchronize-the-local-offline-cache"></a>De lokale Cache voor het Offline synchroniseren

Synchronisatie is in het besturingselement van uw app.  Hier volgt een voorbeeld van de synchronisatie-methode:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

Als de naam van een query is opgegeven voor de `.pull(query, queryname)` methode dan incrementele synchronisatie wordt gebruikt om terug te keren alleen de records die zijn gemaakt of gewijzigd sinds de laatste met succes voltooid pull.

### <a name="handle-conflicts-during-offline-synchronization"></a>Conflicten tijdens het Offline synchroniseren

Als het geval is een conflict tijdens een `.push()` bewerking, een `MobileServiceConflictException` wordt gegenereerd.   Het item server uitgegeven is ingesloten in de uitzondering en kan worden opgehaald door `.getItem()` op de uitzondering.  De pushmelding aanpassen door het aanroepen van de volgende items op het object MobileServiceSyncContext:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

Wanneer alle conflicten zijn gemarkeerd als u wilt, aanroepen `.push()` opnieuw op te lossen alle conflicten.

## <a name="custom-api"></a>Een aangepaste API oproept

Een aangepaste API kunt u aangepaste eindpunten die beschikbaar maken van server-functionaliteit die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API, kunt u hebben meer controle over berichten, zoals lezen en HTTP-bericht headers instellen en definiëren van een indeling van de hoofdtekst van bericht dan JSON.

U aanroepen vanuit een Android-client, de **invokeApi** methode voor het aanroepen van de aangepaste API-eindpunt. Het volgende voorbeeld ziet over het aanroepen van een API-eindpunt met de naam **completeAll**, die de klasse van een verzameling met de naam retourneert **MarkAllResult**.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

De **invokeApi** methode wordt aangeroepen op de client, die een POST-aanvraag naar de nieuwe aangepaste API verzendt. Het resultaat geretourneerd door de aangepaste API wordt weergegeven in een dialoogvenster weergegeven als er fouten zijn. Andere versies van **invokeApi** kunt u eventueel verzenden van een object in de aanvraagtekst, geeft u de HTTP-methode en queryparameters met de aanvraag verzenden. Versies van getypeerde **invokeApi** ook worden opgegeven.

## <a name="authentication"></a>Verificatie toevoegen aan uw app

Het toevoegen van deze functies beschreven zelfstudies al in detail.

App Service ondersteunt [verifiëren van appgebruikers](app-service-mobile-android-get-started-users.md) met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in uw back-end.

Twee verificatiestromen worden ondersteund: een **server** stroom en een **client** stroom. De stroom van de server bevat de meest eenvoudige verificatie-ervaring, afhankelijk van de id-providers webinterface.  Er zijn geen extra SDK's zijn vereist voor het implementeren van server-verificatie voor flow. Serververificatie voor de stroom biedt geen een diepe integratie in het mobiele apparaat en wordt alleen aanbevolen voor het testen van concept-scenario's.

De stroom kunt diepere integratie met de apparaat-specifieke mogelijkheden, zoals eenmalige aanmelding, zoals afhankelijk van SDK's die worden geleverd door de id-provider.  U kunt bijvoorbeeld de Facebook-SDK integreren in uw mobiele App.  De mobiele client Hiermee wisselt u in de Facebook-app en bevestigt de aanmelding voor het wisselen van terug naar uw mobiele app.

Vier stappen zijn vereist voor het inschakelen van verificatie in uw app:

* Registreer uw app voor verificatie met een id-provider.
* Configureer de back-end van uw App Service.
* Tabelmachtigingen beperken voor geverifieerde gebruikers alleen op de back-end van de App Service.
* Verificatiecode op te geven toevoegen aan uw app.

U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen alleen geverifieerde gebruikers te beperken. U kunt ook de SID van een geverifieerde gebruiker gebruiken om te wijzigen van aanvragen.  Raadpleeg voor meer informatie, [aan de slag met verificatie] en de procedure voor Server-SDK-documentatie.

### <a name="caching"></a>Verificatie: Server Flow

De volgende code wordt een server flow-aanmelding met de Google-provider gestart.  Aanvullende configuratie is vereist vanwege de beveiligingsvereisten voor de Google-provider:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Voeg bovendien de volgende methode toe aan de hoofdklasse van de activiteit:

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

De `GOOGLE_LOGIN_REQUEST_CODE` gedefinieerd in uw belangrijkste activiteit wordt gebruikt voor de `login()` methode en binnen de `onActivityResult()` methode.  U kunt een uniek nummer, zolang de hetzelfde nummer wordt gebruikt binnen de `login()` methode en de `onActivityResult()` methode.  Als u de clientcode abstracte in een service-adapter (zoals eerder beschreven), moet u de juiste methoden aanroepen op de service-adapter.

Ook moet u het project voor customtabs configureren.  Eerst een Omleidings-URL opgeven.  Voeg het volgende codefragment `AndroidManifest.xml`:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Voeg de **redirectUriScheme** naar de `build.gradle` -bestand voor uw toepassing:

```text
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

Voeg tot slot `com.android.support:customtabs:23.0.1` aan de lijst met afhankelijkheden in de `build.gradle` bestand:

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Ophalen van de ID van de gebruiker is aangemeld vanaf een **MobileServiceUser** met behulp van de **getUserId** methode. Zie voor een voorbeeld van hoe u toekomstige aanroepen van de asynchrone aanmelding API's, [aan de slag met verificatie].

> [!WARNING]
> Het URL-schema vermeld is hoofdlettergevoelig.  Zorg ervoor dat alle instanties van `{url_scheme_of_you_app}` hoofdlettergevoelig.

### <a name="caching"></a>Verificatietokens cache

Verificatietokens caching, moet u voor het opslaan van de gebruikers-ID en het verificatietoken lokaal op het apparaat. De volgende keer dat de app wordt gestart, controleren van de cache, en als deze waarden aanwezig zijn, kunt u het logboek in procedure overslaan en rehydrate van de client met deze gegevens. Deze gegevens is echter gevoelig en moeten worden opgeslagen voor de veiligheid versleuteld in het geval de telefoon wordt gestolen.  U ziet een compleet voorbeeld van hoe u aan de verificatietokens cache in [verificatiesectie van de tokens in de Cache][7].

Wanneer u probeert een verlopen token wordt gebruikt, ontvangt u een *401-niet gemachtigd* antwoord. U kunt met behulp van filters-verificatiefouten verwerken.  Filters onderscheppen aanvragen naar de back-end van de App Service. De filtercode test van het antwoord voor een 401, activeert de aanmeldprocedure en hervat vervolgens de aanvraag die de 401 gegenereerd.

### <a name="refresh"></a>Vernieuwen van Tokens gebruiken

Het token dat is geretourneerd door de Azure App Service-verificatie en autorisatie heeft een gedefinieerde levensduur van één uur.  Als deze tijd is verstreken, moet u de gebruiker verifiëren.  Als u een lange levensduur hebben token dat u hebt ontvangen via clientstroom verificatie gebruikt, kunt u andere referenties met Azure App Service-verificatie en autorisatie met dezelfde token.  Een ander Azure App Service-token wordt gegenereerd met de levensduur van een nieuwe.

U kunt ook de provider voor het gebruik van Tokens vernieuwen registreren.  Een Token voor vernieuwen is niet altijd beschikbaar.  Aanvullende configuratie is vereist:

* Voor **Azure Active Directory**, een clientgeheim voor de Azure Active Directory-App configureren.  Geef het clientgeheim in Azure App Service bij het configureren van Azure Active Directory-verificatie.  Bij het aanroepen van `.login()`, doorgeven `response_type=code id_token` als een parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Voor **Google**, geven de `access_type=offline` als een parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Voor **Microsoft-Account**, selecteer de `wl.offline_access` bereik.

Als u wilt een vernieuwingstoken aanroepen `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

Als een best practice, moet u een filter die een 401-respons van de server detecteert en vernieuwen van het gebruikerstoken wil maken.

## <a name="log-in-with-client-flow-authentication"></a>Meld u aan met de verificatie van de Client-stroom

Het algemene proces voor logboekregistratie bij verificatie van de client-stroom is als volgt:

* Azure App Service-verificatie en autorisatie configureren als server-flow-verificatie.
* Integreer de verificatieprovider-SDK voor het produceren van een toegangstoken voor verificatie.
* Roep de `.login()` methode als volgt te werk (`result` moet een `AuthenticationResult`):

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Zie het voorbeeld van de volledige code in de volgende sectie.

Vervang de `onSuccess()` methode met wat u code wilt gebruiken op een geslaagde aanmelding.  De `{provider}` tekenreeks is een geldige provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, of **twitter**.  Als u aangepaste verificatie hebt geïmplementeerd, kunt u ook de code van de provider aangepaste verificatie gebruiken.

### <a name="adal"></a>Verificatie van gebruikers met de Active Directory Authentication Library (ADAL)

Gebruikers aanmelden bij uw toepassing met behulp van Azure Active Directory kunt u de Active Directory Authentication Library (ADAL). Met behulp van de aanmelding van een client flow is het vaak beter voor het gebruik van de `loginAsync()` methoden als het biedt een meer systeemeigen UX uiterlijk en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor AAD-aanmelding configureren door de [App Service configureren voor Active Directory-aanmelding] [ 22] zelfstudie. Zorg ervoor dat u de optionele stap voor het registreren van een systeemeigen clienttoepassing.
2. ADAL installeren door het wijzigen van uw build.gradle-bestand om op te nemen van de volgende definities:

    ```
    repositories {
        mavenCentral()
        flatDir {
            dirs 'libs'
        }
        maven {
            url "YourLocalMavenRepoPath\\.m2\\repository"
        }
    }
    packagingOptions {
        exclude 'META-INF/MSFTSIG.RSA'
        exclude 'META-INF/MSFTSIG.SF'
    }
    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        compile('com.microsoft.aad:adal:1.1.1') {
            exclude group: 'com.android.support'
        } // Recent version is 1.1.1
        compile 'com.android.support:support-v4:23.0.0'
    }
    ```

3. Voeg de volgende code aan uw toepassing, waardoor de volgende vervangingen:

    * Vervang **INSERT-instantie-HERE** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com.
    * Vervang **INSERT-RESOURCE-ID-HERE** met de client-ID voor de back-end van uw mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
    * Vervang **INSERT-CLIENT-ID-HERE** met de client-ID die u hebt gekopieerd uit de toepassing native client.
    * Vervang **INSERT-OMLEIDINGS-URI-HERE** met van uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met *https://contoso.azurewebsites.net/.auth/login/done*.

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="filters"></a>De Client-servercommunicatie aanpassen

Verbinding met de Client is normaal gesproken een eenvoudige HTTP-verbinding met behulp van de onderliggende HTTP-bibliotheek is opgegeven met de Android SDK.  Er zijn diverse redenen waarom u wilt wijzigen:

* Wilt u een alternatieve HTTP-clientbibliotheek gebruiken om aan te passen time-outs.
* Wilt u een voortgangsbalk bieden.
* U wilt toevoegen van een aangepaste header ter ondersteuning van API management-functionaliteit.
* Wilt u een mislukte reactie onderschept, zodat u om verificatie wordt uitgevoerd implementeren kunt.
* Wilt u back-end-aanvragen vastleggen in een analytics-service.

### <a name="using-an-alternate-http-library"></a>Met behulp van een andere HTTP-bibliotheek

Roep de `.setAndroidHttpClientFactory()` methode onmiddellijk na het maken van uw client-verwijzing.  Als u bijvoorbeeld de verbindingstime-out ingesteld op 60 seconden (in plaats van de standaard 10 seconden):

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClient();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>Implementeren van een Filter wordt uitgevoerd

U kunt een snijpunt van elke aanvraag implementeren door het implementeren van een `ServiceFilter`.  De volgende updates bijvoorbeeld een vooraf gemaakte voortgangsbalk:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

U kunt dit filter toevoegen aan de client als volgt:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>Aanvraagheaders aanpassen

Gebruik de volgende `ServiceFilter` en het koppelen van het filter op dezelfde manier als de `ProgressFilter`:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="conversions"></a>Configureren van automatische serialisatie

U kunt opgeven dat een conversiestrategie die van toepassing op elke kolom met behulp van de [gson] [ 3] API. Maakt gebruik van de Android-clientbibliotheek [gson] [ 3] achter de schermen Java om objecten te serialiseren naar JSON-gegevens voordat de gegevens worden verzonden naar Azure App Service.  De volgende code gebruikt de **setFieldNamingStrategy()** methode om in te stellen van de strategie. In dit voorbeeld wordt het eerste teken (een "m"), verwijderen en vervolgens kleine het volgende teken, voor elke veldnaam. Bijvoorbeeld, zou het omzetten 'mId' in 'id'.  Een conversiestrategie voor minder zijn vereist voor implementeren `SerializedName()` aantekeningen in de meeste velden.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStrategy)
);
```

Deze code moet worden uitgevoerd voordat het maken van een mobiele client referentie met de **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: https://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Aan de slag met verificatie]: app-service-mobile-android-get-started-users.md
[1]: https://static.javadoc.io/com.google.code.gson/gson/2.8.5/com/google/gson/JsonObject.html
[2]: https://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: https://www.javadoc.io/doc/com.google.code.gson/gson/2.8.5
[4]: https://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: https://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: https://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: https://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: https://www.odata.org/documentation/odata-version-3-0/
[20]: https://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: ../app-service/configure-authentication-provider-aad.md
[Future]: https://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: https://developer.android.com/reference/android/os/AsyncTask.html
