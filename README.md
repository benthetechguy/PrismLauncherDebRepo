# PrismLauncher Debian Repository
## How to use
Run the following commands as root:
```
wget https://Prism-Launcher-for-Debian.github.io/repo/prismlauncher.gpg -O /usr/share/keyrings/prismlauncher-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/prismlauncher-archive-keyring.gpg] https://Prism-Launcher-for-Debian.github.io/repo `. /etc/os-release; echo $VERSION_CODENAME` main" > /etc/apt/sources.list.d/prismlauncher.list
apt update && apt install prismlauncher
```
## How to set up your own repo
1. Create a new PGP key for the repository
2. Clone this branch of this GitHub repo
3. Make a new, empty branch `repo`
4. Create a file `conf/distributions` with blocks like this in it:
```
Codename: trixie
Suite: stable
Architectures: i386 amd64 armhf arm64
Components: main
SignWith: AE237E5508F8CBD562E49CA6D6857C60C62A5951
Origin: PrismLauncher
Label: PrismLauncher
Description: Prism Launcher repository for Debian and Ubuntu
```
Make blocks for every release of Debian or Ubuntu you'd like. Remove the `Suite` variable for Ubuntu releases.
Don't forget to change the `SignWith` variable to the fingerprint of your generated PGP key.
The current `repo` branch of this repository contains an example file with all current Debian and Ubuntu releases.

5. Install `reprepro` on your system
6. Run `reprepro createsymlinks` then `reprepro export` to set up the repository.
7. Export the public key of your generated PGP key to some file in the root folder, called for example `prismlauncher.gpg`.
8. Push your two branches to a new GitHub repo.
9. Go to Settings > Secrets and Variables > Actions then create a new repository secret called `GPG_PRIVATE_KEY` with the ASCII armored private key of your generated PGP key.
10. Enable GitHub Pages to serve the `repo` branch.
11. Go to the Actions page and manually run the workflow every time you want to build new packages for all the releases specified in your `conf/distributions` file; it will automatically upload these packages to the repository.

When new Debian/Ubuntu releases come out, make sure to run step 6 again after editing `conf/distributions`. Also don't forget to edit `.github/workflows/deploy.yml` to build the new releases.
