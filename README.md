# Variscite Debian Package Repositories

This repository hosts Debian packages for various Variscite System on Modules (SoMs). The packages are organized into PPAs (Personal Package Archives) and are served through GitHub Pages. The packages in this repository are designed for specific Variscite SoMs and can be used on Debian-based systems such as Bookworm.

## Table of PPAs

| PPA Name         | Description                                           |
| ---------------- | ----------------------------------------------------- |
| `var`            | Debian packages common to all Variscite SoMs          |
| `var-ti`         | Debian packages common to all Variscite TI-based SoMs |
| `am62x-var-som`  | Debian packages specific to Variscite VAR-SOM-AM62    |

## How to Add New Packages to the PPA

To add new Debian packages to the PPA, follow these steps:

1. Place the `.deb` files in the `pool` or `pool/<release>` directory of the PPA.

    ```bash
    cp /path/to/package.deb /path/to/ppa/pool/
    ```

    or

    ```bash
    cp /path/to/package.deb /path/to/ppa/pool/<release>/
    ```

    For example, add a package to var-ti bookworm release:
    ```bash
    cp /path/to/package.deb var-ti/pool/bookworm/
    ```

2. Run the provided update script to organize the `.deb` files and update the PPA files:

    ```bash
    ./update-ppa.sh
    ```

3. The script will organize the packages into the appropriate subdirectories, generate the `Packages` and `Packages.gz` files, and create the `Release` file for the PPA.

4. After the update, the new packages will be available in the PPA for installation.

## How to Add PPAs to Your System

To use the packages hosted in these PPAs, follow the steps below to add the appropriate repositories to your system's `/etc/apt/sources.list.d/` directory.

### Instructions

1. Open a terminal and navigate to the `/etc/apt/sources.list.d/` directory.

    ```bash
    cd /etc/apt/sources.list.d/
    ```

2. Create a `.list` file for the PPA you want to use. For example, if you want to add the `var-ti` PPA, create a `var-ti.list` file:

    ```bash
    sudo nano <ppa-name>.list
    ```

3. Add the following line to the file, replacing `<ppa-name>` with the name of the PPA (e.g., `var-ti` or `am62x-var-som`), and `<suite>` with the Debian or Ubuntu release (e.g., `bookworm`, `trixie`, etc.):

    ```bash
    deb [trusted=yes] https://nsdrude-varigit.github.io/var-debpkgs/<ppa-name>/dists/<suite> main
    ```

4. Save the file and exit the text editor.

5. Update your system's package list to include the new repository:

    ```bash
    sudo apt update
    ```

6. You can now install packages from the repository using the `apt` command.

### Example for VAR-SOM-AM62 Bookworm:

Add the `var`, `var-ti`, and `am62x-var-som` PPAs for Bookworm:

```bash
echo "deb [trusted=yes] https://nsdrude-varigit.github.io/var-debpkgs/var bookworm main" | sudo tee /etc/apt/sources.list.d/var.list
echo "deb [trusted=yes] https://nsdrude-varigit.github.io/var-debpkgs/var-ti bookworm main" | sudo tee /etc/apt/sources.list.d/var-ti.list
echo "deb [trusted=yes] https://nsdrude-varigit.github.io/var-debpkgs/am62x-var-som bookworm main" | sudo tee /etc/apt/sources.list.d/am62x-var-som.list
sudo apt update
```