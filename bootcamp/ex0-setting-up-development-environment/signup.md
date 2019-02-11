# Sign Up

## Register IBM Cloud Account

1. Open a browser window
2. Navigate to [https://cloud.ibm.com/registration](https://cloud.ibm.com/registration)

   ![Registration page](../../.gitbook/assets/registration.png)

3. Fill in registration form and follow link in the validation email when it arrives.

   ![Registration page](../../.gitbook/assets/email.png)

4. [Login into IBM Cloud](https://console.bluemix.net/login) using the account credentials you have registered.

## Check Default Region

🚨🚨🚨 **PLEASE READ THIS SECTION.** _We know it looks boring but trust us! People often skim this part and then complain they can't login into the CLI. These instructions will save you all that inevitable confusion..._ 🚨🚨🚨

New IBM Cloud accounts default to a [new "lite" account version](https://www.ibm.com/cloud/pricing).

_This account provides free access to a subset of IBM Cloud resources, including IBM Cloud Functions. Lite accounts do not need a credit-card to sign up or expire after a set time period, i.e. 30 days._

Developers using "_Lite accounts_" are restricted to development within a single region. Accounts are automatically assigned to either `eu-gb` or `us-south` regions depending on user profile location.

**When setting up the IBM Cloud CLI, choose the API endpoint for the default account region.**

Follow these instructions to check which default region your lite account has been assigned.

1. Open the [IBM Cloud homepage](https://console.bluemix.net/).
2. Click the _"Manage"_ menu from the page header.
3. Click the _"_[_Account &gt; Cloud Foundry Orgs_](https://console.bluemix.net/account/organizations)_"_ option from the drop-down menu.
4. From the [Cloud Foundry Organisations](https://console.bluemix.net/account/organizations) page, click the organisation name listed in the table.
5. Check the "_Region_" value listed in the organisation details table.

![Registration page](../../.gitbook/assets/default_region.png)

🎉🎉🎉 **Congratulations, you've successfully registered an IBM Cloud account** 🎉🎉🎉

