# azure-ansible

This place is to add Notes how to modules works with AZUR cloud 
Reference Guild 
https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html

Requirements
Using the Azure Resource Manager modules requires having specific Azure SDK modules installed on the host running Ansible.

$ pip install 'ansible[azure]'
If you are running Ansible from source, you can install the dependencies from the root directory of the Ansible repo.

$ pip install .[azure]

You can also directly run Ansible in Azure Cloud Shell, where Ansible is pre-installed.

Authenticating with Azure
Using the Azure Resource Manager modules requires authenticating with the Azure API. You can choose from two authentication strategies:

Active Directory Username/Password

Service Principal Credentials

Follow the directions for the strategy you wish to use, then proceed to Providing Credentials to Azure Modules for instructions on how to actually use the modules and authenticate with the Azure API.

Using Service Principal
There is now a detailed official tutorial describing how to create a service principal.

After stepping through the tutorial you will have:

Your Client ID, which is found in the “client id” box in the “Configure” page of your application in the Azure portal

Your Secret key, generated when you created the application. You cannot show the key after creation. If you lost the key, you must create a new one in the “Configure” page of your application.

And finally, a tenant ID. It’s a UUID (for example, ABCDEFGH-1234-ABCD-1234-ABCDEFGHIJKL) pointing to the AD containing your application. You will find it in the URL from within the Azure portal, or in the “view endpoints” of any given URL.

Using Active Directory Username/Password
To create an Active Directory username/password:

Connect to the Azure Classic Portal with your admin account

Create a user in your default AAD. You must NOT activate Multi-Factor Authentication

Go to Settings - Administrators

Click on Add and enter the email of the new user.

Check the checkbox of the subscription you want to test with this user.

Login to Azure Portal with this new user to change the temporary password to a new one. You will not be able to use the temporary password for OAuth login.

Providing Credentials to Azure Modules
The modules offer several ways to provide your credentials. For a CI/CD tool such as Ansible AWX or Jenkins, you will most likely want to use environment variables. For local development you may wish to store your credentials in a file within your home directory. And of course, you can always pass credentials as parameters to a task within a playbook. The order of precedence is parameters, then environment variables, and finally a file found in your home directory.

Using Environment Variables
To pass service principal credentials via the environment, define the following variables:

AZURE_CLIENT_ID

AZURE_SECRET

AZURE_SUBSCRIPTION_ID

AZURE_TENANT

To pass Active Directory username/password via the environment, define the following variables:

AZURE_AD_USER

AZURE_PASSWORD

AZURE_SUBSCRIPTION_ID

To pass Active Directory username/password in ADFS via the environment, define the following variables:

AZURE_AD_USER

AZURE_PASSWORD

AZURE_CLIENT_ID

AZURE_TENANT

AZURE_ADFS_AUTHORITY_URL

“AZURE_ADFS_AUTHORITY_URL” is optional. It’s necessary only when you have own ADFS authority like https://yourdomain.com/adfs.

Storing in a File
When working in a development environment, it may be desirable to store credentials in a file. The modules will look for credentials in $HOME/.azure/credentials. This file is an ini style file. It will look as follows:

[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
secret=xxxxxxxxxxxxxxxxx
tenant=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
