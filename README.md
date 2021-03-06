# Sara install

### Repository of rosinstall and install script for easy installation
This repo contains an enclosed catkin workspace along with tools designed to help you manage the versioning of your custom configuration.

Note that since this is a repo containing a workspace containing other repos. This will sligtly affect the way you use git on your packages. See the [Workspace versioning layout](#workspace-versioning-layout) section for details.

## Table of content
1. [Recommended tools](#recommended-tools)
1. [Requirements](#requirements)
1. [.rosinstall](#rosinstall)
1. [src](#src)
1. [Workspace versioning layout](#workspace-versioning-layout)
1. [Main configurations](#main-configurations)
1. [Main workflow](#main-workflow)
1. [Branch nomenclature](#branch-nomenclature)
1. [Troubleshooting](#troubleshooting)

## Recommended tools
These tool are recommended by Walking Machine to work on Sara. Those are only mere recommendation. You are invited to use your prefered IDE (except visual studio. Yerk! :P ).
1. atom
1. pycharm
1. clion

## Requirements
1. [Ubuntu 16.04 LTS](http://releases.ubuntu.com/16.04/)
1. Install [ROS Kinetic for Ubuntu](https://wiki.ros.org/kinetic/Installation/Ubuntu) by following all steps up to 1.7. After completing these steps you can continue with the tutorials which are good for learners. Please note that these tutorial are for personal learning only and are not required on SARA. For experimented users you can simply install ROS and skip the tutorial.
1.  git. ``` sudo apt-get install git ```

## .rosinstall
Sara_install uses [wstool](https://wiki.ros.org/wstool). This piece of software uses a configuration file named [.rosinstall](src/.rosinstall) which define the list of packages required to install your configuration. Depending on your needs, you can edit this file to change your configuration.

## src
The src directory is the place where all packages are cloned in a catkin workspace. Sara_install is no exception to this rule. The difference is that the packages within src can be managed by wstool.

## Workspace versioning layout
This workspace is special since it is encapsulated within the sara_install git repo. This means that it's configuration is versionised and you can switch branches to change configuration. Though, you need to keep in mind that the [src directory](#src) is purposely ignored by sara_install. [See .gitignore](.gitignore). This is to let wstool be the main package manager and also to maintain retrocompatibility whith the old manual workflow. Each packages within src won't be modified by sara_install. They remain their own individual git repositories.

![layout](WorkspaceLayout.png)

For instance, if you have a custom configuration within your src and want to try another one, you can checkout the new configuration branch without changing the content of your src. You will need to install this new configuration to update your src.

## Main configurations
Depending on waht you want installed, you can checkout one of these specific branches for the most supported configurations.: 
1. ```config/sara_kinetic``` for installing the full sara workspace.
1. ```config/simulation_kinetic``` for a simulation based configuration.
1. ```master``` is the default. It barely contains anything. Perfect for a minimalistic test.


## Main workflow
### First installation
If you want to install every packages with their main branches.
1. Clone this repo anywhere. Optionally, choose the desired branch.``` git clone https://github.com/WalkingMachine/sara_install.git [-b <desired_branch>] ``` See [Main configurations](#main-configurations) for a list of default branches.
1. Source the setup.sh script. ``` source sara_install/script/setup.sh ```
1. Run the install script ``` INSTALL_SARA``` and pay attention to the screen. There might be prompts for passwords and stuff.
1. Optionnaly, you can add the following line to your ```~/.bashrc```: ``` source <installation path>/sara_install/script/setup.sh ``` Make sure to replace the ```<installation path>``` with the path to sara_install.
1. Your are now ready to code.

### Make modifications
1. Edit code within the desired packages, create feature branches, commit your changes, push them.

```
cd src/my_package
git pull
editing
# To check that you really are on master
git status 
# To go in a newly created branch
git checkout -b feature/<my_awesome_feature>
git commit edited_file
git push -u origin feature/<my_awesome_feature>
```

### Make custom configuration
When you change branches or add packages in the [src](src/) folder, you are actually creating a custon configuration.
To officialise your custom configuration, follow these steps:
1. Use ``` STATUS_SARA ``` to see all modified packages.
1. Use ``` SCRAPE_SARA_CONFIG ``` to take a snapshot of your current configuration.
1. ``` git checkout -b config/<my_awesome_config> ```
1. ``` git commit src/.rosinstall ```
1. ``` git push -u origin config/<my_awesome_config> ```

### Update all packages
If you need to work with all the latest packages, use the update.sh script.
```
UPDATE_SARA
```

 ### Scrape the workspace
 If you need to update the .rosinstall file from your custom configuration, use the scrape_config.sh script.
```
SCRAPE_SARA_CONFIG
```

### List of functions
A full list of functions is available [here](script/).

### Branch nomenclature
Here is a list of namespaces we use to define our branches names:
1. Official branches used in competition: ```config/...```
1. When developping a new feature on a package: ```dev_config/<package_name>/...```
1. To perform tests using a specific set of packages or to test a bugfix: ```test_config/<package_name>/...```
1. When developping a new feature for sara_install itself: ```feature/...```
1. When making a fix for a bug in sara_install itself: ```fix/...```

## Troubleshooting

* catkin_make not found
  1. Close the terminal and reopen it.
* Build error, finished before reaching [100%]
  1. Look for indication of missing packages. Install them if you can and create a issue mentioning the error on [the issue tracker](https://github.com/WalkingMachine/sara_install/issues).
 
