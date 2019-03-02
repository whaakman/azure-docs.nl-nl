## <a name="locating-a-cloud-spatial-anchor"></a>Zoeken naar een cloud ruimtelijke anker

Als u wilt zoeken ruimtelijke cloudankers, moet u de id weten. Anker-id's kunnen worden opgeslagen in de back-end-service van uw toepassing, en toegankelijk is voor alle apparaten die goed bij deze kunnen worden geverifieerd. Voor een voorbeeld van deze Zie [zelfstudie: Ruimtelijke ankers delen tussen apparaten](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Exemplaar van een object AnchorLocateCriteria maken, stelt u de id's die u zoekt, en de CreateWatcher-methode worden aangeroepen in de sessie door te geven uw AnchorLocateCriteria.
