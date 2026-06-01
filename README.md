# Havana Docker containers

Dockerfiles for Docker containers used in the Havana annotation pipeline.

## How it works

Dockerfiles are maintained here on GitHub. This repository is mirrored to an internal EBI GitLab instance. When a change is pushed to the default branch on GitHub, it syncs automatically to GitLab, which triggers the CI pipeline defined in [`.gitlab-ci.yml`](.gitlab-ci.yml). GitLab builds the affected container(s) and pushes them to the EBI container registry.

Each container is tagged with both `latest` and the commit SHA:

```
registry.gitlab.ebi.ac.uk/<project>/<container-name>:latest
registry.gitlab.ebi.ac.uk/<project>/<container-name>:<commit-sha>
```

Only containers whose files have changed in a given commit are rebuilt.

## Available containers

Containers are listed in the GitLab registry for this project. To browse them, go to the GitLab mirror repository and navigate to **Deploy > Container Registry**.

Containers currently defined in this repo:

| Name | Dockerfile |
|------|------------|
| minimap2 | [Containers/minimap2/Dockerfile](Containers/minimap2/Dockerfile) |
| mysql | [Containers/mysql/Dockerfile](Containers/mysql/Dockerfile) |
| lora_python | [Containers/lora_python/Dockerfile](Containers/lora_python/Dockerfile) |

## Adding a new container

1. Create a directory under `Containers/` named after your tool:
   ```
   Containers/<name>/Dockerfile
   ```

2. Add a job block to [`.gitlab-ci.yml`](.gitlab-ci.yml) following the pattern used by existing containers:
   ```yaml
   build_<name>:
     extends: .build_template
     variables:
       CONTAINER_NAME: <name>
     rules:
       - changes:
           - Containers/<name>/**/*
           - Containers/<name>/*
         when: always
   ```

3. Open a PR against the default branch. Once merged, the mirror sync triggers the GitLab pipeline and the container is built and pushed automatically.

Optionally add a `README.md` inside `Containers/<name>/` describing the tool and any usage notes.
