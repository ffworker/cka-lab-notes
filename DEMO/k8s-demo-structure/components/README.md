# create components to for example only deploy certain parts only to a certain base-structure.. 

**for example, a DEV could use a component like LOGGING to just import -component LOGGING and add it to the BASE-DEV and just the DEV.**


**import components into the overlays/kustomization.yaml ->**

```yaml
components:
  - ../../components/db
```
