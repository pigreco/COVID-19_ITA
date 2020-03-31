## Changelog

data|descrizione
----|----------
11/02/2020| Il progetto principale (`main`) che verrà adottato per successive modifiche e migliormanti è `COVID19_3857_noVL_ogrVRT`
12/03/2020| - Sostituito logo del PCM-DPC con il marchio QGIS;<br>- aggiunto file *qpt
13/03/2020| - Aggiunti file di configurazione grafici; <br>- shapefile `reg_provaut3857`; <br>- modifica atlas; <br>- aggiunto nuovo `*.qpt` e file `*qgs` (`COVID19_3857_noVL_ogrVRT_provaut`) nuovo `main`; <br>- aggiunti **Stemmi per Regione**;
14/03/2020|- Aggiunto GeoPackage `wordl_map.gpkg`; <br>- aggiunto link Font `Trueno`; <br>- aggiunta cartella `macOS_Project` con progetti adattati per QGIS mac
15/03/2020| - Aggiunto CSV remoto `codid19-andamento_nazione dpc-covid19-ita-andamento-nazionale`;<br>- aggiunto layout di stampa `Andamento nazionale`; <br>- altri piccoli migliormanti; <br>- modifica decorazioni; <br>- aggiunta gruppi nella TOC; <br>- aggiunta cartella PDF
16/03/2020| - Aggiornato layout dell'Atlas
18/03/2020| - Aggiunto etichetta nella panoramica; <br>- aggiunta cornice al numero verde
19/03/2020| - aggiunto link Ministero della Salute Pandemia; <br> - `dpc-covid19-ita-regioni-latest.vrt`;  <br> - `dpc-covid19-ita-province-latest.vrt`
20/03/2020| - Aggiunto link a WikiPedia definizione Pandemia;
31/03/2020| - _**aggiornamneti vari**_: <br>- Data nel formato `ISO8601 UTC`; <br>- **Aggiunta**: "`Note`" in "`dati-regioni`", "`dati-province`" e `dati-andamento-nazionale`" <br>- **Aggiunta**: dataset "`note`"; <br>- **Modifica**: "`totale_attualmente_positivi`" rinominato in "`totale_positivi`" (ricoverati_con_sintomi + terapia_intensiva + totale_ospedalizzati) in "`dati_regioni`" e "`dati_andamento_nazionale`" <br>- **Aggiunta**: "`variazione_totale_positivi`" (totale_attualmente positivi giorno corrente - totale_attualmente positivi giorno precedente) in "`dati_regioni`" e "`dati_andamento_nazionale`" <br>- **Modifica**: "`nuovi_attualmente_positivi`" rinominato in "`nuovi_positivi`" (totale_casi giorno corrente - totale_casi giorno precedente) in "`dati_regioni`" and "`dati_andamento_nazionale`" <br>- **Modifica**: Regione "`Emilia Romagna`" rinominato in "`Emilia-Romagna`" in "`dati-regioni`" and "`dati-province`" ("`denominazione_regione`")
