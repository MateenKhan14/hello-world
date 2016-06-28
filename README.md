# hello-world
sample
// interfaceMapper
namespace OpticalSimulationCore.Service.Facade
{
    /// <summary>
    /// This interface is used to define a contract for WindowViewModelMappings
    /// </summary>
    public interface IViewModelMapper
    {
        /// <summary>
        /// This method is used to get the window type from the view model type
        /// </summary>
        /// <param name="viewModelType">View Model Type</param>
        /// <returns>Window Type as per view model type</returns>
        Type GetViewTypeFromViewModelType(Type viewModelType);
    }
}

///vm mapper

public class ViewModelMapper : IViewModelMapper
    {
        /// <summary>
        /// This field is used to store the mappings for window and view model
        /// </summary>
        private readonly IDictionary<Type, Type> windowViewModelMappings;

        /// <summary>
        /// Constructor for WindowViewModelMappings
        /// </summary>
        public ViewModelMapper()
        {
            windowViewModelMappings = new Dictionary<Type, Type>();
            windowViewModelMappings.Add(typeof(MainViewViewModel), typeof(MainView));
            windowViewModelMappings.Add(typeof(SetLayOutViewModel), typeof(AddNewItem));
            windowViewModelMappings.Add(typeof(ComparisonViewModel), typeof(Comparison));
            this.windowViewModelMappings.Add(typeof(RevisionHistoryDataViewModel), typeof(RevisionHistoryData));
        }

        /// <summary>
        /// This method is used to get the type of window which is associated with the given view model type
        /// </summary>
        /// <param name="viewModelType">The view model</param>
        /// <returns>The window type as per the given view model</returns>
        public Type GetViewTypeFromViewModelType(Type viewModelType)
        {
            return windowViewModelMappings[viewModelType];
        }
    }
    
    /// Window mapping
    
     private bool? ShowDialog(object ownerViewModel, object viewModel, Type dialogType)
        {
            // Create Dialog and set properties
            var window = (Window)Activator.CreateInstance(dialogType);
            window.Owner = FindOwnerWindow(ownerViewModel);
            window.DataContext = viewModel;

            // Show the window as a dialog
            return window.ShowDialog();
        }

        /// <summary>
        /// This method is used to get the owner window from the given owner view model
        /// </summary>
        /// <param name="ownerViewModel">The owner window view model</param>
        /// <returns>Window as per the given view model otherwise null</returns>
        private Window FindOwnerWindow(object ownerViewModel)
        {
            var view = views.FirstOrDefault(v => v.DataContext == ownerViewModel);

            // NOTE: The below code from IViewModelMapper acts as Fail-Safe 
            // if view was not registered through IsRegisteredView attached property
            if(view == null)
            {
                var type = ServiceLocator.Resolve<IViewModelMapper>().GetViewTypeFromViewModelType(ownerViewModel.GetType());
                view = Application.Current.Windows.Cast<FrameworkElement>().First(window => window.GetType() == type);
            }

            if (view == null)
            {
                throw new ArgumentException("Viewmodel is not referenced by any registered view");
            }

            // Get owner window
            var ownerWindow = view as Window ?? Window.GetWindow(view);

            if (ownerWindow == null)
            {
                throw new InvalidOperationException("View is not contained with in a window");
            }

            return ownerWindow;
        }
        
        // code for grid view
        <Style TargetType="{x:Type DataGridColumnHeader}">
            <Setter Property="VerticalContentAlignment" Value="Center" />
            <Setter Property="HorizontalContentAlignment" Value="Center" />
            <Setter Property="SeparatorBrush" Value="WhiteSmoke" />
            <Setter Property="FontWeight" Value="Bold" />
            <Setter Property="Height" Value="30" />
        </Style>
         x:Name="MainWiew"      
           Title="{Binding MainTitle}" Height="625" Width="935" WindowState="Maximized" 
 xmlns:av="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
 Icon="/OpticalSimulation;component/opticalicon.ico"
            av:RenderOptions.BitmapScalingMode="HighQuality">
            
             <Button ToolTip="Open" Name="OpenFileBtn" Style="{StaticResource ToolBarButtonColor}" Command="{Binding OpenFileCommand}">
                <Image Source="../Resources/Images/Open.png" Name="OpenFileimg"/>
            </Button>
            
             <Style x:Key="ToolBarButtonColor" TargetType="{x:Type Button}" BasedOn="{StaticResource {x:Static ToolBar.ButtonStyleKey}}">
            <Setter Property="Background" Value="Transparent"/>
            <Setter Property="BorderBrush" Value="Transparent"/>    
            <Style.Triggers>
                <Trigger Property="IsEnabled" Value="False">
                    <Setter Property="Opacity" Value="0.5"/>
                </Trigger>
                <Trigger Property="IsEnabled" Value="True">
                    <Setter Property="Opacity" Value="1"/>
                </Trigger>
            </Style.Triggers>
        </Style>
