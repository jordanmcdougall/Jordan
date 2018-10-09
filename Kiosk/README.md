# Windows 10 Kiosk Mode

Using a Windows 10 device, the Smart Factory web interface can be presented to the user in Kiosk Mode using the in-built Windows 10 feature Assigned Access. This limits the user interaction only to a Microsoft Edge Browser Session such that the user cannot do anything on the device outside of the kiosk app.

*Note: This feature requires Windows 10, Version 1809*.

### Configuation

The following steps can be used to configuration a Windows 10 Device to run in Assigned Access (Kiosk Mode):

#### Set up a User Account

Login to the Windows 10 device as a local user.

Navigate to Start > Settings > Accounts > Other Users

Select Set up a kiosk > Assigned access, and then select Get started.

![Setup](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/01_setup_a_kiosk.png)

#### Create a Local User

When prompted enter a new Display Name for the account. E.g. DEL-KIOSK-USER. 

This will create a new local user account named **kioskUser0** with a Display Name that was just defined, then click **Next**.

![Create Account](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/02_create_an_account.png)

#### Choose a Kiosk App

Select **Microsoft Edge** from the list of Kiosk apps, then click **Next**.

![ChooseApp](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/03_choose_a_kiosk_app.png)

#### Run Kiosk App in Fullscreen

Choose the **As a digital sign or interactive display** radio button, then click **Next**. 

This will remove Microsoft Edge features like address bar and forward/back buttons that would allow users to navigate away from the inteded website.

![Fullscreen](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/04_fullscreen.png)

#### Select Kiosk Website

Use the Textbox to enter the URL of the website that should be displayed, then click **Next**.

![Website](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/05_website_url.png)

If successfull, click **Close**.

![Done](https://github.com/jordanmcdougall/Jordan/blob/master/Kiosk/static/images/06_done.png)

To access the kiosk app, sign-out and sign in as the local user **kioskUser0**.
