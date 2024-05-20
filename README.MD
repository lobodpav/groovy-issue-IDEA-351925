# Groovy IntelliJ issue IDEA-351925

Demonstrates Groovy expression evaluator misbehaving in the following environment:
- Build #IU-241.15989.150, built on April 29, 2024
- Runtime version: 17.0.10+1-b1207.14 aarch64
- VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
- macOS 14.5
- GC: G1 Young Generation, G1 Old Generation
- Memory: 4096M
- Cores: 12
- Metal Rendering is ON
- Registry:
- ide.experimental.ui=true
- terminal.new.ui=true
- Non-Bundled Plugins:
- net.sf.opk.avro-schema-support (223.3.0)
- com.mnr.java.intellij.idea.plugin.base64helper (4.0.0)
- org.intellij.plugins.hcl (241.15989.49)
- DevKit (241.15989.199)
- String Manipulation (9.13.0)
- io.intellij-sdk-thread-access (2.2.1)
- gherkin (241.14494.150)
- cucumber-java (241.14494.158)
- com.intellij.nativeDebug (241.15989.65)
- com.github.masooh.intellij.plugin.groovyfier (0.2)
- com.intellij.ml.llm (241.15989.195)
- io.github.lobodpav.spock (1.0.0)
- izhangzhihao.rainbow.brackets (2024.2.3-241)
- me.mbolotov.cypress (1.6.2)
- Kotlin: 241.15989.150-IJ
         
# Replication steps
1. Create a simple Groovy application:
   ```groovy
   class Test {
       static void main(String[] args) {
           def map = ["name": "Pavel", "gender": "Male"]
           println("Name: ${map.name}")
       }
   }
   ```
2. Add a breakpoint to the `println` line
3. Debug the application
4. Open the `Evaluate Expression...` dialogue
5. Evaluate these code fragments:
    1. `map["name"]`
    2. `map.get("name")`

## Expected result

The code fragments should be evaluated without errors.

## Actual result

The former fragment returns `Array reference expected` error.
The latter evaluates but IntelliJ highlights a `Cannot resolve method 'get' in 'Object'` error.

The example code is just one of many. Generally, Groovy evaluation is broken to an extent where any evaluation highlights any object access in red.
