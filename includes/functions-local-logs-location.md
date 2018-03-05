Wanneer de host functies lokaal wordt uitgevoerd, worden logboeken geschreven naar het volgende pad:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

In Windows, `<DefaultTempDirectory>` is de eerste gevonden waarde van de USERPROFILE TMP, TEMP omgevingsvariabelen of de Windows-map.
Op Mac OS- of Linux, `<DefaultTempDirectory>` is de omgevingsvariabele TMPDIR.

[!Note]
Wanneer de host functies wordt gestart, worden de bestaande structuur in de map overschreven.