# MacOS MirrorBot Template

This is my template for running various aria mirrorbots with one common workflow by doing minimal modifications.

## Supported Aria MirrorBots

You can use any of these scripts

- https://github.com/lzzy12/python-aria-mirror-bot
- https://github.com/magneto261290/magneto-python-aria
- https://github.com/SVR666/LoaderX-Bot
- https://github.com/iamLiquidX/MirrorX
- https://github.com/MaxxRider/Maxx-Mirror-Pro

> Let me know if there's any more like these modified forks.

## How To Deploy Them

To deploy these scripts, you first need to setup the configs, credentials and token.
All these requirements are explained in each of the script's repository.

Here's what I'd do to use their repository unmodified.

1. I cloned those repos and got my own `config.env`, `credentials.json` and `token.pickle`.
2. Then I uploaded those files in my GitHub Gist with restricted visibility.
As `token.pickle` is a binary file, I base64-encoded it before uploading it named as `token.pickle.b64`.
3. I didn't use service accounts, so these three things are all I needed.

**If you want to use your custom-designed scripts, you can just remove all mentions of the above three files and ignore step 4 from below.**

After that, I had to add some info for the [workflow file](.github/workflows/main.yml) in Repository (Or Organizaion) Secrets. 

1. Add your own GitHub Credentials as `GitHubMail` and `GitHubName`.
2. Add your GitHub Personal Access Token as `GH_TOKEN` with most of the admin-read-write permissions (but not with delete permission).
3. Add the mirrorbot repo url as `BotRepoSlug`.
4. Add Gist URL for `config.env`, `credentials.json` and `token.pickle.b64` (base64 data of token) as `ConfigSlug`, `CredsSlug` and `PickleSlug`.
5. Add your DockerHub Credentials as `DOCKER_USERNAME` and `DOCKERHUB_TOKEN` (API Token, or specify `DOCKER_PASSWORD` as password).
6. Optionally, give a name for your bot's Docker Image Tag as `BotName` if you want to upload it to your personal DockerHub Registry.

### Some Information

1. The MirrorBot script directory will be `/home/runner/botSource`
2. The workflow will be triggered only on workflow_dispatch.
3. This workflow will run in macos-10.15 runner hosted in GitHub's own VM, and it is a shared VM platform.
So, processes sometimes may freeze as it shares only 3 CPU Cores which is pretty bad for multi-tasking.
That's why I suggest you keep MAX_CONCURRENT_DOWNLOADS down to 2 or 3 in `aria.sh` inside scripts.
3. The workflow will cleanup the workspace which takes 42 to 56 minutes, according to server pressure.
After that, when the bot starts, it will run for exactly 300 minutes.
To save this huge downtime, a second job is used which will automatically trigger another dispatch after 300 minutes from starting point.
4. So you will get an infinite loop of run with maximum 5-10 minutes of bot downtime.

## Disclaimer

This repository is for personal use only.
Do not abuse GitHub Actions by running multiple instances of these bots in a crowded group which exhausts GitHub's Machine Resources.
If you want GitHub to support us by giving out fairly amount of resources to test out our applications, don't get into their radar, keep low.
