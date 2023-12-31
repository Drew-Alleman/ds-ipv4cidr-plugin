# Welcome to ds-cidr-plugin
 a DataSurgeon extension for extracting ip addresses with CIDRS from text. 
 
## Jump Right In To Creating
- [Find Your Plugin File](#find-your-plugin-file)
- [Creating Your Own Plugin](#creating-your-own-plugin)
- [How to Use Your New Plugin](#how-to-use-your-new-plugin)

## Jump Right In To Managing Plugins
- [Adding a new Plugin](#adding-a-new-plugin)
- [Removing a Plugin](#removing-a-plugin)
- [Listing All Plugins](#listing-all-plugins)

## Creating Plugins
### Find Your Plugin File
If you're a Windows user, you'll find the plugin file in the  `C:\ds\` directory. If you're on Linux, look for the plugin file here: `~/.DataSurgeon/plugins.json`. And if by chance the plugin file isn't found in either of these directories, we'll automatically check the current working directory.

### Creating Your Own Plugin
Every field in the json object is important. To ensure your plugin works seamlessly with the DataSurgeon options `--add` and `--remove`, remember to upload your `plugins.json` file to a GitHub repository. Please make sure the filename remains as `plugins.json` and only include the plugin options you wish to upload. Here's a quick guide to the fields:

| Field          | Description                                                                                           |
|----------------|-------------------------------------------------------------------------------------------------------|
| content_type   | This should be a one-word description of the content you're searching for (no spaces). This is the word that gets printed alongside the matched content.                            |
| arg_long_name  | This is the unique argument name for the command-line interface. It must be unique across all plugins.             |
| help_message   | This is a short and sweet description of what your plugin does. It'll appear in the help message of the tool. |
| version        | This is the version number of your plugin (e.g: 1.0.0) |
| regex          | This is the regular expression that's used to match the content. To ensure compatibility with the `--clean` option, your regex should be designed such that the entire match `($0)` contains the exact content you're interested in. This allows the `--clean` option to extract only the relevant matched content. For testing your regex patterns, we recommend using https://regexr.com/|
| source_url | This is the URL to the GitHub repository hosting your plugin | 

Here's an example:

```json
[
    {
        "content_type": "ip_cidr",
        "arg_long_name": "ipv4_cidr",
        "version": "1.0.0",
        "help_message": "Extracts IPV4 Addresses with Cidr's attached",
        "regex": "\b((?:(?:25[0-5]|2[0-4]\\d|1\\d{2}|[1-9]\\d)\\.(?:25[0-5]|2[0-4]\\d|1\\d{2}|[1-9]\\d)|(?:25[0-5]|2[0-4]\\d|1\\d{2}|[1-9]\\d)\\.(?:25[0-5]|2[0-4]\\d|1\\d{2}|[1-9]\\d)\\.(?:25[0-5]|2[0-4]\\d|1\\d{2}|[1-9]\\d))(?::\\d{1,5})?)\b",
        "source_url": "https://github.com/Drew-Alleman/ds-ipv4cidr-plugin"
    }
]
```
### How to Use Your New Plugin
Once your plugin file is loaded, the option will be added as an additional argument. As you can see the name of the argument is the ```arg_long_name```. 
```
drew@DESKTOP-A5AO3TO$ ds -h

Options:
   ......
  -a, --aws                    Extract AWS keys
      --ipv4_cidr              Extracts IPV4 Addresses with Cidr's attached
  -V, --version                Print version
```
And here's how you can run it:
```
C:\Users\DrewQ\Desktop\DataSurgeon\target\release>ds.exe -f registry.txt --ip_cidr --clean
windows_registry: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
windows_registry: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer
windows_registry: HKEY_CLASSES_ROOT\.txt
windows_registry: HKEY_CURRENT_CONFIG\Software\Microsoft\windows\CurrentVersion\Internet
windows_registry: HKEY_USERS\S-1-5-21-3968247570-3627839484-3687258688-1000
windows_registry: HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers\New
windows_registry: HKEY_CURRENT_USER\Software\Microsoft\Command
windows_registry: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters
windows_registry: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

## Managing Plugins
### Adding a New Plugin
To add a new plugin you need to use the ```--add <URL>``` option. The URL needs to be a remote github repository hosting a ```plugins.json``` file. [How to use your new plugin](https://github.com/Drew-Alleman/ds-winreg-plugin/blob/main/README.md#how-to-use-your-new-plugin).
```
drew@DESKTOP-A5AO3TO:~$ ds --add https://github.com/Drew-Alleman/ds-ipv4cidr-plugin
[*] Download and added plugin: https://github.com/Drew-Alleman/ds-ipv4cidr-plugin
```
### Listing All Plugins
To list all plugins you can use the ```--list``` option.
```
drew@DESKTOP-A5AO3TO$ ds --list

Plugin File: /home/drew/.DataSurgeon/plugins.json

Source URL                                      | Argument Long Name
https://github.com/Drew-Alleman/ds-ipv4cidr-plugin | ipv4_cidr
```
### Removing a Plugin
To remove a plugin you don't want anymore you can use the ```--remove``` option.
```
drew@DESKTOP-A5AO3TO:~$ ds --remove https://github.com/Drew-Alleman/ds-ipv4cidr-plugin/
[*] Removed plugin: https://github.com/Drew-Alleman/ds-ipv4cidr-plugin
```
