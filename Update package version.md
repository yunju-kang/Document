1. ##### Update the repositories

   `sudo apt update`

2. ##### Install the new packages

   `sudo apt install <package>`

3. ##### Set the package priority

   Set old version to be less priority

   `sudo update-alternatives --install <package_path> <package_name> <package_path>/<package_name>.<old_version> <lower_number>`

   ex) `sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1`

   Set new version to be higher priority

   `sudo update-alternatives --install <package_path> <package_name> <package_path>/<package_name>.<new_version> <higher_number>`

   ex)  `sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 2`

