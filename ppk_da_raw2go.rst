PPK partendo da raw2go
=======================

Step 1: recupero dei file e programmi necessari
-----------------------------------------------

1. Recuperare file .ubx creato da raw2go dal tablet. Lo si può trovare in “memoria interna/Download/(data in cui si è fatta la geolocalizzazione)”.
   
   Un file di prova è stato creato con raw2go posizionando il ricevitore GNSS in cima alle scale esterne della sede CREA-FL di Trento, per 10 minuti: :download:`prova.ubx <assets/ppk/prova.ubx>`

2. Scaricare dati RINEX da TPOS. Assicurarsi di unire gli output da 15 minuti ciascuno in un singolo file cliccando sul pulsante “Unisci Files”.

 .. image:: assets/ppk/images/TPOS.png

 Il file che ci interessa è quello con formato .25o. Cartella compressa di prova: :download:`tntn318k00.rnx.zip <assets/ppk/tntn318k00.rnx.zip>`. 

3. Scaricare RTKLIB: :download:`RTKLIB_EX_2.5.0.zip <assets/ppk/RTKLIB_EX_2.5.0.zip>`
   
   Manuale di RTKLIB (datato ma comunque utile): :download:`manual_demo5.pdf <assets/ppk/manual_demo5.pdf>`


Step 2: conversione da .ubx a (.obs + .nav)
-------------------------------------------

1. Estrarre la cartella compressa di RTKLIB.
2. Avviare l’applicazione rtklaunch
   
 .. image:: assets/ppk/images/rtklaunch_icon.png

3. Premere sulla seconda icona (RTKCONV)

 .. image:: assets/ppk/images/rtklaunch_gui.png

4. Sotto “RTCM, RCV RAW or RINEX OBS ?”, premere [...] e selezionare il file .ubx prodotto da raw2go 
5. Sotto “RINEX OBS/NAV/GNAV/HNAV/QNAV/LNAV/CNAV/INAV/ and SBS” compariranno i percorsi dei file che verranno creati. Deseleziona l’ultimo file con formato .sbs che non ci serve per i passaggi successivi. 

 .. image:: assets/ppk/images/rtkconv.png

6. Premere [Options...]: 

   * Assicurarsi che la RINEX ver sia la 3.04 (i file scaricati da TPOS sono in questa versione. Per controllare, apri uno dei file con il blocco note e leggi la prima riga) 

   * Selezionare i Satellite Systems che erano disponibili per TPOS (probabilmente solo GPS e GLONASS) 

   * Selezionare I GNSS Signals utilizzati dal nostro ricevitore (L1 e L2) 

   * Premere [OK]. La finestra Options si chiuderà.

 .. image:: assets/ppk/images/rtkconv_options.png

7. Premere [> Convert ] nella finestra di RTKCONV. I file .obs e .nav verranno creati nei percorsi visualizzati. 

Step 3: PPK
-----------

1. Premere [Process...] nella finestra di RTKCONV. Si aprirà la finestra di RTKPOST.

2. Sotto “RINEX OBS: Rover ?” dovrebbe già comparire il file .obs che abbiamo appena creato. Nel caso si volesse elaborare un file diverso, premere [...] e selezionarlo. 

3. Sotto “RINEX OBS: Base Station”, premere [...] e cercare il file .25o che abbiamo scaricato da TPOS. 

4. Sotto “RINEX NAV/CLK, SP3, BIA/BSX, FCB, IONEX, SBS/EMS, or RTCM” premere [...] e selezionare il file .nav che abbiamo creato alla fine dello Step 2. 
 
 .. image:: assets/ppk/images/rtkpost.png

5. Premere [Options...]:
   
   * Premere [Load...], navigare nella cartella di RTKLIB e selezionare il file “f9p_ppk.conf”. Questo file contiene la configurazione ottimale per fare Post Processing Kinematics con il chip F9P, presente all'interno del nostro ricevitore GNSS. 
   
   * Deselezionare i satelliti che non ci interessano.
   * Premere [OK]. La finestra Options si chiuderà.

 .. image:: assets/ppk/images/rtkpost_options.png

6. Premere [> Execute] nella finestra di RTKPOST. Un file con formato .pos dovrebbe essersi creato nel percorso visualizzato in basso.

Step 4: Conversione da .pos a .gpx
----------------------------------
1. Premere il pulsante [KML/GPX...] nella finestra di RTKPOST. Si aprirà la finestra KML/GPX Converter.

2. Assicurarsi che l'Output Format sia GPX.

3. Premere [Convert]. Il file .gpx dovrebbe essersi creato nel percorso visualizzato in basso.

 .. image:: assets/ppk/images/kmlgpx_converter.png

Step 5: Conclusione
-------------------

1. Chiudere tutte le finestre di RTKLIB. Le configurazioni delle varie finestre vengono salvate in automatico. Nel caso si volesse resettarle, entrare nella cartella di RTKLIB ed eliminare i file di tipo “Impostazioni di configurazione” creati recentemente. 

 .. image:: assets/ppk/images/file_configurazione.png

2. Aprire il file gpx in QGIS: il gpx dovrebbe contenere 5 layers, 3 di punti e 2 di linee. Tuttavia, un layer di punti e un layer di linee sembrano vuoti. Il layer “waypoints” contiene anche la posizione della stazione TPOS utilizzata.

 .. image:: assets/ppk/images/qgis.png