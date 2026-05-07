# Javelin Plugin Evaluation

This repository contains five Java projects with **known, pre-seeded bugs** sourced from the [Defects4J](https://github.com/rjust/defects4j) benchmark. These projects are used to evaluate the [Javelin](https://github.com/DesmondQue/javelin-plugin-intellij) IntelliJ IDEA plugin.

**What does Javelin do?**

When a Java project has failing tests, Javelin analyzes which lines of code are executed by passing vs. failing tests, then produces a ranked list of the most "suspicious" lines. The lines most likely to contain the bug are ranked highest.

Instead of manually reading stack traces and stepping through a debugger, you get a prioritized list pointing you toward the fault. This technique is called *Spectrum-Based Fault Localization*.

Each project is a real open-source library with a real bug. To evaluate the plugin, build the project, run Javelin, and answer the [evaluation survey](https://docs.google.com/forms/d/e/1FAIpQLSc-YG8pAwAuWYa9QkohE7aEzuil0uCPXHa46fpCnDZGOqEO1w/viewform?usp=sharing&ouid=113467552741314971171) based on ISO 25010 quality metrics.

> **Note:** You do not need to evaluate all five projects. Pick whichever ones you have time for. We have also included previous run rankings in the [Answer Key](#answer-key) so you can compare your results without running every project yourself. Screenshots of previous runs are available in the [`images-previous-runs/`](images-previous-runs/) folder.

<br>

## Requirements

<div align="center">

| Requirement | Details |
|---|---|
| **IntelliJ IDEA** | **2025.1 to 2025.3.x** (Community or Ultimate) |
| **Java JDK** | **JDK 11** installed and configured in IntelliJ (see [Setting Up JDK 11 in IntelliJ](#setting-up-jdk-11-in-intellij)) |
| **Javelin Plugin** | Already installed and set up in IntelliJ. See the [Javelin Releases page](https://github.com/DesmondQue/javelin-plugin-intellij/releases) or this [setup video](https://youtu.be/1LKd19rKLDM?si=tK2A6MdbYXEMwOdE). |
| **Git** | To clone this repository |

</div>

<br>

## Projects in This Repo

<div align="center">

| Folder | Library | Build System | Description |
|---|---|---|---|
| `Defects4J-Cli-9-buggy` | Apache Commons CLI 1.1 | Maven | Command-line argument parsing library |
| `Defects4J-Csv-8-buggy` | Apache Commons CSV 1.0 | Maven | CSV file reading/writing library |
| `Defects4J-Gson-13-buggy` | Google Gson 2.8.1 | Maven | JSON serialization/deserialization library |
| `Defects4J-JacksonDatabind-13-buggy` | Jackson Databind 2.5.3 | Maven | JSON data-binding library |
| `Defects4J-Jsoup-1-buggy` | jsoup 1.1.2 | Maven | HTML parsing library |

</div>

All five projects require **JDK 11** to build. See [Setting Up JDK 11 in IntelliJ](#setting-up-jdk-11-in-intellij) for setup instructions.

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

## Setting Up JDK 11 in IntelliJ

All five projects require **JDK 11** to build correctly. JDK 11 works the cleanest without warnings for these projects. Newer JDK versions (17, 21) have dropped support for some of the older Java language levels these projects target, which may cause build errors.

If you don't have JDK 11 installed, IntelliJ can download it for you.

<br>

### Downloading JDK 11 through IntelliJ

1. Go to **File > Project Structure** (`Ctrl+Alt+Shift+S`).

2. In the left panel, select **SDKs** under **Platform Settings**.

3. Click the **+** button at the top, then select **Download JDK...**

4. In the dialog that appears:

   - Set **Version** to **11**.
   - Choose a vendor (e.g., **Amazon Corretto**, **Eclipse Temurin**, or **Microsoft OpenJDK**).
   - Choose a download location or leave the default.

5. Click **Download** and wait for it to finish.

<br>

### Setting JDK 11 as the Project SDK

1. Still in **File > Project Structure**, select **Project** under **Project Settings** in the left panel.

2. Under **SDK**, select the JDK 11 you just downloaded from the dropdown.

3. Under **Language level**, select **8** (or match the project's required version from the table above).

4. Click **Apply**, then **OK**.

You will need to set this for each project you open. If IntelliJ prompts you about a missing SDK when you first open a project, use the steps above to configure it.

<br>

## Building the Project

All five projects use Maven. IntelliJ will automatically import dependencies and compile the project after you load the `pom.xml`. Wait for the build to finish (you can check the progress bar at the bottom of the IDE).

If the build does not start automatically, run **Build > Build Project** (`Ctrl+F9`).

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

---

## Answer Key

This section contains the ground truth for each project: what the bug is, where it is, which test fails, and the expected Javelin rankings from previous runs. Use this to verify your evaluation results.

The rankings below use **statement-level dense ranking**, which is the default setting for Javelin. If you switch to method-level granularity, your rankings may differ.

The times listed were recorded on different hardware per project and are provided only as a rough reference. Your actual times will vary depending on your machine.

- **Cli-9, Gson-13**: AMD Ryzen 5 2600 (6 cores / 12 threads)
- **Csv-8, Jsoup-1**: Intel i7-6700K (4 cores / 8 threads)
- **JacksonDatabind-13**: AMD Ryzen 7 5700X (8 cores / 16 threads)

Screenshots of these previous runs can be found in the [`images-previous-runs/`](images-previous-runs/) folder.

<br>

### Cli-9: Apache Commons CLI

A command-line argument parsing library for Java.

**Bug Type:** Exception Handling

**Test Cases:** 110 total (108 passed, 2 failed)

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

| Algorithm | Rank | Time on Previous Run |
|---|---|---|
| Ochiai | 1 | ~2s |
| Ochiai-MS | 1 | ~40s |

</div>

<br>

### Csv-8: Apache Commons CSV

A library for reading and writing CSV files.

**Bug Type:** Data Structure

**Test Cases:** 193 total (192 passed, 1 failed)

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

| Algorithm | Rank | Time on Previous Run |
|---|---|---|
| Ochiai | 1 | ~52s |
| Ochiai-MS | 1 | ~83s |

</div>

<br>

### Gson-13: Google Gson

A JSON serialization and deserialization library.

**Bug Type:** Wrong Condition

**Test Cases:** 1000 total (999 passed, 1 failed)

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

| Algorithm | Rank | Time on Previous Run |
|---|---|---|
| Ochiai | 18 | ~10s |
| Ochiai-MS | 13 | ~8 min |

</div>

<br>

### JacksonDatabind-13: Jackson Databind

> **Not recommended for Ochiai-MS.** This project took over an hour to evaluate with Ochiai-MS on a Ryzen 7 5700X, and the ranking also degraded significantly (from rank 27 with Ochiai to rank 406 with Ochiai-MS). JacksonDatabind's architecture relies heavily on deep inheritance, delegation, and polymorphic dispatch, which means many lines are executed by passing tests without actually being relevant to the bug. Ochiai-MS misinterprets this as low diagnostic value and demotes the actual faulty lines. This project is included so evaluators can see what worst-case performance looks like, but we recommend using Ochiai only when evaluating it.

A JSON data-binding library that maps JSON to Java objects.

**Bug Type:** Null Handling

**Test Cases:** 1357 total (1332 passed, 25 failed)

**Bug Description:** When deserializing a JSON object with a null `id` field (used for object identity references), the code passes the null value to `findObjectId()` without checking for it first. This causes a `NullPointerException` inside `DefaultDeserializationContext.findObjectId()` when it tries to generate a key from the null id.

**Bug Location:**

<div align="center">

| Class | Line(s) |
|---|---|
| `com.fasterxml.jackson.databind.deser.DefaultDeserializationContext` | 88 |

</div>

**Failing Test:** `com.fasterxml.jackson.databind.struct.TestObjectIdDeserialization#testNullObjectId`

**Expected Javelin Rankings:**

<div align="center">

| Algorithm | Rank | Time on Previous Run |
|---|---|---|
| Ochiai | 27 | ~12s |
| Ochiai-MS | 406 | ~71 min |

</div>

<br>

### Jsoup-1: jsoup

An HTML parsing and manipulation library.

**Bug Type:** Wrong Method Call

**Test Cases:** 139 total (138 passed, 1 failed)

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

| Algorithm | Rank | Time on Previous Run |
|---|---|---|
| Ochiai | 1 | ~2s |
| Ochiai-MS | 1 | ~3 min |

</div>

<br>

---

## Troubleshooting

<div align="center">

| Problem | Solution |
|---|---|
| Javelin tool window not visible | Go to **View > Tool Windows > Javelin**, or check that the plugin is installed under **Settings > Plugins** |
| "No compiled classes found" | Build the project first (`Ctrl+F9` or `mvn compile test-compile`) |
| Auto-Detect doesn't find paths | Ensure IntelliJ has finished indexing (check the progress bar at the bottom) and that the project has been imported as a Maven project |
| Maven import fails | Try **File > Invalidate Caches > Invalidate and Restart**, then re-import |

</div>
