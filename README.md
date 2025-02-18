## Deploy to Orbiter GitHub Action

![cover](https://orbiter.host/og.png)

Add automatic deployments to your Orbiter site using the Orbiter Github Action.

This action currently supports Javascript/Typescript flows but more will be added in the future.

## Setup

If you haven't already, [create a free account](https://app.orbiter.host) and create your first site. Then you will need the following information:

- Project Name: This would be the name you chose when you made the site. For example, the site `mysite.orbiter.website` the name would be `mysite`
- Build Directory: The name your build directory, e.g. `dist`, `out`, `public`, etc
- Orbiter API Key: This can be created on the [API Keys Page](https://app.orbiter.host/api-keys) of the Orbiter App
- Node Version (Optional): Define the version of Node you want to use, will default to v20
- Build Command (Optional): Define the command used to build, default is `npm run build`

Once you have all of this info prepared make a new directory called `.github` in the root of your folder, then add a directory called `worflows` to it. Finally make a new file called `deploy.yaml`.

Alternatively run this in terminal:

```bash
mkdir -p .github/workflows && touch .github/workflows/deploy.yaml
```

Paste in the following code into the `deploy.yaml` file with your config listed earlier

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Deploy to Orbiter
        uses: orbiterhost/orbiter-github-actions@v0.1.4 # Update with latest version
        with:
          project-name: "mysite" # Name of your project
          build-dir: "./dist" # Name of the build output directory
          api-key: ${{ secrets.ORBITER_API_KEY }} # Will use repository secret
          # Optional inputs with their defaults
          node-version: "20.x" # Optional, defaults to '20.x'
          build-command: "npm run build" # Optional, defaults to 'npm run build'
```

Lastly you will need to add your Orbiter API Key as a Repository Secret. Navigate GitHub project Settings > Secrets and Variables > Actions. Click `New repository secret`, then use `ORBITER_API_KEY` as the name, and then paste the secret into the box below.

Then you're all set! On your next deployment you can check the `Actions` tab of the GitHub project to make sure it deployed successfully.
