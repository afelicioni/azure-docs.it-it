---
title: Informazioni sulle macchine virtuali per l&quot;analisi scientifica dei dati | Documentazione Microsoft
description: "Informazioni su scenari chiave, funzionalità e installazione di macchine virtuali per l&quot;analisi scientifica dei dati, un ambiente e un toolkit pronti per l&quot;analisi."
keywords: strumenti di analisi scientifica dei dati, macchina virtuale per l&quot;analisi scientifica dei dati, strumenti per l&quot;analisi scientifica dei dati, analisi scientifica dei dati per Linux
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: gokuma
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 77b4f634c2a30d0cbec92d34a7f83866541d7d84
ms.lasthandoff: 04/15/2017


---
# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Introduzione alla macchina virtuale per l'analisi scientifica dei dati basata su cloud per Linux e Windows
La macchina virtuale per l'analisi scientifica dei dati (DSVM) è un'immagine VM personalizzata nel cloud di Microsoft Azure creata in modo specifico per l'analisi scientifica dei dati. Include diversi strumenti comuni per l'analisi scientifica dei dati e altri strumenti preinstallati e preconfigurati per implementare rapidamente la creazione di applicazioni intelligenti per l'analisi avanzata. È disponibile in Windows Server 2012 e in Linux. Si offre la versione Linux di DSVM in Ubuntu 16.04 LTS o nelle distribuzioni di Linux basate su CentOS di OpenLogic 7.2. 

Questo argomento illustra le operazioni possibili con la VM per l'analisi scientifica dei dati, descrive alcuni degli scenari chiave per l'uso della VM, indica in modo dettagliato le funzionalità principali disponibili nelle versioni per Windows e Linux e include le istruzioni su come iniziare a usarle.

## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Usi della macchina virtuale per l'analisi scientifica dei dati
L'obiettivo della macchina virtuale per l'analisi scientifica dei dati è offrire ai professionisti dei dati, in tutti i livelli e i ruoli, un ambiente di analisi scientifica dei dati privo di problemi. La VM consente di risparmiare una notevole quantità di tempo che sarebbe necessario se si volesse implementare un ambiente analogo in autonomia. Questa soluzione consente invece di avviare immediatamente il progetto di analisi scientifica dei dati in una nuova istanza di VM. 

La VM per l'analisi scientifica dei dati è progettata e configurata per l'uso in un'ampia gamma di scenari di utilizzo. È possibile aumentare e ridurre le prestazioni dell'ambiente a seconda delle esigenze del progetto. Può essere usato il linguaggio preferito per programmare le attività di analisi scientifica dei dati. È inoltre possibile installare altri strumenti e personalizzare il sistema in base alle esigenze specifiche.

## <a name="key-scenarios"></a>Scenari chiave
Questa sezione suggerisce alcuni scenari chiave per i quali può essere distribuita la VM per l'analisi scientifica dei dati.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Desktop di analisi preconfigurato nel cloud
La VM per l'analisi scientifica dei dati offre una configurazione di base per i team di analisi scientifica dei dati che vogliono sostituire i desktop locali con un desktop cloud gestito. Questa configurazione di base garantisce che tutti gli scienziati dei dati presenti in un team abbiano una configurazione coerente mediante la quale verificare gli esperimenti e promuovere la collaborazione. Anche i costi diminuiscono, grazie alla riduzione del carico lavorativo per gli amministratori di sistema e del tempo richiesto per valutare, installare e gestire i vari pacchetti di software necessari per eseguire analisi avanzate.  

### <a name="data-science-training-and-education"></a>Preparazione e formazione sull'analisi scientifica dei dati
Gli istruttori e i formatori aziendali che insegnano l'analisi scientifica dei dati in genere forniscono un'immagine di macchina virtuale per garantire che gli studenti abbiano una configurazione coerente e che gli esempi abbiano un comportamento prevedibile. La VM per l'analisi scientifica dei dati consente di creare un ambiente su richiesta con una configurazione coerente che semplifica i problemi relativi a incompatibilità e supporto. Esistono vantaggi sostanziali per i casi in cui tali ambienti devono essere compilati di frequente, in particolare per i corsi di formazione più brevi.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Capacità elastica su richiesta per progetti su larga scala
Gli hackathon e i concorsi di analisi scientifica dei dati o la modellazione e l'esplorazione di dati su larga scala richiedono una maggiore capacità hardware, in genere per brevi periodi di tempo. La VM per l'analisi scientifica dei dati consente di replicare rapidamente e su richiesta l'ambiente di analisi scientifica dei dati su server con maggiore capacità, che consentono di eseguire esperimenti che richiedono l'esecuzione di risorse di calcolo con potenza elevata.

### <a name="short-term-experimentation-and-evaluation"></a>Valutazione e sperimentazione a breve termine
La VM per l'analisi scientifica dei dati può essere usata per valutare o imparare a usare strumenti quali Microsoft R Server, SQL Server, strumenti di Visual Studio, Jupyter, toolkit di deep learning/ML e i nuovi strumenti popolari nella comunità con il minimo sforzo di installazione. La VM per l'analisi scientifica dei dati può essere configurata rapidamente, pertanto può essere usata anche in altri scenari di utilizzo a breve termine, ad esempio nella replica di esperimenti pubblicati, nell'esecuzione di demo e di procedure dettagliate in sessioni online, nonché in esercitazioni in conferenza.

### <a name="deep-learning"></a>Apprendimento avanzato
La VM di analisi scientifica dei dati può essere usata per il training del modello usando gli algoritmi di apprendimento avanzato sull'hardware basato su GPU (unità di elaborazione grafica). La VM di analisi scientifica dei dati è utile per usare l'hardware basato su GPU nel cloud solo se richiesto quando si deve eseguire il training di modelli di grandi dimensioni o se occorrono calcoli ad alta velocità che sfruttano la potenza di una GPU.  In Windows viene attualmente fornito il [toolkit di apprendimento avanzato per la VM di analisi scientifica dei dati](http://aka.ms/dsvm/deeplearning) come un componente aggiuntivo nella parte superiore della VM di analisi scientifica dei dati. Questo componente aggiuntivo installa automaticamente i driver di GPU, i framework e la versione di GPU degli algoritmi di apprendimento avanzato durante la creazione di un'istanza di macchina virtuale. In Linux l'apprendimento avanzato su GPU è abilitato solo nell'[edizione della VM di analisi scientifica dei dati per Linux (Ubuntu)](http://aka.ms/dsvm/ubuntu). È possibile distribuire l'edizione Ubuntu della VM di analisi scientifica dei dati alla macchina virtuale di Azure non basata su GPU, nel qual caso tutti i framework di apprendimento avanzato eseguiranno il fallback alla modalità CPU. L'edizione Linux basata su CentOS della VM di analisi scientifica dei dati contiene solo le build CPU di alcuni strumenti di apprendimento avanzato (CNTK, Tensorflow, MXNet), senza avere i framework e i driver di GPU preinstallati. 

## <a name="whats-included-in-the-data-science-vm"></a>Funzionalità incluse nella VM per l'analisi scientifica dei dati
La macchina virtuale per l'analisi scientifica dei dati ha già installati e configurati numerosi strumenti comuni per l'analisi scientifica dei dati e l'apprendimento avanzato. Include inoltre strumenti che semplificano l'uso di vari prodotti di Azure per l'analisi e per i dati. È possibile esplorare e creare modelli predittivi in set di dati su larga scala usando Microsoft R Server o SQL Server 2016. Sono inclusi anche una serie di altri strumenti della community open source e di Microsoft, nonché esempi di codice e blocchi appunti. La tabella seguente indica in modo dettagliato e confronta i componenti principali inclusi nelle edizioni per Windows e Linux della macchina virtuale per l'analisi scientifica dei dati.


| **Strumento**                                                           | **Edizione per Windows** | **Edizione per Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) con i pacchetti più diffusi pre-installati   |S                      | S             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) Developer Edition include <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started) parallelo e framework R distribuito ad alte prestazioni<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction): nuovi algoritmi ML all'avanguardia da Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [Operazionalizzazione R](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |S                      | S <br/> (MicrosoftML non ancora disponibile)|
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 con i pacchetti più diffusi pre-installati    |S                      |S              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) con i pacchetti più diffusi pre-installati per il linguaggio di programmazione Julia                         |S                      |S              |
| Database relazionali                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) |
| Strumenti del database                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * driver di ODBC/JDBC| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (strumento di query), <br /> * bcp, sqlcmd <br /> * driver di ODBC/JDBC|
| Analisi database scalabile con SQL Server R services | S     |N              |
| **[Jupyter Notebook Server](http://jupyter.org/) con i kernel seguenti,**                                  | S     | S |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | S |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (soltanto Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | S |
| JupyterHub (server notebook multiutente)| N | S |
| **Strumenti di sviluppo, editor di codice e IDE**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2015 (Community Edition)](https://www.visualstudio.com/community/) > con Git Plugin, Azure HDInsight (Hadoop), Data Lake, strumenti SQL Server Data, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs) e [strumenti R per Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim ed Emacs | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git e GitBash | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* .Net Framework | S | N |
| PowerBI Desktop | S | N |
| SDK per accedere alla suite di servizi di Cortana Intelligence e di Azure | S | S |
| **Strumenti di gestione e spostamento dati** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Storage Explorer | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/overview) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy(Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Strumento di migrazione dei dati DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Gateway di gestione dati di Microsoft](https://msdn.microsoft.com/library/dn879362.aspx): spostare i dati tra posizione locale e cloud | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Utilità della riga di comando Unix/Linux | S | S |
| [Apache Drill](http://drill.apache.org) per l'esplorazione dei dati | S | S |
| **Strumenti di Machine Learning** |||
| &nbsp;&nbsp;&nbsp;&nbsp;*- Integrazione con [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (soltanto Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (soltanto Ubuntu) |
| **Strumenti di apprendimento avanzato basati sulla GPU** |Usare il [toolkit di apprendimento avanzato per DSVM](http://aka.ms/dsvm/deeplearning) |Solo edizione Ubuntu|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](http://cntk.ai) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | S | S|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Driver Nvidia, CUDA, CUDNN](https://developer.nvidia.com/cuda-toolkit) | S | S |
| **Piattaforma Big Data (soltanto Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* [Spark](http://spark.apache.org/) locale indipendente | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Hadoop](http://hadoop.apache.org/) locale (HDFS, YARN) | N | S |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Introduzione alla VM Windows per l'analisi scientifica dei dati
* Creare un'istanza della VM in Windows, passando a [questa pagina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) e selezionando il pulsante verde **Crea macchina virtuale**.
* Accedere alla VM dal desktop remoto usando le credenziali specificate durante la creazione della VM.
* Per individuare e avviare gli strumenti disponibili, fare clic sul menu **Start**.

## <a name="get-started-with-the-linux-data-science-vm"></a>Introduzione alla VM Linux per l'analisi scientifica dei dati
* Creare un'istanza della macchina virtuale in Linux
  * Per l'edizione basata su CentOS di OpenLogic accedere a [questa pagina](http://aka.ms/dsvm/centos) e selezionare il pulsante **Get it now** (Ottieni subito).
  * Per l'edizione di Ubuntu accedere a [questa pagina](http://aka.ms/dsvm/ubuntu) e selezionare il pulsante **Get it now** (Ottieni subito).
* Accedere alla VM da un client SSH, ad esempio Putty o SSH Command, usando le credenziali specificate durante la creazione della VM.
* Nel prompt della shell immettere dsvm-more-info.
* Per un desktop grafico scaricare [qui](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) il client X2Go per la piattaforma client e seguire le istruzioni riportate nel documento relativo alla VM Linux per l'analisi scientifica dei dati [Effettuare il provisioning di una macchina virtuale Linux per l'analisi scientifica dei dati](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Passaggi successivi
### <a name="for-the-windows-data-science-vm"></a>Per la VM Windows per l'analisi scientifica dei dati
* Per altre informazioni sull'esecuzione degli strumenti specifici disponibili nella versione per Windows, vedere [Eseguire il provisioning di una macchina virtuale per l'analisi scientifica dei dati di Microsoft](machine-learning-data-science-provision-vm.md).
* Per altre informazioni sull'esecuzione di varie attività necessarie per il progetto di analisi scientifica dei dati sulla VM Windows, vedere [Dieci cose da fare con la macchina virtuale per l'analisi scientifica dei dati](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Per la VM Linux per l'analisi scientifica dei dati
* Per altre informazioni sull'esecuzione degli strumenti specifici disponibili nella versione per Linux, vedere [Effettuare il provisioning di una macchina virtuale Linux per l'analisi scientifica dei dati](machine-learning-data-science-linux-dsvm-intro.md).
* Per una procedura dettagliata che illustra come eseguire diverse attività comuni di analisi scientifica dei dati con la VM Linux, vedere [Analisi scientifica dei dati in una macchina virtuale Linux per l'analisi scientifica dei dati](machine-learning-data-science-linux-dsvm-walkthrough.md).


