# SOP: Multiple Approaches to Share Build Artifacts Between Pipeline A and Pipeline B

This document explains various methods to reuse build outputs from **Pipeline A** in **Pipeline B**. Each approach includes usage scenarios, advantages, and limitations.

---

## ✅ Approach 1 (Recommended): Use an Artifact Store

### Pipeline A (Build)
Run the build and upload the output to an artifact repository.

```
bp artifact upload   --source ./build   --artifact-name my-app   --version $BUILD_VERSION
```

### Pipeline B (Use Build Output)
Download the same artifact using the version number.

```
bp artifact download   --artifact-name my-app   --version $BUILD_VERSION   --destination ./build
```

| Pros | Cons |
|------|------|
| Standard CI/CD practice, secure, reusable | Requires artifact storage (S3, Nexus, Minio, JFrog, etc.) |

---

## 🔷 Approach 2: Use Shared Storage (NFS / PVC / Shared Volume)

If both pipelines run in **the same Kubernetes cluster**, you can use a shared directory.

```
/shared/pipelines/build-output/
```

- Pipeline A saves build output to the shared path
- Pipeline B reads from the same path

| Pros | Cons |
|------|------|
| Very fast (no network overhead) | Requires same cluster + same namespace + shared volume configuration |

---

## 🔷 Approach 3: Commit Build Files to Git (Not Recommended)

### Pipeline A
```
cp -r ./build .
git add build
git commit -m "add build artifacts"
git push
```

### Pipeline B
```
git pull
# use /build folder
```

| Pros | Cons |
|------|------|
| Simple to implement | Makes repo size grow rapidly → performance issues |

---

## 🔷 Approach 4: Upload to S3 / MinIO / Google Cloud Storage

### Pipeline A
```
aws s3 cp ./build s3://my-app/build/$BUILD_ID --recursive
```

### Pipeline B
```
aws s3 cp s3://my-app/build/$BUILD_ID ./build --recursive
```

| Pros | Cons |
|------|------|
| Works across environments and clusters | Requires cloud storage access setup |

---

## 🔷 Approach 5: Use Docker Image for Build Transfer

### Pipeline A
Package the build output inside a Docker image.

```
docker build -t my-app:$BUILD_VERSION .
docker push my-app:$BUILD_VERSION
```

### Pipeline B
Pull the image and extract the build output.

```
docker run --rm -v $(pwd)/build:/output my-app:$BUILD_VERSION cp -r /app/build /output
```

| Pros | Cons |
|------|------|
| Easy versioning, works across environments | Requires Docker knowledge & registry access |

---

## 🎯 Final Recommendation Summary

| Approach | Best Use Case | Difficulty | Recommendation |
|---------|--------------|------------|----------------|
| **Artifact Store** ✅ | Standard CI/CD pipelines | Easy | **Best Choice** |
| Shared Storage (PVC/NFS) | Same cluster setups | Medium | Good for tightly-coupled infra |
| S3 / MinIO Bucket | Multi-cluster / hybrid setups | Medium | Flexible and scalable |
| Git Commit Build Artifacts | Emergency or very small builds | Low | **Not Recommended** |
| Docker Image Transfer | Microservices with container workflows | Medium/Advanced | Useful in container-first environments |

---

## 💡 Key Takeaway

Selecting the right approach depends on your **infrastructure**, **team maturity**, and **deployment strategy**.  
For most CI/CD use cases, **Artifact Store** or **S3 Storage** is the most reliable and scalable solution.
