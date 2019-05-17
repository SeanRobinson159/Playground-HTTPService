Using `Task<T>` to make network calls async-await. Check the Playground file for all of the code.

# .await()

```
let networkFileProvider: NetworkFileProvider = MockNetworkFileProvider()

DispatchQueue.global(qos: .utility).async {
    do {
        let smallFile = try networkFileProvider.getSmallFileAsync().await()
        let fileSize = smallFile.count / 1024 / 1024
        print("SmallFile Size", fileSize, "MiB")
    } catch {
        print("Error getting small file.", error)
    }

    print("\n")
    do {
        let mediumFile = try networkFileProvider.getMediumFileAsync().await()
        let fileSize = mediumFile.count / 1024 / 1024
        print("MediumFile Size", fileSize, "MiB")
    } catch {
        print("Error getting medium file.", error)
    }

    print("\n")
    do {
        let largeFile = try networkFileProvider.getLargeFileAsync().await()
        let fileSize = largeFile.count / 1024 / 1024
        print("LargeFile Size", fileSize, "MiB")
    } catch {
        print("Error getting large file.", error)
    }
}
```

## Output

```
GET http://ipv4.download.thinkbroadband.com/5MB.zip
200 http://ipv4.download.thinkbroadband.com/5MB.zip
SmallFile Size 5 MiB


GET http://ipv4.download.thinkbroadband.com/10MB.zip
200 http://ipv4.download.thinkbroadband.com/10MB.zip
MediumFile Size 10 MiB


GET http://ipv4.download.thinkbroadband.com/20MB.zip
200 http://ipv4.download.thinkbroadband.com/20MB.zip
LargeFile Size 20 MiB
```

## Task.awaitAll(tasks: [])
If you want to run multiple tasks in parallel, we can do that as well.

```
DispatchQueue.global(qos: .utility).async {
    let smallFileTask = networkFileProvider.getSmallFileAsync()
    let mediumFileTask = networkFileProvider.getMediumFileAsync()
    let largeFileTask = networkFileProvider.getLargeFileAsync()

    Task.awaitAll(tasks: [smallFileTask, mediumFileTask, largeFileTask])

    // We would do any error checking here
    print("All Files Downloaded")
    PlaygroundPage.current.finishExecution()
}
```

## Output

```
GET http://ipv4.download.thinkbroadband.com/5MB.zip
GET http://ipv4.download.thinkbroadband.com/10MB.zip
GET http://ipv4.download.thinkbroadband.com/20MB.zip
All Files Downloaded
```
