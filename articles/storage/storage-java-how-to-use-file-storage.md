---
title: Come usare l&quot;archiviazione file da Java | Microsoft Docs
description: Informazioni su come usare il servizio file di Azure per caricare, scaricare, elencare ed eliminare file. Gli esempi sono scritti in Java.
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 60d4bcafa51a6eedea06901a0557d13b4e85ec7a


---
# <a name="how-to-use-file-storage-from-java"></a>Come usare l'archiviazione file da Java
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Panoramica
In questa guida verrà illustrato come eseguire operazioni di base nel servizio di archiviazione file di Microsoft Azure. Gli esempi scritti in Java consentono di apprendere come creare condivisioni e directory, caricare, elencare ed eliminare file. Se non si conosce ancora il servizio di condivisione dei file di Microsoft Azure, la lettura delle seguenti sezioni sarà molto utile per comprendere gli esempi.

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Creare un'applicazione Java
Per compilare gli esempi, saranno necessari il Java Development Kit (JDK) e [Azure Storage SDK per Java][]. È inoltre necessario aver creato un account di archiviazione di Azure.

## <a name="setup-your-application-to-use-file-storage"></a>Configurare l'applicazione per usare la condivisione di file
Per utilizzare le API di archiviazione di Azure, aggiungere le seguenti istruzioni all'inizio del file Java da cui si desidera accedere al servizio di archiviazione.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Configurare una stringa di connessione di archiviazione di Azure
Per utilizzare la condivisione di file, è necessario connettersi all'account di archiviazione di Azure. Il primo passaggio consisterà nel configurare una stringa di connessione che verrà utilizzata per connettersi all'account di archiviazione. È importante definire una variabile statica a tale scopo.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Sostituire your_storage_account_name e your_storage_account_key con i valori effettivi dell'account di archiviazione.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Connessione a un account di archiviazione di Azure
Per connettersi all'account di archiviazione, è necessario usare l'oggetto **CloudStorageAccount**, passando una stringa di connessione al relativo metodo **parse**.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** genera un'eccezione InvalidKeyException, sarà quindi necessario inserirlo in un blocco Try-Catch.

## <a name="how-to-create-a-share"></a>Procedura: creare una condivisione
Tutti i file e directory nell'archiviazione di file si trovano in un contenitore denominato **Share**. L'account di archiviazione può disporre di tante condivisioni quante sono consentite dalla capacità dell'account. Per ottenere accesso a una condivisione e ai suoi contenuti, è necessario utilizzare un client per l'archiviazione file.

```java
// Create the file storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Utilizzando il client per l'archiviazione file, è possibile quindi ottenere un riferimento a una condivisione.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Per creare effettivamente la condivisione, utilizzare il metodo **createIfNotExists** dell'oggetto CloudFileShare.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

A questo punto, **share** contiene un riferimento a una condivisione denominata **sampleshare**.

## <a name="how-to-upload-a-file"></a>Procedura: caricare un file
Una condivisione di archiviazione file di Azure contiene almeno una directory radice in cui possono risiedere i file. In questa sezione verrà illustrato come caricare un file dall'archiviazione locale nella directory radice di una condivisione.

Il primo passaggio del caricamento di un file consiste nell'ottenere un riferimento alla directory in cui risiederà. È possibile eseguire questa operazione chiamando il metodo **getRootDirectoryReference** dell'oggetto condivisione.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Ora che si dispone di un riferimento alla directory radice della condivisione, è possibile caricarvi un file mediante il codice seguente.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="how-to-create-a-directory"></a>Procedura: creare una directory
È inoltre possibile organizzare l'archiviazione inserendo i file all'interno di sottodirectory anziché inserirli tutti nella directory radice. Il servizio di archiviazione file di Azure consente di creare tutte le directory consentite dall'account. Il codice riportato di seguito creerà una sottodirectory denominata **sampledir** nella directory radice.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="how-to-list-files-and-directories-in-a-share"></a>Procedura: elencare i file e le directory in una condivisione
Ottenere un elenco di file e directory all'interno di una condivisione è facile chiamando **listFilesAndDirectories** in un riferimento CloudFileDirectory. Il metodo restituisce un elenco di oggetti ListFileItem che è possibile scorrere. Ad esempio, il seguente codice elencherà i file e le directory all'interno della directory radice.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="how-to-download-a-file"></a>Procedura: scaricare un file
Una delle operazioni più frequenti che verranno eseguite su un archivio di file consiste nello scaricare i file. Nell'esempio seguente, il codice scarica SampleFile.txt e ne visualizza il contenuto.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="how-to-delete-a-file"></a>Procedura: eliminare un file
Un'altra operazione comune nell'archiviazione file è l'eliminazione dei file. Il codice seguente elimina un file denominato SampleFile.txt memorizzato all'interno di una directory denominata **sampledir**.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="how-to-delete-a-directory"></a>Procedura: eliminare una directory
Eliminare una directory è un'attività piuttosto semplice, anche se occorre tenere presente che non è possibile eliminare una directory che contiene ancora file o altre directory.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="how-to-delete-a-share"></a>Procedura: eliminare una condivisione
L'eliminazione di una condivisione viene eseguita chiamando il metodo **deleteIfExists** in un oggetto CloudFileShare. Ecco il codice di esempio che esegue tale operazione.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Passaggi successivi
Per ulteriori informazioni su altre API di archiviazione di Azure, seguire i collegamenti seguenti.

* [Centro per sviluppatori Java](http://azure.microsoft.com/develop/java/)
* [Azure Storage SDK per Java](https://github.com/azure/azure-storage-java)
* [Azure Storage SDK per Android](https://github.com/azure/azure-storage-android)
* [Riferimento all'SDK del client di archiviazione di Azure](http://dl.windowsazure.com/storage/javadoc/)
* [API REST dei servizi di archiviazione di Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog del team di Archiviazione di Azure](http://blogs.msdn.com/b/windowsazurestorage/)
* [Trasferire dati con l'utilità della riga di comando AzCopy](storage-use-azcopy.md)




<!--HONumber=Nov16_HO3-->


