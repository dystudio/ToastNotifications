```
 _______              _   _   _       _   _  __ _           _   _                         ___  
|__   __|            | | | \ | |     | | (_)/ _(_)         | | (_)                       |__ \
   | | ___   __ _ ___| |_|  \| | ___ | |_ _| |_ _  ___ __ _| |_ _  ___  _ __  ___   __   __ ) |
   | |/ _ \ / _` / __| __| . ` |/ _ \| __| |  _| |/ __/ _` | __| |/ _ \| '_ \/ __|  \ \ / // /
   | | (_) | (_| \__ \ |_| |\  | (_) | |_| | | | | (_| (_| | |_| | (_) | | | \__ \   \ V // /_
   |_|\___/ \__,_|___/\__|_| \_|\___/ \__|_|_| |_|\___\__,_|\__|_|\___/|_| |_|___/    \_/|____|

```

# ToastNotifications v2
#### Toast notifications for WPF

## Configuration
[Example code](https://github.com/raflop/ToastNotifications/tree/master-v2/Src/Examples/ConfigurationExample)

### Notification position

Using PositionProvider option you can specify the place where notifications should appear.

There are three built-in position providers:
 * WindowPositionProvider - notifications are tracking position of specified window.
 * PrimaryScreenPositionProvider - notifications appears always on main screen in specified position.
 * ControlPositionProvider - notifications are tracking position of specified UI control.

 Options like "corner", "offsetX", "offsetY" are used to set distance beetween notifications area and specified corner of Window/Screen/Control.

```csharp
// WindowPositionProvider
Notifier notifier = new Notifier(cfg =>
{
    cfg.PositionProvider = new WindowPositionProvider(
        parentWindow: Application.Current.MainWindow,
        corner: Corner.TopRight,
        offsetX: 10,  
        offsetY: 10);
    /* * */
});

// PrimaryScreenPositionProvider
Notifier notifier = new Notifier(cfg =>
{
    cfg.PositionProvider = new PrimaryScreenPositionProvider(
        corner: Corner.BottomRight,
        offsetX: 10,  
        offsetY: 10);
    /* * */
});

// PrimaryScreenPositionProvider
Notifier notifier = new Notifier(cfg =>
{
    MainWindow mainWindow = Application.Current.MainWindow as MainWindow;
    FrameworkElement trackingElement = mainWindow?.TrackingElement;

    cfg.PositionProvider = new ControlPositionProvider(
        parentWindow: mainWindow,
        trackingElement: trackingElement,
        corner: Corner.BottomLeft,
        offsetX: 10,  
        offsetY: 10);
    /* * */
});
```
### Notification lifetime

Using LifetimeSupervisor option you can specify when notifications will disappear.

There are two built-in lifetime supervisors:
 * CountBasedLifetimeSupervisor - notifications will disappear only when number of notifications on screen will be  will be more than MaximumNotificationCount
 * TimeAndCountBasedLifetimeSupervisor - notifications will disappear when the number of notifications on screen will be more than MaximumNotificationCount or the notification will be on screen longer than specified amount of time

If you need unlimited number of notifications on screen use CountBasedLifetimeSupervisor with MaximumNotificationCount.UnlimitedNotifications() option

```csharp
// CountBasedLifetimeSupervisor
Notifier notifier = new Notifier(cfg =>
{
    cfg.LifetimeSupervisor = new CountBasedLifetimeSupervisor(maximumNotificationCount: MaximumNotificationCount.UnlimitedNotifications());
    /* * */
});

// TimeAndCountBasedLifetimeSupervisor
Notifier notifier = new Notifier(cfg =>
{
    cfg.LifetimeSupervisor = new TimeAndCountBasedLifetimeSupervisor(
        notificationLifetime: TimeSpan.FromSeconds(3),
        maximumNotificationCount: MaximumNotificationCount.FromCount(5));
    /* * */
});

```

### Dispatcher
```csharp
Set the dispatcher used by library

Notifier notifier = new Notifier(cfg =>
{
    cfg.Dispatcher = Application.Current.Dispatcher;
    /* * */
});
```

### Notifier DisplayOptions
```csharp
Notifier notifier = new Notifier(cfg =>
{
    cfg.DisplayOptions.TopMost = true; // set the option to show notifications over other windows
    cfg.DisplayOptions.Width = 250; // set the notifications width
    /* * */
});
```

### Message options
```csharp

using ToastNotifications.Messages.Core;
/* * */
var options =  new MessageOptions{
    FontSize = 30, // set notification font size
    ShowCloseButton = false // set the option to show or hide notification close button
    NotificationClickAction = n => // set the callback for notification click event
    {
        n.Close(); // call Close method to remove notification
        notifier.ShowSuccess("clicked!");
    }
};
/* * */
notifier.ShowError(message, options);
```
