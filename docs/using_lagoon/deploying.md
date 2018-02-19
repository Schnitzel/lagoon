# Deploying Lagoon Projects

## `.lagoon.yml`
The other files in our project (`docker-compose.yml`, `Dockerfile`s, and any included configuration) handle all of the pre-deploy setup and configuration, the `.lagoon.yml` file takes care of things after the code has been deployed. Using this file we can set up custom routes, enable SSL, run tools like `drush`, and more.

## Repository Configuration
To allow Lagoon to deploy your project there are a few things which need configuring on the repository itself. Namely, an SSH key which Lagoon will use to authenticate, and a webhook which will send event notifications.
### Basics
### SSH Deploy Key
When a project is added to Lagoon a private key is added. Lagoon will use this key to authenticate to your git provider. Ask your Lagoon administrator for the public half of this key, and add it to a user which has at minimum READ access to your repository. Because of how Lagoon works, publicly available repos will also need this in place.
### Webhook
From your Lagoon administrator, you will also need the route to the webhook-handler. You will add this to your repository as an outgoing webhook, and choose which events to send to Lagoon. Typically you will send all Push and Pull Request \ Merge Request events. In Lagoon it is possible to add a regex to determine which events actually result in a deploy.

### Common Repository Setup
Here are instructions for setting up Lagoon on common git hosting solutions.
#### Github
  1. Proceed to Settings -> Webhooks -> `Add webhook` in your repository
  2. The `Payload URL` is the route to the webhook-handler of your Lagoon instance
  3. Set `Content type` to `application\json`
  ![Github Webhook Configuration 1](/images/gh_webhook_1.png)
  4. Choose which events will trigger your webhook. It is our suggestion that you send `push` and `Pull request` events, and then filter further in the Lagoon configuration of your project.
  ![Github Webhook Configuration 2](/images/gh_webhook_2.png)
  5. `Add webhook`
#### GitLab
  1. Navigate to Settings -> Integrations for your repository
  2. The `URL` is the route to the webhook-handler of your Lagoon instance
  3. Set the `Trigger` for which events will send a notification to Lagoon. It is our suggestion that you send `Push events` and `Merge request events` events, and then filter further in the Lagoon configuration of your project.
  ![Gitlab Webhook Setup](/images/gl_webhook_1.png)
  4. `Add webhook`
#### BitBucket
  1. Navigate to Settings -> Webhooks -> Add new webhook in your repository
  2. `Title` is for your reference, `URL`  is the route to the webhook-handler of your Lagoon instance
  3. `Choose from a full list of triggers` and select
    * Repository
      * push
    * Pull Request
      * Created
      * Updated
      * Approved
      * Approval Removed
      * Merged
      * Declined
  ![Bitbucket Webhook Setup](/images/bb_webhook_1.png)
  4. `Save`