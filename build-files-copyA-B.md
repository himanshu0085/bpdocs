# SOP: Reusing Build Artifacts Between Pipeline A and Pipeline B

## 🎯 Objective
- **Pipeline A** will build the project and upload build artifacts to the Artifact Store.
- **Pipeline B** will dynamically download and use the build artifacts based on the version passed from Pipeline A.
- This workflow ensures versioned, repeatable, and consistent builds.

## 🧱 Step 1: Define `BUILD_VERSION`

Use a **unique build version** for every pipeline run (Pipeline Build Number recommended).

```
BUILD_VERSION=$PIPELINE_BUILD_NUMBER
```

Ensure this value is available as an **environment variable** in both pipelines.

## 🚀 Pipeline A (Build Pipeline)

| Step | Action | Example |
|------|--------|---------|
| 1 | Checkout Source Code | `git clone` / SCM Checkout |
| 2 | Run Build Command | `npm install && npm run build` |
| 3 | Upload Build Artifacts | Upload `./build` folder to Artifact Store |

### Upload Artifact Command
```
bp artifact upload   --source ./build   --artifact-name my-app-build   --version $PIPELINE_BUILD_NUMBER
```

This will store the build output uniquely for each run.

## 🤝 Trigger Pipeline B Automatically

Add this as the **last step** in Pipeline A:

```
bp trigger pipeline   --name "Pipeline-B"   --set BUILD_VERSION=$PIPELINE_BUILD_NUMBER
```

This passes the correct version to Pipeline B.

## 🏗 Pipeline B (Deploy / Use Build Output)

| Step | Action | Example |
|------|--------|---------|
| 1 | Receive `BUILD_VERSION` variable | Auto-passed from Pipeline A |
| 2 | Download Artifact | Fetch build folder |
| 3 | Deploy / Continue Workflow | As per pipeline requirement |

### Download Artifact Command
```
bp artifact download   --artifact-name my-app-build   --version $BUILD_VERSION   --destination ./build
```

This ensures Pipeline B uses the exact same build output from Pipeline A.

## 🧠 Key Concept (Explain to Team)

> Pipeline A creates a unique version of the build and uploads it.  
> Pipeline B downloads the build using the same version.  
> This keeps builds consistent and prevents overwriting or mismatch issues.

## ✅ Benefits
- Fully **dynamic and automated** process
- Ensures **consistent build reuse**
- **Easy debugging** — every build is versioned
- No manual file copy required
