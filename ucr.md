# SOP for Using Utho Container Registry (UCR)

## Step 1: Create a Container Registry

1. Log in to the Utho Console.
2. Search for **Container Registry** in the search bar.
3. Click **Create Repository**.

---

## Step 2: Configure the Registry

1. Select the desired **Data Center (DC) Location**.
2. Choose the appropriate **Plan**.
3. Select the **Registry Type**:

   * Public Repository
   * Private Repository

> In this example, we are choosing a **Public Repository**.

---

## Step 3: Create the Registry

Create and deploy a container registry with a unique name.

Example:

```text
registry-nsb4jrju-container
```

---

## Step 4: Build the Docker Image

Pull the application source code to your cloud instance and create a `Dockerfile` for the application.

Build the Docker image using:

```bash
docker build -t image:version <path-to-dockerfile>
```

Example:

```bash
docker build -t myapp:v1 .
```

---

## Step 5: Test the Docker Image

After the image is built successfully, run and test the container to ensure the application is working correctly.

Example:

```bash
docker run -d -p 80:80 myapp:v1
```

Verify that the application is accessible and functioning as expected.

---

## Step 6: Log in to Utho Container Registry

Authenticate with the Utho Container Registry:

```bash
docker login registry.utho.io
```

You will be prompted to enter:

```text
Username:
Password:
```

These credentials are provided when you create the registry in Utho.

Once logged in successfully, you can tag and push your Docker images.

---

## Step 7: Tag the Docker Image

Tag the Docker image with your Utho Container Registry URL and registry name before pushing it.

Syntax:

```bash
docker tag image:version registry.utho.io/registry-name/image:version
```

Example:

```bash
docker tag myapp:v1 registry.utho.io/registry-nsb4jrju-container/myapp:v1
```

---

## Step 8: Push the Docker Image

Push the tagged image to the Utho Container Registry.

Syntax:

```bash
docker push registry.utho.io/registry-name/REPOSITORY[:TAG]
```

Example:

```bash
docker push registry.utho.io/registry-nsb4jrju-container/myapp:v1
```

---

## Verification

After the push completes successfully:

1. Navigate to the Utho Container Registry dashboard.
2. Open your registry repository.
3. Verify that the image and tag are visible.

The Docker image is now stored in the Utho Container Registry and can be used for deployments across your infrastructure.
