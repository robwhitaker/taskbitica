Taskwarrior-Habitica Bridge
==========

Sync Taskwarrior tasks with Habitica (and _vice versa_).

## What It Does

- Add tasks added through Taskwarrior (e.g. `task add`) to Habitica
- Modify tasks modified through Taskwarrior (e.g. `task modify`) on Habitica
- Bidirectionally synchronize tasks between Taskwarrior and Habitica

## Installation and Setup

### Dependencies

- [Nix](https://nixos.org/nix/)
- Cabal (TODO: update with Nix-only install)

### Compiling the program

1. If you don't have the Nix package manager installed, install it now: `curl https://nixos.org/nix/install | sh`
2. Clone this repository: `git clone https://github.com/robwhitaker/taskwarrior-habitica-bridge.git`
3. `cd` into the project folder: `cd taskwarrior-habitica-bridge`
4. Build the project: `cabal --enable-nix v1-configure && cabal --enable-nix v1-build`
5. The compiled executable will be located at `dist/build/task2habitica/task2habitica`. Put this executable somewhere in your PATH.

### Installing the hooks

1. Copy all the files in the `hooks/` directory into your Taskwarrior hooks folder (`.task/hooks/` by default).

### Adding Habitica UDAs to Taskwarrior

1. Include `habitica_udas.rc` (TODO: add rc file to repo) in your `.taskrc` file: `include path/to/habitica_udas.rc`

### Providing Habitica credentials

In order to interact with your tasks, Taskwarrior-Habitica Bridge needs your Habitica user ID and API key. You must provide them in your `.taskrc` file (or some included rc file) as follows:

```
habitica.user_id=YOUR_USER_ID
habitica.api_key=YOUR_API_KEY
```

## Usage

When editing tasks in Taskwarrior, the hook scripts will automatically update them on Habitica so you can use Taskwarrior as per usual.

`task2habitica sync` will synchronize tasks between Taskwarrior and Habitica in the case things don't sync automatically (e.g. you've added a task on Habitica you want to pull into Taskwarrior).

### A note on recurring tasks in Taskwarrior

Taskwarrior-Habitica Bridge supports syncing instances of recurring Taskwarrior tasks. However, the first time you create a recurring task in Taskwarrior, the `on-add` hook doesn't trigger. This means you will need to run `task2habitica sync` to get that first instance onto Habitica. After that, things should work as expected.

### Other hook scripts

While `task2habitica sync` runs, it runs various task commands which will trigger hooks. An environment variable, `TASK2HABITICA_RUNNING`, is set (to `1`) while the `sync` command runs so that you may check if a sync is occurring and possibly skip the processing of certain hook scripts if needed.

## Not Supported
- Syncing tags
- Syncing description (Taskwarrior has no notion of a description in the Habitica sense)
