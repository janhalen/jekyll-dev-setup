# Jekyll Dev Environment with Podman and SELinux (openSUSE Aeon)

This README documents all the steps and fixes needed to run a local Jekyll development environment using Podman, code-server (VS Code in the browser), and SELinux on an immutable openSUSE Aeon system.

---

## Prerequisites

- **openSUSE Aeon** (or any immutable, SELinux-enabled Linux)
- **Podman** and **Podman Desktop** installed
- **SELinux** enabled (default on Aeon)
- Your project directory, e.g. `/home/jmk/Repositories/jekyll-dev-setup`

---

## Directory Structure

```
jekyll-dev-setup/
├── dev-environment.yaml
├── code-server-secret.yaml   # (git-ignored)
├── .gitignore
└── ... (your Jekyll project files)
```

---

## YAML Manifests

### `dev-environment.yaml`

Defines two pods:  
- **jekyll** (serves your site)
- **code-server** (VS Code in browser)

Both mount your project directory as a shared volume with SELinux relabeling.

```yaml
# Jekyll Pod
apiVersion: v1
kind: Pod
metadata:
  name: jekyll
spec:
  volumes:
    - name: jekyll-shared
      hostPath:
        path: /home/jmk/Repositories/jekyll-dev-setup
        type: Directory
        selinuxRelabel: "shared"
  containers:
    - name: jekyll
      image: ghcr.io/bretfisher/jekyll-serve:latest
      command: ["jekyll", "serve", "--host", "0.0.0.0"]
      ports:
        - containerPort: 4000
          hostPort: 4000
      volumeMounts:
        - mountPath: /srv/jekyll
          name: jekyll-shared
```


## SELinux Fixes

**SELinux will block container access to host directories unless you relabel them.**

Run this on your host:
```bash
sudo chcon -Rt svirt_sandbox_file_t /home/jmk/Repositories/jekyll-dev-setup
```

---

## (Maybe not needed) Permissions Fixes

Ensure your project directory is accessible to the container user (usually UID 1000):

```bash
sudo chown -R 1000:1000 /home/jmk/Repositories/jekyll-dev-setup
chmod -R a+rwX /home/jmk/Repositories/jekyll-dev-setup
```

---

## Running the Environment

**Apply the secret first, then the environment:**

```bash
podman podman play kube dev-environment.yaml
```

To clean up (remove pods/containers):
```bash
podman play kube --down dev-environment.yaml
```


---

## 8. Accessing Your Environment

- **Jekyll site:** [http://localhost:4000](http://localhost:4000)


## Troubleshooting

- **Permission denied in code-server:**  
  - Check SELinux context (`chcon` above)
  - Check directory ownership/permissions

- **Cannot access code-server:**  
  - Ensure `hostPort: 8080` is set in the manifest
  - Use your browser to access `http://localhost:8080`

- **Secret not found:**  
  - Make sure you applied `code-server-secret.yaml` before `dev-environment.yaml`

## Links

- [Podman](https://podman.io/)
- [Jekyll Serve Image](https://github.com/BretFisher/jekyll-serve)
- [openSUSE Aeon](https://en.opensuse.org/Portal:MicroOS)