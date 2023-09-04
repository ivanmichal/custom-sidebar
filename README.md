# Custom Sidebar

This is an extra module for [Home Assistant Frontend](https://www.home-assistant.io/integrations/frontend/#extra_module_url) on [Home Assistant](https://www.home-assistant.io/)

This plugin allows you to rearrange, hide, and add items in the Home Assistant sidebar. (Overview, Map, History, etc)

## Screenshots

### [1] Collapsed Sidebar

Before                     |           After
:-------------------------:|:-------------------------:
![](https://raw.githubusercontent.com/blizzrdof77/custom-sidebar/master/.github/images/sidebar-before-example.png)  |  ![](https://raw.githubusercontent.com/blizzrdof77/custom-sidebar/master/.github/images/sidebar-example.png)

### [2] Expanded Sidebar

Before                     |           After
:-------------------------:|:-------------------------:
![](https://raw.githubusercontent.com/blizzrdof77/custom-sidebar/master/.github/images/sidebar-before-example-expanded.png)  |  ![](https://raw.githubusercontent.com/blizzrdof77/custom-sidebar/master/.github/images/sidebar-after-example-expanded.png)


## Fork justification

Unfortunately, [Villhellm](https://github.com/Villhellm) has [passed away](https://www.gofundme.com/f/home-assistant-community-remembers-villhellm). I wish to offer my respects and sincere
condolences to the family and friends.

This sad event has left this precious plugin orphaned with a [blocking issue](https://github.com/Villhellm/custom-sidebar/issues/40).

## HACS Installation and Configuration

### Step 1

Install Custom Sidebar plugin with HACS

### Step 2

By default this will only function if you load into a Lovelace view. To make sure this plugin works all the time make sure to follow the instructions all the way through.

To load the module into your instance you will need to add it to the frontend configuration in configuration.yaml.

NOTE: If you have [browser_mod](https://github.com/thomasloven/hass-browser_mod) installed then this step is unnecessary. Browser__mod loads all Lovelace resources globally by default.

Ex:
```yaml
frontend:
  extra_module_url:
    - /hacsfiles/custom-sidebar/custom-sidebar.js
```

### Step 3

The order configuration must be stored in `<config directory>/www/sidebar-order.yaml`. You will create this file yourself. All items will be under the root `order:`.
They will be arranged in order from top to bottom.

Ex:
```yaml
order:
  - item: map
    hide: true
  - item: developer tools
    href: /developer-tools/state
  - item: overview
  - item: history
    bottom: true
  - item: logbook
    bottom: true
```

## Manual Installation and Configuration

### Step 1

Install `custom-sidebar` by copying `custom-sidebar.js` from this repo to `<config directory>/www/custom-sidebar.js` on your Home Assistant instance.

### Step 2

To load the module into your instance you will need to add it to the frontend configuration in configuration.yaml.

Ex:
```yaml
frontend:
  extra_module_url:
    - /local/custom-sidebar.js
```

### Step 3

The order configuration must be stored in `<config directory>/www/sidebar-order.yaml`. You will create this file yourself. All items will be under the root `order:`.
They will be arranged in order from top to bottom.

Ex:
```yaml
order:
  - item: map
    hide: true
  - item: developer tools
    href: /developer-tools/state
  - item: overview
  - item: history
    bottom: true
  - item: logbook
    bottom: true
  - new_item: true
    item: Server Controls
    href: /config/server_control
    icon: "mdi:server"
    bottom: true
```

## Title
| Name | Type | Requirement | Description
| ---- | ---- | ------- | -----------
| title | string | **Optional** | Change the default title over the sidebar

## Order

| Name | Type | Requirement | Description
| ---- | ---- | ------- | -----------
| order | list([item](#item-options)) | **Required** | List of items you would like to rearrange.

## Item Options

| Name | Type | Requirement | Description
| ---- | ---- | ------- | -----------
| item | string | **Required** | This is a string that will be checked for in the display name of the sidebar item. It can be a substring such as `developer` instead of `Developer Tools`. It is not case sensitive.
| name | string | **Optional** | Change the name of the existing item to this string.
| bottom | boolean | **Optional** | Setting this option to `true` will group the item with the bottom items (Configuration, Developer Tools, etc) instead of at the top.
| hide | boolean | **Optional** | Hide item in sidebar.
| exact | boolean | **Optional** | Specify whether the item string match will be exact match instead of substring.
| href | string | **Optional** | Define the href for the sidebar link.
| icon | string | **Optional** | Set the icon of the sidebar item.
| new_item | boolean | **Optional** | Set to true to create a new link in the sidebar. Using this option now makes `item`, `href`, and `icon` required.
| open_new | boolean | **Optional** | Set to true to open link in a new window.

## Exceptions
Exceptions can be used if you would like to define an order for a specific user/device.

| Name | Type | Requirement | Description
| ---- | ---- | ------- | -----------
| base_order | bool | **Optional** | If true this will run rearrangement for your base order configuration before running this exception. Default is false.
| user | string, list | **Optional** | Home Assistant user name you would like to display this order for.
| device | string, list | **Optional** | Type of device you would like to display this order for. ex: ipad, iphone, macintosh, windows, android
| not_user | string, list | **Optional** | Every Home Assistant user name *except* this user name.
| not_device | string, list | **Optional** | Every device *except* this device. ex: ipad, iphone, macintosh, windows, android
| order | [order](#order) | **Required** | Define and order.


Ex sidebar-order.yaml using exceptions:
```yaml
title: Hass
order:
  - item: map
    hide: true
  - item: developer tools
    href: /developer-tools/state
  - item: overview
  - item: history
    bottom: true
  - item: logbook
    bottom: true
  - new_item: true
    item: Server Controls
    href: /config/server_control
    icon: "mdi:server"
    bottom: true
exceptions:
  - user: will
    base_order: true
    order:
      - item: map
        hide: false
      - item: developer tools
        hide: true

```

NOTE: Notifications are not part of the same div as the other sidebar items; it is not moveable.

NOTE: If you are using a language other than English make sure to include both the English word and a second entry with the translated word that is displayed in your browser for the `item: ` portion of the config. This will ensure that the item is moved/hidden regardless of the time at which it is rendered.
Example: Language Portuguese
```yaml
  - item: history
    hide: true
  - item: Histórico
    hide: true
```
