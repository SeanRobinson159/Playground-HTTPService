# Playground-HTTPService

Using `DispatchGroup` to make network calls appear syncronous. Check the Playground file for all of the code.

```
let networkFileProvider: NetworkFileProvider = MockNetworkFileProvider()

DispatchQueue.global(qos: .default).async {
    do {
        let smallFile = try networkFileProvider.getSmallFileAsync()
        let fileSize = smallFile.count / 1024 / 1024
        print("SmallFile Size", fileSize, "MiB")
    } catch {
        print("Error getting small file.", error)
    }

    print("\n")
    do {
        let mediumFile = try networkFileProvider.getMediumFileAsync()
        let fileSize = mediumFile.count / 1024 / 1024
        print("MediumFile Size", fileSize, "MiB")
    } catch {
        print("Error getting medium file.", error)
    }

    print("\n")
    do {
        let largeFile = try networkFileProvider.getLargeFileAsync()
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
