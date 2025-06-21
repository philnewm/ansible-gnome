# Gnome Setup

[![Alma9 CI](https://github.com/philnewm/ansible-gnome/actions/workflows/alma9-ci-caller.yml/badge.svg)](https://github.com/philnewm/ansible-gnome/actions/workflows/alma9-ci-caller.yml) [![Rocky9 CI](https://github.com/philnewm/ansible-gnome/actions/workflows/rocky9-ci-caller.yml/badge.svg)](https://github.com/philnewm/ansible-gnome/actions/workflows/rocky9-ci-caller.yml) [![CentOS9 CI](https://github.com/philnewm/ansible-gnome/actions/workflows/centosstream9-ci-caller.yml/badge.svg)](https://github.com/philnewm/ansible-gnome/actions/workflows/centosstream9-ci-caller.yml) [![Debian12 CI](https://github.com/philnewm/ansible-gnome/actions/workflows/debian12-ci-caller.yml/badge.svg)](https://github.com/philnewm/ansible-gnome/actions/workflows/debian12-ci-caller.yml) [![Ubuntu22.04 CI](https://github.com/philnewm/ansible-gnome/actions/workflows/ubuntu2204-ci-caller.yml/badge.svg)](https://github.com/philnewm/ansible-gnome/actions/workflows/ubuntu2204-ci-caller.yml)

This roles installs a gnome desktop environment including a bunch of extensions and themes.

Extensions (not available on every distro):

* [drive-menu](https://extensions.gnome.org/extension/7/removable-drive-menu/)
* [user-theme](https://extensions.gnome.org/extension/19/user-themes/)
* [appindicator](https://extensions.gnome.org/extension/615/appindicator-support/)
* [sound-output-device-chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)
* [no-overview](https://extensions.gnome.org/extension/4099/no-overview/)
* [tiling-assistant](https://extensions.gnome.org/extension/3733/tiling-assistant/)
* [dash-to-panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)
* [system-monitor](https://extensions.gnome.org/extension/120/system-monitor/)
* [clipboard-indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)

Themes:

* shell_theme: [Lavanda-Dark](https://github.com/vinceliuice/Lavanda-gtk-theme)
* icon_theme: [Tela-circle-icon-theme](https://github.com/vinceliuice/Tela-circle-icon-theme)
* cursor_theme: [Qogir-cursors](https://github.com/vinceliuice/Qogir-icon-theme)
* wallpaper: [Dynamic_Wallpapers](https://github.com/saint-13/Linux_Dynamic_Wallpapers)

Additionally, this role includes a full vagrant based molecule testing setup for CentosStream9, Alma9, Rocky9, Ubuntu2210, Debian12 VMs at `molecule/`

## Structure

```code
📦 gnome_setup
 ┣ 📂 defaults
 ┃ ┗ 📂 main
 ┃   ┣ 📜 common.yml
 ┃   ┣ 📜 extensions.yml
 ┃   ┗ 📜 themes.yml
 ┣ 📂 handlers
 ┃ ┗ 📜 main.yml
 ┣ 📂 meta
 ┃ ┗ 📜 main.yml
 ┣ 📂 molecule
 ┃ ┗ 📂 default
 ┃   ┗ 📜, 📜, 📜, scenario_files
 ┣ 📂 tasks
 ┃ ┣ 📜 absent.yml
 ┃ ┣ 📜 dependencies.yml
 ┃ ┣ 📜 install_extensions.yml
 ┃ ┣ 📜 install_themes.yml
 ┃ ┣ 📜 main.yml
 ┃ ┣ 📜 present_extensions.yml
 ┃ ┣ 📜 present_finalize.yml
 ┃ ┣ 📜 present_setup.yml
 ┃ ┣ 📜 present_themes.yml
 ┃ ┣ 📜 present.yml
 ┃ ┗ 📜 tests.yml
 ┣ 📂 templates
 ┃ ┗ 📜 monitors.xml.j2
 ┣ 📂 vars
 ┃ ┗ 📜 main.yml
 ┗ 📜 README.md

```

The variables are split up into three files according to their content (`common.yml`, `extensions.yml`, `themes.yml`).
The same goes for tasks, and they are additionally prefixed with `present` or `absent` depending on them being intended to set up a certain feature or remove it.
As you can see currently the absent implementation only exist in one single file since it's feasible for my current use case.
The tasks related to extensions and themes have their dedicated install_*-file tasks.
The present_*-files meant to take care of preparation steps and check if the extensions/themes are already present or not.
In case not they loop over the tasks to set up and run their install-routine implemented in (`install_extension` and `install_themes`)

## Requirements

The following packages are required but will also be installed by the role automatically.

### RedHat based systems

* python3-psutil
* glibc-langpack-en

### Debian based systems

* python3-psutil

## Role Variables

* defaults/main/common.yml
  * install_dir: Custom install directory
  * additional_rhel_repos: Extra repositories for RedHat based distros like CentOSStream9, AlmaLinux, Rocky, ...
  * gnome_packages: Gnome default package lists per distribution
  * obsolete_packages: Gnome default packages to remove from base install
  * gdm_config_file: Path to gdm configuration file per distribution
* defaults/main/extensions.yml
  * extension_path_system: directories gnome extensions should be installed in
  * obsolete_gnome_extensions: List of extensions to remove from default gnome-shell install
  * gnome_extensions: list of extensions including their ID and address
* defaults/main/themes.yml
  * themes_path: directory gnome themes should be installed in
  * icons_path: directory gnome icons should be installed in
  * wallpapers_path: directory gnome wallpapers should be installed in
  * gnome_themes: Dictionary of themes including their GitHub download package

## Dependencies

This role doesn't depend on any additional ansible-galaxy roles

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---

- name: Install and gnome desktop, extensions and themes
  hosts: client
  vars:
    system_local_lang: "en_US.UTF-8"

  tasks:
    - name: Include gnome role present
      ansible.builtin.include_role:
        name: gnome_setup
      vars:
        state: present

...
```

## License

MIT
