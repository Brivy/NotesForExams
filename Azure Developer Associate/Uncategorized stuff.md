# Stuff that I don't know yet

I will categorize this chapter by service.

## Azure Notification Hubs

Azure Notification Hubs provide an easy-to-use and scaled-out push engine that enables you to send notifications to any platform (iOS, Android, Windows, etc.) from any back-end (cloud or on-premises). Notification Hubs works great for both enterprise and consumer scenarios.

### Code

Sending and receiving toast messages:

```C#
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
            "From " + user + ": " + message + "</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

// iOS
var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);

// Android
var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
outcome = await Notifications.Instance.Hub.SendFcmNativeNotificationAsync(notif, userTag);
```

Registering **templates** and sending toast messages:

```C#
// Windows 8.1 / Windows Phone 8.1
var toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
var registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);

// iOS
var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
var registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);

// Android
var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
var registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);

// Send a notification on the template
var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
```
