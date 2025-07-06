# API Reference

Complete API documentation for ImagePickerKMP library.

## Table of Contents

- [Core Components](#core-components)
- [Data Classes](#data-classes)
- [Enums](#enums)
- [Exceptions](#exceptions)
- [Configuration](#configuration)
- [Platform-Specific APIs](#platform-specific-apis)

## Core Components

### ImagePickerLauncher

The main composable function for launching the image picker.

```kotlin
@Composable
fun ImagePickerLauncher(
    context: Any?,
    onPhotoCaptured: (PhotoResult) -> Unit,
    onError: (Exception) -> Unit,
    customPermissionHandler: ((PermissionConfig) -> Unit)? = null,
    customConfirmationView: (@Composable (PhotoResult, (PhotoResult) -> Unit, () -> Unit) -> Unit)? = null,
    preference: CapturePhotoPreference? = null
)
```

#### Parameters

- `context: Any?` - Context for Android, null for iOS
- `onPhotoCaptured: (PhotoResult) -> Unit` - Callback when photo is captured
- `onError: (Exception) -> Unit` - Callback when error occurs
- `customPermissionHandler: ((PermissionConfig) -> Unit)?` - Custom permission handling
- `customConfirmationView: (@Composable (PhotoResult, (PhotoResult) -> Unit, () -> Unit) -> Unit)?` - Custom confirmation view
- `preference: CapturePhotoPreference?` - Photo capture preferences

#### Example

```kotlin
@Composable
fun MyImagePicker() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            println("Photo captured: ${result.uri}")
        },
        onError = { exception ->
            println("Error: ${exception.message}")
        }
    )
}
```

### RequestCameraPermission

Composable for handling camera permissions.

```kotlin
@Composable
fun RequestCameraPermission(
    titleDialogConfig: String,
    descriptionDialogConfig: String,
    btnDialogConfig: String,
    titleDialogDenied: String,
    descriptionDialogDenied: String,
    btnDialogDenied: String,
    customDeniedDialog: @Composable ((onRetry: () -> Unit) -> Unit)?,
    customSettingsDialog: @Composable ((onOpenSettings: () -> Unit) -> Unit)?,
    onPermissionPermanentlyDenied: () -> Unit,
    onResult: (Boolean) -> Unit,
    customPermissionHandler: (() -> Unit)?
)
```

#### Parameters

- `titleDialogConfig: String` - Title for settings dialog
- `descriptionDialogConfig: String` - Description for settings dialog
- `btnDialogConfig: String` - Button text for settings dialog
- `titleDialogDenied: String` - Title for retry dialog
- `descriptionDialogDenied: String` - Description for retry dialog
- `btnDialogDenied: String` - Button text for retry dialog
- `customDeniedDialog: @Composable ((onRetry: () -> Unit) -> Unit)?` - Custom retry dialog
- `customSettingsDialog: @Composable ((onOpenSettings: () -> Unit) -> Unit)?` - Custom settings dialog
- `onPermissionPermanentlyDenied: () -> Unit` - Callback when permission is permanently denied
- `onResult: (Boolean) -> Unit` - Callback with permission result
- `customPermissionHandler: (() -> Unit)?` - Custom permission handler

#### Example

```kotlin
@Composable
fun CustomPermissionHandler() {
    RequestCameraPermission(
        titleDialogConfig = "Camera Permission Required",
        descriptionDialogConfig = "Please enable camera access in settings",
        btnDialogConfig = "Open Settings",
        titleDialogDenied = "Permission Denied",
        descriptionDialogDenied = "Camera permission is required",
        btnDialogDenied = "Grant Permission",
        customDeniedDialog = null,
        customSettingsDialog = null,
        onPermissionPermanentlyDenied = {
            println("Permission permanently denied")
        },
        onResult = { granted ->
            println("Permission granted: $granted")
        }
    )
}
```

### CameraCaptureView

Composable for camera capture interface (Android only).

```kotlin
@Composable
fun CameraCaptureView(
    activity: ComponentActivity,
    preference: CapturePhotoPreference = CapturePhotoPreference.FAST,
    onPhotoResult: (PhotoResult) -> Unit,
    onError: (Exception) -> Unit,
    customPermissionHandler: ((PermissionConfig) -> Unit)? = null,
    customConfirmationView: (@Composable (PhotoResult, (PhotoResult) -> Unit, () -> Unit) -> Unit)? = null
)
```

#### Parameters

- `activity: ComponentActivity` - Android activity
- `preference: CapturePhotoPreference` - Photo capture preferences
- `onPhotoResult: (PhotoResult) -> Unit` - Callback when photo is captured
- `onError: (Exception) -> Unit` - Callback when error occurs
- `customPermissionHandler: ((PermissionConfig) -> Unit)?` - Custom permission handling
- `customConfirmationView: (@Composable (PhotoResult, (PhotoResult) -> Unit, () -> Unit) -> Unit)?` - Custom confirmation view

## Data Classes

### PhotoResult

Represents a captured photo.

```kotlin
data class PhotoResult(
    val uri: Uri,
    val size: Long,
    val format: String,
    val width: Int,
    val height: Int
)
```

#### Properties

- `uri: Uri` - URI of the captured photo
- `size: Long` - Size of the photo in bytes
- `format: String` - Format of the photo (JPEG, PNG, etc.)
- `width: Int` - Width of the photo in pixels
- `height: Int` - Height of the photo in pixels

#### Example

```kotlin
@Composable
fun PhotoResultExample() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            println("Photo URI: ${result.uri}")
            println("Photo size: ${result.size} bytes")
            println("Photo format: ${result.format}")
            println("Photo dimensions: ${result.width}x${result.height}")
        },
        onError = { exception ->
            println("Error: ${exception.message}")
        }
    )
}
```

### PermissionConfig

Configuration for permission dialogs.

```kotlin
data class PermissionConfig(
    val titleDialogConfig: String = TITLE_DIALOG_CONFIG,
    val descriptionDialogConfig: String = DESCRIPTION_DIALOG_CONFIG,
    val btnDialogConfig: String = BTN_DIALOG_CONFIG,
    val titleDialogDenied: String = TITLE_DIALOG_DENIED,
    val descriptionDialogDenied: String = DESCRIPTION_DIALOG_DENIED,
    val btnDialogDenied: String = BTN_DIALOG_DENIED
)
```

#### Properties

- `titleDialogConfig: String` - Title for settings dialog
- `descriptionDialogConfig: String` - Description for settings dialog
- `btnDialogConfig: String` - Button text for settings dialog
- `titleDialogDenied: String` - Title for retry dialog
- `descriptionDialogDenied: String` - Description for retry dialog
- `btnDialogDenied: String` - Button text for retry dialog

#### Example

```kotlin
val customPermissionConfig = PermissionConfig(
    titleDialogConfig = "Camera Access Required",
    descriptionDialogConfig = "This app needs camera access to take photos",
    btnDialogConfig = "Open Settings",
    titleDialogDenied = "Permission Denied",
    descriptionDialogDenied = "Please grant camera permission to continue",
    btnDialogDenied = "Try Again"
)
```

## Enums

### CapturePhotoPreference

Photo capture quality preferences.

```kotlin
enum class CapturePhotoPreference {
    FAST,           // Fast capture, lower quality
    BALANCED,       // Balanced quality and speed
    HIGH_QUALITY    // High quality, slower capture
}
```

#### Values

- `FAST` - Optimized for speed, lower quality
- `BALANCED` - Balanced quality and performance
- `HIGH_QUALITY` - Maximum quality, slower capture

#### Example

```kotlin
@Composable
fun HighQualityImagePicker() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Handle high quality photo
        },
        onError = { exception ->
            // Handle errors
        },
        preference = CapturePhotoPreference.HIGH_QUALITY
    )
}
```

## Exceptions

### CameraPermissionException

Thrown when camera permission is denied.

```kotlin
class CameraPermissionException(message: String) : Exception(message)
```

#### Example

```kotlin
@Composable
fun PermissionExceptionHandler() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Handle photo capture
        },
        onError = { exception ->
            when (exception) {
                is CameraPermissionException -> {
                    println("Camera permission denied: ${exception.message}")
                    showPermissionErrorDialog()
                }
                else -> {
                    println("Other error: ${exception.message}")
                }
            }
        }
    )
}
```

### PhotoCaptureException

Thrown when photo capture fails.

```kotlin
class PhotoCaptureException(message: String) : Exception(message)
```

#### Example

```kotlin
@Composable
fun CaptureExceptionHandler() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Handle photo capture
        },
        onError = { exception ->
            when (exception) {
                is PhotoCaptureException -> {
                    println("Photo capture failed: ${exception.message}")
                    showCaptureErrorDialog()
                }
                else -> {
                    println("Other error: ${exception.message}")
                }
            }
        }
    )
}
```

## Configuration

### Constants

```kotlin
object Constant {
    const val TITLE_DIALOG_CONFIG: String = "Camera permission required"
    const val DESCRIPTION_DIALOG_CONFIG: String = "Camera permission is required to capture photos. Please grant it in settings"
    const val BTN_DIALOG_CONFIG: String = "Open settings"
    const val TITLE_DIALOG_DENIED: String = "Camera permission denied"
    const val DESCRIPTION_DIALOG_DENIED: String = "Camera permission is required to capture photos. Please grant the permissions"
    const val BTN_DIALOG_DENIED: String = "Grant permission"
    const val PERMISSION_COUNTER: Int = 0
    const val PERMISSION_PERMANENTLY_DENIED: String = "Camera permission permanently denied"
    const val BTN_ACCEPT: String = "Accept"
    const val BTN_RETRY: String = "Retry"
}
```

### Default Values

```kotlin
// Default permission configuration
val defaultPermissionConfig = PermissionConfig()

// Default photo capture preference
val defaultPreference = CapturePhotoPreference.FAST
```

## Platform-Specific APIs

### Android

#### CameraXManager

Manages camera functionality on Android.

```kotlin
class CameraXManager(
    private val context: Context,
    private val activity: ComponentActivity
) {
    fun startCamera()
    fun stopCamera()
    fun capturePhoto(onPhotoCaptured: (PhotoResult) -> Unit)
}
```

#### Example

```kotlin
@Composable
fun AndroidSpecificExample() {
    val context = LocalContext.current
    
    if (context is ComponentActivity) {
        val cameraManager = remember { CameraXManager(context, context) }
        
        DisposableEffect(Unit) {
            onDispose {
                cameraManager.stopCamera()
            }
        }
        
        // Use camera manager
        Button(onClick = {
            cameraManager.capturePhoto { result ->
                println("Photo captured: ${result.uri}")
            }
        }) {
            Text("Capture Photo")
        }
    }
}
```

### iOS

#### CameraPresenter

Manages camera functionality on iOS.

```kotlin
object CameraPresenter {
    fun presentCamera(
        viewController: UIViewController,
        onPhotoCaptured: (PhotoResult) -> Unit,
        onError: (Exception) -> Unit
    )
}
```

#### Example

```kotlin
@Composable
fun IOSSpecificExample() {
    // iOS-specific implementation
    // Note: This is handled internally by ImagePickerLauncher
    ImagePickerLauncher(
        context = null, // iOS doesn't need context
        onPhotoCaptured = { result ->
            println("Photo captured: ${result.uri}")
        },
        onError = { exception ->
            println("Error: ${exception.message}")
        }
    )
}
```

## Utility Functions

### Permission Utilities

```kotlin
// Check camera permission status
fun checkCameraPermission(context: Context): Boolean

// Request camera permission
fun requestCameraPermission(
    context: Context,
    onGranted: () -> Unit,
    onDenied: () -> Unit
)

// Open app settings
fun openAppSettings(context: Context)
```

#### Example

```kotlin
@Composable
fun PermissionUtilitiesExample() {
    val context = LocalContext.current
    
    LaunchedEffect(Unit) {
        if (checkCameraPermission(context)) {
            println("Camera permission already granted")
        } else {
            requestCameraPermission(
                context = context,
                onGranted = {
                    println("Camera permission granted")
                },
                onDenied = {
                    println("Camera permission denied")
                    openAppSettings(context)
                }
            )
        }
    }
}
```

### Image Processing Utilities

```kotlin
// Process captured image
fun processImage(uri: Uri): Bitmap

// Compress image
fun compressImage(uri: Uri, quality: Int): Uri

// Resize image
fun resizeImage(uri: Uri, width: Int, height: Int): Uri
```

#### Example

```kotlin
@Composable
fun ImageProcessingExample() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Process the captured image
            lifecycleScope.launch(Dispatchers.IO) {
                val processedImage = processImage(result.uri)
                val compressedImage = compressImage(result.uri, 80)
                val resizedImage = resizeImage(result.uri, 1024, 1024)
                
                // Use processed images
                withContext(Dispatchers.Main) {
                    // Update UI with processed images
                }
            }
        },
        onError = { exception ->
            println("Error: ${exception.message}")
        }
    )
}
```

## Error Handling

### Common Error Scenarios

```kotlin
@Composable
fun ComprehensiveErrorHandling() {
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Handle successful photo capture
        },
        onError = { exception ->
            when (exception) {
                is CameraPermissionException -> {
                    // Handle permission errors
                    showPermissionErrorDialog()
                }
                is PhotoCaptureException -> {
                    // Handle capture errors
                    showCaptureErrorDialog()
                }
                is SecurityException -> {
                    // Handle security errors
                    showSecurityErrorDialog()
                }
                is OutOfMemoryError -> {
                    // Handle memory errors
                    showMemoryErrorDialog()
                }
                else -> {
                    // Handle generic errors
                    showGenericErrorDialog(exception.message)
                }
            }
        }
    )
}
```

### Error Recovery

```kotlin
@Composable
fun ErrorRecoveryExample() {
    var retryCount by remember { mutableStateOf(0) }
    val maxRetries = 3
    
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Reset retry count on success
            retryCount = 0
            // Handle photo capture
        },
        onError = { exception ->
            if (retryCount < maxRetries) {
                retryCount++
                // Retry after delay
                delay(1000)
                // Show retry dialog
                showRetryDialog {
                    // Retry logic
                }
            } else {
                // Max retries reached
                showMaxRetriesDialog()
            }
        }
    )
}
```

## Best Practices

### 1. Memory Management

```kotlin
@Composable
fun MemoryEfficientImagePicker() {
    var imageUri by remember { mutableStateOf<Uri?>(null) }
    
    ImagePickerLauncher(
        context = LocalContext.current,
        onPhotoCaptured = { result ->
            // Store URI instead of Bitmap to save memory
            imageUri = result.uri
        },
        onError = { exception ->
            // Handle errors
        }
    )
    
    // Load image only when needed
    imageUri?.let { uri ->
        AsyncImage(
            model = uri,
            contentDescription = "Captured photo",
            modifier = Modifier.fillMaxSize()
        )
    }
}
```

### 2. Lifecycle Management

```kotlin
@Composable
fun LifecycleAwareImagePicker() {
    val context = LocalContext.current
    val lifecycleOwner = LocalLifecycleOwner.current
    
    DisposableEffect(lifecycleOwner) {
        val observer = LifecycleEventObserver { _, event ->
            when (event) {
                Lifecycle.Event.ON_PAUSE -> {
                    // Pause camera when app goes to background
                }
                Lifecycle.Event.ON_RESUME -> {
                    // Resume camera when app comes to foreground
                }
                else -> {}
            }
        }
        
        lifecycleOwner.lifecycle.addObserver(observer)
        
        onDispose {
            lifecycleOwner.lifecycle.removeObserver(observer)
        }
    }
    
    ImagePickerLauncher(
        context = context,
        onPhotoCaptured = { result ->
            // Handle photo capture
        },
        onError = { exception ->
            // Handle errors
        }
    )
}
```

### 3. Performance Optimization

```kotlin
@Composable
fun OptimizedImagePicker() {
    val context = LocalContext.current
    
    // Use remember to avoid recreating callbacks
    val onPhotoCaptured = remember {
        { result: PhotoResult ->
            // Handle photo capture
        }
    }
    
    val onError = remember {
        { exception: Exception ->
            // Handle errors
        }
    }
    
    ImagePickerLauncher(
        context = context,
        onPhotoCaptured = onPhotoCaptured,
        onError = onError
    )
}
```

## Support

For API-related issues:

1. **Check the documentation**: Review this API reference
2. **Test examples**: Try the provided examples
3. **Check compatibility**: Ensure you're using compatible versions
4. **Contact support**: Reach out for API-specific help

For more information, refer to:
- [Integration Guide](INTEGRATION_GUIDE.md)
- [Examples](EXAMPLES.md)
- [Customization Guide](CUSTOMIZATION_GUIDE.md) 