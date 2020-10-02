Migrated to Gitlab: https://gitlab.com/mkandco/mast

# mast
Mast is small and lightweight ansible runner for Kubernetes. With minimal requirement it can be used everywhere even it is your local minikube or big production cluster.

```yaml

apiVersion: mkandco.tech/mast/v1
kind: MastJob
metadata:
  name: create_project
spec:
  ansible_version: 1.2
  repository: 
    fromGit:
        url: https://github.com/kivio/example_ansible_repo
  configuration:
    inventory: 
      - fromRepo:
          path: inventories/production.ini
      - fromConfigMap:
          name: new_env
          key: test.ini
          asFile: generic_inventory.ini
    vars:
      - fromConfigMap:
          name: ansible_vars
          key: global.yaml

```

```yaml
apiVersion: mkandco.tech/mast/v1
kind: MastRun
metadata:
  name: run-1
spec:
    job: create_project
    overrides:
        vars:
        - fromConfigMap:
            name: production_vars
            key: production.yaml
```
