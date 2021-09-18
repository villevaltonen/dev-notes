## Container security

- Containers isolate the execution of processes in a given filesystem image
- Container stack consists:
    0. Docker CLI
    0. Docker Engine daemon (Moby)
    0. Containerd high-level container runtime
    0. runC low-level container runtime
- Docker images consists of metadata (exposed ports, volumes etc.) and filesystem image containing the required files for the executed command

### Chroot (isolate)
- Used to change a processes root directory to another directory below the current root
- In the container context, chroot is used to change the container root directory to the container image filesystem

### Namespaces (isolate)
- Namespaces are a way to achieve process isolation via Linux kernel
- Namespaces in use:
    - Mount
        - Creating a new mount namespace allows isolation from the host namespace
        - `unshare --mount -f`
    - Process (PID) 
        - Isolates process ids from the host namespace
        - `unshare --pid`
    - Network
        - Isolates network operations from host name spaces
        - `unshare --net`
    - Interprocess Communication (IPC)
    - Unix Time Sharing (UTS)
        - Allow using different hostnames on a same machine with namespaces
        - `unshare --uts`
    - User
        - Root user within the container is essentially the same as the root user outside the container
        - If a container escape happen, the attacker will be able to grant root priviledges outside the container as well
        - Remapping the user with namespaces allows usage of low priviledge users inside containers
        - User remapping is not enabled by default
- `nsenter`can be used to enter and interrogate the namespaces of a running process

### CGroups (restrict)
- CGroups are a Linux kernel feature for setting resource usage restrictions for a container
- They can be used to limit processes, memory, networking, devices etc.

### Capabilities (control)
- By default Docker drops all capabilities except those explicitly required
- `capsh --print` prints the capabilities of the container

### Seccomp (control)
- Seccomp can be used to restrict the syscalls available to a container
- Docker's default seccomp profile disables access to 44 potentially dangerous system calls, such as those which allow access to the Kernel keyring
- The seccomp profile in use changes based on the capabilities granted to a container, e.g. when a container is granted `CAP_SYS_ADMIN` capabilities, the seccomp restrictions on the `mount` syscall are lifted

### Apparmor (control)
- Apparmor is a Linux kernel security module that can be used to restrict the actions of a process such as limiting network access or file read/write/execute permissions
- The default Docker apparmor profile aims to be moderately protective while providing wide application compatibility

### Authorization plugins
- Authorization plugins can implement additional scenarios like preventing execution of certain Docker commands, limit image registries, deny volume mounts, stop priviledged containers from being created etc.

### Privileged containers
- Privileged containers run with all capabilities, limited seccomp and apparmor profiles and access to the host `/dev` mount
- In privileged mode a container has nearly all the same access to the host as processees running outside containers on the host