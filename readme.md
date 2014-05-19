Cross platform way to migrate PCL Resources over to Android Resources

Put all of your resources in a PCL *.resx file, and run this with your build. It will automatically 
generate your *.xml resource file for android, and a CustomUIColor.cs class for iOS (other iOS 
support to come).

note: start your file name with the type of resource you're generating

IE:
 
    bools.resx       // will generate bool resources
    dimensions.resx  // will generate dimen resources
    integers.resx    // will generate int resources
    colors.resx      // will generate color resources
    items.resx       // will generate item resources
    strings.resx     // will generate string resources
    foo-bar.resx     // will ALSO generate string resources

Usage:

    // generate resources by in default namespace  
    ResourceMigrator.exe /path/to/pcl/resource/dir /my-android-app/resources/values/ /my-touch-app/resources

    // generate resources with a custom namespace  
    ResourceMigrator.exe /path/to/pcl/resource/dir /my-android-app/resources/values/ /my-touch-app/resources MyApp.Custom.Touch.Namespace

Here's an example of our Powershell build script (using PSAKE)

    $defaultResourcePath = Join-Path $rootDir "FutureState.AppCore\Properties\"
    $androidResourcePath = Join-Path $rootDir "FutureState.BreathingRoom.Droid.Ui\Resources\Values\"
    $touchOutputPath =     Join-Path $rootDir "FutureState.BreathingRoom.Touch.Ui\Resources\"
    $migratorPath =        Join-Path $rootDir "build\ResourceMigrator.exe"
    $customUIColorNamespace = "FutureState.BreathingRoom.Touch.Ui.Resources"
    
    task MigrateResources -Description "Generates Android & iOS Resources from the AppCore Resource.resx file" {
        & $migratorPath $defaultResourcePath $androidResourcePath $touchOutputPath $customUIColorNamespace
    }
