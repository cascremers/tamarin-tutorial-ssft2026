# Tamarin Prover Tutorial for the 2026 Fifteenth Summer School on Formal Techniques at SRI


**Welcome to the Tamarin prover\!**

This is the welcome message you will see when you start the Tamarin prover tool. In the following, we describe how you can set it up on your system.

We designed some exercises with increasing difficulty for you. These exercises aim to get you started in modeling cryptographic protocols in the symbolic model. Specifically, you will learn the language of the Tamarin prover.

For further reference, the Tamarin website ([https://tamarin-prover.com/index.html](https://tamarin-prover.com/index.html)) has a lot of documentation and related software and plugins. The Tamarin book ([https://tamarin-prover.com/book/downloads/Tamarin%20book-Draft%20v0.9.5.pdf](https://tamarin-prover.com/book/downloads/Tamarin%20book-Draft%20v0.9.5.pdf)) provides the most extensive explanations. The Tamarin website also contains PDF and HTML versions of the Tamarin manual, both for stable 1.12 and develop versions.

## Installing Tamarin

We recommend using at least Tamarin version 1.12.0 for the exercises. 

### Local Installation

If you would like to install Tamarin locally, we recommend following the instructions [here](https://tamarin-prover.com/install.html). Tamarin runs on Linux, macOS and the WSL2 (Windows Subsystem for Linux). We suggest using package manager distributions, for example, using homebrew or pacman, as building Tamarin from source takes some time. 

### Installing Tamarin using Docker

We provide Tamarin (version 1.13.0 of May 2026\) in a Docker image for you at [docker hub](https://hub.docker.com/r/protocolresearch/tamarin). The container runs both on the linux/amd64 and linux/arm64 platforms. We tested it on Intel, AMD, and Apple CPUs.

#### Docker

Make sure that you have installed Docker on your system. To verify this, you could type `docker run hello-world` in your terminal/console to see if the command is recognized. If not, please install Docker, for example, using [docker desktop](https://docs.docker.com/desktop/) or the [docker engine](https://docs.docker.com/engine/) itself. You could achieve that using homebrew:

`brew install --cask docker`

If you just installed the docker engine: check that your user account is part of the docker group `groups $USER`. If not, add your current user to the docker group. For example, on Linux this can be done through `sudo usermod -aG docker $USER`. To apply the new group membership, log out, log back in, and confirm the change took effect: `id -nG`. Optionally, you can apply group changes directly in (only) your current terminal without logging out: `newgrp docker`.

#### Tamarin

Now, pull the docker image by executing the following command in your terminal. This requires that docker is up and running. You achieve this, for example, by opening docker desktop.

`docker pull docker.io/protocolresearch/tamarin:latest`

To test Tamarin, execute the following:

`docker run -p 3001:3001 --mount type=bind,src=.,dst=/home/tamarin protocolresearch/tamarin:latest`

**Note:** You might need to prefix both commands with `sudo` on certain systems/configurations.

The entrypoint script of this docker image directly starts Tamarin's interactive prover on the container's host port 3001\. Using the command above, we mount this port on your localhost port 3001\. Hence, you can now open Tamarin's interactive prover with your browser at [http://localhost:3001](http://localhost:3001).

If you are still attached to a running container, you can press Ctrl \+ C to stop the container. You can view all running, but unattached, containers using `docker ps` and stop a certain container using `docker stop xyz`, where xyz is the name of the container, given in the rightmost column of the `ps` output.

## Get Started

### Download Exercises

You can find the set of exercises that we will use in this [GitHub repository](hhttps://github.com/cascremers/tamarin-tutorial-ssft2026). Clone it into your working directory:

 `git clone https://github.com/cascremers/tamarin-tutorial-ssft2026.git`

We start with the files in the folder `sessionA`. Open the Markdown file `sessionA/Exercise1.md` and read the exercise description. Once you feel ready, add the missing rules to the skeleton file `sessionA/skeletons/Protocol1.spthy` using your favorite text editor. There are syntax highlighting plugins for [VS Code](https://marketplace.visualstudio.com/items?itemName=tamarin-prover.tamarin-prover), VIM or Emacs described [here](https://tamarin-prover.com/editors.html) that you could use.

#### Practical Exercises

##### \[Session A: Basic\] 

Start with…

- `sessionA/Exercise1.md`  
- `sessionA/Exercise2.md`  
- `sessionA/Exercise3.md`

##### \[Session B: Advanced\]

Easy? Then feel free to continue with ...

- `sessionB/Exercise1.md`  
- `sessionB/Exercise2.md`  
- `sessionB/Exercise3.md`

You find skeletons to work with in the respective subdirectories `skeletons`. Sample solutions that you can use as hints and to check your solution are provided in the subdirectories `solutions`.

### Running Tamarin on your Protocol Models

Now, to start Tamarin, we mount our working directory to the container and forward the port on which Tamarin's interactive prover is launched to our localhost address. Make sure that your working directory (check using `pwd`) is the folder that contains the `.spthy` files you would like to load. Execute the following command (possibly with `sudo` privileges):

`docker run -p 3001:3001 --mount type=bind,src=.,dst=/home/tamarin protocolresearch/tamarin:latest`

Select the protocol model you were working on and familiarize yourself with the tool. You can start to prove a lemma by selecting the “sorry” under the formula.

If you miss the model you are currently working on in the list, there might have been a parsing error. Please check the terminal output or try to load the theory file using the upload button in the web interface. If there is no entry `[Theory xyz] Theory loaded` in the output, please double-check that your model files are present in the working directory (e.g., `ls`) as we have only mounted this directory to the docker container. Tamarin does not look into subdirectories. Also, the file extension of your model must be `.spthy`.

## Troubleshooting

**I cannot get the model running. What can I do?**

For sanity, please check if you can open one of the solution files successfully. If yes, the chance that there is a syntax error in your model file, is high. Does every rule have the following structure?

`rule rulename:`  
`[ ]  --[ ]->  [ ]`

Compare your model file with the solution, are there syntactic differences? If you use the VS Code plugin, you might also get some helpful error messages. Otherwise, the terminal outputs helpful error messages.

**Something is not working, but I do not get error messages on the terminal while running the docker container.**

There is a little inconvenience in the sense that docker omits parts of the outputs during execution sometimes. If you stop the container using Ctrl \+ C, the missing output typically becomes visible. Use the displayed information to fix your models and start the container again afterwards.

**I get an error message “port is already allocated” when running the docker image. What should I do?**

You can either decide to close the other application using port 3001 (likely a second instance of the docker image) or to use a different port. For the latter, exchange the first port number of the `-p 3001:3001` parameter to another value, e.g., `-p 3005:3001`. If your port is blocked by another container, consider stopping it as described above.

**I get well-formedness errors. What should I do?**

Tamarin provides some details on the error. Are there some differences in fact usage among different rules? Do some variable types differ? Occur variables in the action facts or conclusions that are not part of the premises? Check for such problems and fix your model accordingly.  
**I get partial deconstructions. What should I do?**

Tamarin applies backward searching to identify the sources of certain terms. For some rules, these rules do not restrict these terms sufficiently for Tamarin to find all of them within the time bounds. Try to change your model accordingly or try to pass `--auto-sources` as a command line parameter when using Tamarin locally.   


## Further Material

* The folders `sessionX` contain exercises for the respective sessions. (Note that the first session will consist in a lecture and demo). 
* This [cheatsheet](https://github.com/felixlinker/tamarin-workshop/blob/main/cheatsheet.md) from another Tamarin workshop (by Felix Linker) is a good summary of Tamarin's syntax.
* Cas Cremers created a helpful illustration (`dependencygraph.pdf`) of Tamarin's GUI output.

## Summer School

More information at the [school website](https://ssft-sri.github.io/).