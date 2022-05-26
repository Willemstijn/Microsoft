# Managing settings for two-step authentication
See: https://docs.microsoft.com/en-us/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user-manage-settings

Depending on how your company set up Azure Multi-Factor Authentication, there are a few places where you can change your settings like your phone number.

If your company support sent out a specific URL or steps to manage two-step verification, follow those instructions. Otherwise, the following instructions should work for everybody else. If you follow these steps but don't see the same options, that means that your work or school customized their own portal. Ask your admin for the link to your Azure Multi-Factor Authentication portal.

* Sign in to https://myapps.microsoft.com
* Select your account name in the top right, then select profile.
* Select Additional security verification.
* The Additional security verification page loads with your settings.
* Change the settings to your likings and click Save

## I want to change my phone number, or add a secondary number
It is important to configure a secondary authentication phone number. Because your primary phone number and your mobile app are probably on the same phone, the secondary phone number is the only way you will be able to get back into your account if your phone is lost or stolen.

To change your primary phone number:  
* On the Additional security verification page, select the text box with your current * phone number and edit it with your new phone number.
* Select Save.
* If this is the number that you use for your preferred verification option, you have * to verify the new number before you can save it.

To add a secondary phone number:  
* On the Additional security verification page, check the box next to Alternate * authentication phone.
* Enter your secondary phone number in the text box.
* Select Save and your changes are finished.

## Require two-step verification again on a device you've marked as trusted
Depending on your organization settings, you may have a checkbox that says "Don't ask again for X days" when you perform two-step verification on your browser. If you check this box and then lose your device or think that your account is compromised, you should restore two-step verification to all your devices.

* On the Additional security verification page, select Restore multi-factor * authentication on previously trusted devices.
* The next time you sign in on any device, you'll be prompted to perform two-step verification.

## How do I clean up Microsoft Authenticator from my old device and move to a new one?
When you uninstall the app from your device or reset the device, it does not remove the activation on the back end. fOR MORE INFORMATION, SEE: https://docs.microsoft.com/en-us/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to