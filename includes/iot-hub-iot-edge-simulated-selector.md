> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)
> 
> 

La procedura dettagliata dell'[esempio di caricamento nel cloud del dispositivo simulato] illustra come usare [Azure IoT Edge][lnk-sdk] per inviare dati di telemetria da dispositivo a cloud all'hub IoT da dispositivi simulati.

In questa procedura dettagliata verranno trattati i seguenti argomenti:

1. **Architettura**: informazioni importanti sull'architettura relative all'esempio di caricamento su cloud del dispositivo simulato.
2. **Compilazione ed esecuzione**: i passaggi richiesti per compilare ed eseguire l'esempio.

## <a name="architecture"></a>Architettura
L'esempio di caricamento nel cloud del dispositivo simulato mostra come usare IoT Edge per creare un gateway che invia i dati di telemetria da dispositivi simulati a un hub IoT. I dispositivi simulati non possono connettersi direttamente all'hub IoT perché:

* I dispositivi non usano un protocollo di comunicazione riconosciuto dall'hub IoT.
* I dispositivi non riescono a memorizzare l'identità assegnata dall'hub IoT.

Il gateway IoT Edge risolve questi problemi per i dispositivi simulati nei modi seguenti:

* Il gateway riconosce il protocollo usato dai dispositivi simulati, riceve i dati di telemetria dispositivo a cloud dai dispositivi e inoltra i messaggi all'hub IoT usando un protocollo riconosciuto dall'hub IoT.
* Il gateway archivia le identità dell'hub IoT per conto dei dispositivi simulati e funge da proxy quando tali dispositivi inviano messaggi all'hub IoT.

Il diagramma seguente illustra i componenti principali dell'esempio, inclusi i moduli di IoT Edge:

![][1]

> [!NOTE]
> I moduli non si scambiano direttamente i messaggi, ma li pubblicano in un broker interno che li invia ad altri moduli usando un meccanismo di sottoscrizione, come mostrato nel diagramma seguente. Per altre informazioni, vedere [Get started with Azure IoT Edge][lnk-gw-getstarted] (Introduzione ad Azure IoT Edge).
> 
> 

### <a name="protocol-ingestion-module"></a>Modulo di inserimento del protocollo
Questo modulo è il punto di partenza per trasferire i dati dai dispositivi al cloud attraverso il gateway. Nell'esempio, il modulo esegue quattro attività:

1. Crea dati di temperatura simulati. Si noti che se vengono usati dispositivi reali, il modulo legge i dati da tali dispositivi fisici.
2. Inserisce i dati di temperatura simulati nel contenuto di un messaggio.
3. Aggiunge una proprietà con un indirizzo MAC fittizio al messaggio che contiene i dati di temperatura simulati.
4. Rende disponibile il messaggio al modulo successivo della catena.

> [!NOTE]
> Il modulo denominato **Inserimento del protocollo X** nel diagramma precedente viene chiamato **Dispositivo simulato** nel codice sorgente.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>Modulo ID MAC &lt;-&gt; IoT Hub
Questo modulo esegue l'analisi dei messaggi che includono una proprietà che contiene l'indirizzo MAC dell'app del dispositivo simulato, aggiunto dal modulo di inserimento del protocollo. Se il modulo trova questa proprietà, aggiunge un'altra proprietà con una chiave del dispositivo dell'hub IoT al messaggio, quindi rende disponibile il messaggio al modulo successivo nella catena. In questo modo l'esempio associa le identità dei dispositivi di un hub IoT ai dispositivi simulati. Lo sviluppatore imposta manualmente il mapping tra gli indirizzi MAC e le identità dell'hub IoT durante la configurazione del modulo. 

> [!NOTE]
> Questo esempio usa un indirizzo MAC come identificatore univoco del dispositivo e lo associa a un'identità dei dispositivi di un hub IoT. Tuttavia, è possibile scrivere un modulo personalizzato che usa un identificatore univoco diverso. Ad esempio, alcuni dispositivi con numeri di serie univoci o dati di telemetria con un nome univoco del dispositivo incorporato possono essere usati per determinare l'identità dei dispositivi di un hub IoT.
> 
> 

### <a name="iot-hub-communication-module"></a>Modulo di comunicazione dell'hub IoT
Questo modulo accetta messaggi con un'identità dei dispositivi di un hub IoT assegnata dal modulo precedente e invia il contenuto del messaggio all'hub IoT usando HTTP. HTTP è uno dei tre protocolli riconosciuti dall'hub IoT.

Invece di aprire una connessione all'hub IoT per ogni app per dispositivo simulato, questo modulo apre una singola connessione HTTP dal gateway all'hub IoT ed esegue connessioni multiple da tutti i dispositivi simulati su tale connessione. In questo modo, un singolo gateway può connettersi a molti più dispositivi, simulati o meno, rispetto a quanti ne potrebbe raggiungere se fosse aperta una connessione univoca per ogni dispositivo.

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[esempio di caricamento nel cloud del dispositivo simulato]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md