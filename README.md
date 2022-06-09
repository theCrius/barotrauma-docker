# Barotrauma Docker

A [Docker](https://www.docker.com/) image for running a dedicated server for the game [Barotrauma](https://barotraumagame.com/).  
Tested with server version `0.17.15.0`.

---

## Run the Server

An example sequence to build then run:

```bash
export UID=$(id -u)
export GID=$(id -g)
docker-compose build
docker-compose up -d
```

Specifying the STEAM_EPOCH will build the image downloading the latest version available even if the image was built already previously.

If you are trying to run a modded server and need `Lua for Barotrauma`, specify the args `LUA_SERVER=true`.

```bash
export UID=$(id -u)
export GID=$(id -g)
docker-compose build --build-arg LUA_SERVER=true
docker-compose up -d
```

You will then need to add your mods in the mods volume (default `$HOME/barotrauma-server/`) and edit the `config_player.xml`file with the necessary references to the mods. Run `docker-compose down && docker-compose up -d` to have the server read the new configuration. More details below.

## Barotrauma Update

Provided there isn't a breaking change from the developers, to update your server, run:

```
export UID=$(id -u)
export GID=$(id -g)
docker-compose build --no-cache
```
Use the additional argument for modded servers (`--build-arg LUA_SERVER=true`) in case you are running mods.  
Then, finally: `docker-compose up -d` to restart the container.

## Ports

- Port `27015` *must* be opened for client connections
- Port `27016` can optionally be opened for steam communication

## Volumes

The container uses three volumes:

- Configs files at `$HOME/barotrauma-server/configs`, mounted as `/barotrauma/volumes/config` in the container
- Mods files at `$HOME/barotrauma-server/mods`, mounted as `/barotrauma/volumes/mods` in the container
- Subs files at `$HOME/barotrauma-server/subs`, mounted as `/barotrauma/volumes/subs` in the container
- Saves files at `$HOME/barotrauma-server/saves`, mounted as `/barotrauma/volumes/saves` in the container

The various directories/files are then linked with the correct file/directories where the game server expect them to be.
For example, the `<gamedir>/config_player.xml` is actually a link to `/barotrauma/volumes/config/config_player.xml` in the container.

## Mods

To install mods is better to subscribe on them from your steam account so that they are downloaded from the workshop.
Then open the game and choose "Host Server" from within the main menu'. Once you are in the lobby, you can disconnect.  
In your game installation folder you should look for the file `config_player.xml`.  
That will contains the necessary configuration strings including your mods or custom submarines.

Upload mods, subs and `config_player.xml` on your host machine and, once the container is running, copy/move the mods' directories into the `mods` volume and the submarines's directories into the `subs` volume.

Now edit the `config_player.xml` in the `configs` volume to include the `regularpackages` sections that include your mods and submarines.  
You will need to edit the ones referring to the submarins so that they point to `LocalMods` and the ones that refer to mods to point to `Daedalic Entertainment GmbH/Barotrauma/WorkshopMods/Installed`.

The result would be something like:

```
  [...]
    <regularpackages>
      <!--A Downloaded Mod-->
      <package
        path="Daedalic Entertainment GmbH/Barotrauma/WorkshopMods/Installed/987654321/filelist.xml" />
      <!--A Custom Submarine-->
      <package
        path="LocalMods/123456789/filelist.xml" />
    </regularpackages>
  [...]
```

Restart the container with `docker-compose down && docker-compose up -d` and you should be done.

## Credits

Built starting from `FragSoc/barotrauma-docker` image, then adding docker-compose support and changing directories' references to match the latest version of Barotrauma, at this point overhauling the entire thing. Credits left to thanks for the initial image that helped understanding how this game server was supposed to be set up prior to the "mods apocalypse".

## Licensing

<img alt="GitHub" src="https://img.shields.io/github/license/FragSoc/barotrauma-docker?style=flat-square"> The few files in this repository are licensed under the [GPL](https://www.gnu.org/licenses/gpl-3.0.en.html).

However, Barotrauma itself is licensed by [Undertow Games](https://undertowgames.com/) and [Fakefish](http://fakefishgames.com/#home), no credit is taken for the software running in this container.
Read [their EULA](https://github.com/Regalis11/Barotrauma/blob/master/EULA.txt) for more information.
