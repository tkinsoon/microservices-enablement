# Lab 0 - Prerequisites

Before you install the Google Cloud SDK and setup the Jumpbox, you need to ensure the following steps are completed:
  - A **Registered GCP account** - If not please register the [Google Cloud Platform(GCP)](https://cloud.google.com/) with your Google Email ID, please share with your instrucutor your Email ID once your GCP account is created.
  - A **New GCP Project** added into your GCP account - You should see a new project assigned to you from the drop-down list at the top left corner of the GCP main console. Please inform your instructor if you do not see any new project from the list.

## Section 1 - Install and Configure Google Cloud SDK

Install the **gcloud CLI** by referring to the [Quickstarts](https://cloud.google.com/sdk/docs/quickstarts) from Google, select the appropriate **Quickstart** section based on the OS running on your workstation.

Once the **gcloud CLI** is installed and configured, verify your **gcloud CLI** is pointing to the correct GCP project by running:

```
gcloud config list
```
Eg.
```
$ sudo gcloud config list
[compute]
region = us-central1
zone = us-central1-a
[core]
account = tuckkin@gmail.com
disable_usage_reporting = True
project = piv-4a2da3f2-a1b1-c7bf0f621b15
```

## Section 2 - Setup your Jumpbox
### 2a. Create the jumpbox
Run the following command to create a new jumpbox VM in your project.

```
gcloud compute instances create "jumpbox" \
  --image-family "ubuntu-1804-lts" \
  --image-project "ubuntu-os-cloud" \
  --boot-disk-size "200" \
  --zone us-central1-a
```

After some time, the **gcloud CLI** will output some information about the jumpbox that it created.

For simplicity, regardless of your geographical region, you will use **us-central1-a**. Read for more information about [Google Cloud regions and zones](https://cloud.google.com/compute/docs/regions-zones/#available).

The jumpbox needs a relatively large hard drive (200GB) so that in later labs you can use the space for storing backup data.

### 2b. SSH into the jumpbox
You must now ssh to this new jumpbox using the zone you selected earlier. Notice you are logging in as the ubuntu user. There's nothing particularly special about this account. The account and its home directory are automatically generated upon first use and will serve as a shared resource for you and your pair.

The command to connect to your jumpbox will look something like the following snippet. Run this in your current terminal session:
```
gcloud compute ssh ubuntu@jumpbox
```
**Note:** You NEED to provide some passphrase for the first time you SSH to your jumpbox.

Once the command completes and **you are now connected to the jumpbox**, continue with the following installation steps.

### 2c. Initialize the jumpbox for GCP
Once you are on the jumpbox, perform the following:
```
gcloud config list
```
**REMEMBER, you are now logged in to the jumpbox as the ubuntu user**. You will observe that your Google Cloud config/context here is different to what we observed from your own local machine. The project for your new VM has been set accurately by means of association. Conversely you are now logged in to Google Cloud using a default service account which does not have the level of privilege we require. For an example of a command that will fail in this context, try running ```gcloud compute instances list```.

To address this we must repeat the steps we used when we logged in to Google Cloud from our local machine:
```
gcloud auth login
```
Follow the on-screen prompts. This time round, as we are connected to a headless Linux instance without a local browser, we will need to copy-paste the URL into a local browser in order to select the account you have registered for use with Google Cloud. Additionally, you will need to copy-paste the verification code back into your jumpbox session to complete the login sequence.

Eg.
```
ubuntu@jumpbox1:~$ gcloud auth login

You are running on a Google Compute Engine virtual machine.
It is recommended that you use service accounts for authentication.

You can run:

  $ gcloud config set account `ACCOUNT`

to switch accounts if necessary.

Your credentials may be visible to others with access to this
virtual machine. Are you sure you want to authenticate with
your personal account?

Do you want to continue (Y/n)?  y

Go to the following link in your browser:

    https://accounts.google.com/o/oauth2/auth?code_challenge=IrrtlTFttK4w7sov9XBtjzHN2xEZvYBdfyR2JeAyTSM&prompt=select_account&code_challenge_method=S256&access_type=offline&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob&response_type=code&client_id=32555940559.apps.googleusercontent.com&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth


Enter verification code: 4/vQHQuZVeguxdIcVTPVBc4gAgVo6NtWShF3fDHUsCCTC5Uqd-OxLocJY

You are now logged in as [tuckkin@gmail.com].
Your current project is [piv-4a2da3f2-a1b1-c7bf0f621b15].  You can change this setting by running:
  $ gcloud config set project PROJECT_ID
```

Re-run the following command to validate the account has been altered:
```
gcloud config list
```
The ```gcloud compute instances list``` command will now produce a list of all the VMs in your project (i.e. just the jumpbox).

### 2d. Create the Environment file
In the ubuntu home directory on your jumpbox, create a hidden file named .env to store variables, the values of which describe your specific environment. Customize the .env file to suit your target environment before continuing.

Take the template .env file below and substitute in the proper values for your GCP project:
```
PIVNET_TOKEN=CHANGE_ME_PIVNET_TOKEN   # see https://network.pivotal.io/users/dashboard/edit-profile
DOMAIN_NAME=pal4pe.com
ENV_NAME=CHANGE_ME_ENV_NAME           # e.g. instructor will assign you one
OPSMAN_PASSWD=CHANGE_ME_OPSMAN_PASSWD # e.g. for simplicity, recycle your PIVNET_TOKEN

PROJECT_ID=$(gcloud config get-value core/project)
OPSMAN_FQDN=pcf.${ENV_NAME}.${DOMAIN_NAME}
```
Retrieve your PivNet token by logging into your PivNet account

Click on your username in the top right
Click Edit Profile
On the line UAA API TOKEN, click on the button titled
Request New Refresh Token
Place the resulting token in the above template as the value for the variable PIVNET_TOKEN
Before continuing, ask your instructor to validate the contents of your .env file.

Now that we have the .env file with our critical variables, we need to ensure that these get set into the shell, both now and every subsequent time the ubuntu user connects to the jumpbox.
```
source ~/.env
echo "source ~/.env" >> ~/.bashrc
```
### 2e. Install tools
Our vanilla jumpbox does not come equipped with the tools we need.

For now, install unzip and Jq. Both can be installed with the ubuntu package manager (apt):
```
sudo apt update
sudo apt install unzip jq
```
Additional commands will be installed in subsequent labs.

### 2f. Download the Lab Files
```
git clone https://github.com/tkinsoon/microservices-enablement.git
cd microservices-enablement
```
