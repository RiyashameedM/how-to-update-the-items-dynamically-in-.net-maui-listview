# how-to-update-the-items-dynamically-in-.net-maui-listview

This examples explains about how to refresh the view when updating source dynamically using timer.

## C#

public class Behavior : Behavior<ContentPage>
{
    #region Fields

    private Syncfusion.Maui.ListView.SfListView listView;
   
    #endregion

    #region Overrides
    protected override void OnAttachedTo(ContentPage bindable)
    {
        InitializeTimer();
        listView = bindable.FindByName<Syncfusion.Maui.ListView.SfListView>("listView");
        listView.PropertyChanged += ListView_PropertyChanged;
        base.OnAttachedTo(bindable);
    }
    private void ListView_PropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        if (e.PropertyName == "ItemsSource")
            InitializeTimer();
    }

    private async void InitializeTimer()
    {
        await WaitAndExecute(2000, () =>
        {
            var viewmodel = new ContactInfoRepository();
            listView.ItemsSource = viewmodel.ContactInfo;
        });
    }

    private async Task WaitAndExecute(int milisec, Action actionToExecute)
    {
        await Task.Delay(milisec);
        actionToExecute();
    }
    protected override void OnDetachingFrom(ContentPage bindable)
    {
        listView = null;
        base.OnDetachingFrom(bindable);
    }
    #endregion
}

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.
