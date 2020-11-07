# Debugging Node.js applications running in a Docker Container using Visual Studio Code
**Author:** [Alan Mills]
**Date:** [17 June 2017 12:41]
**Tags:** [Visual Studio Code], [Docker], [Node.js]
**Status**: Draft

## Prerequsites
1. Visual Studio Code installed `brew cask install visual-studio-code`
2. Install Visual Studio Code extensions (<kbd>⌘</kbd></kbd>+<kbd>⇧</kbd>+<kbd>X</kbd>)
    1.  **npm by Florian Knopp** `code --install fknop.vscode-npm`
    2. **Docker by Microsoft** `code --install PeterJausovec.vscode-docker`

## Setup new Docker Node.js prroject for debugging in Visual Studio Code
1. Create a new directory to work in `mkdir visual-studio-code-docker-node`
2. Open Visual Studio Code in the new directory `code visual-studio-code-docker-node/`
3. Open Command Pallet (<kbd>⌘</kbd></kbd>+<kbd>⇧</kbd>+<kbd>P</kbd>) npm: initialize npm package
4. Open Command Pallet (<kbd>⌘</kbd></kbd>+<kbd>⇧</kbd>+<kbd>P</kbd>) npm: install and save dev. dependency, **mocha**
5. Open Command Pallet (<kbd>⌘</kbd></kbd>+<kbd>⇧</kbd>+<kbd>P</kbd>) Docker: Add docker files to workspace
5. Create a new debug configuration (<kbd>⌘</kbd></kbd>+<kbd>⇧</kbd>+<kbd>D</kbd>)
    1. From drop down **No Configurations**, select **Add Configuration**
    2. Select **Docker Attach**, then **Node.js: Mocha Tests**

## Setup a Mocha test
1. Add a folder for tests `test`
