# State Persistence

## pv

[pv-def](./yamlFiles/pv-defination.yaml)

## pvc

[pvc-def](./yamlFiles/pvc-definition.yaml)

## pv and pvc

    pv --> pvc --> pod
    pv access-mode and storage-capacity should match with pvc. and claim pvc to pod --> spec --> persistentVolumeClaim --> claimName: <pvc_name>
    delete pod -> pvc to claim pv space.

## Storage Classes

[sc-deg](./yamlFiles/sc-definition.yaml)

    no longer pv is required
    create sc, add it name in pvc --> spec --> storageClassName: <sc_name>. pvc name in pod def
