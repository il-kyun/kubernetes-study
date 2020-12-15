# docker container 목록

### docker ps
- CONTAINER ID : container 고유 아이디
- IMAGE : container를 생성한 기본 이미지
- COMMAND : container가 실행될때 시작되는 명령어. 대부분의 이미지는 미리 내장돼있다.
- CREATED : 생성 후 시간
- STATUS : container 상태
- PORTS : container가 개방한 포트와 호스트에 연결한 포트
- NAMES : container의 고유한 이름
```
➜  ~ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                               NAMES
c149554a6a07   centos:7   "/bin/bash"              24 minutes ago   Up 23 minutes                                       mycentos
0943fbd428b7   mysql      "docker-entrypoint.s…"   3 days ago       Up 3 days       0.0.0.0:3306->3306/tcp, 33060/tcp   bbb-db-mysql
```

### docker inspect
- container 상세 정보를 확인할 수 있다.
```
➜  ~ docker inspect mycentos
[
    {
        "Id": "c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b",
        "Created": "2020-12-15T15:35:20.8260378Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 30583,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-12-15T15:36:04.827379Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:8652b9f0cb4c0599575e5a003f5906876e10c1ceb2ab9fe1786712dac14a50cf",
        "ResolvConfPath": "/var/lib/docker/containers/c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b/hostname",
        "HostsPath": "/var/lib/docker/containers/c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b/hosts",
        "LogPath": "/var/lib/docker/containers/c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b/c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b-json.log",
        "Name": "/mycentos",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/4ce4133d3a4809417896d4fa5f4b280775c5dcbfb61468aa529ed3e02db1efee-init/diff:/var/lib/docker/overlay2/10457ce75ac9eefd3b904fe30404444b0388c0644ae60d111de27f6bd0b15bad/diff",
                "MergedDir": "/var/lib/docker/overlay2/4ce4133d3a4809417896d4fa5f4b280775c5dcbfb61468aa529ed3e02db1efee/merged",
                "UpperDir": "/var/lib/docker/overlay2/4ce4133d3a4809417896d4fa5f4b280775c5dcbfb61468aa529ed3e02db1efee/diff",
                "WorkDir": "/var/lib/docker/overlay2/4ce4133d3a4809417896d4fa5f4b280775c5dcbfb61468aa529ed3e02db1efee/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "c149554a6a07",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos:7",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201113",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS",
                "org.opencontainers.image.created": "2020-11-13 00:00:00+00:00",
                "org.opencontainers.image.licenses": "GPL-2.0-only",
                "org.opencontainers.image.title": "CentOS Base Image",
                "org.opencontainers.image.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "c79fcf66b0e9b38e31a85e3ff31b2862a9754765a54963a14572fd48ffeed146",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/c79fcf66b0e9",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "f20cbcbf2409ae599a075ddbd3ad2fdb5ed865f7482b167bd71cf3a28b870c28",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "8b5e508618902c6d44c1e456aa4f0d2d8b9e1f1e39c0a3ae09432826a2c14dbe",
                    "EndpointID": "f20cbcbf2409ae599a075ddbd3ad2fdb5ed865f7482b167bd71cf3a28b870c28",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
