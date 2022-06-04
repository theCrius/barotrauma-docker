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

## Run the Server

An example sequence to build then run:

```bash
docker-compose build --build-arg STEAM_EPOCH=$(date +%s)
docker-compose up -d
```

Specifying the STEAM_EPOCH will build the image downloading the latest version available even if the image was built already previously.

If you are trying to run a modded server and need `Lua for Barotrauma`, specify the args `LUA_SERVER=true`.

```bash
docker-compose build --build-arg STEAM_EPOCH=$(date +%s) --build-arg LUA_SERVER=true
docker-compose up -d
```

You will then need to add your mods (`docker volume ls` to see the volumes, `docker volume inspect <volume_name>` to see the path) in the mods volume and edit the `config_player.xml`file with the necessary references to the mods.
Run `docker-compose down && docker-compose up -d` to have the server read the new configuration.

**Note:** *the UID of the user in the container defaults to `999`, pass `UID` as a build arg to change this*

### Barotrauma Update

Provided there isn't a breaking change, to update your server, simply run:

```
docker-compose build --build-arg STEAM_EPOCH=$(date +%s) && docker-compose up -d
```

Use the additional argument for modded servers in case you are running mods.

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

The last version messed up how mods works compared to how it was before and there are some bugs here and there, so for now, this is supporting vanilla only.  
There is a script to install mods but it's not reliable right now.

## Licensing

The few files in this repository are licensed under the [GPL](https://www.gnu.org/licenses/gpl-3.0.en.html).

However, Barotrauma itself is licensed by [Undertow Games](https://undertowgames.com/) and [Fakefish](http://fakefishgames.com/#home), no credit is taken for the software running in this container.
Read [their EULA](https://github.com/Regalis11/Barotrauma/blob/master/EULA.txt) for more information.
