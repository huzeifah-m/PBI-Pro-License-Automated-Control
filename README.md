# PBI Pro License Automated Control
### Introduction
When a user is assigned an Office 365 E5 subscription, a Power BI pro licence (being part of the E5 package) is automatically assigned aswell. This may become an issue for organisations with many E5 licences distributed across their users, as a PBI Pro licence allows for a free-for-all in terms of content creation (unless otherwise restricted). 
 
In order to better control the initial assignment of a PBI Pro license and improve PBI governance, users can be added to security groups with the pro license enabled/disabled. Any user added to the group will inherit the license assignment of the security group. For example, if the security group has PBI Pro disabled, any user added to this group will inherit this restriction and will also have PBI Pro disabled. See the GIF below which illustrates this:       
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Demo%20user%20in%20no%20pro%20group.gif)
 
 To better govern PBI Pro Licence assignment, consider the following method:
 1. Create two security groups, one with PBI Pro enabled and the other with PBI Pro disabled.
 2. Users by default are added to the group with PBI pro disabled.
 3. Should a user need a Pro licence, they submit a request.
 4. If the request is approved, they are transferred to the security group with the Pro Licence enabled.

We can take advantage of Power Automate to automate the entire process from when the user submits a request.

### Pre-requisites
•	**Power Automate licence**

•	**Admin of an AAD**

•	**E5 licences**

•	**MSForm for user to fill in**, which is set so only members of your organisation can respond. Here organisations can customise the form to meet their needs such as including terms of use, rules to follow etc..:

<img width="1189" alt="MicrosoftTeams-image" src="https://user-images.githubusercontent.com/99490720/220603003-6d22c454-9882-4a0e-be9d-95c6ed889233.png">

• **Create a Security group with E5 assigned (and PBI Pro enabled):**

From the Azure Active Directory page within the Azure portal, create a new security group, add a suitable name, description. Then assign E5 licences to the security group:
![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/creating%20SG.gif) 


![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/assign%20licence%20to%20SG.gif)


### Building Flow Architecture
 •	Navigate to [make.powerautomate.com]() and sign in.
 
 • Select the 'Create' tab and create a new flow by selecting 'Automated cloud flow'. Then select the MSForms trigger 'When a new response is submitted'.
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Create%20new%20flow.gif)
 
 •	In the form id drop down, select the form created/named by the user.
 
 •	Choose new step and search, then select get response details from the drop down menu.
 
 •	Select the specified form Id and the response Id from the dynamic pop up box in the box for response Id.
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Get%20response%20ID.gif)
 
 •	Add a new step called 'Get User Profile,' and in the user fill in a dynamic response for the responder's email.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Get%20user%20profile.gif)
 
 •	Next, choose a new Post adaptive card step and wait for a response.
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Adaptive%20Card.gif)
 
 •	In response to the approval, we then use a condition. The condition is created by using the adaptive card's approval data and the expression is: body('Post_Adaptive_Card_and_wait_for_a_response')?['submitActionId']
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Condition%201.gif)
 
 •	When a request is denied, the user is notified via email. The flow resumes once the request is approved. To include the approver's message in the email, use an expression that contains the clarification field from the adaptive card. body('Post_Adaptive_Card_and_wait_for_a_response')?['data']?['clarification']
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Rejected%20Email.gif)
 
 •	When the request is approved the user is crosschecked if the user is already a member of the license group. The group ID can be found in the properties page of the license group in Azure AD.
 
 •	Add condition to Check the ID to see if the user is already a member. Use the "does not contain" option in this case.
 
 •	Check the ID to see if the user belongs to the group.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Check%20group%20membership%20and%20condition%202.gif)
 
 •	Add the user to the group if they are not already a member by providing their user ID and the same group ID.
 
 •	Using some form variables, send the user a confirmation email confirming their application has been approved and a licence has been issued.
 
 •	If the user is already a member of the group, a notification email is sent to the user using variables from the form.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Images/Add%20to%20group%20.gif)
