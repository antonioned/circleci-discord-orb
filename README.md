# circleci-discord-orb
A CircleCI orb for sending notifications and messages to Discord. For more
information about how orbs work, see this
[introduction](https://circleci.com/docs/2.0/orb-intro/).

## usage

To use this orb in your `config.yml`, see below for some example usage. In this
example, the `build` job will send a notification only when it fails, while the
`deploy_prod` job will send a notification on success or failure. You can use
the [built-in environment
variables](https://circleci.com/docs/2.0/env-vars/#built-in-environment-variables)
here as well. The last important thing to notice is that you can set the
webhook url as either an environment variable as below, or just as plaintext.

```
version: 2.1
orbs:
  discord: antonioned/discord@0.1.0

jobs:
  build:
    working_directory: ~/repo

    steps:
      - checkout
      - run:
          name: print random text
          command: echo "HELLO"
      - discord/status:
          fail_only: true
          failure_message: "**${CIRCLE_USERNAME}**'s build: **${CIRCLE_JOB}** failed."
          webhook: "${DISCORD_STATUS_WEBHOOK}"

  deploy_prod:
    working_directory: ~/repo
    steps:
      - checkout
      - run:
          name: deploy code
          command: echo "deploying to production"
      - discord/status:
          fail_only: false
          failure_message: "**${CIRCLE_USERNAME}**'s build: **${CIRCLE_JOB}** failed."
          webhook: "${DISCORD_STATUS_WEBHOOK}"
          success_message: "**${CIRCLE_USERNAME}** deployed api to prod."
```



## supported actions

This orb currently supports the `status` and `approval` actions. To see the
configuration details, you can use the `parameters` object under each action to
see what you can pass through.

