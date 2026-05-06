# Javelin Plugin Evaluation

This repository contains five Java projects with **known, pre-seeded bugs** for you to evaluate the [Javelin](https://github.com/DesmondQue/javelin-plugin-intellij) IntelliJ IDEA plugin.

**What does Javelin do?** When a Java project has failing tests, Javelin analyzes which lines of code are executed by passing vs. failing tests, then produces a ranked list of the most "suspicious" lines. The lines most likely to contain the bug are ranked highest. Instead of manually reading stack traces and stepping through a debugger, you get a prioritized list pointing you toward the fault. This technique is called *Spectrum-Based Fault Localization*.

Each project in this repo is a real open-source library with a real bug injected from the [Defects4J](https://github.com/rjust/defects4j) benchmark. Your job is to open each project, run Javelin, and evaluate how helpful the results are.

## Requirements

| Requirement | Details |
|---|---|
| **IntelliJ IDEA** | **2025.1 to 2025.3.x** (Community or Ultimate) |
| **Java JDK** | JDK 8 or later installed and configured in IntelliJ |
| **Javelin Plugin** | Installed in IntelliJ (see below) |
| **Git** | To clone this repository |

### Installing the Javelin Plugin

1. Download the latest **`javelin-plugin-0.4.0.zip`** from the [Javelin Releases page](https://github.com/DesmondQue/javelin-plugin-intellij/releases).
2. In IntelliJ, go to **Settings > Plugins > Gear icon (⚙) > Install Plugin from Disk...**
3. Select the downloaded `.zip` file.
4. Restart IntelliJ when prompted.

## Projects in This Repo

| Folder | Library | Build System | Description |
|---|---|---|---|
| `Defects4J-Cli-9-buggy` | Apache Commons CLI 1.1 | Ant | Command-line argument parsing library |
| `Defects4J-Csv-8-buggy` | Apache Commons CSV 1.0 | Maven | CSV file reading/writing library |
| `Defects4J-Gson-13-buggy` | Google Gson 2.8.1 | Maven | JSON serialization/deserialization library |
| `Defects4J-JacksonDatabind-13-buggy` | Jackson Databind 2.5.3 | Maven | JSON data-binding library |
| `Defects4J-Jsoup-1-buggy` | jsoup 1.1.2 | Maven | HTML parsing library |

## How to Open the Projects

> **Important:** Javelin requires IntelliJ to have a **single project** open. You cannot open the entire repository as one project. Each project folder must be opened individually.

You have two options for getting the projects:

### Option A: Clone the entire repository (recommended if evaluating multiple projects)

```bash
git clone https://github.com/DesmondQue/javelin-plugin-evaluation.git
```

This gives you all five projects at once. You will then open each project folder individually in IntelliJ (see Step 2).

### Option B: Download a single project (if you only need one)

1. Go to the [repository on GitHub](https://github.com/DesmondQue/javelin-plugin-evaluation).
2. Navigate into the project folder you want (e.g., `Defects4J-Csv-8-buggy`).
3. Click the **Code** button near the top-right, then click **Download ZIP**. Alternatively, from the project folder view, click **⋯ (More options) > Download directory**.
4. Extract the ZIP to a location of your choice.

> **Note:** GitHub does not natively support downloading a single subfolder. The simplest alternative is to use a tool like [download-directory](https://download-directory.github.io/). Paste the URL of the project folder (e.g., `https://github.com/DesmondQue/javelin-plugin-evaluation/tree/main/Defects4J-Csv-8-buggy`) and it will download just that folder as a ZIP.

### Step 2: Open a project in IntelliJ

1. In IntelliJ, go to **File > Open**.
2. Navigate **into** the project folder (e.g., `javelin-plugin-evaluation/Defects4J-Csv-8-buggy` if you cloned, or the extracted folder if you downloaded).
3. Click **OK / Open**.
4. If IntelliJ asks "Trust and Open Project?", click **Trust Project**.
5. For Maven projects, IntelliJ will prompt you to import the `pom.xml`. Click **Load as Maven Project** (or enable auto-import).

To switch between projects, use **File > Open** and select a different project folder.

### Step 3: Build the project

For **Maven projects** (Csv, Gson, JacksonDatabind, Jsoup), IntelliJ will automatically import dependencies and compile the project after you load the `pom.xml`. Wait for the build to finish (you can check the progress bar at the bottom of the IDE). If the build does not start automatically, run **Build > Build Project** (`Ctrl+F9`).

For the **Ant project** (Cli-9), see the [special instructions below](#cli-9-ant-project-setup).

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

## Cli-9 Ant Project Setup

The `Defects4J-Cli-9-buggy` project uses Ant instead of Maven, so IntelliJ won't auto-detect the project structure. After opening the folder:

1. Go to **File > Project Structure** (`Ctrl+Alt+Shift+S`).
2. Under **Modules**, click **+** > **Import Module** and select the `Defects4J-Cli-9-buggy` folder.
3. Mark the source directories:
   - `src/java` as **Sources**
   - `src/test` as **Tests**
4. Under **Libraries**, click **+** > **Java** and add any JARs from the `target/lib` folder (if present). You may also need to add JUnit to the classpath manually.
5. Click **Apply** and then build the project with **Build > Build Project** (`Ctrl+F9`).

## Troubleshooting

| Problem | Solution |
|---|---|
| Javelin tool window not visible | Go to **View > Tool Windows > Javelin**, or check that the plugin is installed under **Settings > Plugins** |
| "No compiled classes found" | Build the project first (`Ctrl+F9` or `mvn compile test-compile`) |
| Auto-Detect doesn't find paths | Ensure IntelliJ has finished indexing (check the progress bar at the bottom) and that the project has been imported as a Maven/Ant project |
| Maven import fails | Try **File > Invalidate Caches > Invalidate and Restart**, then re-import |
| Cli-9 won't compile | Ensure source roots are set correctly in Project Structure and JUnit is on the classpath |
