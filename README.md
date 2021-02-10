# Docker ClamAV.

Docker container for starting a [ClamAV](http://www.clamav.net/) daemon.

## Getting Started

These instructions will cover how to start a container both in Docker and within a [Kubernetes][Kubernetes link] cluster.

### Prerequisites

In order to run this container you'll need docker installed.

* [Windows](https://docs.docker.com/windows/started)
* [OS X](https://docs.docker.com/docker-for-mac/)
* [Linux](https://docs.docker.com/linux/started/)

Optionally:

* A [Kubernetes][Kubernetes link] cluster to enable Kubernetes api discovery of other nodes.

### Usage

The example below will start a single ClamAV instance.

```
docker run --name clamav -d -p 3310:3310 quay.io/ukhomeofficedigital/clamav:latest
```

To use with [Kubernetes][Kubernetes link] see the [kubernetes examples](examples/kubernetes.md).


#### Environment Variables

The variables and the defaults are shown below.
By default, the container does not depend on [Kubernetes][Kubernetes link].

* `CLAMD_SETTINGS_CSV="LogVerbose=yes,VirusEvent=/custom_alert.sh"` See [clamd.conf](http://linux.die.net/man/5/clamd.conf) for more details  
  and see [./clamd.conf](./clamd.conf) for the default settings.  
  To use specify a CSV of settings using Key=Value (not Key Value as the clamd.conf file) e.g. `"CLAMD_SETTINGS_CSV='Setting=value'"`
  Note, clamd has already been configured appropriately for a container but some useful settings include:    
  * `VirusEvent=/path/to/alert_script.sh` If mounted in the container, will provide a custom alert facility
  * `LogClean=yes` Will log every scan performed
* `FRESHCLAM_SETTINGS_CSV="LogVerbose=yes"` See [freshclam.conf](http://linux.die.net/man/5/freshclam.conf) for more details  
  and see [./freshclam.conf](freshclam.conf) for the default settings. See above for how this works.  
* `UPDATE=true` (default) will start freshclam daemon in background to watch for update antivirus definitions  
  `UPDATE=false` will watch for first successful update from separate sidecar container before starting
* `UPDATE_ONLY=true` configure as a sidecar container and run the update process in the foreground  
  `UPDATE_ONLY=false` (default) will run clamd and freshclam as normal.  

#### Ports

This container exposes:

* `3310` - Access the [Clam AV API](http://linux.die.net/man/8/clamd).

## Contributing

Feel free to submit pull requests and issues. If it's a particularly large PR, you may wish to discuss
it in an issue first.

Please note that this project is released with a [Contributor Code of Conduct](code_of_conduct.md).
By participating in this project you agree to abide by its terms.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the
[tags on this repository](https://github.com/UKHomeOffice/docker-clamav/tags).

## Authors

* **Lewis Marshall** - *Initial work* - [Lewis Marshall](https://github.com/LewisMarshall)

See also the list of [contributors](https://github.com/UKHomeOffice/docker-clamav/contributors) who
participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgments

* http://www.clamav.net/

## TODO:

* Ensure the DB access doesn't need to be for user 999 (so the volume can be mounted)...
* Long startup time, see point above.
* Add testing for Travis


[Kubernetes link]: https://kubernetes.io/
