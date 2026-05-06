# Javelin Plugin Evaluation

This repository contains five Java projects with **known, pre-seeded bugs** sourced from the [Defects4J](https://github.com/rjust/defects4j) benchmark. These projects are used to evaluate the [Javelin](https://github.com/DesmondQue/javelin-plugin-intellij) IntelliJ IDEA plugin.

**What does Javelin do?**

When a Java project has failing tests, Javelin analyzes which lines of code are executed by passing vs. failing tests, then produces a ranked list of the most "suspicious" lines. The lines most likely to contain the bug are ranked highest.

Instead of manually reading stack traces and stepping through a debugger, you get a prioritized list pointing you toward the fault. This technique is called *Spectrum-Based Fault Localization*.

Each project is a real open-source library with a real bug. To evaluate the plugin, build the project, run Javelin, and answer the [evaluation survey](https://docs.google.com/forms/d/e/1FAIpQLSc-YG8pAwAuWYa9QkohE7aEzuil0uCPXHa46fpCnDZGOqEO1w/viewform?usp=sharing&ouid=113467552741314971171) based on ISO 25010 quality metrics.

<br>

## Requirements

<div align="center">

| Requirement | Details |
|---|---|
| **IntelliJ IDEA** | **2025.1 to 2025.3.x** (Community or Ultimate) |
| **Java JDK** | **JDK 8** installed and configured in IntelliJ (see [Setting Up JDK 8 in IntelliJ](#setting-up-jdk-8-in-intellij)) |
| **Javelin Plugin** | Already installed and set up in IntelliJ. See the [Javelin Releases page](https://github.com/DesmondQue/javelin-plugin-intellij/releases) or this [setup video](https://youtu.be/1LKd19rKLDM?si=tK2A6MdbYXEMwOdE). |
| **Git** | To clone this repository |

</div>

<br>

## Projects in This Repo

<div align="center">

| Folder | Library | Build System | Description |
|---|---|---|---|
| `Defects4J-Cli-9-buggy` | Apache Commons CLI 1.1 | Ant | Command-line argument parsing library |
| `Defects4J-Csv-8-buggy` | Apache Commons CSV 1.0 | Maven | CSV file reading/writing library |
| `Defects4J-Gson-13-buggy` | Google Gson 2.8.1 | Maven | JSON serialization/deserialization library |
| `Defects4J-JacksonDatabind-13-buggy` | Jackson Databind 2.5.3 | Maven | JSON data-binding library |
| `Defects4J-Jsoup-1-buggy` | jsoup 1.1.2 | Maven | HTML parsing library |

</div>

All five projects require **JDK 8** to build. See [Setting Up JDK 8 in IntelliJ](#setting-up-jdk-8-in-intellij) for setup instructions.

<br>

## How to Get the Projects

> **Important:** Javelin requires IntelliJ to have a **single project** open. You cannot open the entire repository as one project. Each project folder must be opened individually.

Clone the repository to get all five projects at once:

```bash
git clone https://github.com/DesmondQue/javelin-plugin-evaluation.git
```

You can also download each project's ZIP individually from the [repository on GitHub](https://github.com/DesmondQue/javelin-plugin-evaluation).

<br>

## Opening a Project in IntelliJ

1. In IntelliJ, go to **File > Open**.

2. Navigate **into** the project folder (e.g., `javelin-plugin-evaluation/Defects4J-Csv-8-buggy`).

3. Click **OK / Open**.

4. If IntelliJ asks "Trust and Open Project?", click **Trust Project**.

5. For Maven projects, IntelliJ will prompt you to import the `pom.xml`. Click **Load as Maven Project** (or enable auto-import).

To switch between projects, use **File > Open** and select a different project folder.

<br>

## Setting Up JDK 8 in IntelliJ

All five projects require **JDK 8** to build correctly. Newer JDK versions (11, 17, 21) have dropped support for some of the older Java language levels these projects target, which will cause build errors.

If you don't have JDK 8 installed, IntelliJ can download it for you.

<br>

### Downloading JDK 8 through IntelliJ

1. Go to **File > Project Structure** (`Ctrl+Alt+Shift+S`).

2. In the left panel, select **SDKs** under **Platform Settings**.

3. Click the **+** button at the top, then select **Download JDK...**

4. In the dialog that appears:

   - Set **Version** to **1.8** (this is Java 8).
   - Choose a vendor (e.g., **Amazon Corretto**, **Eclipse Temurin**, or **Oracle OpenJDK**).
   - Choose a download location or leave the default.

5. Click **Download** and wait for it to finish.

<br>

### Setting JDK 8 as the Project SDK

1. Still in **File > Project Structure**, select **Project** under **Project Settings** in the left panel.

2. Under **SDK**, select the JDK 8 you just downloaded from the dropdown.

3. Under **Language level**, select **8** (or match the project's required version from the table above).

4. Click **Apply**, then **OK**.

You will need to set this for each project you open. If IntelliJ prompts you about a missing SDK when you first open a project, use the steps above to configure it.

<br>

## Building the Project

For **Maven projects** (Csv, Gson, JacksonDatabind, Jsoup), IntelliJ will automatically import dependencies and compile the project after you load the `pom.xml`. Wait for the build to finish (you can check the progress bar at the bottom of the IDE).

If the build does not start automatically, run **Build > Build Project** (`Ctrl+F9`).

For the **Ant project** (Cli-9), see the [special instructions below](#cli-9-ant-project-setup).

<br>

## Running Javelin

Once the project is open and compiled:

1. Open the **Javelin tool window** at the bottom of the IDE.

2. Click **Auto-Detect**. Javelin will find your compiled classes, test classes, and classpath automatically.

3. Click **Run Javelin Analysis** (or press `Ctrl+Shift+J`).

4. Wait for the analysis to complete. The results panel will show a ranked list of suspicious lines.

5. **Double-click any row** to jump to that line in the editor. Suspicious lines are highlighted with colors:

   - **Red**: Critical (top 10% most suspicious)
   - **Orange**: High (top 25%)
   - **Yellow**: Medium (suspicious, score > 0)
   - **Green**: Low (covered by tests but not implicated)

To clear results: **Tools > Clear Javelin Results**.

<br>

## Cli-9 Ant Project Setup

The `Defects4J-Cli-9-buggy` project uses Ant instead of Maven, so IntelliJ won't auto-detect the project structure. After opening the folder:

1. Go to **File > Project Structure** (`Ctrl+Alt+Shift+S`).

2. Under **Modules**, click **+** > **Import Module** and select the `Defects4J-Cli-9-buggy` folder.

3. Mark the source directories:

   - `src/java` as **Sources**
   - `src/test` as **Tests**

4. Under **Libraries**, click **+** > **Java** and add any JARs from the `target/lib` folder (if present). You may also need to add JUnit to the classpath manually.

5. Click **Apply** and then build the project with **Build > Build Project** (`Ctrl+F9`).

<br>

---

## Answer Key

This section contains the ground truth for each project: what the bug is, where it is, which test fails, and the expected Javelin rankings from previous runs. Use this to verify your evaluation results.

The rankings below use **statement-level dense ranking**, which is the default granularity most evaluators will see. If you switch to method-level granularity, your rankings may differ.

<br>

### Cli-9: Apache Commons CLI

A command-line argument parsing library for Java.

**Bug Type:** Exception Handling

**Bug Description:** When multiple required options are missing, the error message concatenates them without a comma separator. For example, it produces `"Missing required options: fx"` instead of `"Missing required options: f, x"`. The loop in `Parser.java` appends each option name directly without any delimiter between them.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `org.apache.commons.cli.Parser` | 322 |

</div>

**Failing Test:** `org.apache.commons.cli.OptionsTest#testMissingOptionsException`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank |
|---|---|
| Ochiai | 1 |
| Ochiai-MS | 1 |
| Winner | Tie |

</div>

<br>

### Csv-8: Apache Commons CSV

A library for reading and writing CSV files.

**Bug Type:** Data Structure

**Bug Description:** When validating a `CSVFormat` with duplicate header names, the code throws an `IllegalStateException` instead of the expected `IllegalArgumentException`. The validation logic in `CSVFormat.validate()` correctly detects duplicates but uses the wrong exception type.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `org.apache.commons.csv.CSVFormat` | 316, 665-671 |

</div>

**Failing Test:** `org.apache.commons.csv.CSVFormatTest#testDuplicateHeaderElements`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank |
|---|---|
| Ochiai | 1 |
| Ochiai-MS | 1 |
| Winner | Tie |

</div>

<br>

### Gson-13: Google Gson

A JSON serialization and deserialization library.

**Bug Type:** Wrong Condition

**Bug Description:** The JSON reader incorrectly parses negative zero (`-0`). When reading the number, it drops the negative sign and returns `"0"` instead of `"-0"`. The condition that determines whether a number is negative has a logic error in `JsonReader.peekNumber()`.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `com.google.gson.stream.JsonReader` | 731 |

</div>

**Failing Test:** `com.google.gson.stream.JsonReaderTest#testNegativeZero`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank |
|---|---|
| Ochiai | 18 |
| Ochiai-MS | 13 |
| Winner | Ochiai-MS Better |

</div>

<br>

### JacksonDatabind-13: Jackson Databind

A JSON data-binding library that maps JSON to Java objects.

**Bug Type:** Null Handling

**Bug Description:** When deserializing a JSON object with a null `id` field (used for object identity references), the code passes the null value to `findObjectId()` without checking for it first. This causes a `NullPointerException` inside `DefaultDeserializationContext.findObjectId()` when it tries to generate a key from the null id.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `com.fasterxml.jackson.databind.deser.impl.ObjectIdValueProperty` | 93 |
| `com.fasterxml.jackson.databind.deser.DefaultDeserializationContext` | 88 |

</div>

**Failing Test:** `com.fasterxml.jackson.databind.struct.TestObjectIdDeserialization#testNullObjectId`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank |
|---|---|
| Ochiai | 27 |
| Ochiai-MS | 406 |
| Winner | Ochiai Better |

</div>

<br>

### Jsoup-1: jsoup

An HTML parsing and manipulation library.

**Bug Type:** Wrong Method Call

**Bug Description:** When parsing an HTML snippet like `"foo <b>bar</b> baz"`, the text `"foo"` ends up in the root node instead of the body. During normalization, the code moves nodes into the body in a way that reorders the text content. The result is `"bar baz foo"` instead of the expected `"foo bar baz"`.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `org.jsoup.nodes.Document` | 125-126 |

</div>

**Failing Test:** `org.jsoup.parser.ParserTest#createsStructureFromBodySnippet`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank |
|---|---|
| Ochiai | 1 |
| Ochiai-MS | 1 |
| Winner | Tie |

</div>

<br>

---

## Troubleshooting

<div align="center">

| Problem | Solution |
|---|---|
| Javelin tool window not visible | Go to **View > Tool Windows > Javelin**, or check that the plugin is installed under **Settings > Plugins** |
| "No compiled classes found" | Build the project first (`Ctrl+F9` or `mvn compile test-compile`) |
| Auto-Detect doesn't find paths | Ensure IntelliJ has finished indexing (check the progress bar at the bottom) and that the project has been imported as a Maven/Ant project |
| Maven import fails | Try **File > Invalidate Caches > Invalidate and Restart**, then re-import |
| Cli-9 won't compile | Ensure source roots are set correctly in Project Structure and JUnit is on the classpath |

</div>
