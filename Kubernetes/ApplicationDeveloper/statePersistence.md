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