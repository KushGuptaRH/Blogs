<p align="center">
  <img src="https://www.redhat.com/cms/managed-files/styles/max_size/s3/RH_BRAND_MCS-illustration.png?itok=ADujQIht" alt="Hybrid Cloud Grapic" width="500" height="500"/>
</p>

# How to Deploy RHEL in the Cloud
### By: Kush Gupta

One of Red Hat’s greatest strengths is enabling the deployment of most products wherever you want. From a bare metal server to virtualization, major public clouds, and the edge, we help you make it happen. I hope to clarify topics around how and why to do this in the cloud and help you make the best decisions when using Red Hat Enterprise Linux (RHEL) outside of your data center.

Red Hat [Cloud Access](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) is a program designed to provide subscription portability for those who want to use their Red Hat subscriptions in the cloud. It is a path towards creating open hybrid cloud infrastructures built on Red Hat technologies. The program is available with most Red Hat subscriptions at no extra cost. It allows you to keep the benefits of Red Hat subscriptions (including our [support](https://www.redhat.com/en/services/support) and discounts) and provides access to value-add capabilities like [gold images](https://www.redhat.com/en/topics/linux/what-is-a-golden-image), and the [Azure Hybrid Benefit for Linux](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/azure-hybrid-benefit-linux). Cloud Access lets you set up account-level registration tooling that auto-connects to the Red Hat [Hybrid Cloud Console](https://cloud.redhat.com/). To unify reporting of your subscriptions across the data center and the clouds, and get predictive analytics, Cloud Access also helps you enable [Subscription Watch](https://access.redhat.com/articles/4897921) and [Insights](https://www.redhat.com/en/technologies/management/insights).

How does it work in terms of product eligibility? If you have an unused, active Red Hat subscription with a cloud-compatible unit of measure (Core, vCPU, RAM, Virtual Guest, etc.), it’s probably eligible. 

Some subscriptions that are not eligible include:
- Virtual Datacenter (VDC) or other unlimited RHEL guest subscriptions that require virt-who
  - **Unless** you are trying to run VDC on VMware Cloud (VMC), Azure VMware Solution (AVS), or Google Compute VMware Engine (GCVE).
  - Virtual Datacenter will apply to these VMware environments like on-premise subscriptions. 
- Red Hat Virtualization products; nested virtualization is not supported
- Subscriptions for Red Hat-hosted offerings

These guidelines are not definitive, and Red Hat product and subscription eligibility change over time as cloud providers and Red Hat introduce new products and capabilities. Refer to the Red Hat product documentation for specific details about the product’s use on a public cloud infrastructure.

The central concept to understanding how [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) (RHEL) specifically operates in the cloud is that RHEL images are available in the cloud as either an On-Demand / Pay as you go (PAYG) model from the cloud console or a bring-your-own-subscription (BYOS) model from Red Hat Cloud Access. When would you do one or the other, and what gets you to cost optimization? It’s going to depend on what you are looking to do with the machines. Let's go over some common scenarios to see which method of procurment helps the most.

#### Scenarios that are best suited for PAYG include the following: 
- You are uncertain about the number of machines, capacity, or utilization needed throughout the year.
- You want the ability to terminate the deployments at any time and stop paying for them.
  - This lets you pay only for what is used.
- You do not need support directly from Red Hat.
- You only need to run these machines in the cloud.
- You do not want any upfront costs or commitment to obtain flexible computing capacity.

#### Scenarios that are best suited for BYOS include the following:
- You have an approximation for how many machines are needed throughout the year.
- You want to pre-pay to obtain Red Hat discounts.
- You want support directly from Red Hat.
- You do **not** want software premiums that increase with your increases to virtual node size (compute and memory).
  - That is the case for PAYG, however a RHEL subscription entitles a customer to [two virtual nodes regardless of virtual sockets](https://www.redhat.com/en/resources/red-hat-enterprise-linux-subscription-guide#section-5:~:text=two%20virtual%20nodes%20regardless%20of%20virtual%20sockets). 
- You want to utilize custom or gold images.

Here is a general comparison of the two models for procurement: 

## On-Demand / Pay As You Go (PAYG)
- When a Red Hat customer uses a product made available by a Certified Cloud and Service Provider (CCSP) in a public image catalog.
  - Post-paid & cannot use your Red Hat discount. You are only charged for what you use.
  - The images are provided by the Cloud Provider, and can be used only within the cloud environment.
- The Red Hat subscription cost is built into the offering through a software premium.
- Customers get support directly from the cloud provider.
- Ideal for taking advantage of the benefits associated with on-demand, such as no upfront cost, and no commitment or planning to obtain flexible computing capacity. 


## Bring Your Own Subscription (BYOS)
- Customers pay Red Hat for product subscriptions & support, and they pay the CCSP for cloud resource consumption.
  - Pre-paid & can use your Red Hat discount.
- Cloud VMs deployed from a [Custom Image](https://www.redhat.com/en/topics/linux/what-is-an-image-builder) or [Gold Image](https://www.redhat.com/en/topics/linux/what-is-a-golden-image).
- It works identically to the [on-premise subscription](https://www.redhat.com/en/resources/red-hat-enterprise-linux-subscription-guide).
- Ideal for obtaining consistency regardless of where your workloads are.

Regardless of which model you use to obtain RHEL in the cloud, you have the option to register to [Red Hat Update Infrastructure](https://access.redhat.com/documentation/en-us/red_hat_update_infrastructure/4/html/configuring_and_managing_red_hat_update_infrastructure/assembly_cmg-about-rhui4_configuring-and-managing-red-hat-update-infrastructure#doc-wrapper) (RHUI) or [Red Hat Subscription Management](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/2022/html/using_red_hat_subscription_management/understanding_rhsm_con) (RHSM) with [Satellite](https://www.redhat.com/en/technologies/management/satellite) & the Red Hat CDN for updates and patches. RHUI enables our Certified Cloud Provider partners to provide a unique & more secure method of allowing their customers to update their RHEL in the cloud without any admin / invasive access to your RHEL environment. RHSM enables users to track their subscription quantity and consumption, Satellite can manage content, patching and other capabilities for your servers. Today, RHUI can provide an identical update strategy in your clouds regardless of your buying model. You still retain the ability to use Satellite and RHSM if that’s your preference. We meet you wherever you want to be & however you want to do it.

What if you want to take advantage of convenient, flexible deployments while keeping Red Hat support and discounts? Red Hat’s 3rd party marketplace offerings on the cloud are our answer.

I’ve listed the step-by-step instructions for enabling Red Hat Cloud Access to gain Bring Your Own Subscription capabilities in AWS & Azure at the end, here are links to the [reference documentation](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/2022/html/red_hat_cloud_access_reference_guide/index) and the [FAQ](https://access.redhat.com/articles/3664231).

---

## AWS
### Manual Method
The process will vary on the applications you chose to set up, I will go over the RHEL Management Bundle for Gold Image access. 
1. Go to sources inside of the Hybrid Cloud Console - https://console.redhat.com/settings/sources
2. Click “Amazon Web Services”
3. Enter a name for the source for your tracking
4. Select “Manual Configuration”
5. Select RHEL Management
6. Create an AWS IAM policy using the contents provided
7. Create a new role, select another AWS account by the ID specified, and attach the permissions policy you created in the last step
8. Copy and paste the role ARN into the Red Hat console
9. Review details and click “Add”
### Account Authorization
Use this method to register your AWS account and enjoy benefits such as instant access to Gold Images.
1. Go to sources inside of the Hybrid Cloud Console - https://console.redhat.com/settings/sources
2. Click “Amazon Web Services”
3. Enter a name for the source for your tracking
4. Click “Account authorization”
5. Enter your account access key ID and secret access key
6. Configure the applications you would like to enable
    - Note: To obtain access to Red Hat gold images and subscription watch data, only the RHEL Management Bundle is needed
7. Review the details and click “Add”
## Azure
### Single Registration
Use this method to register your Azure subscription and enjoy benefits such as instant access to Gold Images.
1. Create a Linux VM running on the Azure subscription you would like to link and ssh into it
    - It should have an external disk attached to it at creation time with at least 8 GB of storage
    - It should be able to connect to the internet
2. By default, you should enter the user's home directory
3. Get or update pip
    - `curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py | python3 get-pip.py`
4. Install the package to create virtual environments
    - `pip3 install virtualenv`
5. Create a virtual environment called ansible
    - `virtualenv -p $(which python3) ansible`
6. Enter the virtual environment
    - `source ansible/bin/activate`
7. Install ansible and requests-oauthlib in the virtual environment
    - `pip3 install ansible requests-oauthlib`
8. Install the insights subscription collection
    - `ansible-galaxy collection install redhatinsights.subscriptions`
9. Create a text file called `inventory.ini` with the following content and insert the user you are using in the Linux system 
```
localhost 

[all:vars]
ansible_python_interpreter=/home/<USERNAME>/ansible/bin/python3
ansible_connection=local
```
10. Obtain a Red Hat offline token here to enter at the end of the next step: https://access.redhat.com/management/api
11. Run the ansible playbook to send the necessary Azure Instance Metadata to Red Hat 
    - `ansible-playbook -i inventory.ini -b ~/.ansible/collections/ansible_collections/redhatinsights/subscriptions/playbooks/verify_account.yml -e rh_api_refresh_token=<OFFLINE_TOKEN>`
12. If everything runs successfully, you can delete the VM. It is not needed for the source anymore. The connection can be managed at https://access.redhat.com/management/cloud under the Microsoft Azure Subscriptions section.

### Manual Entry
Completing this method enables Cloud Access and Golden Images for an Azure subscription.
  - **Note:** This method for accessing gold images is deprecated, and functionality will be removed once its replacement is finished over at the [Hybrid Cloud Console](https://console.redhat.com/).
1. Go to https://access.redhat.com/management/cloud in the Red Hat account you would like to connect
2. Click the blue button that says “Enable a new provider”
3. Select “Microsoft Azure” as the certified cloud provider
4. Click “Add subscriptions manually”
5. Enter a Microsoft Azure Subscription ID, a nickname for it to keep track of it on the Red Hat Customer Portal, and the number of entitlements you want to enable for that subscription
6. Click the blue “Enable” button
    - You may see a green banner that says “Successfully added Microsoft Azure as an enabled Cloud Access provider and activated Red Hat Gold Images.”
    - To enable Gold Image Access quicker, go to the [cloud access tab](https://access.redhat.com/management/cloud), under “Microsoft Azure” click “Microsoft Azure Subscriptions”, right click on the vertical ellipsis ⋮ and click “Activate Red Hat Gold Images”
    - This will say “Red Hat Gold Image activation successful. Your images will be available in your Microsoft Azure console within 3 hours.” However, it can be quicker depending on the number of products enabled.
