
# Integrate Tanzu Build Service

## [Request Harbor Project](https://confluence.eng.vmware.com/display/AO/Harbor+as+a+Service)

## Download TBS and kp

Follow instructions in [Getting Started with TBS](https://tanzu.vmware.com/content/blog/getting-started-with-vmware-tanzu-build-service-1-0)

## Inatall TBS

```bash
kbld relocate -f images.lock --lock-output images-relocated.lock --repository harbor-repo.vmware.com/build_service/build-service
```

```bash
ytt -f values.yaml \
    -f manifests \
    -v docker_repository=harbor-repo.vmware.com/build_service/build-service \
    -v docker_username="$DH_USERNAME" \
    -v docker_password="$DH_PASSWORD" \
    | kbld -f images-relocated.lock -f- \
    | kapp deploy -a tanzu-build-service -f- -y
```

At this point, TBS images should be available in `build_service` repository in [Harbor](https://harbor-repo.vmware.com/harbor/projects)

## Install TBS Dependencies

This process took hours to complete for me

```bash
kp import -f descriptor-100.0.43.yaml
```

## Define credentials for Harbor Registry

```bash
kp secret create my-dockerhub-creds --registry harbor-repo.vmware.com --registry-user $DH_USERNAME
```

```bash
kp image create chachkies --tag harbor-repo.vmware.com/build_service/chachkies --git https://github.com/$GIT_USERNAME/chachkies.git --git-revision master
```

```bash
watch kp build list chachkies
```

## Resources

[Harbor As A Service](https://confluence.eng.vmware.com/display/AO/Harbor+as+a+Service)
[Getting Started with TBS](https://tanzu.vmware.com/content/blog/getting-started-with-vmware-tanzu-build-service-1-0)
[Installing TBS](https://docs.pivotal.io/build-service/1-0/installing.html)