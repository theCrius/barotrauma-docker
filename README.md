<center>
    <a href=https://barotraumagame.com/>
        <img width=100% src="https://ehpodcasts.files.wordpress.com/2019/06/barotrauma-logo.jpg"/>
    </a>
    <br/>
    <img alt="Travis (.com)" src="https://img.shields.io/travis/com/FragSoc/barotrauma-docker?style=flat-square">
    <img alt="GitHub" src="https://img.shields.io/github/license/FragSoc/barotrauma-docker?style=flat-square">
</center>


---

A [docker](https://www.docker.com/) image for running a dedicated server for the game [Barotrauma](https://barotraumagame.com/).
Tested with server version `0.17.12.0`.

Built starting from `FragSoc/barotrauma-docker`, adding docker-compose support and changing directories' references to match the latest version of Barotrauma.

## Usage

An example sequence to build then run:

```bash
docker-compose build --build-arg STEAM_EPOCH=$(date +%s)
docker-compose up
```

Add `-d` to the `docker-compose up` command to have it run in detached mode (background).
Specifying the STEAM_EPOCH will build the image downloading the latest version available.

**Note:** *the UID of the user in the container defaults to `999`, pass `UID` as a build arg to change this*

### Ports

- Port `27015` *must* be opened for client connections
- Port `27016` can optionally be opened for steam communication

### Volumes

The container uses three volumes:

- Server configuration files at `/config`
- Mods files at `/mods`
- Saves at `/saves`

**Note:** *if you use a [bind mount](https://docs.docker.com/storage/bind-mounts/), the host path you mount into the container *must* be owned by the UID you passed to the build (default `999`)*

## Mods

The last version messed up how mods works, so this is supporting vanilla only. There is a script to install mods but it's not reliable right now.

## Licensing

The few files in this repository are licensed under the [GPL](https://www.gnu.org/licenses/gpl-3.0.en.html).

However, Barotrauma itself is licensed by [Undertow Games](https://undertowgames.com/) and [Fakefish](http://fakefishgames.com/#home), no credit is taken for the software running in this container.
Read [their EULA](https://github.com/Regalis11/Barotrauma/blob/master/EULA.txt) for more information.
