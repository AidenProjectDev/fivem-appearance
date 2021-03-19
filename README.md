# cfx-appearance

A flexible player customization script for FiveM.

This resource was designed to manage all GTA V player/ped customization in only one place, with an opinionated way to handle the data.

## Features

- Freemode ped full customization (Head blend, Face features, Head overlays, Components, Props).
- Exports API to get and set every part of player/ped appearance.
- [Built-in customization feature](https://streamable.com/t59gdt "Preview").

## Installation

**Download**

Go to releases and get the latest version.

**Build yourself**

1. Clone the repository into your `resources/[local]` folder.
2. Execute the build script.

   ```bash
   yarn build
   ```

3. Start development.

**Disclaimer**

This is a development resource, if you don't use the exports the resource itself will do nothing.

## ConVars

Since this is a client script, you will need to use **setr** to set these convars.

- **cfx-appearance:customization**: 1 or 0, default **1**, toggle the built-in customization interface and customization exports.
- **cfx-appearance:locale**: the name of one file inside `locales/`, default **en**, choose the locale file for the customization interface.

config.cfg example:

```cfg
setr cfx-appearance:customization 1
setr cfx-appearance:locale "en"
ensure cfx-appearance
```

## Client Exports

### Appearance

| Export              | Parameters                                     | Return            |
| ------------------- | ---------------------------------------------- | ----------------- |
| getPedModel         | ped: _number_                                  | _string_          |
| getPedComponents    | ped: _number_                                  | _PedComponent[]_  |
| getPedProps         | ped: _number_                                  | _PedProp[]_       |
| getPedHeadBlend     | ped: _number_                                  | _PedHeadBlend_    |
| getPedFaceFeatures  | ped: _number_                                  | _PedFaceFeatures_ |
| getPedHeadOverlays  | ped: _number_                                  | _PedHeadOverlays_ |
| getPedHair          | ped: _number_                                  | _PedHair_         |
| getPedAppearance    | ped: _number_                                  | _PedAppearance_   |
| setPlayerModel      | model: _string_                                | _Promise\<void\>_ |
| setPedComponent     | ped: _number_, component: _PedComponent_       | _void_            |
| setPedComponents    | ped: _number_, components: _PedComponent[]_    | _void_            |
| setPedProp          | ped: _number_, prop: _PedProp_                 | _void_            |
| setPedProps         | ped: _number_, props: _PedProp[]_              | _void_            |
| setPedFaceFeatures  | ped: _number_, faceFeatures: _PedFaceFeatures_ | _void_            |
| setPedHeadOverlays  | ped: _number_, headOverlays: _PedHeadOverlays_ | _void_            |
| setPedHair          | ped: _number_, hair: _PedHair_                 | _void_            |
| setPedEyeColor      | ped: _number_, eyeColor: _number_              | _void_            |
| setPlayerAppearance | appearance: _PedAppearance_                    | _void_            |
| setPedAppearance    | ped: _number_, appearance: _PedAppearance_     | _void_            |

### Customization

This export is only available if **cfx-appearance:customization** is set to 1.

| Export                   | Parameters                                                  | Return |
| ------------------------ | ----------------------------------------------------------- | ------ |
| startPlayerCustomization | callback ((appearance: PedAppearance \| undefined) => void) | _void_ |

## Examples

**Customization command (Lua)**

```lua
RegisterCommand('customization', function()
  exports['cfx-appearance']:startPlayerCustomization(function (appearance)
    if (appearance) then
      print('Saved')
    else
      print('Canceled')
    end
  end)
end, false)
```

**Start player customization with callback (TypeScript)**

```typescript
const exp = (global as any).exports;

exp["cfx-appearance"].startPlayerCustomization((appearance) => {
  if (appearance) {
    console.log("Customization saved");
    emitNet("genericSaveAppearanceDataServerEvent", JSON.stringify(appearance));
  } else {
    console.log("Customization canceled");
  }
});
```

**Set player appearance (TypeScript)**

```typescript
const exp = (global as any).exports;

onNet("genericPlayerAppearanceLoadedServerEvent", (appearance) => {
  exp["cfx-appearance"].setPlayerAppearance(appearance);
});
```

## Data

Scripts used to generate some of the resource's data.

[Peds](https://gist.github.com/snakewiz/b37a18e92cc0b112ce0fa57b1096b96b "Gist")

## Credits

- [TomGrobbe](https://github.com/TomGrobbe) for the customization camera behavior
- [root-cause](https://github.com/root-cause) for some of the game data
- [xIAlexanderIx](https://github.com/xIAlexanderIx) for general inspiration
