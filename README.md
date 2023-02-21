# PBI Pro License Automated Control
 A method to automate assigning PBI pro licences to a security group upon requesting access via a form.

 •	Build a basic form that captures all information to be entered and requested, customise it to meet needs, and set it up so that only members of your organisation can view it
Commit Test


### Building Flow Architecture
 •	Navigate to make.powerautomate.com and sign in
 
 • Select the triggered by a designated event tab under the start from scratch section.
 
 ![](https://github.com/huzeifah-m/PBI-Pro-License-Automated-Control/blob/main/Resources/Navigating%20to%20website.gif)
 
 •	In the pop-up window, type and click When a new response is submitted and the option to create is selected
 
 •	In the form id drop down, select the form created/named by the user.
 
 •	Choose new step and search, then select get response details from the drop down menu.
 
 •	Select the specified form Id and the response Id from the dynamic pop up box in the box for response Id.
 
 •	Add a new step called 'Get User Profile,' and in the user fill in a dynamic response for the responder's email.
 
 •	Next, choose a new Post adaptive card step and wait for a response.
 
 •	In response to the approval, we then use a condition. The condition is created by using the adaptive card's approval data and the expression is: body('Post_Adaptive_Card_and_wait_for_a_response')?['data']?['clarification']
 
 •	When a request is denied, the user is notified via email. The flow resumes once the request is approved. To include the approver's message in the email, use an expression that contains the clarification field from the adaptive card. body('Post_Adaptive_Card_and_wait_for_a_response')?['data']?['clarification']
 
 •	When the request is approved the user is crosschecked if the user is already a member of the license group. The group ID can be found in the properties page of the license group in Azure AD.
 
 •	Add condition to Check the ID to see if the user is a member. Use the "does not contain" option in this case.
 
 •	Check the ID to see if the user belongs to the group.
 
 •	Add the user to the group if they are not already a member by providing their user ID and the same group ID.
 
 •	Using some form variables, send the user a confirmation email confirming their application has been approved and a licence has been issued.
 
 •	If the user is already a member of the group, a notification email is sent to the user using variables from the form.
