---
agent: agent
description: 'Gere um projeto completo de servidor MCP em Kotlin com estrutura adequada, dependencias e implementacao usando a biblioteca oficial io.modelcontextprotocol:kotlin-sdk.'
---

# Gerador de Projeto de Servidor MCP em Kotlin

Gere um projeto completo e pronto para producao de servidor Model Context Protocol (MCP) em Kotlin.

## Requisitos do Projeto

Voce vai criar um servidor MCP em Kotlin com:

1. **Estrutura de Projeto**: Layout de projeto Kotlin baseado em Gradle
2. **Dependencias**: SDK MCP oficial, Ktor e bibliotecas kotlinx
3. **Server Setup**: Servidor MCP configurado com transports
4. **Tools**: Pelo menos 2-3 tools uteis com inputs/outputs tipados
5. **Error Handling**: Tratamento adequado de excecoes e validacao
6. **Documentacao**: README com instrucoes de setup e uso
7. **Testing**: Estrutura basica de testes com coroutines

## Estrutura de Template

```
myserver/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
├── src/
│   ├── main/
│   │   └── kotlin/
│   │       └── com/example/myserver/
│   │           ├── Main.kt
│   │           ├── Server.kt
│   │           ├── config/
│   │           │   └── Config.kt
│   │           └── tools/
│   │               ├── Tool1.kt
│   │               └── Tool2.kt
│   └── test/
│       └── kotlin/
│           └── com/example/myserver/
│               └── ServerTest.kt
└── README.md
```

## Template do build.gradle.kts

```kotlin
plugins {
    kotlin("jvm") version "2.1.0"
    kotlin("plugin.serialization") version "2.1.0"
    application
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    implementation("io.modelcontextprotocol:kotlin-sdk:0.7.2")
    
    // Ktor for transports
    implementation("io.ktor:ktor-server-netty:3.0.0")
    implementation("io.ktor:ktor-client-cio:3.0.0")
    
    // Serialization
    implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.7.3")
    
    // Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.9.0")
    
    // Logging
    implementation("io.github.oshai:kotlin-logging-jvm:7.0.0")
    implementation("ch.qos.logback:logback-classic:1.5.12")
    
    // Testing
    testImplementation(kotlin("test"))
    testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.9.0")
}

application {
    mainClass.set("com.example.myserver.MainKt")
}

tasks.test {
    useJUnitPlatform()
}

kotlin {
    jvmToolchain(17)
}
```

## Template do settings.gradle.kts

```kotlin
rootProject.name = "{{PROJECT_NAME}}"
```

## Template do Main.kt

```kotlin
package com.example.myserver

import io.modelcontextprotocol.kotlin.sdk.server.StdioServerTransport
import kotlinx.coroutines.runBlocking
import io.github.oshai.kotlinlogging.KotlinLogging

private val logger = KotlinLogging.logger {}

fun main() = runBlocking {
    logger.info { "Starting MCP server..." }
    
    val config = loadConfig()
    val server = createServer(config)
    
    // Use stdio transport
    val transport = StdioServerTransport()
    
    logger.info { "Server '${config.name}' v${config.version} ready" }
    server.connect(transport)
}
```

## Template do Server.kt

```kotlin
package com.example.myserver

import io.modelcontextprotocol.kotlin.sdk.server.Server
import io.modelcontextprotocol.kotlin.sdk.server.ServerOptions
import io.modelcontextprotocol.kotlin.sdk.Implementation
import io.modelcontextprotocol.kotlin.sdk.ServerCapabilities
import com.example.myserver.tools.registerTools

fun createServer(config: Config): Server {
    val server = Server(
        serverInfo = Implementation(
            name = config.name,
            version = config.version
        ),
        options = ServerOptions(
            capabilities = ServerCapabilities(
                tools = ServerCapabilities.Tools(),
                resources = ServerCapabilities.Resources(
                    subscribe = true,
                    listChanged = true
                ),
                prompts = ServerCapabilities.Prompts(listChanged = true)
            )
        )
    ) {
        config.description
    }
    
    // Register all tools
    server.registerTools()
    
    return server
}
```

## Template do Config.kt

```kotlin
package com.example.myserver.config

import kotlinx.serialization.Serializable

@Serializable
data class Config(
    val name: String = "{{PROJECT_NAME}}",
    val version: String = "1.0.0",
    val description: String = "{{PROJECT_DESCRIPTION}}"
)

fun loadConfig(): Config {
    return Config(
        name = System.getenv("SERVER_NAME") ?: "{{PROJECT_NAME}}",
        version = System.getenv("VERSION") ?: "1.0.0",
        description = System.getenv("DESCRIPTION") ?: "{{PROJECT_DESCRIPTION}}"
    )
}
```

## Template do Tool1.kt

```kotlin
package com.example.myserver.tools

import io.modelcontextprotocol.kotlin.sdk.server.Server
import io.modelcontextprotocol.kotlin.sdk.CallToolRequest
import io.modelcontextprotocol.kotlin.sdk.CallToolResult
import io.modelcontextprotocol.kotlin.sdk.TextContent
import kotlinx.serialization.json.buildJsonObject
import kotlinx.serialization.json.put
import kotlinx.serialization.json.putJsonObject
import kotlinx.serialization.json.putJsonArray

fun Server.registerTool1() {
    addTool(
        name = "tool1",
        description = "Description of what tool1 does",
        inputSchema = buildJsonObject {
            put("type", "object")
            putJsonObject("properties") {
                putJsonObject("param1") {
                    put("type", "string")
                    put("description", "First parameter")
                }
                putJsonObject("param2") {
                    put("type", "integer")
                    put("description", "Optional second parameter")
                }
            }
            putJsonArray("required") {
                add("param1")
            }
        }
    ) { request: CallToolRequest ->
        // Extract and validate parameters
        val param1 = request.params.arguments["param1"] as? String
            ?: throw IllegalArgumentException("param1 is required")
        val param2 = (request.params.arguments["param2"] as? Number)?.toInt() ?: 0
        
        // Perform tool logic
        val result = performTool1Logic(param1, param2)
        
        CallToolResult(
            content = listOf(
                TextContent(text = result)
            )
        )
    }
}

private fun performTool1Logic(param1: String, param2: Int): String {
    // Implement tool logic here
    return "Processed: $param1 with value $param2"
}
```

## Template do tools/ToolRegistry.kt

```kotlin
package com.example.myserver.tools

import io.modelcontextprotocol.kotlin.sdk.server.Server

fun Server.registerTools() {
    registerTool1()
    registerTool2()
    // Register additional tools here
}
```

## Template do ServerTest.kt

```kotlin
package com.example.myserver

import kotlinx.coroutines.test.runTest
import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertFalse

class ServerTest {
    
    @Test
    fun `test server creation`() = runTest {
        val config = Config(
            name = "test-server",
            version = "1.0.0",
            description = "Test server"
        )
        
        val server = createServer(config)
        
        assertEquals("test-server", server.serverInfo.name)
        assertEquals("1.0.0", server.serverInfo.version)
    }
    
    @Test
    fun `test tool1 execution`() = runTest {
        val config = Config()
        val server = createServer(config)
        
        // Test tool execution
        // Note: You'll need to implement proper testing utilities
        // for calling tools in the server
    }
}
```

## Template do README.md

```markdown
# {{PROJECT_NAME}}

A Model Context Protocol (MCP) server built with Kotlin.

## Description

{{PROJECT_DESCRIPTION}}

## Requirements

- Java 17 or higher
- Kotlin 2.1.0

## Installation

Build the project:

```bash
./gradlew build
```

## Usage

Run the server with stdio transport:

```bash
./gradlew run
```

Or build and run the jar:

```bash
./gradlew installDist
./build/install/{{PROJECT_NAME}}/bin/{{PROJECT_NAME}}
```

## Configuration

Configure via environment variables:

- `SERVER_NAME`: Server name (default: "{{PROJECT_NAME}}")
- `VERSION`: Server version (default: "1.0.0")
- `DESCRIPTION`: Server description

## Available Tools

### tool1
{{TOOL1_DESCRIPTION}}

**Input:**
- `param1` (string, required): First parameter
- `param2` (integer, optional): Second parameter

**Output:**
- Text result of the operation

## Development

Run tests:

```bash
./gradlew test
```

Build:

```bash
./gradlew build
```

Run with auto-reload (development):

```bash
./gradlew run --continuous
```

## Multiplatform

This project uses Kotlin Multiplatform and can target JVM, Wasm, and iOS.
See `build.gradle.kts` for platform configuration.

## License

MIT
```

## Instrucoes de Geracao

Ao gerar um servidor MCP em Kotlin:

1. **Gradle Setup**: Crie `build.gradle.kts` adequado com todas as dependencias
2. **Package Structure**: Siga convencoes de package em Kotlin
3. **Type Safety**: Use data classes e kotlinx.serialization
4. **Coroutines**: Todas as operacoes devem ser suspending functions
5. **Error Handling**: Use excecoes e validacao em Kotlin
6. **JSON Schemas**: Use `buildJsonObject` para schemas de tool
7. **Testing**: Inclua utilitarios de teste para coroutines
8. **Logging**: Use kotlin-logging para logging estruturado
9. **Configuration**: Use data classes e variaveis de ambiente
