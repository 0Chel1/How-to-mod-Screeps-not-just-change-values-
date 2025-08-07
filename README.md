# Introduction
So you want to make a custom structures in Screeps. For that you will need to set up a private server but not the official one. It is impossible to test your mod on official private server.
# Setting up private server
Install [Jomik's](https://github.com/Jomik/screeps-server) server. Then install [Docker desktop](https://docs.docker.com/desktop/setup/install/windows-install).

To start the server launch Docker desktop. In server folder right click and open the Terminal. Past this command: docker compose up -d. To make sure server is running open Docker desktop and make sure that you see the server in servers list and little circle doesn't turning green and blue every second. If this happening then it means you did something wrong in config.yml or docker-compose.yml. To open CLI you need to open another Terminal in server folder and past this command(if server running): docker compose exec screeps cli.

Now open the game and go in private server section.

host: localhost

port:21025
# How to make a mod
Create mods folder in server folder if you don't have one. Then create your mod folder and name it. It will be your mod name(actualy it's just for path). In your mod folder create package.json file and enter this:
```json
{
  "name": "Your mod name",
  "version": "0.0.1",
  "description": "Your description",
  "main": "index.js",
  "screeps_mod": true
}
```
Then add index.js file in folder.

Now to implement your mod on server open config.yml and in mods section add this line:
```yml
- /mods/Your mod name
```
Open docker-compose.yml and add this line in screeps: volumes:
```yml
- ./mods:/mods
```
Like this:

<img width="423" height="313" alt="tutorial" src="https://github.com/user-attachments/assets/b860281f-2ef6-461c-9570-aa54a4c68705" />

# How to make custom structure
In index.js you write your code.
```js
module.export = function(config)
{
  //To make our structure and make it visible you need to upload your sprite on internet an then paste url to it in const value
  //if you uploaded file on github the link must be like this https://raw.githubusercontent.com/YourNickname/RepositoryName/refs/heads/main/
  const yourFileUrl = 'your file url here';
  config.backend.renderer.resources['your_texture'] = yourFileUrl;
  config.backend.renderer.metadata['structureName'] = {
    processors: [
        {
            type: 'sprite',
            once: true,
            payload: {
                texture: 'your_texture',
                width: 100,
                height: 100,
            }
        }
    ]
};
  //You can also add sidebar info
  config.backend.customObjectTypes.structureName = {
    sidepanel: '<div><label>Description:</label><span>This is description.</span></div>'
  };
}
```
Save and launch server an CLI. Don't forget to write a command in CLI system.resetAllData() so you can place your spawn on map.

After restart place a spawn and write this command in CLI: 
```js
storage.db['rooms.objects'].insert({type: 'structureName', room: yourRoomName, x: 20, y: 20});
```
# End
For now this is end but maybe in future i will update this guide

Big thanks to Robalian for helping me with moding this game.
