Product: Integration tests for WSO2 ESB Formstack connector

Pre-requisites:

 - Maven 3.x
 - Java 1.6 or above
 - The org.wso2.esb.integration.integration-base project is required. The test suite has been configured to download this project automatically. If the automatic download fails, download the following project and compile it using the mvn clean install command to update your local repository:
   https://github.com/wso2/esb-connectors/tree/master/integration-base

Tested Platform: 

 - Microsoft WINDOWS V-7
 - UBUNTU 14.04
 - WSO2 ESB 4.9.0-BETA

Steps to follow in setting integration test.

 1. Download WSO2 ESB 4.9.0-BETA from official website.

 2. Deploy relevant patches, if applicable and the ESB should be configured as below.

 3. Follow the below mentioned steps for adding valid certificate to access formstack API over https.

    i) 	 Extract the certificate from browser(Mozilla Firefox) by navigating to https://www.formstack.com
    ii)  Go to new ESB 4.9.0 folder and place the downloaded certificate in both "<ESB_HOME>/repository/resources/security/" and "{FORMSTACK_CONNECTOR_HOME}/formstack-connector/formstack-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/keystores/products/" folders.
    iii) Navigate to "<ESB_HOME>/repository/resources/security/" using command prompt and execute keytool -importcert -file CERT_FILE_NAME -keystore client-truststore.jks -alias "CERT_NAME" in command line to import peoplehr certificate in to keystore.
         Give "wso2carbon" as password. Press "Y" to complete certificate import process.

         NOTE : CERT_FILE_NAME is the file name which was extracted from formstack. (e.g. *.formstack.com)
                CERT_NAME is an arbitrary name for the certificate. (e.g. formstack)

    iv) Navigate to "{FORMSTACK_CONNECTOR_HOME}/formstack-connector/formstack-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/keystores/products/" using command prompt and execute keytool -importcert -file CERT_FILE_NAME -keystore wso2carbon.jks -alias "CERT_NAME" in command line to import Formstack certificate in to keystore.
        Give "wso2carbon" as password.

        NOTE : CERT_FILE_NAME is the file name which was extracted from formstack, change it accordingly. (e.g. *.formstack.com)
               CERT_NAME is an arbitrary name for the certificate. (e.g. formstack)

 4. Create a formstack trial account and derive the API Token.
	i) 	 Using the URL "https://www.formstack.com/" create an formstack trial account.
	ii)	 In the account homepage click on user icon, choose 'API' and create a 'New Application'. Fill all the mandatory fields and 'Create Application'. Get the Access token from the next screen and retain it for further use.
	iii) Create a new folder in formstack account (click on 'New Folder' in Forms section).
	iv)  Create a new form inside the folder created in 3. iii)
		 - When creating a form, it should have at least 3 text fields (onto which data has to be entered when submitting).
		 - A form can also be created from an existing template but please make sure that there are at least 3 text fields in the form.
	v) 	 Form created in 3. iv) should have at least 2 submissions before executing the test suite.
		- Data should be entered into all the text field of the form when making a submission.
	
 5. Update the formstack properties file at location "<FORMSTACK_CONNECTOR_HOME>/formstack-connector/formstack-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/artifacts/ESB/connector/config" as below.

	01)	apiUrl -  Base endpoint URL of the API. Use https://www.formstack.com.
	02) authToken - Authentication obtained in 3. ii).
	03)	folderName - Name of the folder created in 3. iii).
	04)	formId - ID of the form created in 3. iv).
	05)	updateSubmissionTimestamp - Text to be used as 'timestamp' while updating submission. It should be in the format: yyyy-MM-dd HH:mm:ss.
	06)	updateSubmissionUserAgent - Text to be used as 'userAgent' while updating submission.
	07)	updateSubmissionRemoteAddress - RemoteAddress to be used while updating submission.
	08)	updateSubmissionPaymentStatus - Text to be used as 'paymentStatus' while updating submission.
	09)	submissionEncryptionPassword - Text to be used as 'encryptionPassword' while updating submission.
	
	10)	notificationFromType - Text to be used as 'fromType' while creating notification.
	11)	notificationFromValue - Email address to be used as 'fromValue' while creating notification. Use valid email format.
	12) notificationRecipients - Email address  to be used as 'recipients' while creating notification. Use valid email format.
	13) notificationSubject - Text to be used as 'subject' while creating notification.
	14) notificationType - Text to be used as 'type' while creating notification.
	15) notificationName - Text to be used as 'name' while creating notification.
	16) notificationMessage - Text to be used as 'message' while creating notification.
	17) notificationAttachLimit - limit to be used as 'attachLimt' while creating notification.
	18) notificationMailformat - Format to be used as 'mailFormat' while creating notification.
	19) notificationHideEmpty - Boolean value to be used as 'hideEmpty' while creating notification.
	20) notificationShowSection - Boolean value to be used as 'showSection' while creating notification.
	
	21) updateNotificationName - Text to be used as 'notification name' while updating notification.
	22) updateNotificationMessage - Text to be used as 'notification message' while updating notification.
	23) updateNotificationFromValue - Email address to be used as 'formValue' while updating notification. Use valid email format.
	24) updateNotificationRecipients - Email address to be used as 'recipients' while updating notification. Use valid email format.
	25) updateNotificationSubject - Text to be used as 'subject' while updating notification.
	
	26) confirmationToField - Text to be used as 'toField' while creating confirmation.
	27) confirmationSender - Email address to be used as 'sender' while creating confirmation. Use valid email format.
	28) confirmationSubject - Text to be used as 'confirmation subject' while creating confirmation.
	29) confirmationMessage - Text to be used as 'confirmation message' while creating confirmation.
	30) confirmationName - Text to be used as 'name' while creating confirmation.
	31) confirmationDelay - Date of delay to be used as 'delay' while creating confirmation.
	
	32) updateConfirmationName - Text to be used as 'name' while updating confirmation.
	33) updateConfirmationToField - Text to be used as 'toField' while updating confirmation.
	34) updateConfirmationSender - Email address to be used as 'sender' while updating confirmation. Use valid email format.
	35) updateConfirmationSubject - Text to be used as 'subject' while updating confirmation.
	36) updateConfirmationMessage - Text to be used as 'message' while updating confirmation.
	37) updateConfirmationDelay - Date of delay to be used as 'delay' while updating confirmation.
	
	Note: i)  Except for properties 4. 03) and 4. 04), test suite can be executed without making any changes to the provided property file.
		  ii) For each execution, repeatedly follow steps 3. iii) , 3. iv) and 3. v).
	
 6. Make sure that the formstack connector is set as a module in esb-connectors parent pom.
            <module>/formstack-connector/formstack-connector-1.0.0/org.wso2.carbon.connector/</module>

 7. Navigate to "{ESB_Connector_Home}/" and run the following command.
          $ mvn clean install

    Note:- Formstack trial account expires within 14 days.

		