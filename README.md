## here is the question I encountered：

boot in jar
```bash
java -jar build/libs/jfr-vth-pin-0.0.1-SNAPSHOT.jar
```
make vth pinned
```bash
curl --request POST --url 'http://localhost:8080/pinning'
```
here is the log that from source file [JfrVirtualThreadPinnedEventHandler.java](src%2Fmain%2Fjava%2Fcom%2Fchaney%2Fjfrvthpin%2FJfrVirtualThreadPinnedEventHandler.java)
![img.png](img/jar-pin-log.png)
```text
Thread '{}' pinned for: {}ms at {}, stacktrace
```
pack as native-image
```groovy
    gradle nativeCompile
```
boot as binary
```bash
./build/native/nativeCompile/jfr-vth-pin -XX:StartFlightRecording="filename=recording.jfr,settings=continue+vth.jfc"
```
in the jfc file, the VirtualThreadPinned is turned on
```xml
  <event name="jdk.VirtualThreadPinned">
    <setting name="enabled">true</setting>
    <setting name="stackTrace">true</setting>
    <setting name="threshold">0 ns</setting>
  </event>
```
however the pin event does not get captured
![img.png](img/native-no-pin-log.png)

