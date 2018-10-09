# Kiosk

Using a Windows 10 device, the Smart Factory web interface can be presented to the user in Kiosk Mode using the in-built Windows 10 feature Assigned Access. This limits the user interaction only to a Microsoft Edge Browser Session such that the user cannot do anything on the device outside of the kiosk app.

*Note: This feature requires Windows 10, Version 1809*

### Configuation

The following steps can be used to configuration a Windows 10 Device to run in Assigned Access (Kiosk Mode):

Login to the Windows 10 device as a local user.

Navigate to Start > Settings > Accounts > Other Users

Select Set up a kiosk > Assigned access, and then select Get started.

![Setup](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/01_setup_a_kiosk.png)

When prompted enter a new name for the account. E.g. DEL-KIOSK-USER 


Note this name is the Display Name and is not the User Logon Name\SAMAccount Name. This will create a new user to run the browser in Assigned Access mode.


