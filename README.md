# PBI Pro License Automated Control
 When a user is assigned an Office 365 E5 subscription, a Power BI pro licence (being part of the E5 package) is automatically assigned aswell. This may become an issue for organisations with many E5 licences distributed across their users, as a PBI Pro licence allows for a free-for-all in terms of content creation (unless otherwise restricted). In order to better control the initial assignment of a PBI Pro license, users can be added to security groups with the pro license enabled/disabled. Any user added to the group will inherit the license assignment of the security group. For example, if the security group has PBI Pro disabled, any user added to this group will inherit this restriction and will also have PBI Pro disabled.       
 
 A method to automate assigning PBI pro licences to a security group upon requesting access via a form.

 •	Build a basic form that captures all information to be entered and requested, customise it to meet needs, and set it up so that only members of your organisation can view it
Commit Test


### Building Flow Architecture
 •	Navigate to make.powerautomate.com and sign in
 
 • Select the triggered by a designated event tab under the start from scratch section.
 
 •	In the pop-up window, type and click When a new response is submitted and the option to create is selected
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Create%20new%20flow.gif)
 
 •	In the form id drop down, select the form created/named by the user.
 
 •	Choose new step and search, then select get response details from the drop down menu.
 
 •	Select the specified form Id and the response Id from the dynamic pop up box in the box for response Id.
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Get%20response%20ID.gif)
 
 •	Add a new step called 'Get User Profile,' and in the user fill in a dynamic response for the responder's email.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Get%20user%20profile.gif)
 
 •	Next, choose a new Post adaptive card step and wait for a response.
 
  ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Adaptive%20Card.gif)
 
 •	In response to the approval, we then use a condition. The condition is created by using the adaptive card's approval data and the expression is: body('Post_Adaptive_Card_and_wait_for_a_response')?['submitActionId']
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Condition%201.gif)
 
 •	When a request is denied, the user is notified via email. The flow resumes once the request is approved. To include the approver's message in the email, use an expression that contains the clarification field from the adaptive card. body('Post_Adaptive_Card_and_wait_for_a_response')?['data']?['clarification']
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Rejected%20Email.gif)
 
 •	When the request is approved the user is crosschecked if the user is already a member of the license group. The group ID can be found in the properties page of the license group in Azure AD.
 
 •	Add condition to Check the ID to see if the user is already a member. Use the "does not contain" option in this case.
 
 •	Check the ID to see if the user belongs to the group.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Check%20group%20membership%20and%20condition%202.gif)
 
 •	Add the user to the group if they are not already a member by providing their user ID and the same group ID.
 
 •	Using some form variables, send the user a confirmation email confirming their application has been approved and a licence has been issued.
 
 •	If the user is already a member of the group, a notification email is sent to the user using variables from the form.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Add%20to%20group%20.gif)
